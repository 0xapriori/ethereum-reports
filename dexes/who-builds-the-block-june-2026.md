---
title: "Who Builds the Block: Builder Economics, Order-Flow Segmentation, and the Prop-AMM Turn"
date: 2026-06-22
---

# Who Builds the Block: Builder Economics, Order-Flow Segmentation, and the Prop-AMM Turn

*On how the Ethereum block builder stopped being a packer and became a trading firm — and why "prop AMMs" and application-specific sequencing are the same move pointed at opposite beneficiaries. | June 2026*

## tl;dr

- **The block builder stopped being a packer and became a trading firm.** Mid-2026 the builder market is one dominant party plus a rotating runner-up: on relayscan.io (7-day, 2026-06-15) Titan holds **51.98%** of blocks, Quasar **23.97%**, BuilderNet **9.11%**, Eureka **8.61%**. Top-2 ≈ 76%, top-3 ≈ 85%, derived HHI ≈ **~3,550** — above the 2,500 "highly concentrated" line (this is our own calculation, not a published index).
- **The "Titan + BuilderNet ≈ 80% duopoly" framing is now stale.** Quasar overtook BuilderNet for the #2 slot (~24% vs ~7-9%). The structural constant is Titan at ~50%+; the runner-up seat rotates.
- **Profit is far more concentrated than share.** Titan booked **+514 ETH** of builder profit over 7 days while several builders ran at a loss to buy share (bombora **−0.79 ETH/7d**) [relayscan.io]. Winning the auction and earning from it are now different games — slower builders eat the winner's curse [arXiv:2311.09083].
- **Private flow is the actual input.** Exclusive/private order flow is roughly **54.6% of block value from ~12% of transactions** [arXiv:2410.12352]; exclusive flow is "as much as 35%" of the market bypassing the public mempool [Flashbots, 2024]; exclusive order flows are ≈ **71%** of trading-related builder revenue [arXiv:2605.04471].
- **Segmentation is the whole game.** AMMs are maximally exposed to LVR (normalized instantaneous LVR = σ²/8 for constant-product [arXiv:2208.06046]) precisely because their quotes are public and non-discretionary — they cannot separate toxic from uninformed flow. Every intent/solver/RFQ/prop-AMM design exists to relax that constraint.
- **Vertical integration is documented, not speculative.** AFT 2025 names Wintermute↔rsync-builder ($71.4M extracted, 66% paid to its builder as tips) and SCP↔beaverbuild ($71.1M, 81%); folding searcher PnL back turns rsync's apparent −2.24% margin into **+27.06%** — the empirical signature of one firm capturing both spread and MEV [arXiv:2507.13023]. (Splits are the authors' upper bounds.)
- **"Prop AMM" and application-specific sequencing are the same structural move at different layers.** Both pull execution/ordering control away from the neutral generic builder. The difference is *cui bono*: a prop AMM **privatizes** the surplus into one firm's P&L; ASS **socializes** it back to a protocol's LPs and users via neutral on-chain rules. That inversion is this report's payoff.

## Table of Contents

1. [The builder's vantage](#1-the-builders-vantage)
2. [The market as it actually stands (mid-2026)](#2-the-market-as-it-actually-stands-mid-2026)
3. [Builder profitability](#3-builder-profitability)
4. [Order flow: the real input](#4-order-flow-the-real-input)
5. [Segmentation: toxic vs uninformed flow](#5-segmentation-toxic-vs-uninformed-flow)
6. [Barriers to entry](#6-barriers-to-entry)
7. [Prop AMMs and how they change block building](#7-prop-amms-and-how-they-change-block-building)
8. [Prop AMMs and application-specific sequencing](#8-prop-amms-and-application-specific-sequencing)
9. [Open questions](#9-open-questions)
10. [Data sources & methodology](#10-data-sources--methodology)

---

## 1. The builder's vantage

There are two places to stand when you look at the Ethereum value supply chain. One is inside the trade — the microstructure view, where the lens is AMM → RFQ → orderbook, latency decides, and what looks like "better execution" is often opacity transferred from one party to another rather than removed. That is the view from where price gets made.

This report takes the other position: the building side. The block builder sits one layer down from the trade and one layer up from the validator. It receives a stream of transactions — some public, most increasingly not — orders them to maximize value, and bids for the right to have its ordering proposed on-chain. The clean mental model from a few years ago was a kind of mechanical packer: take the mempool, run a knapsack, pay the validator, win the slot.

That model is dead. The hinge fact is simple to state and changes everything downstream: only about half of the transactions that later get included actually pass through the public mempool. If half the included flow never touches the public mempool, then a builder's edge is not knapsack-solving — it is *which flow it sees and on what terms*. The block builder has become, functionally, a trading firm that happens to also assemble blocks.

The market leader makes the shift legible. **Titan Builder** and **Titan Relay** are the builder/relay brand of **Gattaca**, the London research-driven crypto technology company [gattaca.com; reverie.ooo]. Worth fixing precisely, because the naming confuses people: Gattaca is the company; Titan is its brand. Gattaca is *not* a rebrand of Titan, and Titan did not "rebrand to Gattaca." This report maps the shift the leader exemplifies: where the market actually stands, who earns, what the real input is, why segmentation is the entire competitive question, and how two superficially different design movements — "prop AMMs" and application-specific sequencing — turn out to be the same structural move pointed at opposite beneficiaries.

## 2. The market as it actually stands (mid-2026)

Start with the ground truth, directly verified on relayscan.io as of 2026-06-15.

**7-day builder share:**

| Builder | Share (7d) | Profit (7d) |
|---|---|---|
| Titan | 51.98% | +514.04 ETH |
| Quasar | 23.97% | +26.75 ETH |
| BuilderNet | 9.11% | +111.33 ETH |
| Eureka | 8.61% | +6.90 ETH |
| beaverbuild | 2.05% | — |
| bombora | 1.45% | −0.79 ETH |
| bobTheBuilder | 1.00% | +32.89 ETH |

**24-hour share:** Titan 53.97%, Quasar 20.16%, beaverbuild 8.35%, BuilderNet 7.80%, Eureka 6.43%.

Concentration: top-2 ≈ 76% (7d), top-3 ≈ 85% (7d). The derived **HHI ≈ ~3,550** sits well above the 2,500 threshold conventionally called "highly concentrated." Flag clearly: that HHI is a calculation run from the share table, not a published index.

Two corrections to the prevailing narrative. First, **MEV-Boost still builds the large majority of blocks** — well over 90% of mainnet blocks are produced via out-of-protocol PBS across ~100+ active builders [mevboost.pics; re-pull the live dashboard before citing a precise figure] — out-of-protocol PBS remains how Ethereum actually constructs blocks. Second, and more important: the early-2026 framing of a "Titan + BuilderNet ≈ 80% duopoly" is **stale**. Quasar overtook BuilderNet for the #2 slot (~24% vs ~7-9%), and BuilderNet fell to single digits. The thing that did *not* change is Titan's ~50%+ floor. The runner-up seat rotates; the leader does not. That is worth dwelling on, because a stable leader with a churning #2 is a very different market structure than a stable duopoly — it suggests the moat protecting position one is stronger than anything protecting position two.

For context on that churn: beaverbuild retired its centralized builder and migrated to BuilderNet on 2025-05-06 [buildernet.org/blog/beaverbuild], which is part of why the runner-up landscape has been in motion.

## 3. Builder profitability

Share and profit have decoupled, and that is the most analytically interesting fact in the table above. Titan captures the overwhelming majority of builder *profit* (+514 ETH/7d) while several builders run at a loss to hold position — bombora at −0.79 ETH/7d is openly subsidizing [relayscan.io]. Winning blocks and earning from them are now distinct games.

The bidding mechanics underneath this (directional, but the magnitudes are stale — data through Mar 2024 [arXiv:2410.12352]): top-3 builders bid ~26.9% lower than everyone else yet win >95% of the time. Strong builders bid ~70% of block value; weak builders ~90%; some bid *above* value, i.e. they pay to lose money for share. The order-of-magnitude cost to buy 1% of weekly share via subsidy was ≈ up to ~1.4 ETH (single-source, stale, treat as shape not number). Subsidization is a leading cause of auction inefficiency — it accounts for ~51% of inefficient auctions in the one study that measures it [arXiv:2405.01329; dataset Sep 2022–Mar 2024, bid-level Apr–Aug 2023].

Why can the strong bid less and still win? **Latency** and the **winner's curse**. Slower builders price blocks off stale CEX/DEX quotes; when those quotes revise against them they get adversely selected, so they rationally shade bids toward zero to avoid overpaying on information they don't yet have. Latency-advantaged builders simply don't face that curse [arXiv:2311.09083]. And the latency gap is measurable, not folkloric: EU builders show ~13ms median decode/delivery vs ~122ms for North America, with simulation adding another 100-200ms; "block delivery latency acts as a forcing function for builders to colocate with relays" [frontier.tech]. Timing games compound it — a median +1.28% MEV per block, up to +24% at the 95th percentile [arXiv:2312.09654].

The compact way to state it: latency is a forcing function that converts a microsecond infrastructure advantage into a structural profit advantage, because it removes the winner's curse for the fast and imposes it on the slow. That is the engine of the profit concentration in the table.

## 4. Order flow: the real input

If profit follows from avoiding adverse selection, the upstream question is what flow you're allowed to see. Here the numbers are unambiguous about where value lives.

Private/exclusive flow is roughly **54.6% of block value from only ~12% of transactions** [arXiv:2410.12352]. Exclusive order flow is "as much as 35%" of the market bypassing the public mempool [Flashbots, state-of-wallets-2024]. The private mempool grew from ~5-10% in 2022 to >30% of transactions by mid-2024, and exclusive order flows account for ≈ **71% of trading-related builder revenue** [arXiv:2605.04471]. Restate the hinge fact in this light: if only about half of included transactions ever pass through the public mempool, then the public mempool is no longer where the contest is decided.

Where does the valuable flow originate? The percentages here are stale (~Nov 2023) but the shape holds [Flashbots, illuminate-the-order-flow]: Uniswap frontend $4.03B (29% — flag this as **volume**, not revenue), 1inch $3.9B (28%, again volume), solver auctions/OFAs ~26%, and Telegram bots like Maestro and Banana Gun at ~25% of transaction *count* combined (Maestro ~15% + Banana Gun ~10%) — the high-value sniping flow that builders pay premiums to source. Retail private RPCs (MEV-Blocker, Flashbots Protect) routed ~41% of retail transactions.

The competitive consequence, in Flashbots' own framing: exclusive-flow deals "entrenched a duopoly," and builders retained ~1,000 ETH/week distributed almost entirely back to exclusive-flow providers [buildernet.org/blog/introducing-buildernet, stated figure]. That last point is the quiet one — the economic surplus of building isn't all kept by builders; a large share is competed away to whoever controls the flow. Which means the real bottleneck isn't building skill. It's flow access.

## 5. Segmentation: toxic vs uninformed flow

Now the conceptual core. Why does flow *access* dominate building *skill*? Because all of this reduces to a single old idea from equity-market microstructure: adverse selection.

The Glosten-Milgrom canon: a liquidity provider posts a quote, and the *taker* chooses when to hit it. An informed taker executes precisely when the quote has gone stale in their favor. So the LP's average fill is worse than the mid — not from bad luck, but from a structural option the taker holds against them.

This splits flow into two kinds. **Toxic** flow adversely selects the market maker — the MM provides liquidity at a loss. **Uninformed/benign** flow is retail noise, uncorrelated with the next price move, and profitable to internalize [Easley/López de Prado/O'Hara 2012]. One precision worth keeping crisp: toxic ≠ informed. A fill is toxic if the counterparty can *profitably unwind within some horizon*, whether or not they "knew" anything [arXiv:2312.05827, an FX-broker study whose framing carries over cleanly].

The crypto-specific twist is the whole reason this matters [Multicoin, 2026-02-17]: on-chain, the public mempool makes every order look identical, so liquidity gets "priced as if every trader is informed." Market makers widen quotes to survive the worst-case counterparty, which means "less experienced users subsidize more advanced ones." Segmentation — separating toxic from benign before quoting — is what relaxes that constraint.

The continuous-time formalization of the cost toxic arbitrage flow imposes on an AMM is **LVR**, loss-versus-rebalancing [Milionis/Moallemi/Roughgarden/Zhang, arXiv:2208.06046]. For a constant-product pool, normalized instantaneous LVR = **σ²/8** — it scales quadratically with volatility, and it equals the best-case arbitrageur profit. Keep two distinctions clean: LVR ≠ impermanent loss (IL is path-independent loss-vs-holding; LVR is path-dependent loss-vs-rebalancing), and LP profitability reduces to whether fees exceed accumulated LVR.

Here is the punchline that ties the microstructure to the building side: **AMMs are maximally exposed to LVR precisely because their quotes are public, fixed, and non-discretionary — they cannot segment.** A passive x·y=k curve offers the same price to the arbitrageur and the retail buyer because it has no way to tell them apart. Every intent, solver, RFQ, and prop-AMM design in existence is, at root, an attempt to recover the segmentation ability that the passive AMM structurally lacks. Segmentation is the whole game.

(One paragraph on OFAs: an order-flow auction routes a user's order into an auction where searchers/solvers bid for the right to execute or build on it, returning a portion to the user as price improvement or rebate. Benign backrunning — which realigns price — is permitted and rebated; harmful sandwiching is structurally prevented. Live designs include CoWSwap, UniswapX, 1inch Fusion, and MEV-Share / MEV Blocker [a16z, 2025]. The PFOF analogy is exact: traditional PFOF sells the right to *fill*; a crypto OFA can sell *exclusive* access to do more around the order [Flashbots, illuminate-the-order-flow].)

## 6. Barriers to entry

Stack the prior sections and the moat assembles itself. A new builder must do three things at once [arXiv:2605.04471; arXiv:2410.12352]: (1) secure exclusive order flow — which is granted mostly to large, high-win-rate builders, a chicken-and-egg problem; (2) subsidize losing bids off its own balance sheet; and (3) colocate for sub-100ms latency. Each is individually expensive; together they're a wall.

The consolidation timeline makes the wall visible. Late 2022: no single builder held >20%. By September 2024 two dominant builders (Titan and beaverbuild) won ~95% of auctions [arXiv:2412.18074, citing Wahrstätter 2023]. The origin mechanism is Kilbourn's 2022 spin-wheel: exclusive flow → higher inclusion rate → more flow wants to route to you → more exclusive flow [Flashbots, order-flow-auctions-and-centralisation].

Then a sharper, more recent, and genuinely contested claim — the **decoupling thesis** [arXiv:2605.04471, May 2026, single-paper]: by late 2024 the correlation between market share and exclusive-flow income *decoupled*. Dominant builders no longer needed marginal exclusive flow to stay dominant; the moat became self-sustaining; centralization is "an emergent property of the PBS architecture itself." Present it carefully — it's one recent paper, and if true it's a strong claim. But it reframes the policy question. If the flywheel still depended on flow deals, you'd attack the flow deals. If centralization is emergent from PBS itself, no individual intervention on flow fixes it.

## 7. Prop AMMs and how they change block building

"Prop AMM" is informal, analyst-coined terminology that gained currency ~2024-2025, mostly out of the Solana ecosystem. The *model* predates the label — Lifinity ran a proactive, oracle-priced single-operator AMM on Solana from Jan 2022. It isn't a named primitive the way ASS is. What it points at: a market-making venue run by a *single firm* off its *own balance sheet*, with no public passive LPs. Two architectures sit under the label: (a) off-chain signed-quote RFQ — the older "PMM" lineage of Bebop, Hashflow, 0x RFQ, Native, ~2019-2023; and (b) on-chain oracle-priced programs — the Solana "dark AMMs" like HumidiFi, SolFi, Tessera-V (2024-25), and Bebop's bopAMM.

How it differs from a constant-function AMM: single operator vs passive LPs; dynamic quoting vs a public x·y=k curve; the operator internalizes inventory risk and hedges on a CEX rather than being passively adversely selected; near-zero slippage on the quoted leg. In the language of section 5: a prop AMM is a venue built *to segment*. It quotes discretionarily because it can refuse the toxic counterparty.

The connection to block building is the part that is documented rather than speculative. AFT 2025 [arXiv:2507.13023] names two integrated searcher↔builder pairs: **Wintermute↔rsync-builder** extracted $71.4M (paid 66% to its builder as tips), and **SCP↔beaverbuild** extracted $71.1M (paid 81%); the authors estimate these structures "transfer ~90% of arb revenue to affiliated builders." The empirical *signature* is precise: folding the searcher PnL back into the builder turns rsync's apparent **−2.24%** margin into **+27.06%**. A builder running at near-breakeven on tips because the profit is actually booked upstream at the trading desk is exactly what vertical integration looks like in the data. (Flag: those splits are the authors' upper bounds, since off-chain hedge prices are unobservable.)

For scale: CEX-DEX arbitrage moved $233.8M across 7.2M arbitrages (Aug 2023–Mar 2025) via 19 searchers, occupying <2% of block space but >15% of block value [arXiv:2507.13023]. A tiny, high-value slice — exactly the slice an integrated firm most wants to capture.

The structural shift this produces: if a firm controls the quoting *and* becomes or integrates a builder, then searcher, builder, and market-maker collapse into one entity capturing the bid-ask *spread and* the MEV on the same flow. EigenPhi's "builders = market makers 2.0" is a useful *lens* on this, not a documented corporate structure — label it as commentary [eigenphi.substack.com].

The counter-move worth naming: **BuilderNet** — TEE-based (Intel TDX), multi-operator, with encrypted order flow shared equally across operators and an open-source refund rule returning value to users and apps; launched Nov 2024 by Flashbots, Beaverbuild, and Nethermind [buildernet.org]. The relevant fact: the market leader, Titan, has *not* joined. It remains the largest independent builder and BuilderNet's main competitor — which is what makes the shared-network-vs-independent path a live, unresolved fork rather than a settled question.

A precision worth stating carefully, because it's easy to get backwards. AFT 2025 [arXiv:2507.13023] does **not** classify Titan as a vertically integrated builder — on the contrary, it singles Titan out as earning the highest total builder profit *despite not being vertically integrated with any searcher*. What the paper does document for Titan is an *exclusive order-flow* relationship: searchers such as Kayle and Graves route a large share of their volume (52% and 100% respectively) to Titan. Exclusive flow and vertical integration are different structures — the former is a routing arrangement, the latter folds the searcher's P&L into the builder's. Titan's public stance — "neutral builder," "we don't do any searching on Ethereum… to avoid any conflict of interest" [titanbuilder.xyz] — is consistent with the paper's classification. The genuinely open question is therefore not "is Titan secretly integrated" (the literature says no) but the subtler one in section 9: what *neutral* means when a single searcher routes effectively all of its flow to one builder.

## 8. Prop AMMs and application-specific sequencing

This is the intellectual climax, and the framing here is *ours* — analysis, not an industry-defined equivalence.

**Application-specific sequencing (ASS)** is a named concept, unlike "prop AMM." It's the ability of apps to sequence their own users' transactions — customizing how sequencing rights are auctioned — so apps can protect users from harmful MEV and capture value otherwise lost to validators, all *without* building an appchain. It was popularized by Yuki Yuminaga / Sorella Labs, canonical post Oct 14 2024 [corroborated via Archetype 2024-10-16; Uniswap Foundation 2025-01-21]. The term is genuinely unstable — "app-specific sequencing" vs "app-specific sequencer network" — and its lineage predates the name: McAMM (josojo, ethresear.ch Aug 2022), Verifiable Sequencing Rules (Ferreira & Parkes, arXiv:2209.15569), and am-AMM (Harberger-lease pool-manager right, arXiv:2403.03367).

There's a spectrum of how far an app pulls ordering back from the builder, least-to-most sovereign: **Angstrom** (Sorella, Uniswap v4 hook; per-block batch auction plus a top-of-block arb auction rebated to LPs as LVR compensation; live Ethereum mainnet ~July 2025, $7.5M seed led by Paradigm) → **Atlas/FastLane** (generalized execution-abstraction, DAppControl modules) → **Vertex** (off-chain matching, on-chain settlement) → **dYdX v4** (a fully sovereign appchain).

Now the payoff. **Prop AMMs and ASS are the same structural move at different layers.** Both pull execution and ordering control *away* from the neutral, generic block builder and internalize it closer to whoever generates or quotes the flow. Look at section 5 again: the generic builder is exposed to the same problem the passive AMM is — it can't fully segment, and value leaks. Both designs are answers to "the generic block builder shouldn't be the one capturing this value."

But they answer it pointed in opposite directions — they invert *cui bono*:

- A **prop AMM privatizes** the surplus. The spread and the MEV on the flow collapse into one firm's P&L, and — fused with a builder — that same move entrenches the leader's position in the builder market charted in sections 2-3.
- **ASS socializes** it. The surplus is returned to the protocol's LPs and users through credibly-neutral, on-chain rules (Angstrom's LVR rebate to LPs being the cleanest live example).

The cleanest one-liner: both designs agree that *the generic block builder shouldn't capture this value* — and then split on whether to **privatize it to a firm** or **return it to the protocol's users.** That fork is, in miniature, the entire political question of the MEV supply chain. Everything in sections 2 through 7 — concentration, profit decoupling, the flow moat, vertical integration — is the privatize branch playing out. ASS is the bet that the socialize branch can be made to win on neutral rules rather than balance-sheet scale.

(Two terminology footnotes so nothing collides. First, **"bopAMM"**: Bebop's actual public "bopAMM" is "Block Oracle Priced AMM," an oracle-priced on-chain product — keep it distinct from RFQ/JAM solver-auction designs. Second, **"PMM"** has three live meanings — Private Market Maker (RFQ), Proactive Market Maker (DODO), and "prop market maker" — this report means **Private Market Maker** throughout.)

Roadmap context, kept brief: out-of-protocol PBS via MEV-Boost is still how Ethereum builds blocks. ePBS (EIP-7732) is the consensus headliner for Glamsterdam (~H2 2026 target, date fluid and not locked — implementation "trickier than anticipated" per EF Checkpoint #9, 2026-04-10). FOCIL inclusion lists (EIP-7805) were *declined* for Glamsterdam and pushed to the next fork, Hegotá. Fusaka already shipped mainnet 2025-12-03 (PeerDAS), and contained neither ePBS nor FOCIL. The relevance: the in-protocol fixes for the centralization this report describes are real but not imminent, which keeps the out-of-protocol market structure — the one Titan leads — the operative one for the foreseeable future.

## 9. Open questions

The analysis above leaves a set of genuinely unresolved questions. Stated as open, not answered.

1. **The stable leader, the rotating runner-up.** Titan's floor sits around 50%+ while the #2 seat rotated from BuilderNet to Quasar. What protects position one is evidently different in kind from what protects position two — but the literature doesn't yet separate the two moats cleanly.

2. **Subsidy as a competitive instrument.** Some builders openly run at a loss to hold share. Where is the line between healthy competition for flow and an auction that is simply inefficient — and does sustained subsidy ever convert into a durable position, or only rent the seat?

3. **What "neutral" means under exclusive flow.** It's worth being precise here, because the easy version of this question is simply wrong. Titan's public position is "neutral builder," "we don't do any searching on Ethereum… to avoid any conflict of interest" [titanbuilder.xyz], and Titan released its relay source "in contribution to the public good" while being transparent that the builder+relay combination is itself vertically integrated [titanbuilder.substack.com]. The AFT 2025 paper *supports* the no-searching claim — it explicitly notes Titan is **not** vertically integrated with any searcher, yet earns the highest total builder profit [arXiv:2507.13023]. So the open question is not "is Titan secretly a searcher." It's subtler and more interesting: that same paper documents exclusive order-flow relationships in which individual searchers (Kayle, Graves) route 52% and 100% of their volume to Titan. Where is the line between a credibly *neutral* builder and an *exclusive-flow* relationship that, from the outside, confers much of the same advantage? Neutrality at the ordering layer and privileged access at the flow layer are not the same property, and a builder can hold one without the other.

4. **The decoupling thesis.** The claim that market share decoupled from exclusive-flow income by late 2024 — that centralization is "an emergent property of PBS itself" — is a single recent paper. If it holds, interventions aimed at flow deals miss the target. It needs independent replication before it should anchor policy.

5. **Where the collapsed stack ends.** When searcher, builder, and market-maker fuse into one firm capturing spread *and* MEV on the same flow, is that the equilibrium of this market — or a phase that BuilderNet, ASS, or ePBS breaks?

6. **Privatize vs socialize.** The deepest question: where does the surplus pulled off the generic builder ultimately land — privatized into firms' P&L, or socialized back to protocols' users through neutral rules? The two branches are both live, and the market has not chosen.

## 10. Data sources & methodology

This report synthesizes a fixed set of verified facts; no figures were generated or estimated beyond two clearly-labeled derived calculations. Market-share and profit figures were directly verified on relayscan.io as of 2026-06-15. The HHI (~3,550) and the top-2/top-3 concentration percentages are our own calculations from that share table, not published indices — flagged as such in-text. Volume figures (Uniswap $4.03B, 1inch $3.9B) are flagged as **volume**, never conflated with revenue. Several quantitative claims carry explicit staleness tags where the underlying data predates 2025 (bidding magnitudes through Mar 2024; originator percentages ~Nov 2023; subsidization data 2023), and are used directionally. The decoupling thesis (arXiv:2605.04471) is a single recent paper and is presented as contested. Vertical-integration profit splits (arXiv:2507.13023) are the authors' upper bounds, since off-chain hedge prices are unobservable. That same paper classifies Titan as *not* vertically integrated with a searcher; this report follows the paper and frames the open question around Titan's documented *exclusive order-flow* relationships, not around any integration claim — stated as an open question, not a finding. "Prop AMM" is flagged throughout as informal, analyst-coined (~2025) terminology; application-specific sequencing is a named concept (Sorella/Yuminaga, Oct 2024). The prop-AMM ↔ ASS equivalence in section 8 is explicitly our analytical framing, not an industry-defined relationship.

## Sources

**Firms / brands**
- https://gattaca.com
- https://www.titanbuilder.xyz/
- https://titanbuilder.substack.com/p/titan-relay-launch-on-ethereum-mainnet
- https://blog.arbitrum.io/gattaca-titan-timeboost-live-on-arbitrum/

**Market data**
- https://relayscan.io (2026-06-15)
- https://mevboost.pics
- https://buildernet.org/blog/beaverbuild

**Builder economics / profitability / latency**
- https://arxiv.org/abs/2410.12352
- https://arxiv.org/abs/2405.01329
- https://arxiv.org/abs/2311.09083 (Pai & Resnick — structural advantages / winner's curse for slow builders)
- https://arxiv.org/abs/2312.09654
- https://frontier.tech/optimistic-relays-and-where-to-find-them

**Order flow**
- https://writings.flashbots.net/state-of-wallets-2024
- https://writings.flashbots.net/illuminate-the-order-flow
- https://writings.flashbots.net/order-flow-auctions-and-centralisation
- https://arxiv.org/abs/2605.04471
- https://buildernet.org/blog/introducing-buildernet

**Segmentation / LVR / adverse selection**
- https://arxiv.org/abs/2208.06046 (LVR)
- https://arxiv.org/abs/2312.05827
- https://multicoin.capital/2026/02/17/adverse-selection-rules-everything-around-me/
- Easley / López de Prado / O'Hara (2012)

**Vertical integration / prop AMMs**
- https://arxiv.org/abs/2507.13023 (AFT 2025)
- https://arxiv.org/abs/2412.18074
- https://eigenphi.substack.com
- https://buildernet.org/blog/introducing-buildernet

**Application-specific sequencing**
- Sorella / Yuki Yuminaga (canonical post, Oct 14 2024)
- Archetype (2024-10-16); Uniswap Foundation (2025-01-21)
- https://arxiv.org/abs/2209.15569 (Verifiable Sequencing Rules)
- https://arxiv.org/abs/2403.03367 (am-AMM)
- McAMM (josojo, ethresear.ch, Aug 2022)

**OFAs**
- a16z, MEV Explained (2025)

**Roadmap**
- EF Checkpoint #9 (2026-04-10); EIP-7732 (ePBS); EIP-7805 (FOCIL)
