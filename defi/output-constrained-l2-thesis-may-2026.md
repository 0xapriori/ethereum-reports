---
title: "Output-Constrained L2: A May 2026 Update on the Intent + Proving + Ordering Stack"
date: 2026-05-03
---

# Output-Constrained L2: A May 2026 Update on the Intent + Proving + Ordering Stack

*A synthesis report extending a March 2026 conversation, written with the apriori-writer agent. | May 2026*

## tl;dr

- **The unified-L2 thesis is stronger now than when we wrote it** — every adjacent piece (real-time proving, intent standardization, sequencer fragmentation, L1 governance velocity) has moved in our favor, while the lane's most direct competitor (Essential) [walked away in November 2025](https://blog.essential.builders/introducing-essential/).
- **Real-time EVM proving landed on consumer hardware.** [SP1 Hypercube proves 99.7% of Ethereum L1 blocks under 12 seconds on 16 RTX 5090s](https://blog.succinct.xyz/real-time-proving-16-gpus/) (~$32K GPU, ~$100K full rig); [Brevis Pico Prism reached parity in February 2026](https://blog.brevis.network/2026/02/25/pico-prism-update-from-64-to-16-gpus/). Constraint-shape proving still trails by an estimated 12-24 months.
- **Anoma is partially live, not vapor.** The [EVM Protocol Adapter deployed on Ethereum mainnet November 18, 2025](https://anoma.net/blog/anoma-is-now-live-on-ethereum-mainnet); [AnomaPay's BNB Chain public beta launched April 2, 2026](https://anoma.net/blog/the-anomapay-public-beta-is-live-on-bnb-chain) with user-perceived ~15-second confirmations. The full Resource Machine vision (private solving, FHE, MPC) remains forward-facing.
- **The shared-sequencer thesis cracked.** [Astria sunsetted at block 15,360,577 in December 2025](https://www.theblock.co/post/381138/celestia-based-astria-network-sunsets-sequencer-network-after-raising-18-million); [Espresso pivoted to "global confirmation layer" and went full PoS March 4, 2026](https://medium.com/@espressosys/espresso-mainnet-0-is-live-deedc2505081). The patterns that survive are vertical integration and app-specific sequencing above existing chains.
- **L1 reform is even slower than we said.** [ePBS (EIP-7732) has been an active draft for ~22 months](https://eips.ethereum.org/EIPS/eip-7732); [Glamsterdam slipped to Q3-Q4 2026](https://blog.ethereum.org/2026/04/10/checkpoint-9); FOCIL (EIP-7805) lands no earlier than Q1-Q2 2027 in Hegotá; encrypted mempool and MEV-burn are 2027-2029+ territory. Fusaka raised the gas limit from [36M to 60M, not 45M to 150M as occasionally cited](https://eips.ethereum.org/EIPS/eip-7935).
- **Bebop is the canonical execution-coordination model — and it works.** Bebop's PMM RFQ (Wintermute-incubated) and [JAM solver auction (December 2023)](https://medium.com/bebop-dex/bebop-unveils-jam-intent-based-liquidity-aggregation-system-e14a6feedaae) deliver tighter execution than naive AMMs. But coordination is not architecture: sandwich, frontrun, and proof-of-no-extraction remain unverifiable on the base layer, and Bunni V2's [$8.4M exploit](https://decrypt.co/345621/decentralized-exchange-bunni-pulls-the-plug-following-8-4m-flash-loan-exploit) and permanent shutdown is the cautionary tail.
- **The bottom line:** an L2 that ships ERC-7683 ingress + based-rollup ordering + proof-verified solving in 2026 has a 12-18 month window where L1 cannot architecturally match it. The pieces exist. The interesting question is no longer technical feasibility — it is who has the discipline to assemble them, and what anchor app bootstraps the cold start.

---

## Table of Contents

1. [Where the thesis stood vs. where it stands now](#i-where-the-thesis-stood-vs-where-it-stands-now)
2. [The proving piece is no longer the bottleneck](#ii-the-proving-piece-is-no-longer-the-bottleneck)
3. [The intent-layer realignment](#iii-the-intent-layer-realignment)
4. [Sequencer architecture is fragmenting in our favor](#iv-sequencer-architecture-is-fragmenting-in-our-favor)
5. [The L1 governance clock is even slower than we said](#v-the-l1-governance-clock-is-even-slower-than-we-said)
6. [Coordination vs. architecture: Bebop, am-AMM, CoW AMM](#vi-coordination-vs-architecture-bebop-am-amm-cow-amm)
7. [What integration actually looks like in 2026](#vii-what-integration-actually-looks-like-in-2026)
8. [The forcing function, revisited](#viii-the-forcing-function-revisited)
9. [What I want to push on next](#ix-what-i-want-to-push-on-next)
10. [Sources](#x-sources)

---

# I. WHERE THE THESIS STOOD VS. WHERE IT STANDS NOW

When we wrote the seed of this thesis in March, the framing depended on three claims about the future: that the proving piece would land on a "known trajectory," that the political L1 path would stay slow, and that nobody had yet assembled the unified stack. In May 2026 all three are still true — but the texture has shifted in our favor. The proving primitive is real-time on production EVM workloads, not theoretical. The L1 governance clock is even slower than we wrote. And the closest unified-stack candidate (Anoma) is partially live, while the closest dedicated competitor (Essential) quietly walked away from the lane.

The thesis is stronger now, not weaker, because the adjacent infrastructure has advanced past the points we were trying to wave at and the political path we contrasted against has slipped further. The interesting question is no longer "is this technically achievable?" — it is "given that the pieces exist, who has the discipline to assemble them, and what is the right anchor app to bootstrap the cold start?"

This update reframes the seed in two ways. First, with two months of additional research and audited primary sources, several specifics tighten into hard numbers — proving rigs at $32K GPU, AnomaPay confirming in ~15 seconds, Across running 88% of its volume through ERC-7683, Timeboost auction revenue collapsing from 62.7% to 14.8% of best bid. Second, an ambiguity in the seed is now resolved: the project we referenced as "bopAMM" is [Bebop](https://docs.bebop.xyz/bebop/how-bebop-works), the Wintermute-incubated execution stack. That changes the tone of the comparison — Bebop is not a stop-gap that died, it is a well-engineered coordination layer that genuinely ships better execution. The disagreement with that approach is architectural, not directional, and we owe it the precision the seed lacked.

# II. THE PROVING PIECE IS NO LONGER THE BOTTLENECK

The seed treated real-time proving as "the hardest engineering piece but on a known trajectory." A year later that is the wrong framing. It understates what shipped.

[SP1 Hypercube went live on mainnet in Q4 2025](https://blog.succinct.xyz/sp1-hypercube-is-now-live-on-mainnet/) proving 99.7% of Ethereum L1 blocks under 12 seconds on 16 RTX 5090s — roughly $32K of GPUs, $100K full rig. [Brevis's Pico Prism reached parity at the same hardware footprint by February 2026](https://blog.brevis.network/2026/02/25/pico-prism-update-from-64-to-16-gpus/), down from 64 GPUs in October 2025. [RISC Zero's Boundless launched a decentralized proof market on Base in September 2025](https://www.coindesk.com/tech/2025/09/12/boundless-launches-mainnet-on-base-ushering-in-universal-zero-knowledge-compute) and is now production for EigenLayer slashing, Celestia DA, Lido staking, Taiko's multi-prover, and XRPL Commons confidential payments. The "9-month curve from 16 minutes to 16 seconds" that Justin Drake summarized last fall is the real artifact. The Ethereum Foundation's own [L1-zkEVM target](https://blog.ethereum.org/en/2025/07/10/realtime-proving) (≤$100K hardware, ≤10kW, 99% of blocks under 10s, 128-bit security, ≤300KB proof) is now matched by deployed systems, not aspirational ones. So when the seed said "known trajectory," the trajectory landed.

What the seed underplayed is *which kind* of proving became real-time. The state of the art today proves *EVM-shaped* workloads at block latency — meaning the trace is a known shape (an Ethereum block executing the EVM against a known state), the witness is bounded, and the proving system is tuned for that exact circuit family. Constraint-shape proving — proving "this output satisfies these declared invariants for an arbitrary intent shape the user just expressed" — is a different beast. It is in production today only for *fixed-shape* circuits: [Aztec's Noir functions](https://aztec.network/blog/client-side-proof-generation) (compiled per-function, statically bounded; alpha mainnet March 31, 2026, with a critical proving-system vuln in v4 disclosed March 17 and v5 fix scheduled July 2026), [Aleo's Varuna circuits](https://medium.com/@CFrontier_Labs/aleo-roadmap-2025-and-in-depth-exploration-of-varuna-540ea05a4e8d) at 2^22 gates, [Penumbra's Groth16 shielded transfers](https://www.penumbra.zone/blog/faster-client-side-proving-with-parallelism) (~1s native M1, ~30s parallelized in browser), and [Anoma's Resource Machine compliance and logic proofs over RISC0](https://specs.anoma.net/main/implementation/risc0.html).

The audited verdict from the proving research is that arbitrary-invariant solver proofs at sub-block latency are 12-24 months out, contingent on three things: ASIC ramp ([Cysic C1 at 1.31M Keccak/s is the current vector](https://medium.com/@0xjacobzhao/cysic-research-report-the-computefi-path-of-zk-hardware-acceleration-3b4517cd183b)), Boundless-style market-cleared pricing actually clearing under load, and circuit-size discipline at the protocol layer so we are not proving open-ended recursion. That is a much more concrete trajectory than "the hard part" — it is an engineering plan with named bottlenecks.

The architectural implication is that an L2 shipping today should anchor on EVM-shaped proving, where the primitive is real and cheap, and treat constraint-shape circuits as the next-version surface that becomes available as proving market pricing and ASIC throughput compound. AnomaPay is the existence proof that real production traffic can settle through RISC0-backed RM proofs today — its public beta on BNB Chain delivers user-perceived ~15-second confirmations, which is much closer to "real-time" than the multi-minute prove times that older Anoma forum threads described. The narrow circuit (private payments) is what makes that work; the broader programmable-intent surface still needs ASIC ramp and circuit discipline to land at the same latency.

# III. THE INTENT-LAYER REALIGNMENT

The seed described Anoma as "intent-centric architecture conceptually but in development for years without shipping." That is stale. Anoma's [XAN token and governance went live on Ethereum mainnet on September 29, 2025](https://anoma.net/blog/xan-is-live); the EVM Protocol Adapter deployed to Ethereum mainnet on November 18, 2025, with [audits from Informal Systems and Nethermind](https://github.com/anoma/evm-protocol-adapter); the latest tagged release is `bindings/v2.1.2` from April 9, 2026. AnomaPay's [public beta on BNB Chain](https://anoma.foundation/press/anomapaybeta) is live as of April 2, 2026. The mainnet PAs on Ethereum, Base, Arbitrum, Optimism, and Aurora are still gated for general apps pending one final audit and a governance vote, but the contracts are deployed and the path is in motion.

Worth being precise about scope: what is shipping is the near-term subset — RISC0-backed RM compliance and logic proofs with Groth16 wrapping for L1 settlement. The full RM vision (private solving, FHE, MPC, threshold encryption, Chimera chains) is forward-facing and still on the roadmap, not at user fingertips. AnomaPay is constrained to private payments; the broader programmable-intent surface is not where production traffic is yet. But the structural fact is that someone built the constraints + solving + proof-verified settlement triplet and got it live, with real users.

Essential is the negative finding that hurts. The seed cited Essential as the team building closest to the unified-L2 vision; in [November 2025 the team announced its "new home" as VOID apps](https://blog.essential.builders/introducing-essential/). [Pint](https://github.com/essential-contributions/pint), the declarative intent DSL we hoped would become the constraint expression layer, never reached public testnet under the Essential brand. The lane that was most closely targeted at our thesis is now empty.

Meanwhile [ERC-7683](https://www.erc7683.org/spec) has consolidated as the de facto cross-chain intent standard. Across, UniswapX, CoW, and Eco have all shipped production endpoints. [Across alone runs 88% of its volume through 7683 orders, ~$50M/day, $19B+ lifetime, with 54% of daily active bridge users in January 2026](https://across.to/blog/unifying-ethereum-path-to-seamless-crosschain-interoperability). The [Open Intents Framework reached production status as part of Ethereum's 2026 protocol priorities](https://blog.ethereum.org/en/2026/02/18/protocol-priorities-update-2026). So the constraint-expression layer at the edge has standardized — but it has standardized into a fragmented, application-specific, host-chain-settled shape, not into a unified L2 with a sequencer that ingests intents directly. Each major intent system owns one slice (cross-chain bridging, same-chain swaps, aggregation, batch auctions) without owning ordering or proof-verified solving. CoW's [$9B ATH month in July 2025](https://cow.fi/learn/cow-dao-2025-in-review) and ~22-34% DEX-aggregator share is the closest thing at scale to "intents as product," and CoW still inherits host-chain ordering with no proofs of solver behavior beyond reputation and slashable bonds.

SUAVE collapsed back to BuilderNet. [Flashbots' February 2026 post](https://writings.flashbots.net/decentralized-building-wat-do) explicitly frames SUAVE as "an aspirational endpoint" reached at the end of a multi-step path; [BuilderNet is the live artifact](https://buildernet.org/blog/introducing-buildernet). BuilderNet is TEE-attested, not proof-verified, and processes pre-formed transactions rather than declarative constraints. The SUAVE-as-a-chain story we wrote against in the seed is not a competitor to the unified-L2 thesis anymore — it is a non-event. Flashbots is iterating on builder decentralization through hardware attestation, not building the chain we were measuring against.

# IV. SEQUENCER ARCHITECTURE IS FRAGMENTING IN OUR FAVOR

The shared-sequencer thesis cracked. [Astria sunsetted its mainnet at block 15,360,577 in early December 2025](https://www.theblock.co/post/381138/celestia-based-astria-network-sunsets-sequencer-network-after-raising-18-million), citing rollup adoption and a saturated market — $18M of capital, fourteen months of operation, no PMF for "shared sequencer as standalone product." [Espresso pivoted hard from "shared sequencing" to "global confirmation layer / base layer"](https://medium.com/@espressosys/espresso-mainnet-0-is-live-deedc2505081) through 2025 and [went full PoS on March 4, 2026](https://www.hokanews.com/2026/02/espresso-goes-full-pos-10-esp-airdrop.html), with ESP token live on Coinbase and Binance from February 12. Most Espresso integrations are now confirmation/DA, not full sequencing handover. The market actively rejected the middle path of "shared sequencer plus FCFS" — the architectures that survive either own the full stack or sit above an existing chain as an app-level primitive.

That is directly favorable to the thesis. The two patterns that *are* working are exactly the ones we framed: vertical integration (own the sequencer, define the rule, post the proofs) and app-controlled escape hatches above whatever chain happens to be underneath. [Angstrom's app-specific sequencing on Uniswap V4](https://docs.angstrom.xyz/l1/core-mechanisms/app-specific-sequencing) is the canonical live example of the second pattern — the app delegates ordering of its own state transitions to a separate operator network with a prescribed rule, while inheriting the underlying L1 for everything else. [Atlas, the most app-controllable OFA primitive in production, was acquired by Chainlink in January 2026](https://www.theblock.co/post/386743/chainlink-acquires-transaction-ordering-solution-atlas-accelerating-rollout-of-its-non-toxic-mev-tool) to power Smart Vaults and OEV. [MEV Blocker was acquired by Special Mechanisms Group (a Consensys division)](https://www.ainvest.com/news/special-mechanisms-group-acquires-mev-blocker-rpc-advance-state-art-backrunning-auction-infrastructure-2601/) in the same month. The MEV-mitigation infrastructure is consolidating into platforms, which is itself a market signal: the platforms are buying it because they think the centralized economic capture is worth more than the disaggregated tooling.

The auctioned-priority model is empirically fragile. [Arbitrum's Timeboost cleared $6.74M in lifetime auction revenue as of February 2026](https://www.tronweekly.com/arbitrum-arb-reports-6-74-million-in-timeboost/), but the primary auction collapsed mid-February when Wintermute and Selini moved to [Kairos' just-in-time resale market](https://collective.flashbots.net/t/when-ahead-of-time-allocation-fails-the-transition-to-kairos/5640) — paid bids dropped from ~62.7% of the highest bid pre-Kairos to ~14.8% post-Kairos. The arXiv literature now [treats this as a textbook ahead-of-time auction failure](https://arxiv.org/abs/2603.20175). The lesson, which the seed implied without articulating, is that "auction the right to be first" entrenches a small searcher cartel and is structurally fragile to JIT resale markets. Deterministic FIFO sidesteps both problems.

The honest part: no production EVM L2 ships pure FIFO. [Unichain's TEE block builder](https://blog.uniswap.org/rollup-boost-is-live-on-unichain) still uses priority-fee ordering inside the SGX enclave; that is "fair *given* you pay," not FIFO. [Radius's PVDE encrypted-mempool design](https://docs.theradius.xyz/testnet/portico-testnet/encrypted-mempool) is the closest pure-research analog, but it is testnet-only with mainnet targeted for late 2026.

The new live primitive worth absorbing is Taiko's based-rollup model. [Taiko shipped phase-1 preconfirmations on mainnet in August 2025](https://www.prnewswire.com/news-releases/taiko-opens-up-the-next-era-of-ethereum-with-preconfirmations-on-mainnet-302527696.html) with whitelisted preconfirmers and ~2-second tx times. [Surge on the Taiko stack is the Namechain (ENS L2) framework](https://docs.taiko.xyz/taiko-alethia-protocol/protocol-design/based-rollups/), with public testnet in Q2 2026. Based rollups inherit the L1 proposer set for ordering — no separate sequencer — and are conceptually closer to "deterministic L1-driven ordering" than any other live category. The trade-off is throughput bounded by L1 cadence and MEV surfaces that follow L1 rules, which means based rollups do not escape the L1 MEV problem so much as inherit it. But they do escape the sequencer-as-political-actor problem, and they pair naturally with FOCIL-style inclusion guarantees once those land. [Base announced February 18, 2026 it is leaving OP-stack for an in-house unified stack on Reth](https://www.theblock.co/post/390380/coinbase-incubated-base-network-to-ditch-optimism-for-unified-solution) — code overlap remains huge, but the governance posture matters. The sequencer landscape we were writing against has not just stayed fragmented; it has fragmented in directions that all push toward "the chain owns the rule, the chain proves the rule, or the chain explicitly delegates the rule to an app."

# V. THE L1 GOVERNANCE CLOCK IS EVEN SLOWER THAN WE SAID

The seed warned about "years of EIP debates with incumbents who benefit from the current structure actively resisting." The audited governance research makes that look optimistic.

[Glamsterdam, the next fork](https://ethereum.org/roadmap/glamsterdam/), was originally targeted H1 2026 and is now realistic Q3-Q4 2026 at earliest, per the EF's own [April 2026 Checkpoint](https://blog.ethereum.org/2026/04/10/checkpoint-9). [ePBS (EIP-7732)](https://eips.ethereum.org/EIPS/eip-7732) — the closest mechanism to an enshrined PBS replacement — has been in active draft for ~22 months and is still in devnet phase; [Sigma Prime publicly opposed the design](https://blog.sigmaprime.io/glamsterdam-eip7732.html) citing two-party coordination complexity. [FOCIL (EIP-7805)](https://eips.ethereum.org/EIPS/eip-7805), the censorship-resistance committee inclusion list, was [moved out of Glamsterdam in January 2026](https://blog.ethereum.org/en/2026/01/20/checkpoint-8) to cut fork scope and is now the proposed CL headliner for [Hegotá](https://ethereum-magicians.org/t/hegota-headliner-proposal-focil-eip-7805/27604), which lands Q1-Q2 2027 at earliest given that Hegotá follows a slipping Glamsterdam.

Encrypted mempool — [EIP-8105 Universal Enshrined Encrypted Mempool](https://eips.ethereum.org/EIPS/eip-8105) — was a Hegotá candidate but was *not* selected as a headliner. Precompiles for the underlying threshold/MPC/TEE/delay/FHE schemes are explicitly out of scope of EIP-8105 itself, meaning even if 8105 ships in 2027 or 2028, an additional fork is needed to make the schemes practical at EVM speed. [MEV-burn](https://ethresear.ch/t/mev-burn-a-simple-design/15590) has been "researched" since August 2023 with no EIP draft — call it 2028+ if it ships at all, conditional on a working ePBS. [Execution tickets / APS-burn](https://ethresear.ch/t/execution-tickets/17944) are at ethresear.ch stage with no fork target.

One audit correction worth surfacing: [Fusaka raised the gas limit from 36M to 60M per EIP-7935](https://eips.ethereum.org/EIPS/eip-7935), not the 45M-to-150M figure that occasionally circulates. [Fusaka shipped December 3, 2025](https://blog.ethereum.org/2025/11/06/fusaka-mainnet-announcement) with PeerDAS (EIP-7594), the gas-limit raise, the per-tx gas cap (EIP-7825), secp256r1 (EIP-7951), deterministic proposer lookahead (EIP-7917), and DoS hardening — none of which touched PBS or MEV reform. The MEV/ordering reform stack is staged across three forks, not one. ePBS in Glamsterdam (~Q4 2026 best case). FOCIL in Hegotá (~Q1-Q2 2027). Encrypted mempool, MEV-burn, anti-MEV precompiles, and execution tickets in unscheduled post-Hegotá territory (2027-2029+).

That changes the strength of the seed's "build on L2 and let the results force the conversation" framing. When we wrote it, we were arguing against a hypothetical timeline. Now we are arguing against a measurable one. An L2 that ships intent-architecture, deterministic-or-encrypted ordering, and proof-verified solving in 2026 has roughly a 12-18 month window where L1 cannot match it on execution quality even on its own roadmap. Not because L1 is uninterested — the [EF L1-zkEVM workshop in February 2026](https://ethereum-magicians.org/t/l1-zkevm-roadmap-2026-integrating-zkevm-proofs-into-ethereums-core-protocol/27595) was a real coordination signal — but because the social process for changing four client teams' shipping priorities across three fork cycles is the binding constraint, not the cryptography.

# VI. COORDINATION VS. ARCHITECTURE: BEBOP, am-AMM, COW AMM

We need to clear up an ambiguity from the seed. "bopAMM" referred to [Bebop](https://bebop.xyz), the Wintermute-incubated execution stack [launched in June 2022](https://decrypt.co/113819). Bebop is not literally an AMM — the suffix in the seed's reference is misleading. Bebop is a coordinated execution stack with two production primitives: [PMM RFQ](https://docs.bebop.xyz/bebop/bebop-api-pmm-rfq/pmm-rfq-api-intro), where private market makers stream quotes for 0% slippage on-chain settlement, and [JAM (Just-in-Time Aggregation Model)](https://docs.bebop.xyz/bebop/bebop-api-jam/jam-api-introduction), [unveiled December 2023](https://medium.com/bebop-dex/bebop-unveils-jam-intent-based-liquidity-aggregation-system-e14a6feedaae), where independent solvers compete on signed user intents. The system [runs on 12+ chains](https://forum.arbitrum.foundation/t/bebop-ltipp-application-final/21419), and the [bebop-jam-contracts repo](https://github.com/bebop-dex) has continued to ship as recently as June 2025.

Resolving the name lets us frame the comparison precisely. Bebop is the canonical example of "coordinate execution through builder/maker relationships on a generic base layer." It works. RFQ-from-Wintermute genuinely delivers tighter execution than naive AMMs for the trades Bebop covers, because professional market makers price off their broader books and absorb inventory that AMMs would push into slippage. JAM's solver competition is real, and the signed-intent + auction-clearing structure puts Bebop architecturally adjacent to UniswapX and CoW Protocol rather than to a pool-based DEX.

But coordination is not architecture. Bebop does not fix the base layer. Sandwich, frontrun, ordering manipulation, and proof-of-no-extraction remain impossible to verify; the user trusts Bebop's reputational and slashable infrastructure plus the L1 or L2 sequencer underneath. If a builder colludes with a solver, if a maker quotes worse to a wallet they recognize, if a sequencer reorders for a private order-flow deal — none of that is detectable from an executed trade. Bebop's defense against those failure modes is institutional: Wintermute's reputation, JAM's slashable solver bonds, the public quote stream that makes blatant misbehavior visible across enough trades. Those are real defenses. They are not cryptographic.

The architectural alternative absorbs Bebop's coordination wins into the protocol layer rather than overlaying them. Output-constrained intents specify min-output plus invariants — no sandwich, no frontrun, no extraction beyond declared fee — and the chain posts a proof that the executed trade satisfied the intent. Solver competition still happens; it happens *inside* a settlement environment that mathematically rules out the failure modes Bebop's reputation is paying to cover. The same RFQ stream that makes Bebop competitive could plug into such an L2 directly. The difference is where the trust sits — at the relationship layer (Bebop, today) or at the protocol layer (the unified L2, in the next 12-24 months).

It is worth being careful. Bebop is not a denigrated stop-gap. It is a legitimate, well-engineered intermediate step that ships better execution today than the architectural alternative will ship next year. The disagreement is about where the durable execution-quality moat sits. If you believe coordination relationships compound (Wintermute's information edge, JAM's solver network, Bebop's integrations) faster than architectural differentiation, Bebop's model wins. If you believe protocol-level proofs eventually beat reputational guarantees the way smart contracts beat legal contracts on settlement, the unified-L2 thesis wins. The seed implicitly took the second position; the data still supports it, but the case is sharper when the comparison is to Bebop and not to a strawman.

Two adjacent comparison points still matter as cautionary tails for the broader category. [am-AMM](https://arxiv.org/abs/2403.03367) (Auction-Managed AMM, Adams/Moallemi/Reynolds/Robinson) ran in production as Bunni V2's Uniswap v4 hook, at one point processing a meaningful share of tracked v4 hook volume. [Bunni V2 was exploited for $8.4M via a flashloan in September 2025](https://www.quillaudits.com/blog/hack-analysis/bunni-v2-exploit) and [shut down permanently in October 2025](https://www.coindesk.com/business/2025/10/23/bunni-dex-shuts-down-cites-recovery-costs-after-usd8-4m-exploit), with founders citing six-to-seven-figure relaunch costs as prohibitive. The exploit was in the rebalancer math, not the auction primitive — but the project is dead and its TVL is zero. The lesson is not that auction-managed AMMs are bad; it is that stacking app-level complexity on a missing protocol-level primitive creates new attack surface the protocol layer would not have had.

[CoW AMM](https://cow.fi/cow-amm) on Balancer, the second comparison point, runs batch auctions every ~30 seconds where solvers move pool invariants only when the move is "free" — capturing arbitrage surplus to LPs rather than to external arbs. CoW AMM is alive but small in absolute terms — DAO-treasury LPing scale, not Uniswap mainline volumes. Architecturally it is bounded to single-pool LVR recapture; it does not fix cross-pool MEV, cross-rollup MEV, or sandwich attacks at the order/intent level. Those need encrypted mempool, FOCIL, or full intent architecture to land structurally.

The category-level read: Bebop demonstrates that coordination-layer execution is real and durable today. Bunni demonstrates that coordination-layer answers to protocol-layer absences accumulate fragility. CoW AMM demonstrates that coordination wins compound *narrowly* — they fix what they fix and do not generalize. Each is consistent with the seed's broader claim that the protocol-layer answers (ePBS + FOCIL + encrypted mempool) are years out, and that the gap between "coordination patch" and "architectural fix" is the entire window the unified-L2 thesis identifies. The patch's track record so far is "useful while you wait, and Bebop continues to be useful, but it does not compound the way an architecture would."

# VII. WHAT INTEGRATION ACTUALLY LOOKS LIKE IN 2026

Given everything above, the unified L2 we sketched in the seed has a much more concrete shipping shape than it had two months ago. The pieces, in order:

The bundle interface should be **ERC-7683**, not a bespoke DSL. That is the standardization fight that already finished — Across, UniswapX, CoW, Eco, and OIF are the de facto endpoint set, and an L2 that does not speak 7683 has to bootstrap its own intent supply, which Essential's pivot suggests is a losing position. Use 7683 as the constraint-expression schema at the user/app boundary; if you need richer constraints than 7683 expresses (declarative invariants over post-state, not just min-output amounts), extend 7683 with optional fields rather than fork. Composability beats DSL purity in this market.

The proving stack should be **RISC0/Boundless or SP1 for EVM-shaped circuits initially**, with a constraint-shape circuit family — built either on the [Anoma RM's RISC0 backend](https://github.com/anoma/arm-risc0) or on a purpose-built Halo2/UltraHonk family — for the constraint-satisfaction pieces. For the first 12-24 months, accept that arbitrary-invariant proofs are not at sub-block latency. This is the trade-off to be honest about: EVM-shaped proving works today at $32K-$100K rig footprints; arbitrary intent-solver constraints will lag until ASIC ramp and proof-market pricing compound. Anchor on the EVM-shape for the bulk of the chain's traffic, expose constraint-shape proving for the high-value flows that actually need it (privacy, complex multi-party intents, cross-chain coordination), and let the latter expand as the proving market matures.

The sequencer model should borrow from [Taiko's based-rollup playbook](https://docs.taiko.xyz/taiko-alethia-protocol/protocol-design/based-rollups/) — let L1 proposers sequence, inherit L1 ordering for the chain's general flow, and add app-specific sequencing hooks (Angstrom-style) for apps that want to define their own rule. This is the cleanest path to "deterministic ordering" in 2026 because pure FIFO has effectively been abandoned in production EVM L2 design, but L1-driven ordering is live, paired naturally with FOCIL's eventual landing, and avoids the Astria failure mode of "shared sequencer as standalone product." For apps that need a different rule (batch auctions, encrypted-mempool, output-constrained solving), give them an ASS-style hook that delegates sequencing to a slashable operator set with a prescribed rule that is eventually proof-verified as constraint-shape proving matures.

The anchor app should be a **single-DEX with provably better execution than the same swap on L1 or competing L2s** — a swap where the user's signed intent expresses min-output plus invariants (no sandwich, no front-run, no extraction beyond declared fee), and the chain posts a proof that the executed trade satisfied the intent. This is the seed's original framing and it still looks right. The data speaks louder than EIPs. CoW's 22-34% DEX-aggregator share, Across's 54% bridge-user share, [1inch's 60% aggregator recovery](https://www.theblock.co/post/356650/1inch-dex-aggregator-60-market-share-solana-expansion), and Bebop's continuing PMM RFQ growth on 12+ chains are the existence proofs that "intent UX wins on execution quality" is a real market signal at scale. Capturing a meaningful share of that flow with an L2 that *also* posts proofs is the differentiator. The Bebop comparison sharpens the framing: a Bebop-quality RFQ stream wired into an output-constrained settlement layer is strictly better than the same stream wired into a generic L2, because the protocol picks up the failure modes that today fall on Bebop's reputational defenses.

The trade-off worth naming explicitly: building this in 2026 means accepting that the constraint-shape proving for the most ambitious intents will lag the EVM-shape proving by 12-24 months. That is not fatal — most DEX flow is EVM-shaped — but it means the first version of the chain looks like "Taiko-style based ordering + Angstrom-style ASS hooks + ERC-7683 bundle interface + EVM-shape proving via Boundless/SP1" and the *second* version is "constraint-shape proving for arbitrary intents at sub-block latency." Shipping the first version does not require waiting on the second.

# VIII. THE FORCING FUNCTION, REVISITED

The seed's closing argument was that an L2 with measurably better execution forces the L1 conversation more than EIPs ever could. That argument is now operating on a measurable runway. ePBS slipping to Q3-Q4 2026, FOCIL landing Q1-Q2 2027, encrypted mempool unscheduled, MEV-burn 2028+. An L2 that ships a meaningfully better execution model in 2026 has 12-18 months minimum where L1 cannot architecturally match it — not because the EF does not want to, but because the cross-client coordination process is the binding constraint and the EF's own checkpoints say so.

By the time L1 lands ePBS plus FOCIL, the L2 with two years of production data, integrated app ecosystem, and demonstrated execution-quality wins becomes either (a) the template L1 absorbs into its enshrinement, the way rollups taught L1 about off-chain execution, or (b) the place where the durable execution-quality moat sits because the L1 absorption is partial and the L2's design depth still wins on the marginal trade. Either outcome is good. Both are better than the current state, where the conversation is "ePBS exists in research and Glamsterdam might ship it next year." We do not have to wait for that to be true — we can ship the architecture, route real flow, and let the data reframe what L1 enshrines.

The Bebop comparison reinforces rather than weakens this argument. Bebop has had four years to establish the relationship-layer execution moat, and it has done so credibly within its scope. The unified-L2 case is not that Bebop's approach fails — it is that the moat at the protocol layer is structurally deeper, because it is enforced by mathematics rather than by the continued goodwill of the entities it abstracts. Bebop will still exist five years from now and will still serve the trades it serves well. The question is whether the marginal trade — the high-value, high-complexity, multi-party intent — settles through a coordinated maker network or through a settlement environment that proves no extraction occurred. Two years of data on an output-constrained L2 is what answers that question, and waiting on Glamsterdam to answer it is strictly worse than shipping.

# IX. WHAT I WANT TO PUSH ON NEXT

A few threads I want to pull on in the next round.

First: how aggressively should we lean into based-rollup architecture vs. an own-sequencer L2 that mimics based properties? The first inherits L1 censorship resistance once FOCIL lands and avoids the Astria failure mode entirely; the second gives more design freedom on ordering rules — particularly the ability to ship encrypted-mempool or output-constrained orderings before L1 — at the cost of having to argue why you are not another shared-sequencer corpse. I lean toward based, but the answer probably depends on which anchor app commits, because the anchor app's ordering needs constrain the sequencer choice more than the other way around.

Second: the EVM-shape vs. constraint-shape proving sequencing question is sharper than I framed it. The Anoma data point — AnomaPay confirming in 15 seconds in production, despite older forum threads quoting multi-minute RM proves — suggests the constraint-shape window may open earlier for *narrow* circuits than the 12-24 month estimate implies. If you can ship a constraint-shape circuit family for the specific intent shapes your anchor app needs (a single-DEX's output-constrained swap, say), and let the rest of the chain run on EVM-shape proofs, the trade-off looks different. That is closer to Aztec's per-function compiled-circuit pattern than to "wait for general-purpose constraint proving." Worth thinking about whether the chain ships with a constraint-shape ABI from day one for the high-value flows, even if the proof generation is GPU-farm-bound rather than commodity.

Third: the right way to engage Bebop directly. The straightforward path is integration — a Bebop RFQ feed into the L2's solver auction is strictly accretive for both sides. The harder question is whether the L2 should *also* operate its own RFQ surface, which competes with Bebop on origination but inherits the same execution-quality-via-coordination moat that Bebop has built. My instinct is integrate first, compete later, but the timing depends on how fast the constraint-shape proving for the bulk of swap traffic lands. If it lands within 12 months, the L2 can lean entirely on protocol-layer differentiation and never needs an in-house RFQ; if it lands at 24 months, an in-house RFQ may be the bridge.

Fourth: the cold-start question is the one we have been least precise about. Across grew to 54% of daily bridge users on a single, well-targeted use case (cross-chain transfers with relayer competition). CoW grew on a single, well-targeted use case (batch-auction same-chain swaps with MEV protection). Bebop grew on a single, well-targeted use case (RFQ from Wintermute for institutional-size flows). The unified L2 thesis has been fuzzy about which single use case it bootstraps from. The output-constrained DEX is a candidate, but "a swap with provable better execution" is a feature, not a use case. The use case is the audience that signs intents, and we have not nailed down whether that is institutional flow following Bebop's pattern, retail flow following CoW's pattern, or cross-chain flow following Across's pattern. Each implies different anchor partnerships, different fee economics, and different sequencer trade-offs. I would rather sit with the disagreement than flatten it. The next thing worth writing is the version of this report where the anchor-app commitment is concrete enough that the architectural choices follow from it instead of the other way around.

# X. SOURCES

## Anoma and Intent Architecture
- [Anoma — XAN is live (2025-09-29)](https://anoma.net/blog/xan-is-live)
- [Anoma — EVM Protocol Adapter live on Ethereum mainnet (2025-11-18)](https://anoma.net/blog/anoma-is-now-live-on-ethereum-mainnet)
- [Anoma — Live on Base (2025-12-10)](https://anoma.net/blog/anoma-is-now-live-on-base)
- [Anoma roadmap](https://anoma.net/roadmap)
- [AnomaPay public beta on BNB Chain](https://anoma.net/blog/the-anomapay-public-beta-is-live-on-bnb-chain)
- [Anoma Foundation press: AnomaPay beta](https://anoma.foundation/press/anomapaybeta)
- [anoma/evm-protocol-adapter GitHub](https://github.com/anoma/evm-protocol-adapter)
- [anoma/arm-risc0 GitHub](https://github.com/anoma/arm-risc0)
- [Anoma Resource Machine optimization explorations](https://forum.anoma.net/t/resource-machine-optimization-explorations/2084)
- [Anoma Execution Proof Circuit forum](https://forum.anoma.net/t/execution-proof-circuit/2640)
- [Anoma RISC0 backend specification](https://specs.anoma.net/main/implementation/risc0.html)
- [Anoma EVM Protocol Adapter spec](https://specs.anoma.net/main/arch/integrations/adapters/evm.html)

## Essential / Pivot
- [Essential introduces VOID apps (Nov 2025)](https://blog.essential.builders/introducing-essential/)
- [The Block — Essential Series A coverage](https://www.theblock.co/post/310948/intent-centric-ethereum-l2-essential-funding)
- [essential-contributions/pint GitHub](https://github.com/essential-contributions/pint)

## Bebop
- [Bebop — How Bebop works](https://docs.bebop.xyz/bebop/how-bebop-works)
- [Bebop JAM API introduction](https://docs.bebop.xyz/bebop/bebop-api-jam/jam-api-introduction)
- [Bebop PMM RFQ API introduction](https://docs.bebop.xyz/bebop/bebop-api-pmm-rfq/pmm-rfq-api-intro)
- [bebop-dex GitHub (bebop-jam-contracts)](https://github.com/bebop-dex)
- [Bebop unveils JAM (Medium, Dec 2023)](https://medium.com/bebop-dex/bebop-unveils-jam-intent-based-liquidity-aggregation-system-e14a6feedaae)
- [Bebop LTIPP application — Arbitrum forum](https://forum.arbitrum.foundation/t/bebop-ltipp-application-final/21419)
- [Decrypt — Wintermute incubates Bebop (June 2022)](https://decrypt.co/113819)

## Proving and zkVMs
- [SP1 Hypercube — Real-time proving on 16 GPUs](https://blog.succinct.xyz/real-time-proving-16-gpus/)
- [SP1 Hypercube live on mainnet](https://blog.succinct.xyz/sp1-hypercube-is-now-live-on-mainnet/)
- [Brevis Pico Prism — 16-GPU update (Feb 2026)](https://blog.brevis.network/2026/02/25/pico-prism-update-from-64-to-16-gpus/)
- [Brevis Pico Prism — 99.6% real-time (Oct 2025)](https://blog.brevis.network/2025/10/15/pico-prism-99-6-real-time-proving-for-45m-gas-ethereum-blocks-on-consumer-hardware/)
- [Boundless launches mainnet on Base (CoinDesk)](https://www.coindesk.com/tech/2025/09/12/boundless-launches-mainnet-on-base-ushering-in-universal-zero-knowledge-compute)
- [EF — Shipping an L1 zkEVM #1: Realtime Proving](https://blog.ethereum.org/en/2025/07/10/realtime-proving)
- [zkVM SoK (eprint 2026/525)](https://eprint.iacr.org/2026/525.pdf)
- [Cysic ZK ASIC research report](https://medium.com/@0xjacobzhao/cysic-research-report-the-computefi-path-of-zk-hardware-acceleration-3b4517cd183b)
- [Aztec — Client-side proof generation](https://aztec.network/blog/client-side-proof-generation)
- [Aleo Varuna deep dive](https://medium.com/@CFrontier_Labs/aleo-roadmap-2025-and-in-depth-exploration-of-varuna-540ea05a4e8d)
- [Penumbra — Browser proving with parallelism](https://www.penumbra.zone/blog/faster-client-side-proving-with-parallelism)
- [Scroll Euclid upgrade (April 2025)](https://docs.scroll.io/en/technology/overview/scroll-upgrades/euclid-upgrade/)
- [Polygon zkEVM sunset announcement (July 2026)](https://forum.polygon.technology/t/sunsetting-polygon-zkevm-mainnet-beta-in-2026/21020)

## Sequencers, Ordering, MEV
- [Live on Unichain: Fair Transaction Ordering and MEV Protection](https://blog.uniswap.org/rollup-boost-is-live-on-unichain)
- [The First L2 TEE Block Builder is Live on Unichain Mainnet (Flashbots)](https://writings.flashbots.net/unichain-mainnet)
- [Astria sunsets shared sequencer (The Block)](https://www.theblock.co/post/381138/celestia-based-astria-network-sunsets-sequencer-network-after-raising-18-million)
- [Espresso Mainnet 0 is Live](https://medium.com/@espressosys/espresso-mainnet-0-is-live-deedc2505081)
- [Espresso goes full PoS (Mar 2026)](https://www.hokanews.com/2026/02/espresso-goes-full-pos-10-esp-airdrop.html)
- [Radius — encrypted mempool docs](https://docs.theradius.xyz/testnet/portico-testnet/encrypted-mempool)
- [Arbitrum Timeboost gentle introduction](https://docs.arbitrum.io/how-arbitrum-works/timeboost/gentle-introduction)
- [Arbitrum reports $6.74M Timeboost revenue (TronWeekly)](https://www.tronweekly.com/arbitrum-arb-reports-6-74-million-in-timeboost/)
- [Just-in-Time Resale in Ahead-of-Time Auction (arXiv 2603.20175)](https://arxiv.org/abs/2603.20175)
- [Kairos transition — Flashbots Collective](https://collective.flashbots.net/t/when-ahead-of-time-allocation-fails-the-transition-to-kairos/5640)
- [Base reaches Stage 1](https://blog.base.org/base-has-reached-stage-1-decentralization)
- [Base to ditch Optimism for unified Reth stack (Feb 2026)](https://www.theblock.co/post/390380/coinbase-incubated-base-network-to-ditch-optimism-for-unified-solution)
- [Chainlink acquires Atlas](https://www.theblock.co/post/386743/chainlink-acquires-transaction-ordering-solution-atlas-accelerating-rollout-of-its-non-toxic-mev-tool)
- [Special Mechanisms Group acquires MEV Blocker RPC](https://www.ainvest.com/news/special-mechanisms-group-acquires-mev-blocker-rpc-advance-state-art-backrunning-auction-infrastructure-2601/)
- [Angstrom — App-Specific Sequencing](https://docs.angstrom.xyz/l1/core-mechanisms/app-specific-sequencing)
- [Sorella Labs — A New Era of DeFi with ASS](https://sorellalabs.xyz/writing/a-new-era-of-defi-with-ass)
- [Taiko — Based rollups](https://docs.taiko.xyz/taiko-alethia-protocol/protocol-design/based-rollups/)
- [Taiko preconfirmations on mainnet (PR Newswire)](https://www.prnewswire.com/news-releases/taiko-opens-up-the-next-era-of-ethereum-with-preconfirmations-on-mainnet-302527696.html)
- [Flashbots — decentralized building: wat do (Feb 2026)](https://writings.flashbots.net/decentralized-building-wat-do)
- [Introducing BuilderNet](https://buildernet.org/blog/introducing-buildernet)

## Intent Standards and Cross-Chain
- [ERC-7683 specification](https://www.erc7683.org/spec)
- [EIP-7683 (Ethereum EIP)](https://eips.ethereum.org/EIPS/eip-7683)
- [Across — unifying Ethereum / cross-chain](https://across.to/blog/unifying-ethereum-path-to-seamless-crosschain-interoperability)
- [Across — OP Superchain adopts ERC-7683](https://across.to/blog/Optimism-adopts-ERC-7683-to-enable-high-speed-transfers-on-the-Superchain)
- [Ethereum Foundation — 2026 protocol priorities update](https://blog.ethereum.org/en/2026/02/18/protocol-priorities-update-2026)
- [CoW DAO — 2025 in Review](https://cow.fi/learn/cow-dao-2025-in-review)
- [Eco — Eco Routes settlement modularity](https://eco.com/blog/eco-routes-settlement-modularity-for-the-best-cross-chain-ux/)
- [The Block — 1inch DEX aggregator 60% market share](https://www.theblock.co/post/356650/1inch-dex-aggregator-60-market-share-solana-expansion)

## L1 Governance and Forks
- [Ethereum Foundation — Checkpoint #9 (April 2026)](https://blog.ethereum.org/2026/04/10/checkpoint-9)
- [Ethereum Foundation — Checkpoint #8 (January 2026)](https://blog.ethereum.org/en/2026/01/20/checkpoint-8)
- [Pectra mainnet announcement](https://blog.ethereum.org/2025/04/23/pectra-mainnet)
- [Fusaka mainnet announcement](https://blog.ethereum.org/2025/11/06/fusaka-mainnet-announcement)
- [Glamsterdam — ethereum.org roadmap](https://ethereum.org/roadmap/glamsterdam/)
- [EIP-7732 (ePBS)](https://eips.ethereum.org/EIPS/eip-7732)
- [EIP-7805 (FOCIL)](https://eips.ethereum.org/EIPS/eip-7805)
- [EIP-7935 (Fusaka gas-limit raise)](https://eips.ethereum.org/EIPS/eip-7935)
- [EIP-8105 (Universal Enshrined Encrypted Mempool)](https://eips.ethereum.org/EIPS/eip-8105)
- [Hegotá Headliner Proposal: FOCIL (Magicians)](https://ethereum-magicians.org/t/hegota-headliner-proposal-focil-eip-7805/27604)
- [Hegotá Headliner Proposal: EIP-8105 EEM (Magicians)](https://ethereum-magicians.org/t/hegota-headliner-proposal-eip-8105-universal-enshrined-encrypted-mempool-eem/27448)
- [Sigma Prime — Case against EIP-7732](https://blog.sigmaprime.io/glamsterdam-eip7732.html)
- [L1-zkEVM Roadmap 2026 (Ethereum Magicians)](https://ethereum-magicians.org/t/l1-zkevm-roadmap-2026-integrating-zkevm-proofs-into-ethereums-core-protocol/27595)
- [MEV burn — a simple design (Drake)](https://ethresear.ch/t/mev-burn-a-simple-design/15590)
- [Execution Tickets (ethresear.ch)](https://ethresear.ch/t/execution-tickets/17944)

## am-AMM, Bunni, CoW AMM
- [am-AMM paper (arXiv 2403.03367)](https://arxiv.org/abs/2403.03367)
- [Bunni V2 exploit analysis (Quillaudits)](https://www.quillaudits.com/blog/hack-analysis/bunni-v2-exploit)
- [Bunni shutdown (Decrypt)](https://decrypt.co/345621/decentralized-exchange-bunni-pulls-the-plug-following-8-4m-flash-loan-exploit)
- [Bunni shutdown (CoinDesk)](https://www.coindesk.com/business/2025/10/23/bunni-dex-shuts-down-cites-recovery-costs-after-usd8-4m-exploit)
- [CoW AMM (CoW Protocol)](https://cow.fi/cow-amm)
- [CoW AMM — first MEV-capturing AMM](https://cow.fi/learn/cow-dao-launches-the-first-mev-capturing-amm)

## Data Sources & Methodology

This report synthesizes a four-team research pipeline (intent layer, proving, sequencer, governance/coordination layer) audited independently as of 2026-05-02. Where the audit DISPUTED a claim — most notably the Fusaka gas-limit figures — the audited correction is reflected in the body text. Volume and revenue figures cite primary project blogs, Messari, The Block, and DLNews where DefiLlama protocol pages were unreachable; these are flagged inline as appropriate. The Bebop section is sourced exclusively from Bebop's official documentation, GitHub, and reporting (Decrypt June 2022 launch coverage, the December 2023 JAM unveil), per the May 2026 ambiguity resolution.

---
