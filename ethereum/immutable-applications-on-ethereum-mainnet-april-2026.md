---
title: "Immutable Applications on Ethereum Mainnet"
date: 2026-04-20
---

# Immutable Applications on Ethereum Mainnet

*A synthesis report written by the apriori-writer agent. | April 2026*

## tl;dr

- **Ethereum's most durable applications — WETH9, Uniswap V2, Liquity V1, Tornado Cash — share one property the last cycle under-valued: they cannot be changed.**

- **The philosophical frame is credible neutrality: open, simple, unchanging, and not written around specific people or outcomes — the properties an immutable deployment realizes by construction.**

- **Five structural forces pulled builders away from immutability — gas costs, token-warrant economics, founder liability after Tornado Cash, institutional demand for responsible parties, and the status economy of governance — and only the first two are loosening.**

- **Morpho Blue (~$6B TVL, April 2026) and Ajna (no oracle, no token, no governance) now define the spectrum of immutable lending, with the layered pattern — immutable core, governed periphery — emerging as the dominant architecture.**

- **The highest-leverage work on Ethereum for the next several years is likely quiet, boring, and permanent: deploying the right primitive once and walking away from the admin key.**

---

## Table of Contents

1. [What "Immutable" Actually Means, and the Credible-Neutrality Frame](#1-what-immutable-actually-means-and-the-credible-neutrality-frame)
2. [How Ethereum Forgot About Immutability (2017-2024)](#2-how-ethereum-forgot-about-immutability-2017-2024)
3. [Why Now: The Mainnet Cost Constraint Is Loosening](#3-why-now-the-mainnet-cost-constraint-is-loosening)
4. [The Historical Ledger: What Immutability Has Produced](#4-the-historical-ledger-what-immutability-has-produced)
5. [Application Categories: Where Immutability Belongs](#5-application-categories-where-immutability-belongs)
6. [Application Categories: Where It Does Not](#6-application-categories-where-it-does-not)
7. [The Counter-Arguments, Steelmanned](#7-the-counter-arguments-steelmanned)
8. [The Layered Pattern: Immutable Core, Governed Periphery](#8-the-layered-pattern-immutable-core-governed-periphery)
9. [What Should Be Built](#9-what-should-be-built)
10. [Data Sources & Methodology](#10-data-sources--methodology)
11. [Sources](#11-sources)

---

# 1. What "Immutable" Actually Means, and the Credible-Neutrality Frame

"Immutable" is one of those words that sounds precise and is used imprecisely. Before any argument can be made about which applications belong in this category, the definition needs to be set clearly — and the philosophical frame behind it has to be named.

The frame is **credible neutrality**. Vitalik's four properties of a credibly neutral mechanism are: *don't write specific people or outcomes into the mechanism; open source and publicly verifiable; keep it simple; don't change it often*. The last property is the one immutability realizes by construction: a contract that cannot change cannot have its neutrality compromised by a future discretionary act. The other three are preconditions that must be satisfied at deployment time, because they can never be fixed later.

The adjacent frame, from [state-of-ethereum-march-2026.md](/ethereum/state-of-ethereum-march-2026.md), is **sanctuary technology**: infrastructure designed so that no one — including the creators — can revoke it. Sanctuary technology is the intended output of a credibly neutral design process. Immutability is the mechanism through which code becomes sanctuary rather than service.

Every section of this report refers back to this frame. When we ask whether a given application belongs on immutable mainnet, the question is not merely "does it work without upgrades?" but "is its neutrality load-bearing, such that the ability to change it would itself be a failure mode?"

With the frame stated, the working definition. For the purposes of this report, an *immutable application* on Ethereum mainnet satisfies all of the following:

1. **No upgradable proxy, no admin key, no governance-controlled parameter changes that affect user funds.** This is the strict reading. A deployment can have a `feeTo` address or an owner that controls non-consequential surfaces, but nothing that can drain a pool, change a liquidation threshold, re-point an oracle, or modify accounting math can be in the hands of any party — DAO, multi-sig, or founder.

2. **No trusted offchain components required for safety.** Offchain components for UX and liveness (indexers, frontends, keepers) are fine, but if the protocol cannot degrade safely when those components go down, it is not immutable in any meaningful sense. The bytecode has to be the whole safety story.

3. **No reliance on external chains, bridges, or rollups for security.** A contract deployed on a governed L2 inherits that L2's governance. An "immutable" application on Base or Arbitrum today is not immutable — it is running on a machine whose instruction set can be changed by a multi-sig.

4. **No dependency on oracles that can be rug-pulled.** If an oracle is used, it must be a Uniswap TWAP, a well-understood Chainlink feed with the trust assumptions made explicit, or a reference-free design. The oracle is part of the trust surface; immutability claims need to account for it.

5. **Deployed once, runs forever, behavior fully specified by bytecode.** What the contract does is what its compiled EVM code does. There is no "real" behavior elsewhere.

6. **The contract is credibly neutral — it doesn't privilege specific users or outcomes.** The mechanism is blind to who is interacting with it and blind to what the "correct" outcome is supposed to be. Uniswap V2 does not know which traders should profit; it applies `x * y = k` regardless. Liquity does not know which troves should be liquidated; it applies a deterministic rule to the ones below threshold.

This definition is strict. Most contracts people call "immutable" don't meet it. That is the point. The goal is not to gatekeep the word but to identify the design space worth exploring seriously.

There are useful adjacent categories worth naming:

- **Governance-minimized.** Uniswap V3 and V4 have governance, but the governance cannot drain pools. The governance surface is the protocol fee switch and nothing more. This is not immutable, but it is meaningfully close, and the design discipline is similar.

- **Immutable core with governed periphery.** The dominant modern pattern. ENS has an immutable registry (`ENSRegistry.sol`) that can never be changed, with registrars and resolvers that can be upgraded or replaced by domain owners. Morpho Blue has immutable market contracts with governed (and user-selectable) vaults and curators on top.

- **Progressive decentralization / "ungovernance."** Reflexer's framing. The protocol begins with governance capacity and progressively revokes privileges, aiming to freeze parameters that can be safely frozen and make the rest as costly to change as possible. This is a path toward immutability, not the state itself.

These distinctions matter because most of the useful work in 2026 lives in the second and third categories. Pure immutability is the right choice for a narrower set of applications than its advocates sometimes claim. But the design discipline — the habit of asking "what is the minimum authority this contract needs over user funds?" — applies much more broadly.

## 1.1 The ossification paradox

There is an obvious objection to writing a pro-immutability report in the middle of a period when the base layer itself is changing faster than it has in years. Pectra, Fusaka, and the Strawmap's seven forks through 2029 are the opposite of ossification. Why argue for application-layer permanence on top of a protocol that is explicitly refusing it?

The answer is that application immutability is viable *because* the base layer is still being made correct. The sequence matters. An application deployed today inherits the current EVM semantics, the current signature schemes, the current gas schedule. If those are wrong in a way that will need fixing, the fix has to happen at the protocol layer before the application can credibly commit to immutability. Post-quantum cryptography is the clearest case: any application deployed before the post-quantum transition is betting that its signature scheme will be preserved through the transition, not replaced.

The correct framing is that L1 is in its *final constructive phase* — the remaining changes are the ones that bring the protocol to a state worth freezing. Strawmap's end state (gigagas L1, ZK-EVM, post-quantum security, sub-second finality) is, arguably, the state after which the argument for L1 ossification becomes strong. Until then, the aggressive upgrade cadence is the price of getting the substrate right. Applications that commit to immutability today are making a bet that the remaining L1 changes will be backward-compatible with their assumptions. For most of the application categories discussed in Section 5, that bet is reasonable. For signature-dependent applications, it is a harder call.

This is not a clean answer. It is the honest one. Application immutability on a mutable base layer is a layered bet, and the report's enthusiasm for both the Strawmap and for immutable applications is coherent only if the sequencing claim holds.

# 2. How Ethereum Forgot About Immutability (2017-2024)

To understand why Ethereum under-built immutable applications during the last cycle, it helps to inventory the structural forces that pulled builders in the opposite direction. A two-force story — gas costs and token-warrant economics — captures roughly half of the drift. A fuller account names five. None of them was about what users actually needed; all of them were about what was easiest to finance, ship, and defend.

## 2.1 Gas costs drove users off mainnet

By 2020-2021, mainnet gas had become prohibitive for most consumer use cases. A simple ERC-20 transfer could cost tens of dollars during peak demand; a DEX swap regularly cost more than the slippage it saved. The response, reasonable at the time, was to build elsewhere: L2 rollups, sidechains, and parallel EVMs absorbed the volume. This was not a mistake in the aggregate — data-availability scaling via rollups is a genuine innovation, and blob space introduced in Dencun materially reduced L2 costs.

But the L2 ecosystem shipped with an implicit bargain that was rarely stated plainly: to get fast iteration and low costs, users accepted admin keys, upgradable contracts, and centralized sequencers. Most L2s today retain a multi-sig or security council that can, in an emergency, upgrade the rollup's core contracts, pause withdrawals, or re-point the proving system. Stage 2 rollups — fully trust-minimized, no admin override — are still the exception rather than the rule.

Vitalik's own framing shifted on this in early 2026: "the original vision of L2s and their role in Ethereum no longer makes sense, and we need a new path," citing "L2 decentralization has lagged (no major rollup at Stage 2), and L1 is scaling directly" ([The Block, Feb 2026](https://www.theblock.co/post/388285/vitalik-buterin-reevaluates-rollup-centric-roadmap-arguing-l2s-decentralized-far-slower-while-ethereum-base-layer-advanced)).

The point is not that L2s are bad. They are useful for a wide range of applications. But an "immutable" application deployed on a governed L2 is not immutable in the sense that matters. It inherits the L2's governance surface. The rollup-centric roadmap absorbed a great deal of the energy that might otherwise have gone toward deploying durable, ossified primitives on L1.

## 2.2 Token-warrant economics reshaped what got built

The second force was financial. Between 2020 and 2023, the dominant path to funding a crypto protocol was:

1. Form a company.
2. Sell equity *with token warrants* to venture investors.
3. Build a protocol whose governance is controlled by a token the company will launch.
4. Launch the token, satisfying the warrant.

This structure works if the protocol has a token. It does not work if the protocol is a pure primitive that would be harmed by the governance surface a token introduces. A token-warrant-financed team building an AMM is structurally pushed toward designs like Curve or Balancer (rich governance, token-directed incentives, upgradable components) rather than toward Uniswap V2 (no token at deployment, immutable contracts, no governance over pools).

It is useful to distinguish two modes of VC participation here, because the record is genuinely mixed. VC funding of *public goods infrastructure* — the Rust compiler, Foundry, ZK research that produced practical production systems, distributed systems research that became the protocol-engineering basis for modern rollups, mechanism-design work that clarified problems crypto didn't know it had — has been a net positive. This work does not require a token; it required capital willing to be deployed against infrastructure, and that capital came from somewhere. VC funding that *pushed token economics onto protocol designs that would otherwise not have them* is the distortive mode, and the one that matters here. It is the second mode that rewarded teams who could ship a token-governance-compatible architecture and made "deploy an immutable contract and walk away" commercially irrational — not because the work wasn't valuable, but because there was no financing structure that captured its value for the team that did the work. You could build Uniswap V2 in 2020, but you could not easily raise a venture round against the plan "we will deploy it and never touch it again."

This shaped the population of applications that got built. Governed stablecoin protocols with elaborate token economics proliferated. Governance-free stablecoins like Liquity remained a small corner of the market. Governed lending protocols with tokens (Aave, Compound) became the default; governance-minimized lending (Morpho Blue, post-2024) took years to emerge as the reference design.

The consequences of this drift are now visible. The Euler hack of March 13, 2023 — a ~$197M exploit that traced directly to a new function (`donateToReserves`) introduced in a protocol patch ([Cyfrin, 2023](https://www.cyfrin.io/blog/how-did-the-euler-finance-hack-happen-hack-analysis); [BlockSec, 2023](https://blocksec.com/blog/euler-finance-incident-the-largest-hack-of-2023)) — is the canonical case study. The upgrade surface was the attack surface. An immutable deployment would not have had the function to exploit.

This is not to say immutable protocols are safer in general. They are safer against this specific class of bug, the "upgrade introduced a new vulnerability" bug. They are riskier against other classes, particularly bugs latent in the original deployment. The tradeoff is real, and we will return to it in Section 7.

## 2.3 Regulatory pressure and founder liability

The 2022-2025 arc of the Tornado Cash cases changed the calculus for founders. The Roman Storm prosecution (indictment August 2023, split verdict August 2025) established that shipping immutable code is a personal act, not an impersonal one, and that prosecutors were willing to pursue developers of immutable protocols. Counsel to crypto founders during this period advised against pure immutability for a straightforward reason: a protocol with an admin key has a designated party who can be compelled to comply; a protocol without one does not. The absence of institutional recourse against the protocol leaves only recourse against the people.

This dynamic has softened with the Fifth Circuit ruling and the Treasury de-listing, but not fully. A founder in 2026 considering whether to deploy an immutable privacy protocol faces a different risk calculus than a founder in 2021 considering whether to deploy an immutable AMM. The chilling effect is specific and asymmetric, and it bears on the categories in Section 5 unevenly.

## 2.4 Institutional demand-side network effects

By 2023-2024, DeFi was being underwritten, audited, insured, and intermediated by institutions with clear requirements: there must be a responsible party. Custodians need a counterparty for operational issues. Auditors attach their signatures to protocols that can receive and respond to findings. Insurance underwriters price risk on governance-responsiveness. Market-makers need a point of contact when the math goes wrong. An immutable protocol satisfies none of these requirements by design — the contract is the whole counterparty. Institutions can and do interact with immutable protocols, but the institutional overlay has a structural preference for protocols with a governance surface, and that preference shapes which protocols get the largest flows.

## 2.5 Founder psychology and the status economy

The final force is the hardest to quantify and the most honest to name. A founder who ships an immutable protocol forfeits a specific kind of ongoing presence: the governance-proposal victory lap, the parameter-tuning update, the roadmap announcement. The Twitter-native culture of crypto rewards ongoing participation, and ongoing participation is structurally easier for teams with mutable protocols. This is selection pressure, not a moral failing — but it is real, and the 2020-2024 cohort of protocol founders skewed toward the teams for whom ongoing presence was the point.

## 2.6 The probabilistic picture

Putting the five forces together: immutability's share of the new-protocol distribution is recovering from a period of aggressive contraction, but the recovery is partial and contested. The mainnet cost constraint (2.1) and the token-warrant constraint (2.2) *appear* to be loosening. The regulatory (2.3), institutional (2.4), and psychological (2.5) constraints are not. An outcome in which the 2026-2029 cohort ships 5-15% of new protocols as immutable is the base case; 25%+ is the upside; a reversion under new packaging (points, airdrops, RWA-with-governance) is the downside. Section 3 quantifies the mechanical part of this; Section 7 returns to the probabilistic picture in full.

# 3. Why Now: The Mainnet Cost Constraint Is Loosening

The case for revisiting immutability on mainnet in 2026 rests on a specific empirical claim: the scaling trajectory of L1 has changed enough that "deploy on mainnet" is no longer an exotic choice for most applications in the categories that benefit from immutability.

## 3.1 The gas limit arc

The relevant sequence:

- **February 4, 2025:** Gas limit increased by ~20% via validator signaling, from 30M to 36M — the first meaningful bump in years ([Blockworks, 2025](https://blockworks.co/news/ethereum-increases-gas-limit-pectra)).
- **May 7, 2025:** Pectra activated on mainnet (epoch 364032) with the groundwork for further increases ([ethereum.org](https://ethereum.org/roadmap/pectra/)).
- **December 3, 2025:** Fusaka activated on mainnet. Fusaka introduces the *protocol capacity* for a 150M gas limit via PeerDAS and EIP-7918. The actual validator-signaled gas limit post-activation has ramped more gradually; as of April 2026 active gas limits are in the ~60-100M range, with further increases expected to be signaled incrementally ([CoinGecko, 2025](https://www.coingecko.com/learn/what-is-ethereum-fusaka-upgrade)).

The practical change over twelve months is roughly 2-3x on the active signaled gas limit, with the ceiling set high enough (150M) that further signaling can continue for some time without another hard-fork. The distinction between *enabled capacity* and *active capacity* matters: Fusaka did not instantly triple throughput, but it removed the protocol-level ceiling that would otherwise have bound further growth.

## 3.2 The Strawmap trajectory

The Ethereum Foundation's Strawmap — published in February 2026 and endorsed by Vitalik as "a very important document" ([state-of-ethereum-march-2026.md](/ethereum/state-of-ethereum-march-2026.md); [strawmap.org](https://strawmap.org/)) — charts seven forks through 2029 on a six-month cadence, targeting:

| Long-range objective | Target |
| --- | --- |
| Gigagas L1 | ~1 gigagas/sec (~10K TPS on L1) |
| Sub-second finality | Target latency ~1s |
| Teragas L2 | ~1 GB/sec DA (~10M TPS across rollups) |
| ZK-EVM on L1 | Stateless validity proofs, rollups settle without fraud windows |
| Post-quantum security | New signature and hashing primitives |

If the Strawmap ships as planned — and Pectra and Fusaka both shipped on schedule in 2025, suggesting the six-month cadence is achievable — then the mainnet cost environment of 2028 looks nothing like 2020.

## 3.3 The implication for application design

For applications where the throughput demands are modest (a stablecoin redemption, a DEX swap, a vesting release, an ENS registration), mainnet in 2026 is already cheap enough. For applications where throughput matters more (high-frequency prediction markets, onchain games), the Strawmap trajectory *appears* to make L1 competitive within 3-5 years, though whether the cadence holds through 2029 is not yet certain.

This is the window in which the "deploy immutable on mainnet" decision becomes reasonable again for a large class of applications that were priced out in 2021-2024. If a team is designing a new primitive today — a lending market, a privacy pool, a time-lock, a settlement contract — the question "should this run on L1 or on an L2?" has a different answer than it did three years ago. For applications where immutability and inheritance of Ethereum's security properties are the defining value proposition, L1 is now a viable choice in ways it was not three years ago. Whether it becomes the *default* choice for this category will depend on which of the forces in Section 2 dominates over the next two years.

# 4. The Historical Ledger: What Immutability Has Produced

Before arguing about which applications *should* be immutable, it is worth surveying what the immutable applications Ethereum already has have actually produced. The track record is older and better than the current discourse gives it credit for.

## 4.1 The DAO and legitimacy as a scarce resource

The starting point for any serious discussion of immutability on Ethereum is The DAO. Deployed in April-May 2016, exploited on June 17, 2016, drained of roughly 3.6M ETH via a recursive-call vulnerability. The Ethereum Foundation's initial response proposed a soft fork — invalidating transactions touching the vulnerable contract from block 1,760,000 — and ultimately a hard fork that rolled back the exploit, producing the Ethereum / Ethereum Classic split ([Ethereum Foundation, June 2016](https://blog.ethereum.org/2016/06/17/critical-update-re-dao-vulnerability)).

The lesson the ecosystem took from this, not always consciously, is often summarized as "code is law has a governance layer above it." That framing is correct but incomplete, and the incomplete reading has done damage.

The more rigorous reading is that the social layer is a *coordination mechanism of last resort whose legitimacy depends on not being used*. The DAO fork was not simply a one-time response to a specific fact (that the loss represented a large fraction of all ETH in existence). It was a *legitimacy expenditure that could not be repeated without debasement*. The community spent a finite store of social consensus to roll back one attack, and the cost of spending it was precisely that the next attack could not be rolled back on the same terms. Every non-invocation since — the dozens of hacks and protocol failures that were all left to affected users — has reinforced the norm that the social layer will not be re-invoked. Each non-invocation is a deposit into the legitimacy account; each invocation is a withdrawal.

This is the frame that matters for thinking about immutability. An immutable application deployed today is operating inside a system whose credibility as a commitment device depends on the scarce-use property of the social layer. If the community invoked its fork power routinely, no application's immutability would be credible; the threat of rollback would always loom. Because the community has maintained the norm for a decade, application-layer immutability carries meaning.

"Code is law" is a useful first-order model because the limiting case is structurally rare. The correct reading is that immutability at the application layer is practically binding for any failure smaller than existential, and the credibility of that binding is a public good maintained by the community's discipline in not invoking the exception.

## 4.2 2018-2019: early immutable primitives

**Uniswap V1** (November 2018) was the first constant-product AMM to get real usage. The architecture was crude — separate factory, one pool per pair, no routing — but it established the design principle that has carried through the AMM lineage: the pool behavior is fully specified by the bonding-curve math and the contract is deployed once.

**Augur V1** (July 2018) is the cautionary tale. Augur shipped as a fully immutable, fully decentralized prediction market. The UX was a disaster: settlement took weeks, user-interface state was hard to reconstruct, oracles were slow. The protocol was correct but unusable. Augur V2 introduced more sophisticated reporting and settlement — and moved away from strict immutability in the process. The lesson was not that immutability is wrong but that immutable applications need to be *correctly designed for immutability*. Shipping an ambitious design and relying on upgrades to fix UX later is not compatible with never upgrading.

**MakerDAO's single-collateral Dai (SAI)** launched in 2017 and was migrated to multi-collateral Dai (MCD) in November 2019. The MCD migration required a user-initiated swap from SAI to DAI. Maker made the migration work, but the episode illustrated the cost of on-protocol changes even when they are "optional" for users.

## 4.3 2020-2021: the canonical deployments

**Uniswap V2** (May 18, 2020) is the reference point for immutable DEX design. The contracts are deployed once, have no admin key that can touch pool math, and have run with identical bytecode for nearly six years of continuous operation. The only "governance" surface is the `feeTo` parameter — a field that can redirect a fraction of swap fees to a specified address but cannot otherwise affect pool accounting. V2's `feeTo` switch has never been activated; V3's protocol fee *has* been activated on specific pools since 2024 via governance, which is a smaller surface than a mutable pool but not zero. V2 still handles billions of dollars of daily volume years after V3 and V4 shipped with superior capital efficiency. Its persistence is not nostalgia; it is the value of an integration point that every wallet, aggregator, and bot in the ecosystem has calibrated against. V2's legitimacy is *performance* legitimacy in the six-sources framework: it is canonical because it works, and has continued to work, for every caller that has ever depended on it.

**WETH9** is the extreme case. Deployed in 2017, never changed. Since the Merge alone (September 2022), it has processed roughly 275M operations on mainnet L1 — including reads and state-changing calls; the narrower `transfer` count was ~179M and `transferFrom` ~24M per Limit Break's Dune breakdown ([Limit Break, 2024](https://medium.com/limit-break/introducing-wrapped-native-a-modern-replacement-for-wrapped-ether-cc0431c8a964)). Every DEX, lending protocol, wallet, and bridge on Ethereum has hard-coded its address. There have been proposals for a modernized WETH (WETH10, cWETH, wrapped-native), and some of them are technically superior. None has displaced WETH9, because the cost of coordinating a migration across thousands of integrators exceeds the benefit of the improvement. WETH9 is a Schelling point; its immutability is what makes it one. In the six-sources-of-legitimacy framework (brute force, continuity, fairness, process, performance, participation), WETH9's legitimacy is almost purely *continuity*: it is canonical because it has been canonical.

**Tornado Cash** (deployed in 2019-2020 across multiple pool sizes) is the single hardest test of immutability ever conducted on Ethereum. In August 2022, OFAC sanctioned the protocol, placing smart contract addresses on the SDN list — an unprecedented legal theory that immutable bytecode could itself be "property" subject to blocking under IEEPA. The contracts continued to run; they could not not run. In November 2024, the Fifth Circuit held that OFAC had overstepped, specifically because the immutable contracts "lacked the hallmarks of ownership, control, and exclusivity" ([Mayer Brown, 2024](https://www.mayerbrown.com/en/insights/publications/2024/12/federal-appeals-court-tosses-ofac-sanctions-on-tornado-cash-and-limits-federal-governments-ability-to-police-crypto-transactions)). On March 21, 2025, Treasury de-listed the contracts. The protocol had outlasted the sanctions.

The Roman Storm trial (verdict August 6, 2025) is a separate matter. The jury convicted Storm of conspiracy to operate an unlicensed money-transmitting business, deadlocked on money-laundering and sanctions-violation counts ([Coindesk, 2025](https://www.coindesk.com/policy/2025/08/06/roman-storm-guilty-of-unlicensed-money-transmitting-conspiracy-in-partial-verdict)). The significance for immutability is a specific one: developer liability for having *shipped* code is being litigated even as the protocol itself — the immutable deployment — has been legally cleared. The protocol is durable; the people who wrote it are not. This is exactly the separation immutability is supposed to enable. It is also the hardest part of it to build.

**Liquity V1** (April 2021) is the purest case of a governance-free stablecoin. No admin key, no upgradability, parameters fixed at deployment, redemption-and-stability-pool mechanism specified entirely in bytecode. LUSD is overcollateralized with ETH at a 110% minimum collateral ratio; one LUSD is redeemable for one USD of ETH at any time; if LUSD trades below $1, arbitrageurs redeem; if it trades above, borrowers draw more. The protocol has now run for roughly five years without protocol-level governance of any kind ([docs.liquity.org/liquity-v1](https://docs.liquity.org/liquity-v1/faq/general)). Liquity V2 introduced minimal governance — limited to distributing protocol liquidity incentives, with no other powers — specifically because the V1 team found that *even optional* governance over core mechanics creates a long-term capture surface they did not want to inherit.

**Reflexer / RAI** (February 2021) took a different path: a non-pegged stable asset governed by a PID controller that targets the asset's market price rather than the dollar. Reflexer named its token FLX — the "Reflexer Ungovernance Token" — precisely to commit publicly to progressive removal of governance over the protocol. The experiment produced a stable asset detached from the dollar and an explicit template for "marching towards ungovernance" ([Medium, Reflexer Labs, 2021](https://medium.com/reflexer-labs/marching-towards-ungovernance-19a220dbdefd)). RAI never reached the scale of LUSD, but its design philosophy — and Stefan Ionescu's framing of ungovernance as a discipline — deserves more attention than the market cap suggests.

## 4.4 2022-2024: the reassessment

**Euler's March 2023 exploit** is the clearest cautionary tale of the upgrade era. The `donateToReserves` function that became the attack vector was introduced in a protocol patch; the upgrade itself was what created the vulnerability ([Cyfrin, 2023](https://www.cyfrin.io/blog/how-did-the-euler-finance-hack-happen-hack-analysis)). This is the specific failure mode that immutable protocols cannot exhibit: a deployed-once contract cannot introduce new bugs via patches. It can have latent bugs, but not introduced ones.

**Morpho Blue** (launched January 11, 2024) is the modern reference design. Singleton contract, ~650 lines of Solidity, not upgradable, five parameters fixed at market creation (loan asset, collateral asset, LLTV, oracle, interest rate model), permissionless market deployment ([morpho.org, 2024](https://morpho.org/blog/morpho-blue-and-how-it-enables-our-vision-for-defi-lending/)). The governance token exists and has scope, but the scope is explicitly limited: "Morpho Governance cannot halt the operation of a market or manage funds on users' behalf, nor does it impose specific oracle implementations." Risk management is externalized to a separate layer — MetaMorpho vaults and curators — that users choose. Roughly 27 months after launch: deposits reached ~$10B by late 2025 and sit at ~$5.8-6.9B as of April 2026 per DefiLlama (a drawdown from peak, not a structural failure), with active curators and deployments across 18+ chains.

**Ajna Finance** is the more radical case in the same category. Ajna has no governance token and no governance surface at all, and uses no oracle — liquidations are driven by borrower-set collateralization ratios enforced by market participants rather than by price feeds. This is a more extreme design than Morpho Blue (which still has a governance token with bounded scope). Ajna's adoption is smaller, but it is the purer expression of the immutable-lending thesis. The existence of both deployments matters: the design space is being explored at different points along the governance-minimization axis, and users can select which end suits them.

**ENS** has been running the immutable-registry / governed-periphery split since 2017. The `ENSRegistry.sol` contract was deployed immutably. An important nuance: in 2020, the ENS DAO migrated to `ENSRegistryWithFallback`, which reads from the original registry for legacy names while allowing new allocations on the new contract. The original registry is still immutable and still on-chain; the *canonical* registry that most applications now read from has changed. This is a distinction worth being precise about. The immutability of the underlying contract is preserved; the Schelling point has migrated via DAO action. Resolvers and registrars that sit on top are upgradable by domain owners. Nick Johnson's design anticipated the pattern Morpho Blue later formalized: the part that must never change (the namespace ownership mapping) is fixed per-contract; the parts that need to iterate (how a name resolves to an address, how names are allocated) are mutable at the user's discretion, not the protocol's. ENS's legitimacy is *participation* legitimacy — the DAO migration was achieved through community coordination, not unilateral action.

These are not isolated examples. They are a lineage. The applications that have produced durable value on Ethereum *appear* to disproportionately share the immutability property, and several of the largest exploit losses have traced to upgrade surfaces. A survivorship-bias caveat applies: the immutable protocols cited in this report are survivors, and a complete denominator of immutable deployments from 2018-2022 — including the many that were exploited, abandoned, or simply never used — is not publicly available. A rigorous counterfactual study would enumerate that denominator and compare attrition rates against a matched population of governed protocols. That study has not been done. The correlation argued here is suggestive rather than conclusive, and the claim is that the pattern is strong enough to take seriously as a design input, not that it has been empirically established.

# 5. Application Categories: Where Immutability Belongs

This section argues through the application categories where immutability is the correct default. The standard throughout is whether the application's behavior is *specifiable in a way that does not need to change over time*. If it is, immutability is an asset. If it isn't, the argument is weaker.

## 5.1 Spot DEXes and constant-function AMMs

**The case for immutability is strongest here.** A constant-product AMM is fully specified by the invariant `x * y = k`. There are no parameters that genuinely need to change over time. The fee rate at deployment is a design choice that can be varied across deployments (V2 pools are 30 bps; V3 pools can be 1, 5, 30, or 100 bps per pool tier); but once a pool is deployed, there is no legitimate reason for its math to change. The pool either handles the liquidity pair correctly or it doesn't.

Uniswap V2 is the proof of concept. V3 introduced concentrated liquidity and kept the same design discipline — the core pool contracts are not upgradable, governance is limited to the fee switch. V4 introduces hooks, which re-complicate the picture by making individual pools extensible with custom logic, but the core pool manager retains the same design philosophy.

The counter-argument — that V3 and V4 are "better" than V2 and therefore V2's immutability prevented progress — has the argument backward. V2 did not prevent V3 from being deployed; V3 is deployed alongside V2. Users and integrators migrate at their own pace. The cost of V2 remaining frozen is that some integrations still use it when they could use a more capital-efficient alternative; the benefit is that those integrations have a stable contract to point to. The correct model is not "upgrade V2 to V3" but "deploy V3 as a new contract and let users choose."

## 5.2 Non-custodial stablecoins

Liquity V1's argument is simple: *a stablecoin that can be governed is a stablecoin that can be captured*. The value proposition of a decentralized stablecoin is that it is not subject to discretionary decisions about who can hold it, what collateral is acceptable, and how redemptions work. If those decisions can be made by a DAO, the DAO is a capture surface.

The counter-argument — that a governance-free stablecoin cannot adapt to new collateral types or respond to market stress — is real but weaker than it first appears. Liquity V1 accepts only ETH as collateral by design. This is a limitation, but it is also a feature: the collateral is the most credibly neutral asset in the ecosystem. New collateral types are not added; instead, new governance-free stablecoin protocols are deployed with their own collateral choices. The population of protocols, rather than any single protocol, handles the diversification.

Stress response is handled algorithmically. When LUSD trades below $1, the redemption mechanism activates: any holder can exchange 1 LUSD for $1 of ETH from the riskiest troves, which both burns LUSD and gives arbitrageurs a profit. When LUSD trades above $1, borrowers can draw more LUSD against their ETH at a lower cost than buying it. The system's response to depeg is specified entirely in contract logic. This is less flexible than Maker-style governance but also less vulnerable to governance attack.

A necessary disclosure: Liquity V1 uses Chainlink as its primary price oracle with Tellor as a fallback. This is not a protocol that runs with zero external dependencies. Per the Section 1 definition, oracles are part of the trust surface and have to be disclosed; a strict reading of "immutable application with no rug-pullable oracle" requires examining what happens under oracle failure or manipulation. Liquity's two-oracle design (with algorithmic fallback logic between them) is a serious attempt to minimize that surface, but it does not eliminate it. Any claim that Liquity is strictly immutable has to carry this asterisk.

The argument for immutability in stablecoins is strongest for stablecoins that target pure decentralization (Liquity, RAI). It is weaker for stablecoins that explicitly embrace real-world-asset collateral (USDS/DAI in its current form). The two design choices are not in the same category; the governance cost of managing a billion dollars of treasury bills is different from the governance cost of managing an ETH-collateral trove system. Protocols should pick one lane and commit.

## 5.3 Privacy protocols

**The case for immutability in privacy protocols is the strongest of any category.** A privacy protocol that can be governed is a privacy protocol that can be revoked. The Tornado Cash episode is not an edge case; it is the central use case for the argument.

Privacy that depends on a governance vote or admin key is not privacy in any meaningful sense. It is a privilege granted by the governance body, revocable at will. Users who relied on that privacy have no recourse when the governance body changes its mind, is sanctioned, or is captured.

Immutable privacy protocols make this explicit. The protocol does not have a pause button. It does not have a user-blocklist. It does not have an oracle that can be re-pointed. It either preserves anonymity for everyone or preserves it for no one.

The OFAC case established the legal stakes. In its November 2024 ruling, the Fifth Circuit held that the Tornado Cash smart contracts "cannot be classified as 'property' under IEEPA because they lacked the hallmarks of ownership, control, and exclusivity. Once deployed, these contracts could not be changed, deleted, or restricted. No person or entity — including Tornado Cash's original developers — had the ability to control their operation" ([Mayer Brown, 2024](https://www.mayerbrown.com/en/insights/publications/2024/12/federal-appeals-court-tosses-ofac-sanctions-on-tornado-cash-and-limits-federal-governments-ability-to-police-crypto-transactions)). The immutability was legally material. If the contracts had had an admin key, the analysis would likely have gone the other way.

Privacy Pools (the Ameen Soleimani / Vitalik / et al. framing from 2023) extends this with an opt-in compliance mechanism — users can prove their deposit came from a subset that excludes known-bad addresses — while preserving the underlying immutability. This is the right pattern: the privacy layer is immutable; the compliance overlay is application-layer and opt-in.

Railgun takes a similar approach, embedding privacy in an immutable contract. The design philosophy is that the privacy primitive is a public good and must be designed to never be captured.

The cost of this discipline is real: immutable privacy protocols cannot add new features, cannot patch newly-discovered cryptographic weaknesses in-place, and cannot respond to regulatory pressure with anything other than "deploy a new protocol." This is the right tradeoff. The alternative — a governed privacy protocol — is a contradiction in terms.

## 5.4 Escrow, streaming, vesting, time-locks

Applications in this category are pure computation over time. A token streaming contract specifies a stream rate and a duration; nothing about it needs to change once the stream is started. A vesting contract specifies a cliff, an end date, and a recipient; the math does not need governance.

**Sablier** pioneered token streaming with this design discipline. **Superfluid** adds more sophisticated flow math. **Llamapay** offers a streamlined streaming primitive. All benefit from being deployed as immutable primitives that can be composed by higher-level applications.

The counter-argument is weak: "what if the recipient address is lost?" is answered by user-side recovery flows, not by admin keys on the streaming contract itself. "What if the token being streamed is upgraded?" is a question about ERC-20 ecosystem behavior, not about the streaming primitive.

## 5.5 Prediction markets with canonical resolution

This category is more subtle than it looks. A prediction market has two parts: the market contract (which tracks positions and pays out on resolution) and the oracle (which decides the outcome). The market contract can and should be immutable. The oracle is where the nuance lives.

UMA's optimistic oracle is a useful reference. It is not an immutable oracle in the strict sense — disputes trigger a governance-adjacent voting process — but it is credibly neutral within its design: any party can propose an outcome, any party can dispute, and the resolution process is specified. The market contract that consumes the oracle's output does not need to be upgradable.

Augur V1's failure was not that it was too immutable; it was that the immutable design was wrong. The reporting mechanism took weeks. The UX burden fell on users. An immutable design has to be correct to work; Augur's was not.

Polymarket's choice to run on Polygon with governance-compatible tooling is a pragmatic compromise. For a market that prioritizes UX and mainstream reach, it is defensible. But it is not the design a protocol designed for maximum censorship resistance would choose.

## 5.6 Identity, naming, and attestation

ENS is the dominant pattern: the registry is immutable, everything above it is upgradable by the domain owner. This is precisely correct. The namespace ownership model — who owns `vitalik.eth`, full stop — must never change, because the entire downstream infrastructure (wallets, email, websites) depends on that invariant being stable. But what a domain *resolves to*, what records it exposes, what subdomain policies it enforces — those are domain-owner decisions, not protocol decisions.

EAS (Ethereum Attestation Service) and ERC-725/735 follow a similar pattern. The attestation primitive is a minimal, stable layer; the attestation schemas and verification logic can be upgraded per-use-case.

## 5.7 Onchain games and creative permanence

For games, there is a specific argument that cuts against upgradability: **if the rules can change, it isn't a game, it's a service.** A chess engine that can change the rules of chess is not playing chess. An onchain game whose rules can be patched is a vendor relationship, not a game.

Dark Forest (Brian Gu and collaborators, 2020) is the canonical example, and it is more than a game. It is a d/acc info-defense artifact — a demonstration that an adversarial-information-environment game (players cannot see each other's positions without ZK proofs) can be deployed as an immutable public good. The game is deployed as an immutable smart contract plus a ZK circuit; the rules cannot change mid-season. Players who invest strategic effort do so against a stable rule set. That the same primitive pattern — immutable contract plus ZK circuit — underlies both an adversarial game and serious privacy infrastructure (Tornado Cash, Railgun, Privacy Pools) is not a coincidence. Both require the same property: mechanism resistant to mid-flight modification.

Loot (2021) and the autonomous-worlds movement (MUD, Dojo) extend this philosophy. The core argument is that onchain games should be world-scale public goods whose state and rules are verifiable by anyone and modifiable by no one. A governance layer would destroy the property that makes them interesting.

The counter-argument — that games need iteration for fun and balance — is real but is addressed by *deploying new games*, not by patching old ones. The Super Nintendo cartridge for *Super Mario World* is immutable; if Nintendo wants to improve the game, they ship *Super Mario World 2*. Onchain games can follow the same pattern.

## 5.8 Asset wrappers and canonical primitives

WETH9 is the extreme case discussed above. The argument generalizes: any contract whose role is to be a canonical Schelling point — a place that thousands of other contracts will hard-code — benefits enormously from being immutable. The coordination cost of migrating off a canonical address exceeds the technical benefit of any improvement except in the most extreme cases.

This argues for a specific discipline: when designing a primitive that is likely to be adopted as a Schelling point, design for immutability from the start. Do not ship with an admin key "just in case." The admin key is not free; it is a commitment device that makes the contract less canonical.

## 5.9 Lending primitives

Morpho Blue has become the reference design here. The lending primitive itself is immutable: the market contracts, the liquidation math, the interest-rate accrual logic. What is upgradable is the curation layer — the MetaMorpho vaults that allocate user deposits across markets, and the risk curators who choose which markets to use. This is precisely the right split. Lending math does not need to change after deployment. Risk curation (which collateral is acceptable at what LTV for what borrower profile) needs to change constantly, because the world changes.

Ajna Finance is the more radical reference. Ajna has no governance, no governance token, and — crucially — no oracle. Liquidations are driven by borrower-set collateralization parameters, enforced by market participants who stand to profit from bad positions. The design trades some of Morpho Blue's capital efficiency for a stricter immutability property: there is no admin surface at all, and the protocol inherits no trust assumptions from any external oracle. For the category of lending primitives that belong on immutable mainnet, Ajna sits further out on the axis than Morpho Blue, and its existence is important even if its TVL is smaller.

Aave's design makes the opposite choice: a monolithic governed protocol where the lending math is changeable via governance. This is defensible if you trust the governance; it is a larger trust surface if you don't. The empirical history — Aave has avoided a governance-attack exploit for the better part of a decade — supports the Aave side of this argument, but the track record is not as long as the Uniswap V2 side.

The right framing is that the design space for lending has now been explored at three distinct points: monolithic governed (Aave), layered governance-minimized (Morpho Blue), and strictly ungoverned (Ajna). These are different products, not better and worse versions of the same product. The lesson for protocol designers is that the immutable-primitive approach is viable for lending without being the only viable approach.

## 5.10 Plural mechanism design and public-goods funding primitives

This category has a stronger immutability argument than yield strategies or even stablecoins, and it deserves more attention than it usually gets.

Quadratic funding, quadratic voting, pairwise-comparison matching, and retro-funding are all *mechanisms whose integrity depends on the rule being fixed for the duration of the round*. A QF round whose matching formula can change mid-round is not a QF round; it is a discretionary allocation dressed in QF aesthetics. The game-theoretic property of QF — that it approximates a Pareto-optimal allocation under individually-rational contributions — holds only if participants know the allocation rule will not change after they commit. If the rule can change, participants game the expected future state of the rule instead of their actual preferences, and the mechanism's properties collapse.

This is a *stronger* immutability case than the one for yield strategies. Yield strategies benefit from immutable accounting and mutable strategy, because the strategy's job is to respond. Plural mechanisms are the opposite: the allocation rule is the thing, and it has to be committed in advance to be credible. The mechanism's integrity *is* its immutability-within-a-round.

The appropriate design pattern is: the mechanism logic (QF math, pairwise-comparison math, retro-funding scoring) is deployed as an immutable primitive that any round calls into; per-round configuration contracts instantiate specific rounds and then become immutable for the round's duration; off-chain governance handles eligibility and disputes but cannot override the arithmetic.

Gitcoin's Allo protocol is moving in this direction. Optimism's RetroPGF contracts formalize parts of this split. The design space is still young — pairwise-comparison voting, conviction voting, holographic consensus are all candidates for canonical immutable primitives that have not yet been deployed. The work to be done is to deploy minimal, immutable implementations of each mechanism once on L1 so any application needing a QF round can call into the reference contract without re-implementing the math.

# 6. Application Categories: Where It Does Not

Immutability is not the right default for every application. The discipline matters more than the absolute rule. There are categories where the ability to iterate is load-bearing for the application's value proposition, and forcing immutability on them produces worse outcomes.

## 6.1 Yield strategies

A yield strategy is a specific claim about where to deploy capital for optimal return. That claim is necessarily time-varying: the optimal strategy in 2022 (lending USDC on Compound) is not the optimal strategy in 2026 (a mix of Pendle PT, Morpho Blue markets, and delta-neutral positions). Yield strategies need to iterate. They need to respond to new opportunities, retire strategies that no longer work, and rebalance positions.

Yearn V2's architecture — immutable vault accounting primitives with upgradable strategy contracts — is the correct pattern. The accounting (how shares map to underlying, how deposits and withdrawals work) is immutable. The strategy (what the vault actually does with the capital) is upgradable. Users who don't trust the strategy can redeem.

ERC-4626 formalizes the accounting primitive. Strategies on top are not, and should not be, immutable.

## 6.2 Risk curation in lending

Morpho Blue externalizes this: the primitive is immutable, but the decisions about which markets are prudent, at what LTVs, with what oracles — those are curation decisions that must iterate. A curator who froze their market configuration in 2024 would have left users exposed to collateral that no longer makes sense by 2026.

## 6.3 Oracle feeds

An oracle that cannot be updated cannot respond to changes in the underlying data source. Chainlink feeds need to handle exchange delistings, asset redenominations, and upstream data failures. A frozen oracle is a broken oracle the first time one of those events happens.

This is why oracle-consuming applications need to think carefully about the oracle as part of the trust surface. "Immutable application using mutable oracle" is a meaningful architectural decision, not an oxymoron — the application is immutable, but it inherits the oracle's trust model.

## 6.4 L2 sequencer software

L2 rollups are products. They need to iterate on proof systems, fee mechanics, MEV-handling, sequencer behavior, and fraud-proof implementations. Forcing a rollup to be immutable at the software layer would freeze bugs in place. The right pattern for L2s is different: *make the L2's governance surface explicit and verifiable* (L2Beat's stages framework is the current standard), not eliminate it.

The Stage 2 framework is the L2 equivalent of Morpho Blue's split. Stage 2 L2s have no admin override on user funds in the fallback case (users can withdraw to L1 even if the operator goes rogue). The sequencer software can still change; the trust model has been minimized.

## 6.5 Consumer-facing UX layers

Wallets, aggregators, and frontends need to iterate. The pattern that works is: the backend protocol is immutable; the frontend is one of many possible clients. Users who dislike the default frontend can use an alternative. This is the structural defense: not that the frontend is trustworthy, but that it is replaceable.

## 6.6 Bridges

A bridge is a specific claim about the state of a remote chain. That claim depends on validators, relayers, and light-client implementations that have all had vulnerabilities discovered post-deployment. Historical bridge hacks have collectively cost billions — Ronin, Wormhole, Nomad, Harmony, and others. Forcing a bridge to be immutable would freeze those bugs in place.

Bridges are an example of a category where the tradeoffs push toward careful governance, not toward immutability. The right design is a bridge with a clearly specified governance surface, time-locks on parameter changes, and a security council with explicit scope.

# 7. The Counter-Arguments, Steelmanned

A report that only makes the case for immutability is not useful. The counter-arguments are real and need to be engaged on their strongest form.

## 7.1 "Immutability produces technical debt"

The case: Uniswap V2 has known capital-inefficiency issues that V3 fixed. The Augur V1 UX was unusable. A protocol frozen at deployment carries its 2020-era bugs forward forever.

The response: this is true but incomplete. Immutability does not prevent better versions from being deployed; it prevents the *same deployment* from being mutated. V3 is deployed alongside V2. Users and integrators migrate at their pace. The "technical debt" argument assumes that the value is in a single deployment; the correct frame is that the value is in the ecosystem of deployments, and users choose.

The sharper version of the objection is about latent vulnerabilities: a subtle bug deployed once will sit there forever. This is real. The mitigation is the same mitigation that applies to non-upgradable systems in other domains: heavy auditing before deployment, formal verification where possible, simple designs that minimize attack surface. Morpho Blue's 650 lines of code is not a coincidence; it is the precondition for shipping immutably. You cannot ship a million-line immutable protocol with confidence. You can ship a thousand-line one.

## 7.2 "Users want recourse when things go wrong"

The case: retail users expect that if a protocol has a problem, someone can fix it. A governance body or admin key gives them someone to appeal to. Immutability eliminates that appeal.

The response: the right frame is to distinguish *protocol-level recourse* from *application-level recourse*. The protocol does not need to provide recourse; the applications and services on top of it can. If a user is confused by a Uniswap V2 interaction, the frontend they used can help them. If the frontend fails them, they can use a different frontend. If they want insurance, they can buy coverage from Nexus Mutual.

The failure mode of "protocol-level recourse" is that the recourse surface becomes the capture surface. The fraud committed *against* users by MakerDAO governance votes is a different failure than the fraud prevented *for* users by the same votes. Both exist. The honest analysis is that protocol-level recourse is a double-edged surface: it helps when used well, and it is the vector when used badly. Immutability removes the vector at the cost of the help.

For applications where the stakes are high and users are sophisticated (DeFi primitives, privacy protocols, stablecoins), removing the vector is the right choice. For applications where the stakes are lower and users are naive (consumer apps, low-value NFT marketplaces), the tradeoff is different.

## 7.3 "Governance is how a community coordinates"

The case: governance is not a bug, it is a feature. A protocol with active governance can respond to new circumstances, fund ecosystem development, and evolve with its users.

The response: this conflates two different things, and the distinction is worth naming precisely. **Protocol governance** is the set of on-chain powers that token holders can exercise over protocol parameters that affect user funds — fee rates, collateral factors, oracle configurations, upgrade authority. **Ecosystem coordination** is the set of off-chain and on-chain activities by which a foundation, DAO, or community funds development, runs grants programs, coordinates narrative, and manages the social layer of a protocol. The second kind of activity is valuable and entirely compatible with immutable protocols. A protocol can have zero protocol governance and a thriving ecosystem coordination function. Liquity is the clearest example: no protocol governance over the stablecoin, but an active community, documentation, tooling, and third-party integrations. WETH9 is the extreme: no protocol governance, no coordination function, and yet it remains canonical because canonicity does not require either.

Vitalik's 2021 framing is precise here: "on-chain governance only for applications, not base layers," and even within applications, "limited to very few parameter choices" and "fork-friendly" where possible ([vitalik.eth.limo, 2021](https://vitalik.eth.limo/general/2021/08/16/voting3.html)). The correct default is governance-minimization at the protocol layer, not governance-elimination in all senses. Some protocols genuinely need some protocol governance (Aave's parameter tuning for lending-market risk) and should embrace it with clear scope and slow time-locks. Other protocols (Liquity's stablecoin, WETH9) benefit from no protocol governance at all. Ecosystem coordination can and should continue in both cases.

The mistake of the 2020-2023 cycle was not "having governance." It was *giving protocol-level governance surfaces that could drain user funds to token holders*, as a default, because token-warrant financing made it the path of least resistance.

## 7.4 "Protocol fees and monetization require upgradability"

The case: a protocol that cannot change its fee schedule cannot monetize.

The response: Uniswap V2's `feeTo` mechanism is a half-answer. It allows the fee destination to change without allowing the fee *amount* or *accounting* to change. This is enough to capture value if activated. The fact that it has never been activated by Uniswap governance is, arguably, a flaw of the design — it was introduced as a latent monetization path and then politically impossible to activate. The better pattern is to design the fee mechanism into the protocol from deployment: Morpho Blue's design, Liquity's redemption-fee mechanism, Curve's gauge-directed emissions are all examples.

The deeper point: monetization and immutability are not in tension if you design for both from the start. They are in tension if you try to bolt monetization onto a protocol that wasn't designed for it. The solution is better initial design, not upgradability.

## 7.5 "L2s are fine for this"

The case: if an application needs immutability, it can deploy on an L2 and inherit L1 security. L2s are cheaper and faster.

The response: this gets the security model backwards. *Most L2s today have admin keys.* A contract deployed on Base, Arbitrum, Optimism, or the majority of the Superchain inherits those L2s' governance surfaces. "Immutable on L2" is not a coherent claim unless the L2 is Stage 2 trust-minimized, and as of 2026 essentially no major L2 is.

This will change. L2Beat's stages framework is driving public pressure toward Stage 2. The ZK-EVM-on-L1 trajectory will eventually allow rollups to settle without fraud-proof delays. But the pressure is not yet strong enough that an application team can default to "deploy on any L2" and claim immutability.

In the interim, the correct answer for applications where immutability is load-bearing is: deploy on L1, pay the gas cost, accept the throughput constraints. As the Strawmap ships, the gas cost will drop and the throughput will rise, and this choice becomes easier.

## 7.6 "Immutability is fragile against bugs"

The case: immutable protocols cannot patch zero-days. A single critical bug in the deployment is unrecoverable.

The response: this is true and is the strongest version of the objection. The correct responses are:

1. **Keep the deployment small.** Morpho Blue's 650 lines, UniswapV2Pair.sol's ~344 lines for the core pool, WETH9's ~75 lines (or ~55 excluding comments and blank lines). Small code surfaces are auditable in ways that large ones are not.
2. **Ship after extensive formal verification.** This is now standard practice for immutable protocols and is the right standard.
3. **Deploy immutable primitives, not immutable applications.** The primitive is the narrow part. The application on top can still iterate.
4. **Accept that immutability is a commitment, not a risk-free path.** Some protocols will fail because of latent bugs. The failures will be expensive. The aggregate track record — over many deployments and many years — is what matters, not individual failures.

## 7.7 The distributional critique: immutability as a regressive tax on sophistication

The case: immutability exports risk to users. Institutions can afford risk analysts, formal verification consumption, and insurance overlays. Retail gets rekt. The population that benefits most from a protocol's immutability is the population least able to verify that the protocol is correctly implemented.

The response: this critique has force and should not be dismissed. Immutable protocols are not distributionally neutral; they shift the verification burden from a responsible party to the user. The argument for accepting that shift rests on two supports. First, *the curation layer absorbs the burden*: most retail users of Morpho Blue interact with MetaMorpho vaults curated by Gauntlet, Steakhouse, or B.Protocol, who employ the risk analysts that retail cannot afford individually. Sophistication is concentrated and shared at the curation layer. Second, *the alternative to immutability is rarely benevolent governance*; it is governance that eventually becomes the attack vector. The counterfactual user is not "rekt by immutability" but "rekt later by governance capture."

But the critique has force where these supports are weak. A retail user interacting directly with an immutable primitive without a curation layer does bear the full verification burden. The design response is to make sure the curation layer exists and is accessible, not to pretend the burden-shift is inconsequential.

## 7.8 The frozen-worldview critique

The case: an immutable protocol locks in the values of the deploy-time designer. The choice of oracle, collateral, fee model, and liquidation curve are discretionary choices frozen forever. The designer's 2020 assumptions about what markets will look like in 2030 become permanent features of the protocol.

The response: this is true, and it is the correct reason to be selective about which applications belong on immutable mainnet. Applications whose right configuration depends on time-varying judgments about the world (yield strategies, risk curation, dynamic fee schedules) should not be immutable. Section 6 enumerates these.

The argument for immutable deployment is not that the designer's worldview is correct forever; it is that some applications' correct behavior is specifiable in a way that does not depend on a worldview. A constant-product AMM does not require the designer to predict trading pairs; it requires only the invariant math. A canonical namespace registry does not require predicting which applications will read from it; it requires only the ownership mapping. For these applications, the "frozen worldview" concern collapses. For applications where the designer *is* encoding a worldview, immutability is the wrong choice.

## 7.9 Governance-minimization creates off-chain governance surfaces

The case: a governance-minimized protocol does not eliminate governance; it relocates it to surfaces that often have *worse* legitimacy properties — fork choice, frontend curation, wallet default lists, aggregator routing. These are less transparent and less contestable than an on-chain vote.

The response: partly right, and important to acknowledge. An immutable protocol whose default frontend is a single company, whose routing is a small oligopoly, whose wallet inclusion is gated by a handful of teams, does not achieve the neutrality it claims. The sanctuary property is real; the neutrality property is weaker than the protocol alone suggests.

The design response is layered. The protocol itself should be structured so any frontend or aggregator can interact on equal terms — no fee preferences, no whitelists. The ecosystem should actively support plural frontends and aggregators. And some concentration will happen regardless because of network effects; the defense is ecosystem-level (plural aggregator competition, wallet-level transparency), not solved by the immutability of the core alone. The report should not pretend otherwise.

## 7.10 The Ethereum Classic steelman

Ethereum is a political community, and the DAO fork was its founding political act. Immutability-maximalism — the refusal to fork even for catastrophic exploits — is closer in spirit to Ethereum Classic than to contemporary Ethereum. A report arguing for application-layer immutability on Ethereum has to acknowledge this tension.

This report is defending a domain-specific version of immutability. The framing from Section 4.1 — social-layer invocation as a scarce legitimacy expenditure — is not immutability-maximalism. It is the position that the social layer's legitimacy is precisely what makes it unusable in all but the rarest cases, and that application-layer immutability is credible *because* the social layer has self-disciplined into near-non-use. ETC's position is stronger philosophically and deserves to be named as such, not straw-manned. The Ethereum position traded a cleaner theoretical commitment for a commitment device that is both credible and recoverable-from-catastrophe, and reasonable people can prefer either.

## 7.11 The "already-scaled luxury" and "governed-outperformed" objections

**Already-scaled luxury**: Uniswap could ossify V2 because it had already captured the market. A new AMM in 2026 with no token and no liquidity mining cannot bootstrap immutably. The response: partly true. Immutability makes bootstrapping harder but not impossible — Morpho Blue and Ajna shipped immutable cores without Uniswap's decade-long bootstrap. Teams that bootstrap immutably do so by having a technical proposition strong enough to attract users without token incentives. Higher bar, not impossible.

**Governed-outperformed**: Aave, Compound, and Curve dwarf Liquity by TVL. The response: TVL is the wrong metric for "has this protocol done what its design was supposed to do." Users who want a governed lending market should choose Aave; users who want a stablecoin with no capture surface should choose Liquity. The populations are not the same. By the metric of "preserving the property users came for," both succeed at different things. The comparison is a category error.

# 8. The Layered Pattern: Immutable Core, Governed Periphery

The dominant modern pattern, visible across the most successful 2024-2026 deployments, is:

1. **Immutable core primitive.** The math, accounting, and trust-critical logic are deployed once, with no admin key.
2. **Permissionless periphery deployment.** Other contracts can wrap the core in their own logic, adding features the core does not need.
3. **Opt-in curation.** Users choose which periphery to trust. Curators compete on risk management, UX, and feature set.
4. **Minimal governance scope.** Where governance exists, it is limited to parameters that cannot drain user funds — protocol fee destinations, ecosystem incentives, narrow scope.

Morpho Blue is the canonical example. The core is immutable. MetaMorpho vaults are permissionless. Curators (Gauntlet, Steakhouse, B.Protocol, others) compete on risk management. Users choose which vault. The Morpho governance token has scope but cannot drain markets.

ENS is the older example. The registry is immutable. Registrars and resolvers are permissionless. Domain owners choose which resolver to use. The ENS DAO has scope over the name-allocation process but cannot re-point an existing name.

Uniswap is partially in this pattern. Core pools are immutable. V4 hooks are permissionless periphery. The governance has scope over fee-switch activation but cannot drain pools. The deviation from the pattern is that Uniswap still has a protocol-wide governance with ambitious scope — the fee-switch debate has consumed enormous effort for years — but the core pools remain uncapturable.

Liquity V1 does not have a periphery layer in the strict sense; the protocol is a single system and the users interact with it directly. This is viable for stablecoins because the surface area is smaller. But even Liquity has seen community-built frontends and aggregators that act as a de facto periphery.

The layered pattern has a name in political theory: **subsidiarity**, the principle that authority should be located at the lowest competent level. The immutable core is the lowest competent level for the invariants that must not change (the bonding-curve math, the namespace ownership mapping, the liquidation math). The periphery is the higher layer where judgment, iteration, and discretion belong. Forcing either kind of decision into the wrong layer produces pathology: a governed AMM is a capture risk, a protocol-governed risk curator is too slow to matter.

Other deployments confirm the pattern. **Balancer V2**'s vault is immutable; the pool logic on top is upgradable per-deployment. **Curve V1 pools** are each immutable once deployed, so each pool is a frozen market with governance operating a layer up on gauges and incentives rather than on pool math. **Seaport** (OpenSea's marketplace settlement contract) is immutable and any frontend can read and write to it. **0x Protocol v4**'s settlement is immutable. **Nouns DAO**'s auction contract is immutable. In each case the layer that must not change is the settlement or accounting primitive; the layers that must change (risk, curation, incentives, UX) are kept separate and mutable by design.

The layered pattern is, arguably, the most important architectural insight of the post-DeFi-Summer era. It resolves the tension between immutability's safety properties and upgradability's iteration properties by *not forcing them into the same contract*. The primitive is safe because it cannot change; the periphery is adaptive because it can be replaced. Users choose the combination that suits them.

There is a residual critique worth acknowledging: if 95% of users interact through a small number of governed peripheries, describing the underlying system as "immutable" can be marketing rather than substance. The layered pattern is genuine only if the periphery is genuinely plural and the immutable core is genuinely reachable without passing through a gatekeeper. When periphery concentration is severe, the layered pattern becomes a shell over a functionally governed system. The discipline is to keep the periphery plural.

For teams designing new protocols, the right question is: *what is the minimum viable immutable primitive, and what can be externalized?* A good answer to that question is worth more than a year of token-design effort.

# 9. What Should Be Built

The inventory of applications that belong on immutable mainnet in 2026 is larger than what currently exists. Several categories have canonical deployments that are good enough; others have obvious gaps.

**Categories with good canonical deployments:**

- Spot AMMs (Uniswap V2/V3).
- Overcollateralized ETH-backed stablecoins (Liquity V1).
- Privacy pools (Tornado Cash, Railgun, Privacy Pools).
- Token streaming / vesting (Sablier, Llamapay).
- Canonical asset wrappers (WETH9).
- Name registry (ENS).
- Permissionless lending primitive (Morpho Blue).

**Categories with gaps:**

- **Non-USD, non-ETH-backed decentralized stable assets.** RAI is small. The design space is underexplored.
- **Immutable prediction markets with good UX.** The Augur lessons need to be absorbed into a design that is both immutable and usable. UMA-style optimistic oracles plus carefully designed market contracts are a starting point.
- **Immutable onchain-native payment primitives.** x402-style agent-payment infrastructure is growing fast but much of it is running on governed L2s. An immutable L1 settlement primitive for programmable-payment flows would anchor the stack.
- **Immutable public-goods funding allocation.** Gitcoin's Allo is a step; canonical QF and pairwise-comparison primitives deployed once on mainnet would be useful.
- **Immutable identity attestation rails.** EAS is useful but the primitive is not yet at the Schelling-point threshold.
- **Immutable escrow with optimistic dispute resolution.** A combination of time-locked escrow and UMA-style oracle dispute resolution, deployed once, would be a widely useful primitive.
- **Immutable savings-account primitive.** An auto-compounding wrapper around staked ETH or a basket of stable yield sources, with immutable accounting and pluggable strategy layers. ERC-4626-compatible. The primitive is simple; the pluggable strategies are where iteration happens.
- **Immutable canonical-auction primitives.** English auction, Dutch auction, batch auction — each deployed once as a reference contract. Specialized auctions compose on top.
- **Immutable generic settlement for intent systems.** CoWSwap's settlement contract and UniswapX's Reactor are close to this pattern; a fully neutral, protocol-independent settlement contract would be useful.

The work in each category is modest in engineering scope and significant in ecosystem value. A small team can deploy a canonical primitive and walk away. The commercial model is not token-based; it is either foundation-funded (Ethereum Foundation grants, Gitcoin, etc.) or service-based (the team that deploys the primitive can operate the canonical frontend, audit other integrations, or run curator infrastructure on top).

The pattern that emerges is not "startup building a protocol" but "craftspeople depositing a primitive into the public commons." The closest real-world analog is the people who wrote the Unix tools — `awk`, `grep`, `sed`. They built the substrate on which everything runs and captured none of the rent. That is the sanctuary thesis in one sentence: infrastructure that becomes valuable precisely because no one owns it, deployed by people who accept not owning it.

Historical base rates for rules that constrain discretionary power are mixed. **Central bank independence** is the closest political-economy parallel: a rule that actors commit to not intervening, with value proportional to credibility. It has held in most developed economies for decades but erodes under fiscal stress. **Constitutional constraints** are hard to amend by design and tend to hold, but partly because the amendment procedure is itself a high-bar mechanism. **TCP/IP and the core internet standards** ossified after roughly a decade of adoption; the IPv4-to-IPv6 transition has taken over 25 years. **Patent expiry and generic drugs** are the closest economic parallel to "deploy and walk away": the inventor relinquishes exclusivity on a schedule, and the resulting generics market is a real public good.

The less comforting parallel is **Glass-Steagall**: installed after the 1929 crisis, eroded steadily over 60 years, repealed before the 2008 crisis. The base rate for "rules installed after a crisis erode during the next expansion" is not encouraging. The post-DAO discipline of not invoking the social layer and the post-Euler recognition of the governance-attack surface are both crisis-triggered disciplines, and base rates suggest they weaken during expansions unless actively reinforced.

The honest claim is not that immutability has "won." It is that a class of applications benefits from immutability in ways the last cycle under-weighted; that the mainnet cost environment is improving enough to make immutable deployment practical again for many of them; and that the remaining forces pulling builders away (regulatory exposure, institutional demand, founder psychology) will not automatically resolve in immutability's favor. Signals to watch over the next eighteen months: does any top-10-TVL new protocol ship immutable? Do VC term sheets accept non-token revenue models? Does any major L2 reach Stage 2 by end of 2027? The answers are more informative than anything arguable from first principles now.

What can be argued now is that the work is available to be done. Gas is cheap enough. Tooling is mature. The design patterns are established. The binding constraint — if the immutability tradition is to recover — is not financial or technical but closer to discipline: the willingness to deploy a small, correct thing once and stop touching it. Whether that discipline takes hold is an empirical question being answered in deployments that are happening now.

The applications that will still be running on Ethereum in 2036 are, disproportionately, the ones running on it today without admin keys. Most of them have not yet been built. The sanctuary thesis is a bet that enough people will decide the work is worth doing at a discount to what financialized alternatives still pay.

---

# 10. Data Sources & Methodology

This is a synthesis report. It draws on primary-source documents (Ethereum Foundation blog posts, protocol documentation, whitepapers, court rulings, and Strawmap documentation) and on the existing ethreportseth.xyz report series (particularly `state-of-ethereum-march-2026.md` for Strawmap and sanctuary-technology context, and `state-of-intents-2026.md` for settlement-contract framing).

Where specific numeric claims are made — gas limits, deposit figures, operation counts — each has been tied to a primary source and cited inline. Where the argument is interpretive rather than quantitative, no source is claimed beyond the general literature. Where a claim could not be verified to a primary source, it has been omitted.

The application categorization in Sections 5 and 6 reflects a specific design judgment rather than a survey of existing deployments. It is structured around the property "is the application's correct behavior specifiable in a way that does not need to change over time" and uses that as the filter.

This report does not make claims about protocol-specific TVL, market share, or competitive positioning. For those, `state-of-ethereum-l2-ecosystem-march-2026.md` and the DeFiLlama / Token Terminal dashboards are the authoritative sources.

---

# 11. Sources

**Ethereum protocol and scaling:**
- [strawmap.org](https://strawmap.org/) — Ethereum Foundation roadmap through 2029
- [ethereum.org, Pectra upgrade](https://ethereum.org/roadmap/pectra/) — Pectra upgrade reference
- [CryptoRank, 2025](https://cryptorank.io/news/feed/ba929-post-pectra-ethereum-now-targets-efficiency-with-60-million-gas-limit-expansion) — Post-Pectra gas limit expansion
- [CoinGecko, 2025](https://www.coingecko.com/learn/what-is-ethereum-fusaka-upgrade) — Fusaka upgrade reference
- [Blockworks, 2025](https://blockworks.co/news/ethereum-increases-gas-limit-pectra) — Gas limit increase
- [The Block, Feb 2026](https://www.theblock.co/post/388285/vitalik-buterin-reevaluates-rollup-centric-roadmap-arguing-l2s-decentralized-far-slower-while-ethereum-base-layer-advanced) — Vitalik on L2 roadmap reevaluation

**Vitalik Buterin essays:**
- [Moving beyond coin voting governance (2021)](https://vitalik.eth.limo/general/2021/08/16/voting3.html)
- [Notes on Blockchain Governance (2017)](https://vitalik.eth.limo/general/2017/12/17/voting.html)
- [Why I support privacy (2025)](https://vitalik.eth.limo/general/2025/04/14/privacy.html)
- [Balance of Power (2025)](https://vitalik.eth.limo/general/2025/12/30/balance_of_power.html)
- [Don't overload Ethereum's consensus (2023)](https://vitalik.eth.limo/general/2023/05/21/dont_overload.html)

**Protocol documentation:**
- [Morpho Blue blog post, 2024](https://morpho.org/blog/morpho-blue-and-how-it-enables-our-vision-for-defi-lending/)
- [Morpho Blue GitHub](https://github.com/morpho-org/morpho-blue)
- [Liquity V1 FAQ](https://docs.liquity.org/liquity-v1/faq/general)
- [Ajna Protocol whitepaper and documentation](https://www.ajna.finance/)
- [Seaport Protocol, OpenSea GitHub](https://github.com/ProjectOpenSea/seaport)
- [0x Protocol v4 documentation](https://protocol.0x.org/en/latest/)
- [Balancer V2 vault architecture](https://docs.balancer.fi/concepts/vault/)
- [Reflexer: Marching Towards Ungovernance (Reza Jafery, 2021)](https://medium.com/reflexer-labs/marching-towards-ungovernance-19a220dbdefd)

**Primary contracts and canonical deployments:**
- [WETH9, Etherscan](https://etherscan.io/address/0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2)
- [Limit Break on WETH9 and Wrapped Native (2024)](https://medium.com/limit-break/introducing-wrapped-native-a-modern-replacement-for-wrapped-ether-cc0431c8a964)
- [ENSRegistry.sol, ensdomains GitHub](https://github.com/ensdomains/ens/blob/master/contracts/ENSRegistry.sol)

**The DAO and early Ethereum history:**
- [Ethereum Foundation, DAO vulnerability response (June 2016)](https://blog.ethereum.org/2016/06/17/critical-update-re-dao-vulnerability)

**Tornado Cash legal history:**
- [Mayer Brown on Fifth Circuit ruling (Dec 2024)](https://www.mayerbrown.com/en/insights/publications/2024/12/federal-appeals-court-tosses-ofac-sanctions-on-tornado-cash-and-limits-federal-governments-ability-to-police-crypto-transactions)
- [Venable LLP, Treasury de-listing Tornado Cash (April 2025)](https://www.venable.com/insights/publications/2025/04/a-legal-whirlwind-settles-treasury-lifts-sanctions)
- [Coindesk, Roman Storm verdict (Aug 2025)](https://www.coindesk.com/policy/2025/08/06/roman-storm-guilty-of-unlicensed-money-transmitting-conspiracy-in-partial-verdict)
- [Mayer Brown on the Storm mixed verdict (2025)](https://www.mayerbrown.com/en/insights/publications/2025/08/the-tornado-cash-trials-mixed-verdict-implications-for-developer-liability)

**Euler hack (March 2023):**
- [Cyfrin, Euler exploit analysis](https://www.cyfrin.io/blog/how-did-the-euler-finance-hack-happen-hack-analysis)
- [BlockSec, Euler Finance Incident](https://blocksec.com/blog/euler-finance-incident-the-largest-hack-of-2023)

**Companion reports (ethreportseth.xyz):**
- state-of-ethereum-march-2026.md — Strawmap, sanctuary technologies, protocol context
- state-of-intents-2026.md — Intent settlement contracts and solver architecture
- state-of-ethereum-l2-ecosystem-march-2026.md — L2 governance surface, Stage 2 framework
