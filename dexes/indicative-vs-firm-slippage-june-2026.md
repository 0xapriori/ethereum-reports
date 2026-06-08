---
title: "Indicative vs. Firm: Where Swap Aggregators Hide Slippage"
date: 2026-06-07
---

# Indicative vs. Firm: Where Swap Aggregators Hide Slippage

> A map of which Ethereum swap venues use indicative/fadeable RFQ, firm-signed quotes, or batch-uniform-clearing — and where the slippage-tolerance attack surface actually lives.

**Author:** apriori
**Date:** June 2026

## tl;dr

- **Overquoting is a symptom; the disease is the free option.** Every time a user sets a slippage tolerance, they write a standing option to their counterparty: *fill me anywhere between the quoted price and my worst-case floor.* That option has been trained into users since Uniswap v1, where the tolerance band was load-bearing for a constant-product curve sitting in a public mempool. The band is not a safety feature. It is unpriced optionality the user gives away on every trade.
- **One inherited primitive, two predators.** The same tolerance band is the attack surface for *both* sandwich MEV and RFQ overquoting. A sandwich bot reverse-engineers your `amountOutMin` and sizes its front-run to fill you at the floor. An RFQ maker wins the aggregator's display ranking with a tight indicative, then fills at the band floor or fades. Different venues, same exploited object.
- **On-chain RFQ settlement is usually firm — so true last-look is hard to reproduce on-chain.** The maker signs an EIP-712 quote with fixed amounts, expiry, and a single-use nonce; the settlement contract enforces it atomically and reverts otherwise. There is no hold-window in which the maker re-prices *after* the taker commits, the way FX last-look works. This is the nuance most write-ups miss.
- **The real overquoting lives in the indicative→firm transition.** Because the *signed* leg is firm, the abuse migrates upstream: a maker streams a tight *soft* quote to win the router's ranking, then returns a worse *firm* quote, refuses to sign, or lets the short TTL lapse. 0x RFQ-M, Native FirmQuote, and the generic 0x `/price`→`/quote` split are exactly this surface. The contract can only enforce whatever order finally got signed; it cannot enforce the indicative.
- **A firm limit has no band to attack.** Batch auctions (CoW), firm-quote RFQ (Hashflow, Bebop PMM, Clipper), and committed-output intents (Across, deBridge DLN, UniswapX's signed floor) all collapse the indicative/firm gap. There is no tolerance band for a sandwich bot to size against and no indicative quote for a maker to fade below.
- **But slippage does not vanish — it relocates.** On an AMM over a public mempool, zero tolerance means reverts, not free execution. The defensible upgrade is not "kill slippage"; it is to stop treating slippage as an *unpriced option held by an uninformed user* and start treating it as a *priced commitment held by a professional filler* — priced into the spread, with surplus above the floor returned to the user. That is the axis this report maps venues onto.
- **Volume is not revenue.** Where a 30-day DefiLlama volume is present in the data, it is labeled as *volume only*. Several figures are all-chain aggregates, not Ethereum-isolated, and are flagged as such. No fee or revenue figure is implied by any volume number here.

## Table of Contents

- [1. The free option no one prices](#1-the-free-option-no-one-prices)
- [2. One band, two predators](#2-one-band-two-predators)
- [3. Why on-chain RFQ is firm — and where the fade actually happens](#3-why-on-chain-rfq-is-firm--and-where-the-fade-actually-happens)
- [4. The taxonomy table](#4-the-taxonomy-table)
- [5. The five quote models, venue by venue](#5-the-five-quote-models-venue-by-venue)
- [6. Where the surplus goes — the second axis](#6-where-the-surplus-goes--the-second-axis)
- [7. The defensible claim: priced commitment, not unpriced option](#7-the-defensible-claim-priced-commitment-not-unpriced-option)
- [8. What's open](#8-whats-open)
- [Sources](#sources)

## 1. The free option no one prices

Start with the primitive, because everything else is downstream of it.

When you swap on a constant-product AMM, you set a slippage tolerance. The UI converts that percentage into an on-chain floor — `amountOutMin` on the way out, `amountInMax` on the way in — and the router reverts if execution would breach it. Uniswap's own documentation is plain about what the setting is: "the maximum percentage you'll allow the quoted price to move," and "if the price moves beyond your tolerance, the swap won't go through." The default presets (0.5%, 1%) are UI conventions that have propagated to nearly every aggregator since.

The band exists for a real reason. Two distinct uncertainties stack between the moment you see a quote and the moment your transaction is included:

1. **Deterministic price impact.** The `x*y=k` invariant moves the pool ratio as your trade consumes liquidity. This part is computable from trade size and pool depth.
2. **Pending-block drift.** Other trades land in the same or an earlier block, moving the pool before yours executes. This part is non-deterministic.

The tolerance band absorbs both. Without it, on an AMM in a public mempool, any adverse drift between quote and inclusion reverts the transaction. So zero tolerance does not buy you a perfect fill — it buys you a failed trade. Hold this; it is the honest counterpoint that the rest of the report has to respect.

But notice what the band *is*, structurally. It is an option you write to whoever ends up on the other side of your trade. You have told the system: *I will accept any execution between the quoted price and this floor.* You priced that option at zero. You receive nothing for granting it. And the party best positioned to exercise it — a searcher who can observe your floor, or a maker who quoted you to win ranking — is strictly more informed than you about where the real price is at fill time.

That is the disease. Overquoting and sandwiching are two symptoms of the same underlying condition: **the user holds a free, standing slippage-tolerance option, and someone better-informed gets to exercise it.** The slippage primitive is not a property of "trading on Ethereum." It is a property of *pool-routed AMM execution in a public mempool*, and as we will see, it disappears entirely under firm signed quotes and batch clearing. The fact that users carry the band into venues where it is no longer load-bearing — RFQ, intents, batch auctions — is the inherited reflex worth naming.

## 2. One band, two predators

The two attacks are usually discussed in separate literatures: MEV/sandwich research on one side, RFQ/market-making discourse on the other. They are the same exploit on the same object.

**Predator one: the sandwich bot.** Your signed transaction sits in the public mempool with `amountOutMin` encoded in the calldata. That floor is the *worst price you have pre-committed to accept*. A front-running bot reads it, buys exactly enough to push the pool price until you would fill at approximately `amountOutMin`, lets your trade execute at that floor, then back-runs to unwind. The bot's optimal extraction is bounded by your tolerance: a 2% setting lets a well-tuned bot extract up to roughly the full 2% while keeping your trade just inside the band so it doesn't revert. The Speedrun Ethereum MEV guide describes the mechanic directly — the bot "buys before you, pushing the price up; your trade executes at a worse price (max slippage)." The band is the bot's budget.

**Predator two: the RFQ overquoter.** Now move to an aggregator that polls market makers. The maker's incentive is to win the router's display ranking, because the best-displayed quote captures the flow. So the maker shows a tight *indicative* price. If the maker can then fill nearer your floor than your displayed quote — or decline to commit when the market has moved against it — the indicative was a marketing number, not a price. The user, who set a tolerance band to be safe, has again handed the better-informed party an option to fill within that band.

The symmetry is exact. In both cases:

- The user commits to a worst-case floor *before* knowing where the real price will be at execution.
- The counterparty learns more about the real price *between* the user's commitment and execution.
- The gap between displayed quote and enforced floor is the extractable region.

This is why the report treats "where does slippage hide" as a single question across sandwiching and overquoting. The hiding place is the same: the region between the number the user was shown and the floor the user signed. Every venue in the taxonomy below either keeps that region open (AMM-route-with-slippage, indicative-then-firm) or collapses it (firm-signed, batch-uniform-clearing, committed-output intent).

## 3. Why on-chain RFQ is firm — and where the fade actually happens

Here is the nuance that has to be central, because getting it wrong produces a sloppy report that conflates on-chain RFQ with FX last-look. They are not the same thing.

**FX last-look — the reference contrast.** In OTC FX, a liquidity provider streams a *non-firm* quote. After the taker requests to trade, the LP applies an additional hold time (AHT), re-evaluates against market data that arrived *after* the request, and then chooses to fill, requote, or reject. The defining abuse — the one that drew regulatory investigations and fines, and that the FX Global Code pushed toward symmetric last-look — is asymmetric optionality: reject the trades that moved against the LP during the hold window, fill the ones that moved in its favor. The taker has no firm execution guarantee at the quoted price. The re-price happens *after* the taker commits.

**On-chain RFQ structurally cannot do this.** When a maker signs an EIP-712 RFQ order, the signed payload fixes the amounts, the tokens, the expiry, and a single-use nonce. The settlement contract recovers the signer and enforces the exact terms atomically — fill at the signed price or revert. There is no post-commitment hold window in which the maker re-prices. Concretely:

- **Hashflow's** `HashflowPool` enforces the signed quote via `quoteHash.recover(quote.signature) == signerConfig.signer`; there is no `amountOutMin` parameter anywhere in the trade path, the signed *rate* is preserved exactly (partial fills scale output at the same ratio, never worse), and replay is blocked by a used-txid mapping with a short `quoteExpiry`. Once signed, fade is impossible.
- **Bebop's** `BebopSettlement` (RFQ at `0xbbbbbBB520d69a9775E85b458C58c648259FAD5F`) verifies the taker's EIP-712 signature on-chain and checks the execution price meets the agreed minimums; the order carries an expiry field as its only validity out.
- **0x's** `OtcOrdersFeature` fixes `makerAmount`/`takerAmount` in a signed order gated by a packed `expiryAndNonce`; the contract marks the order `INVALID` once the nonce is consumed or `EXPIRED` past its timestamp.

So the FX-style "re-price after the taker commits" move is genuinely hard to reproduce on EVM. The signed leg is firm.

**Which means the overquoting migrates upstream, into the indicative→firm transition.** This is the locus that matters. A two-stage RFQ API is the standard shape:

- 0x's `/price` is a read-only **indicative** quote — it returns no submittable order, commits no maker assets, and creates no obligation. `/quote` returns the **firm** maker-signed order with calldata.
- Native's orderbook streams **indicative** auto-sign levels updated every second; the router picks a level, then sends the chosen MM an exclusive **firm-quote** request with a ~100ms budget, and the MM can return `success: false` with an `errorMessage` to fade *before the taker signs*.
- 0x **RFQ-M** additionally hands the maker a last-look at the *submission* step: because the maker is the on-chain submitter, after the taker signs the meta-transaction the maker can simply decline to broadcast `fillTakerSignedOtcOrder` if the market moved — fading at zero gas cost. 0x frames this as a feature ("makers have no fear of their stale quotes being arbitraged ... they give tighter spreads"), which is the same optionality property as last-look, just located at the submit step rather than a hold window.

The distinction to carry: **the settlement contract can only enforce the firm order that finally got signed. It cannot enforce the indicative.** A maker who wins display ranking on a tight soft quote and then returns a worse firm quote (or no quote) has overquoted, and no on-chain check catches it, because the check only ever sees the firm leg. This is why "firm-signed" at the *contract* level and "indicative-then-firm (fadeable)" at the *protocol* level are different categories even when they share the same settlement contract — the fade lives off-chain, before signature.

## 4. The taxonomy table

Five quote models, mirroring the bopAMM report's bucket discipline. *Slippage borne by* answers who eats adverse price movement between quote and fill. *Overquote exposure* rates the indicative→firm / sandwich attack surface. *Surplus* answers who keeps price improvement above the user's floor.

| Venue | Category | Quote model | Slippage borne by | Overquote exposure | Surplus |
|---|---|---|---|---|---|
| **Uniswap v1/v2 AMM** | AMM route | amm-route-with-slippage | User (tolerance band) | n/a maker; **canonical sandwich surface** | User keeps upside to band; no surplus auction |
| **1inch Classic (Pathfinder)** | AMM-route aggregator | amm-route-with-slippage | User (`minReturn` floor) | low — no maker between quote and fill | User gets actual on-chain output (no maker to skim) |
| **Odos Router V2** | AMM-route aggregator | amm-route-with-slippage | User (min-out floor) | medium — estimate-vs-fill gap | **Odos keeps positive slippage** on standard swaps |
| **KyberSwap Aggregator** | AMM-route aggregator | amm-route-with-slippage | User (`slippageTolerance` floor) | medium — estimate-vs-fill gap | **KyberSwap keeps positive slippage** |
| **LI.FI** | AMM-route / orchestration | amm-route-with-slippage | User (`toAmountMin` floor) | medium — `toAmount`→`toAmountMin` band, compounds on multi-step | User gets actual output above floor (no LI.FI filler) |
| **Flashbots Protect** | AMM route, private relay | amm-route-with-slippage | User (band as backstop) | low — band hidden from mempool | `amountOut ≥ min`; 90% of MEV refund to `tx.origin` |
| **0x RFQ-T** | RFQ | firm-signed | Maker (firm price) | low — committed before taker submits | Maker keeps drift above signed quote |
| **0x RFQ-M** | RFQ | last-look (maker-submitted) | Maker on fills that land | **high — maker last-look at submit** | Maker keeps spread; no surplus to user |
| **0x RFQ (`/price`→`/quote`)** | RFQ | indicative-then-firm (fadeable) | Maker on firm leg | **high — free fade at indicative stage** | Maker keeps spread |
| **0x Settler v2** | AMM route + RFQ overlay | amm-route-with-slippage | User (AMM leg) / maker (RFQ leg) | medium | AMM surplus **plan-dependent (often 0x-collected)**; RFQ maker keeps it |
| **Hashflow** | RFQ | firm-signed | Maker (signed rate) | low at fill; pre-signature decline only | Maker keeps spread; zero-slippage cert to user |
| **Bebop PMM RFQ** | RFQ | firm-signed | Maker (signed amounts) | low at fill; pre-signature only | Maker edge priced into quote; user gets exact amount |
| **Clipper FMM** | Formula MM (self-liquidity) | firm-signed | Clipper server (signed formula) | low — single signer, on-chain formula check | ~5–20bps input haircut accrues to **Clipper LPs** |
| **Native FirmQuote** | RFQ | indicative-then-firm (fadeable) | Maker on firm leg | **high — MM can `success:false` in 100ms window** | Not specified in docs |
| **CoW Protocol** | Batch auction | batch-uniform-clearing | Solver (signed limit floor) | low — quote is estimate, limit is binding | **User keeps 100% of surplus above limit** |
| **UniswapX (Dutch + RFQ exclusivity)** | Dutch-auction intent | indicative-then-firm (fadeable) | Filler (decay floor) | medium — winning quoter can fade, override-must-improve caps it | Filler keeps margin above decay; surplus via competition |
| **1inch Fusion** | Dutch-auction intent | limit-price-intent | Resolver (signed `minReturn`) | low — user signs decay curve, not a maker quote | **"Spread surplus" returned to user** (minus protocol surplus fee) |
| **1inch Fusion+** | Cross-chain intent | limit-price-intent | Resolver (signed minimum) | low — escrow-locked, refund on non-completion | Spread surplus to user; resolver keeps execution profit |
| **Bebop JAM** | Solver auction intent | limit-price-intent | Solver (signed min-out, ~0.2% smart slippage) | medium — solver keeps upside above floor by design | **Solver keeps excess above signed minimum** |
| **Velora (ParaSwap) Delta** | RFQ / agent auction | firm-signed | Agent (signed `destAmount`) | low — firm min before auction | Surplus **split: integrator 50% + solver**; not user by default |
| **Across Protocol** | Cross-chain intent | firm-signed | Relayer (firm `outputAmount`) | low — exact output or no valid fill | Relayer keeps spread above committed output |
| **deBridge DLN** | Cross-chain intent | firm-signed | Solver (fixed take-offer) | low — fill the firm offer or refund | Solver keeps give-minus-take spread |
| **Bungee Auto / Socket** | Cross-chain intent | firm-signed | Filler (protected output) | low (Auto); higher in Manual mode | Filler keeps margin; Manual surplus to user |
| **FX Last Look** (off-chain ref) | OTC FX | last-look | Taker (no firm guarantee) | **high — re-price after commit** | LP monetizes the optionality |

Read the table as a single axis with two poles. At one pole sits the **AMM tolerance band** (and its degenerate twin, FX last-look): the user holds an unpriced option, exposure is high, the predator picks the user off inside the band. At the other pole sits the **firm signed limit / batch clearing**: there is no band, the floor is the binding object, and any improvement above it is either the user's (CoW, 1inch Fusion) or the priced margin of a filler who committed capital (Across, deBridge, Hashflow). The indicative-then-firm venues (0x RFQ-M, Native, UniswapX's RFQ seed) are the interesting middle — firm *at settlement*, fadeable *before signature*, which is precisely where the overquoting that this report is about actually happens.

## 5. The five quote models, venue by venue

### 5.1 AMM-route-with-slippage — the band-carriers

These venues route the user's own transaction (or a near-equivalent) against on-chain pools, bounded by a user-set floor. The defining property: **there is no maker between the quote and the fill, so there is no last-look fade.** The exposure is the estimate-vs-fill gap plus public-mempool MEV inside the band.

**Uniswap v1/v2** is the origin of the primitive (section 1). It carries no maker overquote risk because there is no maker; it carries the canonical *sandwich* surface because `amountOutMin` is broadcast in the clear.

**1inch Classic (Pathfinder)** is the cleanest large-scale example of the model done honestly. Pathfinder computes a multi-venue route; the user's own transaction executes it through `AggregationRouterV6` (`0x111111125421ca6dc452d289314280a0f8842a65`), which reverts with `ReturnAmountIsNotEnough(result, minReturn)` if the realized output is below the signed floor. Default slippage is "automatic (based on the token pair's volatility profile)," custom up to a 49% cap, and 1inch's own help center concedes that excessive tolerance "can open the door to front-running and sandwich attacks." Overquote exposure is low precisely because no third party sits between indicative and fill — degradation is normal AMM impact/MEV within the user's chosen band. *(Two soft spots flagged in the research: the "weighted directed graph / 60+ venues" framing is secondary, not in 1inch's primary blog; and a "positive-slippage fee" could not be confirmed from contract source — so I make no surplus-skim claim for Classic.)* DefiLlama 30-day aggregate volume (Classic + Fusion combined, not Classic alone): **$4.118B — volume only.**

**Odos** and **KyberSwap** are where the model turns extractive on the surplus side. Both enforce a user-set min-out and revert below it — standard. But both *keep the positive slippage*. Odos's Zellic audit documents that the Router V2 `_swap` function "collects positive slippage when it occurs (defined as the difference between the executed and quoted output when the executed output is higher)" as protocol revenue; the `_swapMulti` path (Zaps) instead charges a flat fee and collects none. KyberSwap's API spec states outright that a token surplus above the estimated output "will initially accrue to KyberSwap," while the user is guaranteed only the min-received. This is the quiet asymmetry of the AMM-aggregator model: the user eats downside to the floor, the aggregator pockets the upside above the estimate. KyberSwap layers "Smart Settlement" (intra-block re-routing if the chosen pool is sandwiched) as resilience, but that narrows the downside band, it does not return the upside. Odos offers an opt-in "Protected Swap" (25bps volatile / 5bps stable) that guarantees the exact quoted output — a priced version of the commitment, which is the right direction. Volumes (DefiLlama, *volume only*): KyberSwap Aggregator **$7.73B 30d**; Odos **$593.79m 30d**.

**LI.FI** is an orchestration layer, not a filler. It returns `toAmount` (the displayed estimate) and `toAmountMin` (the enforced floor based on the slippage setting); execution must deliver at least `toAmountMin` or it reverts. The exposure is the `toAmount`→`toAmountMin` band, which *compounds* on multi-step routes — LI.FI's docs note a swap+bridge+swap route can accumulate ~1.5% total, because each step's `toAmountMin` becomes the next step's input "because this is what we know we get for sure." It is not a maker-fade surface (no LI.FI filler to spoof), but the displayed-estimate-vs-enforced-floor gap is real degrade-at-fill exposure, wider on high-tolerance multi-step routes. First-look properties are *inherited* from whichever bridge it routes through (Across exclusivity if via Across, plain pool execution if via an AMM). LI.FI docs themselves advise integrators to track `toAmountMin`, not the user-set slippage — good hygiene, exactly because the band is the hiding place.

**Flashbots Protect** is the "hide the band" branch rather than the "replace the band" branch. The AMM `amountOutMin` is unchanged; the transaction is routed to participating builders via a private relay so a searcher cannot observe the floor and size a sandwich to it. Slippage protection becomes a backstop, privacy the primary defense. Protect also rebates MEV — primary docs specify 90% of MEV refunds to `tx.origin`, the rest to the validator, tunable via a `refund` parameter (keeping more can slow inclusion). *(Flag: a widely-cited "95%+ sandwich-bot success" statistic is not attributable to a Flashbots primary source — it appears only in secondary renderings — so it is not used as evidence here.)* This is a legitimate mitigation, but note what it does and doesn't do: it removes predator one (the mempool sandwich) by hiding the band; it does nothing about the band as an *object*, because on the AMM path there is no maker to overquote in the first place.

### 5.2 Firm-signed — the band-killers

Here the maker signs an EIP-712 quote with fixed amounts, and the settlement contract enforces it exactly. There is no user tolerance band on the signed leg and no fade at fill. The residual exposure is entirely *pre-signature*: the maker can decline to sign or let the short TTL lapse, which is failure-to-quote, not a worse fill.

**Hashflow** is the canonical firm-signed venue. "The signature locks the rate — it cannot be changed between quote and execution." The user receives exactly the signed *rate* (size can scale down on partial fills, but never at a worse price), or the transaction reverts. There is no `amountOutMin` because there is no slippage to bound. The MM captures the spread; the user takes certainty and no surplus. Overquote exposure is low at fill, non-zero only in the request→sign window where an MM can decline or widen in volatility. DefiLlama (*volume only*): **$289.4M 30d** (Ethereum ~$288.16M).

**Bebop PMM RFQ** is architecturally identical in shape — multiple authorized MMs stream firm executable quotes, the best signed quote fills, and `BebopSettlement` enforces the agreed minimums on-chain with a per-order expiry. Bebop's own framing is "guaranteed execution and guaranteed fill" with zero slippage; the maker edge is priced into the quote. DefiLlama all-chain aggregate (*volume only, not Ethereum-isolated*): **$1.975B 30d.**

**Clipper FMM** is the formula-MM variant — single-venue, self-pooled liquidity, one signer (the Clipper server), pricing computed off-chain from oracle marks plus pool token ratios and verified on-chain against the signed formula and a `goodUntil` expiry. "What you're quoted is what you get." The only failure mode is staleness (a prior quote goes "dirty" after another swap, or the expiry lapses) which reverts rather than fills worse. The economic model is a ~5–20bps input haircut that preserves the maker invariant in lieu of explicit fees, accruing to Clipper Pool LPs — the user gets exactly the quoted output, no positive slippage returned. Residual risk here is oracle manipulation feeding the off-chain formula, not indicative→firm fade. *(No clean DefiLlama 30d figure retrieved; do not treat the endpoint's 0 as real volume.)*

**0x RFQ-T** is the firm-signed leg of 0x's RFQ. The maker pre-signs and commits the `OtcOrder`; the taker submits `fillOtcOrder` directly, so the maker gets no last-look. Spoofing an indicative then degrading at fill is not possible on RFQ-T — the only failure is a clean expiry/nonce revert. Contrast this directly with RFQ-M below.

**Velora (formerly ParaSwap) Delta** is firm-signed via an agent auction. The user signs an EIP-712 Delta order whose minimum `destAmount` is computed from the quoted `deltaPrice` minus a user-chosen `slippagePercent`; after submission "an auction is performed between all the available Agents," and the winning agent must fill at or above the signed floor. Fade is structurally limited to non-fills. The one wrinkle is surplus: Velora's docs describe a model where integrator partners collect 50% of the order surplus (or a partner fee up to 200bps), with the remainder to the settling solver — so price improvement above the floor is *not* returned to the user by default. DefiLlama (*volume only*): **$2.3B 30d.**

### 5.3 Indicative-then-firm (fadeable) — where the overquoting actually lives

This is the category the report exists to sharpen. The settlement is firm; the *protocol* exposes an indicative stage the maker can fade from. The contract enforces the firm order; it never sees the indicative.

**0x RFQ-M** is the cleanest case. `/price` is read-only indicative — no committed order. `/quote` returns the firm maker-signed order. Because RFQ-M makes the *maker* the on-chain submitter, the maker holds last-look at the submit step: after the taker signs, the maker can decline to broadcast `fillTakerSignedOtcOrder` at zero gas if the market moved. The fade is free and by design. Overquote exposure: **high.**

**Native FirmQuote** makes the indicative→firm transition explicit in the API surface. The orderbook streams auto-sign indicative levels updated every second; the router picks a level then requests an exclusive firm-quote from the chosen MM with a 100ms response budget, and the MM can return `success: false` with an `errorMessage` — a documented fade *before* the taker signs. The on-chain `maxSlippageBps` floor caps execution worse than the *finally-signed* amount, but it does nothing about the indicative→firm degradation, which is the classic last-look surface. *(Flags from research: Native's docs do not use the term "LastLook" as a named mode — the documented modes are "auto-sign" and "firm-quote"; surplus allocation is not specified; the cited `0x3ba16AC2…` is Native's Lend Vault, not the settlement router.)* DefiLlama all-chain (*volume only, not Ethereum-isolated*): **$1.92B 30d.**

**UniswapX (Ethereum mainnet)** is a hybrid: a Dutch-auction intent with an RFQ exclusivity phase grafted onto the front. The off-chain "hard quote" RFQ picks a single winning quoter who gets an exclusive fill window before the Dutch decay opens permissionlessly. That quoter can fade — Uniswap's docs concede "the system can penalize quoters who fade too frequently by ignoring their quotes." The damage is bounded, not eliminated: a faded quote falls back to an open Dutch auction that decays only down to the user's signed minimum output, and "soft exclusivity" lets a non-exclusive filler override the window *only by paying the swapper more*, so "the swapper is never worse off if exclusivity is overridden." So UniswapX's indicative seed is fadeable (exposure medium), but the signed floor plus the override-must-improve rule cap how far the user can be degraded. The filler captures spread above the decay price at fill; the user's benefit comes from competition pushing fills to earlier (better) points on the curve, not a separate rebate.

### 5.4 Limit-price-intent (Dutch decay) — the band becomes a curve the user signs

In these venues the user signs a decaying limit curve up front. There is no separate maker indicative to fade from, because the binding object *is* the user-signed order with a firm floor. A resolver cannot fill below the signed minimum (on-chain threshold checks revert), and the only worse-than-floor outcome is non-fill/expiry, which returns the order for free resubmission.

**1inch Fusion** is the reference. The user signs a Limit-Order-Protocol order whose effective rate is a piecewise-linear, gas-adjusted curve decaying from a high "spot" rate to a signed `minReturn` — "the minimum amount you are guaranteed to receive." `SimpleSettlement.sol` enforces `TakingAmountTooHigh`/`MakingAmountTooHigh` thresholds, so a whitelisted resolver (1IP-33: dynamic whitelist, max 10 resolvers, 10%-of-Unicorn-Power threshold) cannot fill below the floor. Critically, surplus is returned: "spread surplus" — the difference between the initial and final rates — is credited *to the user* (1inch reports ~0.2% better rates on average), though `SimpleSettlement` also takes a protocol surplus fee, so the user does not necessarily receive 100%. Overquote exposure is low because the user never relies on a resolver's indicative — they're bound to the curve they signed. **Fusion+** extends the identical pricing logic cross-chain via linked hashlock/timelock escrows; the realistic failure mode is non-completion → timelock refund, a liveness risk, not a price-fade risk.

**Bebop JAM** is the instructive counter-example *within* the intent category, because it shows the floor-with-surplus-to-solver design. The user signs an EIP-712 intent specifying a minimum amount to receive; independent solvers compete; `JamSettlement` (`0xbeb0b0623f66bE8cE162EbDfA2ec543A522F4ea6`) enforces the signed minimum. But JAM applies "smart slippage" (~0.2% max) rather than a zero-slippage exact amount, and — verified directly from the contract NatSpec — "as long as the trade is fulfilled, the solver is allowed to keep any potential excess." The `settle()` path transfers exactly the signed `buyAmounts` to the receiver; anything above stays with the solver. So JAM's overquote exposure is medium *by design*: downside capped at the floor, upside kept by the solver, with off-chain solver competition the only thing compressing the signed minimum toward best price. This is the honest contrast to Fusion's surplus-to-user and CoW's surplus-to-user — same intent category, different surplus politics.

### 5.5 Cross-chain firm-output intents — committed output, no band

The cleanest expression of "priced commitment, not unpriced option," because cross-chain inventory risk forces the filler to commit capital against a firm number.

**Across** has the user sign a deposit specifying a firm `outputAmount`; a relayer fronts its own capital and must deliver that exact amount via `fillV3Relay()` or there is no valid fill. The relayer bears all price movement and inventory risk between deposit and fill. `exclusiveRelayer`/`exclusivityDeadline` grant configurable *first-look* (not last-look — once filled, the output is locked and cannot be faded); after the deadline any relayer may fill. The relayer keeps the input-minus-output spread as its priced margin. DefiLlama (*volume only*): **$664.4M 30d bridge volume.**

**deBridge DLN** is the same shape with a fixed "take offer": the order fixes the exact destination amount at creation, and any taker fills by supplying exactly that amount or the order goes unfilled and the maker is refunded. Open competitive fill (no exclusivity), solver keeps the give-minus-take spread. *(No clean DefiLlama 30d figure; a secondary ~$1.53B Nov-2025 monthly figure is not a verified 30d field, so reported n/a.)*

**Bungee Auto / Socket** signs a gasless Permit2 request with slippage protection and a timeout; a marketplace of fillers competes via automated auctions, and the winner delivers the protected output or the request expires unfilled with funds safe on the source chain — firm-output semantics, low overquote exposure. Bungee's Manual mode, by contrast, behaves like an AMM-route aggregator: the user picks a route and bears in-band slippage on the underlying bridge/DEX legs. Zero platform fee; filler keeps the inventory margin in Auto. *(Flag: docs describe provers as native-bridge-integration whitelisted by BungeeDAO, not "on-chain verifiable proofs" as sometimes glossed; the surplus split is reasonable inference, not a doc quote.)*

### 5.6 Batch-uniform-clearing — no band, surplus to user

**CoW Protocol** is the architectural answer that collapses *both* predators at once. The user signs a limit-price intent (max sell / min buy); for market orders the indicative quote plus tolerance is converted to a firm signed limit *before* signing. Orders are grouped into a batch settled at a uniform clearing price, and `GPv2Settlement` "cannot execute an order if its limit price is violated." The pre-trade quote is only an estimate — it binds the user only by setting the signed limit; a solver cannot show a generous indicative and degrade at fill, because (1) execution is bounded by the user's signed limit, and (2) the winning execution price is chosen by *competition to maximize surplus*, not by the quoting party. Score-inflation/overbidding ("pennying") is explicitly slashable, and an EBBO check guards against worse-than-market clearing. Surplus above the signed limit is returned to the *user* — "all order surplus is forwarded to you" — including on limit orders; solvers are paid in COW-token rewards, not by skimming user surplus. Coincidence-of-wants matches bypass pools entirely, paying neither pool fee nor slippage on the matched portion. DefiLlama (*volume only*): **$3.223B 30d.**

This is the clearest production instance of the report's thesis: replace the tolerance band with a signed limit plus uniform clearing, and the band — the object both predators exploit — simply has no analogue. There is nothing for a sandwich bot to size against (the user never broadcasts an `amountOutMin` into the public mempool; solvers bear routing slippage) and nothing for a maker to fade below (settlement is on-chain against the signed limit, and competition is for delivered surplus, not for a shown indicative).

## 6. Where the surplus goes — the second axis

The slippage axis (band vs. firm) is the headline, but it pairs with a second axis that the table makes visible: **who keeps the price improvement above the user's floor.** This is the cleanest tell for whether a venue has actually fixed the option problem or merely relocated the rent.

Three regimes:

- **Surplus to user.** CoW (100% above limit), 1inch Fusion/Fusion+ ("spread surplus" minus a protocol fee). These are the venues that complete the upgrade: the user's floor is firm *and* the upside is theirs. The filler/solver earns from execution skill on top of honoring the user, not from the user's spread.
- **Surplus to filler (priced into the commitment).** Across, deBridge, Hashflow, Bebop PMM, Bebop JAM. The filler keeps the spread above the floor — but in the firm-output and firm-signed cases that spread is the *priced margin on a capital commitment*. The maker/relayer took inventory and price risk to deliver a guaranteed number; the spread is the price of that option, now held by the professional rather than written for free by the user. This is the defensible version of the report's thesis: the option moved from the user to the filler, and got priced.
- **Surplus to the aggregator/intermediary.** Odos and KyberSwap keep positive slippage as protocol revenue; 0x Settler's AMM-route surplus is plan-dependent and frequently 0x-collected; Velora Delta splits surplus between integrator (50%) and solver. These are the cases to watch, because the user still holds the downside-to-floor option *and* surrenders the upside to a party that is neither the user nor the filler taking the risk. That is the worst of both axes: band-style downside exposure with firm-style upside capture, accruing to the router.

The honest reading: "firm-signed" is necessary but not sufficient. A venue can give you a firm floor and still keep your upside (KyberSwap, Odos standard swaps). The full upgrade requires *both* a binding floor *and* surplus accruing either to the user or to the filler who priced the commitment — not silently to the routing intermediary.

## 7. The defensible claim: priced commitment, not unpriced option

It is tempting to write the maximalist version: firm quotes and batch auctions abolish slippage, so the tolerance band is obsolete. That version is wrong, and the report should not make it.

Slippage does not vanish. On an AMM over a public mempool, zero tolerance means reverts — the band exists because real price uncertainty exists between quote and inclusion, and someone has to absorb it. The firm-quote and intent venues do not make that uncertainty disappear. They *reassign* it:

- On Hashflow, Bebop PMM, Across, deBridge, the **market maker / relayer** absorbs the price risk between quote and fill. They can do this because they hedge on CEXes and run short quote-validity windows (seconds), and because they are professionals who can *price* the risk into the spread. The user pays for certainty in the bid/ask; the user does not write a free option.
- On CoW and 1inch Fusion, the **solver/resolver** absorbs routing slippage and competes the surplus back toward the user. The user signs a firm limit; the price risk lives with the party that searches and settles.

So the defensible claim is narrow and exact: **slippage should stop being an unpriced option held by an uninformed user and become a priced commitment held by a professional filler — priced into the spread, with surplus above the floor returned to the user or earned by the filler who took the risk.**

That reframing dissolves both predators without pretending to dissolve price risk:

- The **sandwich** predator depends on the user broadcasting `amountOutMin` into a public mempool. A firm signed quote has no `amountOutMin`; a batch auction never exposes the user's limit to a builder who can order around it; a private relay hides the band. The mempool-observable floor is the vulnerability, and three different architectures remove it.
- The **overquote** predator depends on the gap between a tight indicative and a worse firm. A firm-signed RFQ where the maker commits before the taker submits (RFQ-T) has no indicative stage to fade from. A batch auction selects on *delivered* surplus, so a spoofed tight quote wins nothing at settlement. The indicative→firm gap is the vulnerability, and these architectures close it.

What remains — and what the report should not paper over — is that the firm-quote venues *do* relocate optionality to the filler, and the filler does keep the spread (except where surplus is explicitly returned). That is fine. A priced option held by a hedging professional who guarantees the user a firm number is a categorically better deal than an unpriced option written by the user to whoever happens to be better-informed at fill time. The trade the user makes is "certainty now, spread paid" instead of "a displayed number, and an option I gave away for free." The first is a market. The second is a leak.

## 8. What's open

**The indicative stage has no on-chain accountability.** Every fadeable venue (0x RFQ-M, Native, UniswapX's RFQ seed) relies on *off-chain* reputational penalties for overquoting — ignore-the-quoter-for-a-while (UniswapX), penalize-deviation-vs-published-levels (Native, Hashflow servers). There is no on-chain receipt that the firm quote matched the indicative the maker used to win ranking. Until there is, "firm at settlement" coexists with "fadeable before signature," and the overquoting this report maps remains an off-chain accountability problem, not a contract-enforced one. The clean fix — a signed indicative that the firm quote is checked against — is not standardized anywhere as of this writing.

**Surplus capture is under-disclosed.** Several venues (Native, Bebop JAM until the contract was read, 0x Settler AMM-route default) make it genuinely hard to learn who keeps the upside above the floor without reading contract source. For a user, "do I keep positive slippage" is a first-order question and it is frequently buried. The venues that return surplus to the user (CoW, 1inch Fusion) advertise it; the venues that keep it (Odos, KyberSwap) disclose it in API specs and audits, not in the UI. That asymmetry is itself a finding.

**The firm-output cross-chain model is the cleanest, and the least visible.** Across, deBridge, and Bungee Auto have arguably the best version of the upgrade — a firm committed output with the relayer pricing the risk — precisely because cross-chain inventory risk made the unpriced-option model untenable from the start. The single-chain swap world inherited the tolerance band from Uniswap v1 and is still unwinding it; the cross-chain intent world never had it. That is a hint about where the single-chain venues end up if the indicative→firm gap keeps getting closed.

**Volume tells you flow, not health.** The figures in this report are 30-day *volume* and several are all-chain aggregates, not Ethereum-isolated (Bebop $1.975B, Native $1.92B). KyberSwap's $7.73B and 1inch's $4.118B lead the AMM-route aggregators by throughput; CoW's $3.223B leads the surplus-returning batch model. None of these is a revenue figure, and none should be read as one — the venue that returns the most surplus to users is not necessarily the one with the most volume, and that gap is exactly the point.

What the map establishes: the slippage-tolerance band is not a fact of trading on Ethereum. It is a primitive specific to pool-routed AMM execution in a public mempool, inherited reflexively into venues where it is no longer load-bearing, and it is the single object that both the sandwich bot and the RFQ overquoter exploit. The venues that have grown up — firm signed quotes, batch clearing, committed-output intents — did not abolish slippage. They moved it from an unpriced option the user gives away to a priced commitment a professional holds. Where a venue sits on that move, and who keeps the surplus when it does, is the whole story.

## Sources

**The slippage primitive (AMM origin + MEV surface):**
- [Uniswap — What is Slippage?](https://blog.uniswap.org/what-is-slippage-crypto)
- [Uniswap — Minimize Slippage on Swaps](https://blog.uniswap.org/minimize-slippage-on-swaps)
- [Uniswap V3 Book — Slippage Protection](https://uniswapv3book.com/milestone_3/slippage-protection.html)
- [Speedrun Ethereum — Front-running & MEV Mitigation](https://speedrunethereum.com/guides/front-running-mev-mitigation)
- [RareSkills — Uniswap v2 Price Impact](https://rareskills.io/post/uniswap-v2-price-impact)
- [Flashbots Protect — Overview](https://docs.flashbots.net/flashbots-protect/overview)
- [Flashbots Protect — Settings Guide (MEV refund split)](https://docs.flashbots.net/flashbots-protect/settings-guide)
- [Flashbots — 2M Protect Users](https://writings.flashbots.net/2m-protect-users)

**FX last-look (reference contrast):**
- [Wikipedia — Last look (foreign exchange)](https://en.wikipedia.org/wiki/Last_look_(foreign_exchange))
- [Databento — Last look microstructure](https://databento.com/microstructure/last-look)
- [Goldman Sachs — FX Last Look Disclosure (PDF)](https://www.goldmansachs.com/disclosures/FX-last-look-disclosure.pdf)
- [Global FXC — Last Look report](https://www.globalfxc.org/uploads/gfxc_report_last_look-1.pdf)

**AMM-route aggregators (band-carriers):**
- [1inch — Slippage tolerance setting](https://help.1inch.com/en/articles/4585126-what-is-the-slippage-tolerance-setting)
- [1inch — AggregationRouterV6 contract](https://etherscan.io/address/0x111111125421ca6dc452d289314280a0f8842a65)
- [1inch — How the network ensures best swap prices](https://1inch.com/blog/post/how-the-1inch-network-ensures-the-best-swap-prices/)
- [Odos — Fees docs](https://docs.odos.xyz/build/fees)
- [Zellic — Odos audit, fees on positive slippage](https://reports.zellic.io/publications/odos/sections/fees-on-positive-slippage-positive-slippage)
- [Odos — Router V2 repo](https://github.com/odos-xyz/odos-router-v2/blob/main/README.md)
- [KyberSwap — Aggregator API spec (EVM swaps)](https://docs.kyberswap.com/developer-guide/aggregator-api/aggregator-api-specification/evm-swaps)
- [KyberSwap — Smart Settlement](https://docs.kyberswap.com/developer-guide/start-here/foundational-solutions/smart-settlement-better-swap-output-with-lower-slippage)
- [LI.FI — Slippage & price impact guide](https://docs.li.fi/guides/integration-tips/slippage-price-impact)
- [LI.FI — Multi-step slippage (toAmountMin)](https://help.li.fi/hc/en-us/articles/12111462474139-How-does-slippage-and-toMinAmount-works-in-routes-with-several-steps)

**Firm-signed RFQ (band-killers):**
- [Hashflow — Taker API v3](https://docs.hashflow.com/hashflow/taker/getting-started-api-v3)
- [Hashflow — HashflowPool.sol](https://github.com/hashflownetwork/x-protocol/blob/main/evm/contracts/pools/HashflowPool.sol)
- [Hashflow — IQuote.sol](https://github.com/hashflownetwork/x-protocol/blob/main/evm/contracts/interfaces/IQuote.sol)
- [Bebop — Settlement smart contracts](https://docs.bebop.xyz/core-concepts/settlement-smart-contracts.md)
- [Bebop — PMM RFQ API intro](https://docs.bebop.xyz/bebop/bebop-api-pmm-rfq/pmm-rfq-api-intro)
- [Clipper — How it works](https://clipper.exchange/how-it-works)
- [Clipper — Integrating with Clipper RFQ](https://docs.clipper.exchange/disclaimers-and-technical/integrating-with-clipper-rfq/guides/interacting-with-the-clipper-exchange-contracts)
- [Shipyard — What is a Formula Market Maker?](https://www.shipyardsoftware.org/post/what-is-a-fmm)
- [Velora (ParaSwap) — Delta SDK docs](https://github.com/paraswap/paraswap-sdk/blob/master/docs/DELTA.md)
- [Velora — Delta API](https://developers.velora.xyz/api/velora-api/velora-delta-api/)

**Indicative-then-firm / last-look (where the fade lives):**
- [0x — OtcOrdersFeature.sol](https://github.com/0xProject/protocol/blob/development/contracts/zero-ex/contracts/src/features/OtcOrdersFeature.sol)
- [0x — Delivering superior trade execution with RFQ](https://0x.org/post/delivering-superior-trade-execution-with-0x-rfq)
- [0x — Meta-transactions (RFQ-M flow)](https://docs.0xprotocol.org/en/latest/advanced/mtx.html)
- [0x — Unlock optimal trades with RFQ liquidity](https://0x.org/post/unlock-optimal-trades-in-swap-api-with-0x-rfq-liquidity)
- [Cinjon — 0x systems analysis (RFQ-T vs RFQ-M)](https://cinjon.com/systems-0x-analysis)
- [0x — Settler repo (Permit2 RFQ settlement)](https://github.com/0xProject/0x-settler)
- [0x — ISettlerActions.sol](https://github.com/0xProject/0x-settler/blob/master/src/ISettlerActions.sol)
- [Dedaub — 0x Settler intent upgrade audit](https://dedaub.com/audits/0x/0x-settler-intent-upgrade-jan-23-2025/)
- [Native — FirmQuote swap APIs](https://docs.native.org/native-dev/build-with-native/swap-aggregators/firmquote-swap-apis)
- [Native — MM firm-quote endpoint](https://docs.native.org/native-dev/build-with-native/market-makers/mm-pricing-and-signing-api/firm-quote)
- [UniswapX — Auction types (RFQ + soft exclusivity)](https://developers.uniswap.org/contracts/uniswapx/auctiontypes)
- [UniswapX — Overview](https://developers.uniswap.org/contracts/uniswapx/overview)
- [UniswapX — repo](https://github.com/Uniswap/UniswapX)

**Limit-price intents (Dutch decay):**
- [1inch Fusion — FAQ](https://help.1inch.com/en/articles/6800254-1inch-fusion-faq)
- [1inch — Intent swap introduction](https://business.1inch.com/portal/documentation/apis/swap/intent-swap/introduction)
- [1inch — Spread surplus](https://1inch.com/blog/post/spread-surplus-1inch-dapp/)
- [1inch — SimpleSettlement.sol](https://github.com/1inch/limit-order-settlement/blob/master/contracts/SimpleSettlement.sol)
- [1inch — 1IP-33 dynamic resolver whitelist](https://gov.1inch.network/discussion/12048-1ip33-implement-dynamic-fusion-mode-resolver-whitelist-based-on-unicorn-power-threshold)
- [MixBytes — Modern DEXes: 1inch limit order protocols](https://mixbytes.io/blog/modern-dex-es-how-they-re-made-1inch-limit-order-protocols)
- [Bebop — JAM API introduction](https://docs.bebop.xyz/bebop/bebop-api-jam/jam-api-introduction)
- [Bebop — JamSettlement.sol (solver keeps excess)](https://github.com/bebop-dex/bebop-jam-contracts)

**Cross-chain firm-output intents:**
- [Across — Intent lifecycle](https://docs.across.to/concepts/intent-lifecycle-in-across)
- [Across — Intents architecture](https://docs.across.to/concepts/intents-architecture-in-across)
- [deBridge DLN — Creating an order](https://docs.debridge.finance/dln-the-debridge-liquidity-network-protocol/integration-guidelines/interacting-with-the-api/creating-an-order)
- [deBridge — DLN contracts](https://github.com/debridge-finance/dln-contracts)
- [Bungee — Routing methods](https://docs.bungee.exchange/overview/routing-methods/routing-methods.md)
- [Bungee — Auto mode](https://docs.bungee.exchange/overview/routing-methods/bungee-auto/)

**Batch-uniform-clearing:**
- [CoW Protocol — Competition rules](https://docs.cow.fi/cow-protocol/reference/core/auctions/competition-rules)
- [CoW Protocol — Solvers](https://docs.cow.fi/cow-protocol/concepts/introduction/solvers)
- [CoW Protocol — Settlement contract](https://docs.cow.fi/cow-protocol/reference/contracts/core/settlement)
- [CoW Swap — Surplus-capturing limit orders](https://blog.cow.fi/how-to-user-cow-swaps-surplus-capturing-limit-orders-24324326dc9e)
- [CoW — Understanding batch auctions](https://cow.fi/learn/understanding-batch-auctions)

**Market data (DefiLlama, fetched 2026-06-07; volume only, several all-chain not Ethereum-isolated):**
- [DefiLlama — 1inch](https://defillama.com/protocol/1inch) — $4.118B 30d (Classic + Fusion aggregate)
- [DefiLlama — CoW Swap](https://defillama.com/protocol/cowswap) — $3.223B 30d
- [DefiLlama — KyberSwap Aggregator](https://defillama.com/protocol/kyberswap-aggregator) — $7.73B 30d
- [DefiLlama — Odos](https://defillama.com/protocol/odos) — $593.79m 30d
- [DefiLlama — Velora](https://defillama.com/protocol/velora) — $2.3B 30d
- [DefiLlama — Hashflow](https://defillama.com/protocol/hashflow) — $289.4M 30d (ETH ~$288.16M)
- [DefiLlama — Bebop](https://defillama.com/protocol/bebop) — $1.975B 30d (all-chain)
- [DefiLlama — Native](https://defillama.com/protocol/native) — $1.92B 30d (all-chain)
- [DefiLlama — Across](https://defillama.com/protocol/across) — $664.4M 30d (bridge volume)
