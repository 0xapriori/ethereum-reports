---
title: "BuilderNet: TEEs, Shared Flow, and the Endgame of Imperative MEV Coordination"
date: 2026-05-19
---

# BuilderNet: TEEs, Shared Flow, and the Endgame of Imperative MEV Coordination

*An honest look at the shared-builder bet eighteen months in: what it solved, what it didn't, and where it sits on the map of coordination-layer answers to MEV.*

## tl;dr

- **BuilderNet shipped November 2024.** Announced November 18 and producing live mainnet blocks under the `BuilderNet` `extra_data` tag by November 26, with a first test block on November 12. It is a multi-operator block-building network running [rbuilder](https://buildernet.org/docs/architecture) inside Intel TDX confidential VMs, with shared orderflow across all operators and a marginal-contribution-based refund rule. Operators ship code openly via reproducible builds verified against [measurements.builder.flashbots.net](https://measurements.builder.flashbots.net).
- **The TEE hardware is Intel TDX, not SGX or AMD SEV.** Production runs on Microsoft Azure, with a [GCP-based Data Center Execution Assurance (DCEA) prototype](https://writings.flashbots.net/mind-the-gap-tee-poc) demonstrating how to extend trust to the data center operator. Flashbots is unusually candid that attestations prove *what code runs*, not *where* — and the [TEE.fail disclosure on October 28, 2025](https://tee.fail) showed that physical-access attacks on DDR5 buses can extract attestation keys for under $1000.
- **Beaverbuild migrated in May 6, 2025 — a migration, not an acquisition.** No money, equity, or governance rights changed hands publicly. Beaver retired its centralized builder and joined as an operator and development partner ([Beaverbuild × BuilderNet](https://buildernet.org/blog/beaverbuild)). The two historical centralized builders (Flashbots, Beaver) now sit inside the same shared network, with Nethermind as the third operator.
- **The adoption arc was gradual, not a single-day inflection.** Per per-day relayscan archives, BuilderNet ran at ~1% through Dec 2024, ~7% by Feb 2025, was already at 17% on May 5 2025 (day before the Beaverbuild announcement), peaked in the high 30s / low 40s in mid-August 2025, and has since settled into a 21–35% band. Today's 7-day trailing is 21.80% ([relayscan.io 7d](https://www.relayscan.io/?t=7d)) with a 24h dip to 15.91%. Titan still leads at 50.36% 7-day. Quasar at 18.42% is buying share with subsidies. The top three account for ~90%+ — concentration moved sideways from "two centralized builders" to "one TEE network + Titan + Quasar."
- **The refund rule is the design hook.** A marginal-contribution formula `ϕᵢ(T,c) = (μᵢ(T)/Σμⱼ(T)) × min(v(B(T))−c, Σμⱼ(T))` distributes block value back to orderflow contributors — explicitly excluding public mempool transactions ([refund docs](https://buildernet.org/docs/refunds)). Operators are paid $0. As shea put it on the forum: ["There is no economic benefit to running a node in BuilderNet today."](https://collective.flashbots.net/t/questions-about-buildernet-block-building-refund-rule-and-tee-guarantees/4934) Rent is pushed up to orderflow origination and PMM pricing.
- **SUAVE-the-chain is over; SUAVE-the-vision lives inside BuilderNet.** [`suave-geth` was archived on May 12, 2025](https://github.com/flashbots/suave-geth). Flashbots has not published a "SUAVE is over" post, but the programmable-MEV-via-TEE idea now ships through BuilderNet, [Rollup-Boost on Unichain mainnet](https://blog.uniswap.org/rollup-boost-is-live-on-unichain), Signal-boost, and the broader Mosaik / NAMP research line ([decentralized building: wat do?](https://writings.flashbots.net/decentralized-building-wat-do)).
- **The endgame Flashbots is openly betting on:** "The core boilerplate of block building will become a public utility which supports this decentralized network." That quote is verbatim from [Introducing BuilderNet](https://buildernet.org/blog/introducing-buildernet). If that bet holds, competition migrates up to orderflow origination and PMM pricing, and down to inventory/hedge pipes — the builder layer commoditizes. If operator economics don't stay aligned with the "no economic benefit" frame, it collapses back toward rent-extracting builders.

## Table of Contents

1. [What BuilderNet Is](#what-buildernet-is)
2. [Architecture: Intel TDX, Attestation, and the Orderflow Proxy](#architecture-intel-tdx-attestation-and-the-orderflow-proxy)
3. [Credible Neutrality and the Measurement Registry](#credible-neutrality-and-the-measurement-registry)
4. [Proof of Cloud and the Honest Threat Model](#proof-of-cloud-and-the-honest-threat-model)
5. [The Beaverbuild Migration](#the-beaverbuild-migration)
6. [Adoption: What the Numbers Actually Say](#adoption-what-the-numbers-actually-say)
7. [The Refund Rule and Operator Economics](#the-refund-rule-and-operator-economics)
8. [Roadmap and Strategic Center of Gravity](#roadmap-and-strategic-center-of-gravity)
9. [The SUAVE Question](#the-suave-question)
10. [Rollup-Boost: The L2 Sibling](#rollup-boost-the-l2-sibling)
11. [Builder Centralization in Context](#builder-centralization-in-context)
12. [A Decentralization Scorecard](#a-decentralization-scorecard)
13. [Where Should MEV Flow? The Normative Question](#where-should-mev-flow-the-normative-question)
    - [What Does BuilderNet Look Like After FOCIL, Encrypted Mempools, and MEV Burn Ship?](#what-does-buildernet-look-like-after-focil-encrypted-mempools-and-mev-burn-ship)
14. [The Endgame Question: Does Every Venue Roll Its Own Builder?](#the-endgame-question-does-every-venue-roll-its-own-builder)
15. [Imperative vs Declarative — Where This Fits](#imperative-vs-declarative--where-this-fits)
16. [Closing](#closing)

## What BuilderNet Is

BuilderNet is a multi-operator, TEE-attested block-building network. The design goals stated at launch were straightforward: reduce builder centralization without surrendering the performance characteristics that made centralized builders dominant; keep private orderflow private without trusting any single operator with that flow; and route value back to whoever generated it, rather than letting it accrue to the entity running the box.

The network was [announced on November 18, 2024](https://collective.flashbots.net/t/introducing-buildernet/4173), with a first test mainnet block landing on November 12 and the production `BuilderNet` `extra_data` tag appearing on blocks from November 26 onward. As of May 2026, the operator set is **Flashbots, Beaverbuild, and Nethermind** — permissioned, three nodes deep, and not yet open to anyone who can prove an attestation. That permissioning is a deliberate Phase 1 choice; the [public roadmap](https://buildernet.org/docs) names a Phase 2 in which the operator set opens up under the same attestation discipline.

Two things distinguish BuilderNet from a federated-builder-cartel framing. First, the orderflow is *shared* across all operator nodes: a transaction submitted to any node is visible to all, so the builder network behaves more like a single distributed builder than three coordinated ones. Second, the operators are paid nothing. The refund formula pays orderflow contributors, not the people running the hardware. We'll return to both of these.

## Architecture: Intel TDX, Attestation, and the Orderflow Proxy

The hardware bet is Intel TDX (Trust Domain Extensions), not SGX and not AMD SEV. Production deployment is on Microsoft Azure confidential VMs. There's a parallel DCEA prototype running on GCP, which we'll discuss in the threat-model section. The [October 2025 engineering update](https://collective.flashbots.net/t/engineering-update-october-2025/5343) signaled active diversification work — evaluating GCP, OpenMetal, and OVH as TDX hosts — driven in part by an Azure Gen5→Gen6 forced VM migration that caused measurable performance degradation. Single-cloud is the current state; it is not stated as the target. Each operator runs an identical software stack inside the TDX boundary: [rbuilder](https://buildernet.org/docs/architecture) as the block builder, Reth as the execution client, a minimal Linux image, and an RA-TLS reverse proxy that handles orderflow ingress.

The reproducible-build discipline is the load-bearing claim. Every operator's running image must produce the same TDX measurement as the one published at [measurements.builder.flashbots.net](https://measurements.builder.flashbots.net). The orderflow proxy enforces this on connection: if a node's attestation doesn't match a published measurement, the proxy refuses to send it private flow. Orderflow ingress runs on ports 443 and 7936; senders sign their requests with an `X-Flashbots-Signature` header bound to their reputation key, the same pattern Flashbots has used since the original MEV-Boost ecosystem.

The shared-orderflow piece deserves emphasis. When a searcher or originator submits a bundle to any one of the operator nodes, that bundle is replicated across all three. This means:

- No operator has a private orderflow advantage over the others.
- The "decentralized" frame is honest at the builder layer: removing any one operator does not strand orderflow with the survivors.
- The threat model collapses to the *intersection* of operator trust and TDX integrity — if all three operators colluded *and* TDX held, they could not extract orderflow value beyond what the refund rule allocates; if TDX failed but the operators were honest, they still couldn't extract because the keys never live outside the enclave.

That second-to-last point is where the architecture actually buys you something. Centralized builders historically had to be trusted to not front-run their own orderflow. BuilderNet replaces operator-as-trusted-party with TEE-as-verifier-of-software, which is a meaningfully different (and weaker, but more verifiable) trust assumption.

## Credible Neutrality and the Measurement Registry

It is worth pausing on what kind of legitimacy BuilderNet is and isn't building.

The standard frame for credible neutrality in crypto systems — most cleanly articulated in Vitalik's [Credible Neutrality As A Guiding Principle](https://nakamoto.com/credible-neutrality/) — distinguishes four sources of legitimacy: *process* (the rules are written down, applied uniformly, and verifiable), *performance* (the thing works), *participation* (anyone affected has a path to participate in operating it), and *fairness* (the outcomes the system produces are defensible against the people they affect). Systems don't need all four to be useful, but the ones they lack are the ones their critics correctly latch onto.

BuilderNet leans hard on two of these. On *process*, the reproducible-build discipline against published measurements is real: the running binary's hash on any operator node has to match a measurement signed and posted at `measurements.builder.flashbots.net`, and the orderflow proxy enforces this on every connection. On *performance*, the network is producing live mainnet blocks under load, with a roadmap that has shipped rather than slipped. Both are credible — and credible in a way most "decentralized" infrastructure in this space is not.

It is structurally weaker on the other two. *Participation* is bounded by a permissioned three-operator set, with admission criteria decided by the network's founder. *Fairness*, on the orderflow side, is contingent on the marginal-contribution refund formula being the right rent-distribution rule — which it might be, but which is a single design choice made by a single team, and which defines "user" as orderflow originator (PMMs, frontends) rather than as the human whose loss the rent is extracted from. Both are open questions, not solved ones.

The piece worth naming explicitly is the **measurement registry**. Whoever signs and serves `measurements.builder.flashbots.net` has, in operational terms, the right to declare what code is "BuilderNet code." A measurement that isn't on the list does not receive orderflow. A measurement that gets added to the list does. That is a quiet form of governance: the act of curating the measurement set is the act of deciding which operators get to participate and which versions of the software constitute the canonical network. Today Flashbots holds that key. Nothing in the public design changes that.

This is not a gotcha. A reproducible-build attestation pipeline has to be anchored somewhere, and "the team that wrote the code" is the obvious anchor for a Phase 1 network. The point is that the measurement-registry signer is a real centralization vector that doesn't show up in the operator count. Two things can be true at once: reproducible builds plus open measurements is a serious move toward credible neutrality on the dimensions process and performance, *and* a permissioned operator set whose admission criteria and measurement registry are controlled by the network's founder is not yet credible neutrality on the dimensions participation and fairness. Both are true. Holding them together is the right posture.

The natural Phase 2/3 question is who signs the measurement registry after Flashbots. A multi-party threshold-signed registry, a transparency-log-anchored registry, a council of measurement-registry signers with rotating membership — there are several shipping-grade answers, and none of them have been publicly chosen. That's the choice to watch for.

## Proof of Cloud and the Honest Threat Model

Flashbots' [Mind the Gap: TEE PoC](https://writings.flashbots.net/mind-the-gap-tee-poc) post is one of the more intellectually honest pieces of writing the MEV ecosystem has produced. It concedes the core gap explicitly: TEE attestations prove *what code is running*, but they do not prove *where it is running*. An attacker with physical access to a TDX-capable machine can, in principle, extract attestation keys and impersonate a legitimate enclave from arbitrary hardware.

[TEE.fail](https://tee.fail), disclosed October 28, 2025, demonstrated this concretely with [under $1000 of off-the-shelf hardware](https://www.bleepingcomputer.com/news/security/teefail-attack-breaks-confidential-computing-on-intel-amd-nvidia-cpus/). The attack works by interposing on the DDR5 memory bus and exploiting the determinism of AES-XTS encryption to recover attestation keys from Intel SGX, Intel TDX, and AMD SEV-SNP. It hits the entire current generation of x86 confidential computing.

Flashbots' response is the DCEA (Data Center Execution Assurance) line of work. DCEA is prototyped on GCP and uses signed attestations from the data center operator to bind a TDX measurement to a physical machine in a specific facility. Intel's POE (Platform Ownership Endorsement) program is the chip-vendor-side equivalent — Intel signs an endorsement that a specific endorsement key belongs to a specific machine sold to a specific customer. POE has no public deployment timeline.

The honest reading: BuilderNet's TEE bet is load-bearing, the public attacks on TEE are credible, and Flashbots is putting more public-facing effort into closing the gap than any other team in the space. They are not pretending the gap doesn't exist. They are also not pretending DCEA-on-GCP is a production answer for BuilderNet — it's a prototype demonstrating a path. Until POE ships in production silicon or a meaningful subset of operators run on DCEA-attested hardware, "Proof of Cloud" remains a research direction, not a shipped property.

## The Beaverbuild Migration

On May 6, 2025, Beaverbuild announced it was retiring its centralized block builder and joining BuilderNet as an operator and development partner ([Beaverbuild × BuilderNet](https://buildernet.org/blog/beaverbuild), [forum announcement](https://collective.flashbots.net/t/announcement-beaverbuild-buildernet/4924)). The Flashbots writeup ([Migrating to BuilderNet](https://writings.flashbots.net/migrating-to-buildernet)) frames it explicitly as a migration, not an acquisition.

This distinction matters. No purchase price, no equity swap, no governance seat, no token allocation has been disclosed because none was involved. Beaverbuild's team chose to stop running their own builder and to start operating a BuilderNet node instead, because the shared-flow model captures the same economic surface as their previous standalone operation while removing the operational burden of being the sole trusted party for their orderflow providers.

The context is worth stating plainly. Pre-migration, Beaverbuild was the #1 block builder by share, frequently above 40% of MEV-Boost blocks. Combined with Titan, the two historical centralized builders accounted for roughly 86% of blocks through early 2025. The migration consolidates two of the three pre-BuilderNet centralized builders into the shared network. (The third, Rsync, deprioritized around the same period — Eigenphi's [phrasing](https://eigenphi.substack.com/p/mev-space-hidden-mechanics-of-searcher-builder-integration) is the most accurate qualitative description we have: "Wintermute and Rsync just basically gave up and stepped out of the market.")

The Beaverbuild migration is the single largest piece of evidence available for the public-utility hypothesis. A team that *was* extracting builder rent voluntarily moved into a model where they extract zero builder rent. The simplest explanation is that they expect orderflow rent — what they earn from the originators they continue to source flow from — to outweigh what they were earning from the marginal builder edge.

## Adoption: What the Numbers Actually Say

The richest public time-series for BuilderNet share is [relayscan.io's daily stats archive](https://www.relayscan.io/stats), which exposes per-day builder breakdowns at `relayscan.io/stats/day/YYYY-MM-DD` going back to BuilderNet's launch. The table below is built from single-day relayscan snapshots, with each row independently re-verified against the canonical `BuilderNet` combined row (which equals the sum of its `Flashbots`, `Beaver`, and `Nethermind` variants — adding them on top would double-count). [Coin Metrics' State of the Network #356](https://coinmetrics.substack.com/p/state-of-the-network-issue-356) (March 24 2026) and [ByteNoob's February 2026 deep-dive](https://bytenoob.io/2026-02-12-block-builder-buildernet-1.html) corroborate the late-period data points.

Other major public dashboards — [mevboost.pics](https://mevboost.pics), [payload.de](https://payload.de), [libmev.com](https://libmev.com), [mevWatch](https://www.mevwatch.info), [rated.network](https://explorer.rated.network/builders), and most Dune queries — are client-rendered and could not be extracted server-side for this snapshot; their published values are consistent with the relayscan series where reportable.

**Table A — BuilderNet share trajectory, single-day snapshots since inception** (source: `relayscan.io/stats/day/YYYY-MM-DD` for each date)

| Date | Context | BuilderNet share |
|------|---------|------------------|
| 2024-11-27 | T+1 after Nov 26 production launch | 1.32% |
| 2024-12-15 | 10 days after Flashbots retired its own builder | 1.13% |
| 2025-02-19 | v1.2 release day | 7.62% |
| 2025-05-05 | day before Beaverbuild migration announcement | 17.30% |
| 2025-05-06 | Beaverbuild migration announcement day | 11.21% |
| 2025-05-07 | day after announcement | 11.94% |
| 2025-08-14 | – | 36.34% |
| 2025-08-15 | mid-August high | 37.04% |
| 2025-08-16 | – | 41.49% |
| 2025-09-15 | – | ~35% |
| 2025-10-15 | post-Quasar emergence | 27.20% |
| 2025-11-15 | – | 31.06% |
| 2025-12-09 | day of the BPO1 incident | 30.44% |
| 2026-02-12 | within days of [Coin Metrics #356](https://coinmetrics.substack.com/p/state-of-the-network-issue-356) point estimate (BuilderNet 26.0% / Titan 47.6%) | 26.01% |
| 2026-05-19 (7d) | trailing 7-day window | 21.75% |
| 2026-05-19 (24h) | most recent day | 15.82% |

The arc is not what the marketing usually claims and it is not a single-day cliff. BuilderNet sat at ~1% for its first three months while the Beaverbuild–Titan duopoly held ~93%+ of blocks. The line climbed to ~7–8% by February 2025, was already at 17% on May 5 — *the day before* the Beaverbuild migration announcement — and continued rising through summer 2025 to a peak in the high 30s / low 40s in mid-August. The Beaverbuild announcement formalized a migration that was already underway in the data, rather than triggering it. BuilderNet then settled into a 25–35% band as Quasar emerged as a real third force in late 2025. The recent two-week window shows a sharp pullback — 21.75% on a trailing 7-day basis and 15.82% on the latest 24h, with Quasar absorbing the share BuilderNet shed.

**Table B — Current builder market share** ([relayscan.io 7d](https://www.relayscan.io/?t=7d), accessed 2026-05-19)

| Builder | 7d share | 24h share |
|---------|----------|-----------|
| Titan | 50.36% | 53.22% |
| BuilderNet | 21.80% | 15.91% |
| Quasar | 18.42% | 23.97% |
| Other | 9.42% | 6.90% |

**Table C — 7-day builder profit and subsidy** ([relayscan.io builder-profit](https://www.relayscan.io/builder-profit?t=7d), accessed 2026-05-19)

| Builder | 7d profit (ETH) | 7d subsidy (ETH) |
|---------|-----------------|------------------|
| Titan | +414 | — |
| BuilderNet | +106 | — |
| Quasar | +8.41 | +4.73 |

A few things to read off this carefully:

- **The narrative arc is gradual, not flip-of-a-switch.** Roughly 1% → 7% → 17% (pre-Beaver) → 37% (mid-August) → 21% (today). The Beaverbuild migration was the most-watched moment but not the inflection point — the inflection started months earlier, around the v1.2 release in February 2025. The drift down from the late-summer peak coincided with Quasar's emergence and a partial Titan reassertion.
- **Titan still leads materially.** Roughly half of all MEV-Boost blocks. BuilderNet does not displace Titan in the current data — it has taken a meaningful number-two slot, then partially given it back to Quasar over the last six months.
- **Quasar is buying share, not earning it.** Roughly half of Quasar's 7-day profit number is a subsidy line. A builder spending 4.73 ETH to capture 8.41 ETH of net profit on ~18% of blocks is in market-acquisition mode, not steady-state competition. This is fine and normal for an entrant; the analytic point is that the headline share number overstates Quasar's competitive position relative to BuilderNet's near-zero-subsidy operation.
- **Concentration moved sideways.** Pre-Beaver-migration, the top two builders (Beaver + Titan) sat around 86% in March 2025. Post-migration, the top three (BuilderNet + Titan + Quasar) sit at ~90%+. The number of *operators* changed; the *distribution of blocks across competing entities* did not improve. Honest reading: "decentralized builder network" and "decentralized block-building market" are not the same property, and BuilderNet has shipped the first without shipping the second.
- **Cumulative lifetime blocks built is not publicly published** on any dashboard surveyed. The closest reference point is [ByteNoob's February 2026 snapshot](https://bytenoob.io/2026-02-12-block-builder-buildernet-1.html) of 56,449 BuilderNet blocks across 198,615 in its sample window (28.39%) — a window count, not a lifetime total. Reconstructing a cumulative figure would require summing the relayscan daily archive day by day, which is a derivation, not a published number.

## The Refund Rule and Operator Economics

The marginal-contribution refund formula is the design choice most worth understanding:

`ϕᵢ(T, c) = (μᵢ(T) / Σⱼ μⱼ(T)) × min(v(B(T)) − c, Σⱼ μⱼ(T))`

In English: each orderflow contributor `i` gets paid in proportion to their marginal contribution `μᵢ(T)` to total block value, capped by the realized block value `v(B(T))` minus operator cost `c`, and capped further by the sum of all marginal contributions. Public mempool transactions are explicitly excluded from `μᵢ` — they don't generate refunds because no specific contributor brought them ([refund docs](https://buildernet.org/docs/refunds)).

This is genuinely interesting design. Most rent-distribution mechanisms in MEV land are either Shapley-style (compute the cooperative-game value, which is expensive) or proportional-to-tip (trivially gameable). The marginal-contribution formula approximates the Shapley value at much lower computational cost while remaining hard to manipulate, because increasing your own `μᵢ` requires actually contributing more value to the block.

The operator side is the symmetric design choice. Operators run at cost. There is no operator fee. There is no token. There is no governance seat being implicitly compensated. As [shea wrote on the forum](https://collective.flashbots.net/t/questions-about-buildernet-block-building-refund-rule-and-tee-guarantees/4934): "There is no economic benefit to running a node in BuilderNet today." The [Combined Refunds proposal (Sep 2025)](https://collective.flashbots.net/t/combined-refunds/5270) goes further — it would move refunds from monthly off-chain payouts to atomic same-block on-chain refunds, hardening the rent-flows-to-contributors property at the protocol level rather than relying on operator integrity for the post-hoc accounting. [FlowProxy: Approaching Optimality](https://collective.flashbots.net/t/flowproxy-approaching-optimality/5459) (January 2026, Chainbound × Flashbots) is the empirical companion piece — order losses and latency measurements across BuilderNet's flow ingress that the refund accounting depends on.

The strategic logic is consistent with the public-utility framing. If you're trying to commoditize the builder layer, you do not want operators to develop independent rent-extraction incentives. The model only holds if running an operator is something teams do for strategic reasons (proximity to orderflow, infrastructure expertise, reputation in the ecosystem) rather than for builder margin.

Two open questions sit on top of this:

1. **Does "at cost" hold in steady state?** Hardware, bandwidth, and operations are real costs. If the network needs to subsidize operators to keep three independent teams running TDX-capable nodes, the public-utility frame holds. If the network has to start paying operators a margin to retain them, it collapses back toward rent-extracting-builders-with-extra-steps.
2. **What happens when the operator set expands?** A permissionless attestation-gated entry policy is the Phase 2 target. The economic question of who *wants* to operate a permissionless node, under what cost structure, with what reputational stake, is unsolved.

## Roadmap and Strategic Center of Gravity

The [decentralized building: wat do?](https://writings.flashbots.net/decentralized-building-wat-do) post (February 2026) is the most recent public framing of where the work is going. The shorthand is Phase 0 → 3:

- **Phase 0 — Centralized building.** Pre-BuilderNet status quo.
- **Phase 1 — Replicated privacy.** Where BuilderNet sits today. Multi-operator, shared orderflow, TEE-attested replication, permissioned operator set.
- **Phase 2 — Modular distributed building.** Operator set opens. Components (orderflow ingress, simulation, ordering, sealing) become composable across operators rather than locked into a single rbuilder binary. Mosaik and rblib are the engineering primitives being built toward this.
- **Phase 3 — Decentralized building.** Permissionless participation, with attestation discipline as the gate. End-state is "anyone who can prove they're running the right code can route orderflow."

The newer primitives worth tracking:

- **Flashnet** — a low-latency broadcast layer in the [Network-Anonymized Mempools (NAMP)](https://writings.flashbots.net/network-anonymized-mempools) research line, designed to handle high-frequency orderflow distribution under privacy constraints. The most recent technical step is the [March 2026 proposal for IBLT-based anonymous broadcast with auction-based scheduling](https://collective.flashbots.net/t/anonymous-broadcast-with-auction-based-scheduling/5630), positioning Flashnet as the connective tissue moving BuilderNet toward permissionlessness.
- **rblib** — a library factoring out reusable block-building components so that Phase 2's modular operator set can plug components together rather than forking the whole stack. The companion piece is [multi-client rbuilder support over IPC](https://collective.flashbots.net/t/announcing-multi-client-support-for-rbuilder/4895) (announced April 2025), which Nethermind has implemented — a direct client-diversity step that breaks the "everyone runs the same binary" frame.
- **Mosaik** — the composition framework for Phase 2 modular building.
- **Signal-boost** — the L2-side priority signal mechanism, related to but distinct from Rollup-Boost.
- **DCEA / POE / "Proof of Cloud"** — the trust-the-physical-machine work covered in the threat-model section.

The strategic center of gravity is moving from "build the shared builder" (done in Phase 1) to "decompose the builder into modular components and make the operator boundary fluid" (Phase 2-3). That's a multi-year arc.

## The SUAVE Question

SUAVE was the previous Flashbots flagship: a programmable-MEV chain that would let anyone deploy MEV-aware applications inside TEE-attested execution. The [original SUAVE post](https://writings.flashbots.net/the-future-of-mev-is-suave) framed it as the future of MEV.

[`suave-geth` was archived on May 12, 2025](https://github.com/flashbots/suave-geth). Flashbots has not published a "SUAVE is over" post. What they have done, in practice, is rebuild the most useful pieces of the SUAVE vision inside BuilderNet, Rollup-Boost, and Signal-boost. The TEE-attested-programmable-execution primitive that SUAVE-the-chain was supposed to provide is now provided by BuilderNet's TDX boundary. The cross-rollup MEV coordination piece is being prototyped inside Rollup-Boost on Unichain and Base. The programmable-priority piece sits in Signal-boost.

The honest read: SUAVE-the-chain was shelved because it was the wrong abstraction for the problem. Building a separate L1/L2 to coordinate MEV across chains is heavier than necessary if you can get the same coordination by embedding TEE-attested builders into the chains that already exist. SUAVE-the-vision — programmable MEV with verifiable execution — is alive and shipping; it just doesn't need its own chain to ship.

## Rollup-Boost: The L2 Sibling

Rollup-Boost is the BuilderNet primitive packaged as a sidecar for L2 sequencers. Same TEE bet, same attestation discipline, different deployment target.

[Rollup-Boost went live on Unichain mainnet on May 2, 2025](https://blog.uniswap.org/rollup-boost-is-live-on-unichain). The initial integration shipped **Verifiable Priority Ordering** — priority-fee-based transaction ordering enforced inside the TEE, which gives Unichain a credible "priority gas auction with attested fairness" property — and laid the foundation for **Flashblocks**, 200ms streaming sub-blocks that went live in August 2025 and give traders sub-second confirmation feel.

Base has Flashblocks. World Chain runs Priority Blockspace for Humans, a Rollup-Boost-derived ordering rule that bumps verified-human transactions ahead of bot traffic. On [August 21, 2025, Optimism and Flashbots announced a Superchain-wide partnership](https://www.optimism.io/blog/optimism-partners-with-flashbots) ([Flashbots side](https://writings.flashbots.net/flashbots-superchain)) to deploy Rollup-Boost across the OP Stack ecosystem.

The strategic shape: BuilderNet is the L1 instance, Rollup-Boost is the L2 instance, and the underlying primitive (TEE-attested, multi-operator, shared-flow, refund-aware building) is the actual product. The L2 work also clarifies what kind of business Flashbots is building — not a builder, but an infrastructure layer that builders run inside.

## Builder Centralization in Context

Pre-Merge, MEV concentration was a validator problem. MEV-Boost solved the proposer side: by separating proposer from builder, it removed the requirement that every validator either run sophisticated MEV extraction or accept being out-competed. By 2024, roughly 90% of blocks were proposed via MEV-Boost relays. The proposer-side problem was resolved by relocating it to the builder side, where a small number of well-resourced teams accumulated structural advantages: low-latency colocation, deep searcher integrations, exclusive orderflow agreements, and the operational capacity to run a builder at the latency frontier.

[Heimbach et al. (2023)](https://arxiv.org/abs/2305.19037) is the canonical academic framing of this concentration. Builder-side HHI sat at roughly 3,892 in March 2025, calculated from the top builders' two-week market shares (44.46² + 42.53² + 9.98² + 1.88² + 1.18² ≈ 3,891.91) — roughly twice the DOJ's "highly concentrated" market threshold (1,800). The [ChainSafe overview](https://blog.chainsafe.io/beyond-mev-navigating-ethereums-decentralization-and-block-building-future/) frames the broader concentration problem; for academic HHI series see [Heimbach et al.](https://arxiv.org/abs/2305.19037) (normalized 0–1, mean ~0.21). The [arXiv 2410.12352](https://arxiv.org/abs/2410.12352) paper found that private orderflows contributed to 54.59% of block value across its sample period of January 2023 to May 2024 — a single number that captures why builders compete more on orderflow access than on raw block-building skill. Robert Miller's [MEV and the Limits of Scaling](https://writings.flashbots.net/mev-and-the-limits-of-scaling) (June 2025) is Flashbots' clearest framing of why this matters — MEV-driven spam consumes a substantial fraction of blockspace (with Base cited as an extreme case), which is why BuilderNet and Rollup-Boost are framed as scaling infrastructure, not just builder reform.

The [January 2024 Illuminate post](https://writings.flashbots.net/illuminate-the-order-flow) noted that SCP and Wintermute together built 61% of MEV-Boost blocks. That was a January 2024 snapshot and the names have shifted — Wintermute's Rsync stepped out, Beaver migrated in, Quasar appeared — but the structural picture has not. The relay side has its own concentration story, with [mevWatch](https://www.mevwatch.info) tracking OFAC-compliance among relays as a parallel concern.

The point is not that BuilderNet failed to solve a hard problem. The point is that the problem has two distinct dimensions — *who builds the box that builds the blocks* and *who controls the orderflow that the box builds blocks from* — and BuilderNet is a decisive intervention on the first dimension and only an indirect one on the second.

## A Decentralization Scorecard

"Concentration moved sideways" is a useful shorthand but it isn't operational. Decentralization isn't one axis, it's several, and the only way to be honest about what BuilderNet improves and what it doesn't is to look at the axes separately. The framing here borrows from the standard *architectural / political / logical* decomposition Vitalik laid out in [The Meaning of Decentralization](https://medium.com/@VitalikButerin/the-meaning-of-decentralization-a0c92b76a274) and refines it for the specific surface BuilderNet operates on.

| Dimension | BuilderNet (May 2026) | Notes |
|---|---|---|
| Operator count | 3, permissioned | Phase 2 target is permissionless attestation-gated entry; not shipped yet. |
| Client / software diversity | rbuilder primary; multi-client over IPC shipped (Nethermind) | Genuine improvement on the "everyone runs the same binary" frame. |
| Hardware vendor diversity | Intel TDX only | Single-vendor — load-bearing. TEE.fail hit the entire current x86 generation; no AMD SEV or ARM fallback. |
| Cloud / data-center diversity | Microsoft Azure (production); GCP/OpenMetal/OVH in evaluation | Active diversification work, not shipped. Azure Gen5→Gen6 forced migration is the most concrete reason the issue is live. |
| Geographic / jurisdictional diversity | Not documented publicly | Real gap. If all three operators terminate in the same jurisdiction, the network has one regulatory exposure surface. |
| Governance / social-graph diversity | Flashbots + Beaver (now inside Flashbots' orbit) + Nethermind | Two of the three operators are in the same coordination network. Nethermind is the only outside-coordination operator. |
| Measurement-registry control | Flashbots-signed | The quiet governance vector covered above. |
| Orderflow source diversity | Shared across all nodes by design | The strongest score on the chart. This is the dimension the architecture was built to solve, and it does. |

Read this carefully and the picture is honest in both directions. The architecture is a genuine improvement on at least two axes that prior centralized builders couldn't claim — client diversity (Nethermind's IPC integration is real client-diversity work, not just "Reth with a different binary name") and orderflow source diversity (shared flow across all operators removes the per-operator private-flow moat that was the original centralization story). It hasn't moved on hardware-vendor diversity, geographic diversity, or measurement-registry control, and Phase 1's three-operator set is what it is.

The standard rejoinder is "Phase 1 is meant to be temporary." That's true and fair. The question for an honest reader is which of these axes Phase 2 actually changes. Permissionless attestation-gated entry changes the operator-count axis directly and the social-graph axis indirectly. It doesn't change the hardware-vendor axis (TDX-only is a function of which silicon exists, not which operators run). It doesn't, by itself, change the measurement-registry axis — adding operators doesn't redistribute the signing key. The scorecard is useful precisely because it lets you tell which roadmap items move which axes.

## Where Should MEV Flow? The Normative Question

The scorecard above is a diagnostic. The harder question sitting underneath the whole BuilderNet conversation is normative: *given that MEV exists as a rent surface on Ethereum's current architecture, who should it accrue to?*

Most discussion of MEV mitigation skips this and goes straight to mechanism. That's a mistake. The mechanism you build depends on the answer to the normative question, and different teams in the space are quietly betting on different answers. It's worth naming them.

There are roughly four candidate destinations.

1. **To users (refunds).** This is BuilderNet's bet. The marginal-contribution refund rule pays MEV back to the orderflow contributor who brought the transaction in, on the theory that the contributor is the closest available proxy for the user whose value was on the line. The strength of this bet is that it aligns the incentives of the people building the infrastructure with the people whose value the infrastructure is touching — operators don't earn from MEV, contributors do. The weakness is that "user" gets defined as *orderflow originator*, which in practice is PMMs, frontends, intent-protocol solvers, and aggregators. Those entities can pass refunds through to end-users; the architecture doesn't compel it. The refund is paid to whoever holds the X-Flashbots-Signature key, not to whoever clicked "swap."

2. **To validators (MEV burn / MEV smoothing).** This is the protocol-layer bet. [Vitalik's MEV burn writeup](https://ethresear.ch/t/mev-burn-a-simple-design/15590) and the broader family of [MEV smoothing proposals](https://ethresear.ch/t/committee-driven-mev-smoothing/10408) target a different equilibrium: have the protocol capture MEV and either burn it (which redistributes it to all ETH holders proportionally, like EIP-1559 base fee) or smooth it across the validator set. The strength is broad distribution across a large set of participants rather than concentration at any one layer. The weakness is the implicit message it sends to the end-user: your loss is acceptable because validators (or all ETH holders) get paid. That isn't quite right either — burn does benefit users indirectly via deflation — but the framing is closer to "MEV is a tax we collect and redistribute" than to "MEV is a thing we should not be charging in the first place."

3. **To no one (encrypted mempools, fair ordering, batch auctions).** This is the structural bet. The literature here is older than people remember: [Kelkar et al.'s Order-Fairness for Byzantine Consensus](https://eprint.iacr.org/2020/269) and the follow-up [Themis](https://eprint.iacr.org/2021/1465) lay out the theory of order-fair consensus; [Bormet et al.'s Encrypted Mempools survey](https://arxiv.org/abs/2503.04977) is the most current consolidated framing; [CoW Protocol's batch auctions](https://docs.cow.fi/cow-protocol/concepts/introduction/batch-auctions) are the closest thing the production ecosystem has to a working batch-auction settlement layer. The strength of this bet is that it tries to dissolve the rent surface rather than redistribute it. The weakness is that "to no one" usually turns out to mean "to whoever benefits from the new structure": information advantages migrate to whoever sees decryption keys first, latency advantages migrate to whoever sits closest to the decryption committee, solver edge migrates to whoever optimizes the batch-clearing solver hardest. Rent doesn't disappear; it moves into harder-to-measure places.

4. **To builders (status quo with extra steps).** This is the implicit bet of any design that pays operators a margin, regardless of whether the design markets itself as decentralized. If running infrastructure earns rent — even if the rent is split across many operators — the underlying claim is that MEV is a service fee and the people who deliver the service should be compensated.

Where does this report land? Honestly: the long-run target should be mostly (3), with some (2), and BuilderNet's excellence at (1) is the best available transitional architecture while (3) is being built and (2) is being shipped at the protocol layer. Each of these has different time horizons. (1) is shipping today. (2) is targeting Glamsterdam-and-after. (3) is partially shipping in the application layer (CoW's batch auctions, Anoma's resource machine in research) and is years away from being default at the protocol layer.

The criticism worth making of BuilderNet's framing is not that (1) is wrong — it is the best mitigation available for the architecture Ethereum actually has today. The criticism is that any infrastructure built on top of (1) accumulates incentives to make (1) permanent. Refund mechanisms are sticky. Operator infrastructure is sticky. Orderflow agreements are sticky. The architecture that makes (1) excellent is the architecture least incentivized to advocate for (3). That's not an accusation, it's just structural — and it's the thing the next two years of MEV-mitigation politics will turn on.

### What Does BuilderNet Look Like After FOCIL, Encrypted Mempools, and MEV Burn Ship?

The honest answer is: smaller, possibly redundant, and possibly still useful as defense-in-depth.

Each protocol-level fix shrinks a different rent surface that BuilderNet currently covers.

- [FOCIL (EIP-7805)](https://eips.ethereum.org/EIPS/eip-7805) reduces the *censorship* value of being a builder. Once a fork-choice-enforced inclusion list compels builders to include a committee-curated transaction set, the ability to selectively omit transactions stops being a builder-side lever. That removes one of the harder-to-measure components of builder rent — the value of being the entity that decides what doesn't get in.
- Encrypted mempools — surveyed in [Bormet et al.](https://arxiv.org/abs/2503.04977), with deployment proposals like [Shutter Network](https://shutter.network) and [SUAVE-derived threshold encryption schemes](https://writings.flashbots.net/towards-better-mev-staking) — reduce the *orderflow-extraction* value of being a builder. If the builder can't see transaction contents during the bidding window, the most lucrative extraction patterns (sandwich, front-run, predatory backrun) become structurally harder or impossible.
- [MEV burn](https://ethresear.ch/t/mev-burn-a-simple-design/15590) reduces the *rent* value of being a builder. If protocol-level burn captures a significant fraction of MEV before it reaches the builder-proposer payment, the margin available to builders for outbidding each other shrinks toward whatever is needed to cover real costs.

Each of these is targeted at a specific component of builder rent. Each ships on its own timeline. None of them, individually, eliminates the role of a builder — but the combination significantly reduces the surface area where a builder's choices have rent-extraction consequences.

The strategic question worth asking openly: *does an application-layer fix whose value depends on the rent surface existing have the right incentive to advocate for protocol-layer fixes that shrink that surface?* No specific evidence is being claimed here that Flashbots opposes any of FOCIL, encrypted mempools, or MEV burn — the public posture has been broadly supportive across all three. The question is structural, about long-run incentive alignment, not about anyone's stated position today.

The strongest framing for BuilderNet's position post-protocol-fixes is defense-in-depth. Protocol-level fixes ship slowly and partially; application-layer mitigations harden the layers that are exposed in the meantime. A TEE-attested shared builder is a meaningful safeguard even if FOCIL eliminates censorship rent, even if encrypted mempools eliminate extraction rent, and even if MEV burn eliminates a fraction of the residual rent. The combination is layered protection, not redundant work. That's the framing that ages well; the framing that doesn't age well is treating BuilderNet as the destination rather than as one layer of a stack still being built.

## The Endgame Question: Does Every Venue Roll Its Own Builder?

This is the most important question to sit with. The Flashbots framing in [Introducing BuilderNet](https://buildernet.org/blog/introducing-buildernet) is direct: "The core boilerplate of block building will become a public utility which supports this decentralized network." That sentence is verbatim and states the bet plainly.

If you take that bet seriously, the implication is that the *building* layer commoditizes — a small set of TEE-attested operators running rbuilder inside TDX VMs, with shared flow and a known refund rule, becomes the default substrate. Competition migrates:

- **Up the stack** to orderflow origination: who can route the most valuable flow into the shared builder, with the best privacy and the best execution quality. This is where PMMs, intent-protocol solvers, and frontends compete.
- **Down the stack** to inventory and hedge infrastructure: who can warehouse risk, hedge across venues, and clear flow at the tightest spreads. This is where market-makers earn, and it's a different business from running a builder.

The Beaverbuild migration is the strongest direct evidence for this trajectory. The Rsync deprioritization is consistent evidence on the other side — a builder operation that wasn't willing or able to make the migration exited rather than compete. Two of the three pre-BuilderNet centralized builders have folded into or out of the model. Titan remains the holdout, and the 7-day numbers show that Titan is still a real, profitable, standalone operation.

**The honest caveat.** No PMM has publicly committed to operating a BuilderNet node as of May 2026. Not [Bebop](https://docs.bebop.xyz/bebop/smart-contracts/pmm-rfq-smart-contract), not Hashflow, not 0x, not Native, not 1inch Fusion, not CoW. The public-utility thesis is *inferred* from builder-side behavior (Beaver in, Rsync out, the explicit Flashbots framing, the Phase 2 roadmap) — not from PMM-side adoption announcements. PMMs currently reach BuilderNet via the standard solver-and-bundle path, and that is unlikely to change without a specific reason for them to run a node rather than route flow.

Bebop is worth a brief note because the architecture is illustrative. Bebop is the [Wintermute-incubated PMM RFQ + JAM solver auction model](https://docs.bebop.xyz/bebop/bebop-api-pmm-rfq/bebop-trading-apis-comparison) — what I've called *bopAMM* — and it's a coordination-layer artifact rather than an architectural one. It does not run its own builder. It could, in principle, plug into BuilderNet as a privileged orderflow provider with co-located simulation; nothing has been announced.

The operator economics are the unresolved variable. If "no economic benefit to running a node" remains true through Phase 2's permissionless expansion, the model is structurally aligned with the public-utility framing — operators run because they want strategic positioning, not because they want builder margin. If operators start being paid out of the refund pool to keep the network funded, the model drifts back toward an oligopoly with extra cryptographic ceremony. Watch the operator-pay line.

## Imperative vs Declarative — Where This Fits

BuilderNet is the canonical *imperative coordination layer* answer to MEV. The shape of the bet is: keep the current Ethereum architecture (proposer-builder separation, mempool-style orderflow, builder-constructed blocks), and *replace the trusted builder with a TEE-attested shared one*. Push rent up to PMM pricing and orderflow origination. Don't try to dissolve the scheduling-rent surface; route it to the people who actually generate value.

The core power claim worth stating directly: **in an imperative system, the scheduler has rent because the scheduler chooses.** Whoever decides which transactions go where, in what order, with what surrounding state, captures the option value of those choices. That is the rent surface MEV-Boost separated from validators and that builders inherited. BuilderNet shares the scheduling role across a TEE-attested operator set, which makes the choices more constrained and the rent more transparent — but the rent surface is still the same surface. Scheduling rent exists *because the scheduler chooses*. Distributing the scheduler doesn't eliminate that property; it spreads who benefits from it.

A *declarative architecture* changes the underlying claim. In a declarative system — [Anoma's resource machine](https://anoma.net/anoma/intents) is the most worked-out example, with [Vitalik's note on intent-centric protocols](https://vitalik.eth.limo/general/2023/06/09/superchain.html) and the broader [intent-centric design literature](https://writings.flashbots.net/the-future-of-mev-is-suave) as adjacent work — users express what they want to be true after state transition, not which transactions to execute. Ordering becomes a side effect of *predicate satisfaction* rather than a sequence the builder constructs. The rent surface, if it exists, is the predicate-satisfaction market, not the transaction-ordering market. That market can be made competitive in ways the imperative scheduler cannot, because predicate satisfaction is something many parties can attempt in parallel — there is no privileged sequencing role.

Two existence proofs sit just outside Anoma's framing. [SUAVE's original framing](https://writings.flashbots.net/the-future-of-mev-is-suave) was effectively a half-declarative version of this — preferences are expressed, executors compete to satisfy them — though it kept builder-side construction as the settlement step. [CoW Protocol's batch auctions](https://docs.cow.fi/cow-protocol/concepts/introduction/batch-auctions) are the closest thing in production to declarative-ish settlement at scale: solvers compete in a sealed auction to maximize trader surplus, ordering is a side effect of solver choice rather than a primary lever, and the protocol pays the winning solver only the marginal value they provided. Order-fair consensus is the protocol-layer cousin — [Kelkar et al.'s Order-Fairness for Byzantine Consensus](https://eprint.iacr.org/2020/269) and [Themis](https://eprint.iacr.org/2021/1465) lay out what it would mean for consensus itself to enforce a fairness property on ordering, dissolving a different piece of the rent surface at a different layer.

The symmetric honest point: **declarative systems have their own centralization risk.** The solver set, the predicate-satisfaction market, the operator of the matching engine — these are concentration vectors of their own, and there is nothing about being declarative that makes them automatically competitive. CoW's solver set has historically been small. Anoma's resource-machine solver/satisfier market is research-stage and its competitive dynamics are unproven. The honest framing is that declarative architectures **relocate** rent rather than dissolving it — moving the rent surface from "who constructs the block" to "who satisfies the predicates" — and the relocation may or may not produce better power distribution. It depends on whether the predicate-satisfaction market is structurally more competitive than the block-construction market, which is an empirical question about specific architectures, not a property that comes for free with declarativeness.

These are not zero-sum. BuilderNet improves the current market — it makes the imperative pipeline more honest, more verifiable, more competitive on the margin that matters. Declarative architectures bet that you can change the abstraction the market runs on top of. Both are coherent answers to "what do you do about MEV." They have very different time horizons and very different evidentiary standards.

There's also a third axis worth naming: protocol-level changes. [FOCIL / EIP-7805](https://eips.ethereum.org/EIPS/eip-7805) targets the Hegota upgrade (H2 2026), having been moved off Glamsterdam after the Base team warned that bundling FOCIL with ePBS risked delaying the upgrade beyond 2026. ePBS targets Glamsterdam, currently scheduled for H1 2026 (May/June) with possible slip to Q3 if testing reveals issues. Vitalik's broader push for [Big FOCIL and encrypted mempools as anti-centralization primitives](https://www.theblock.co/post/391840/vitalik-buterin-eyes-big-focil-and-encrypted-mempools-to-prevent-centralization-in-block-building-pipeline) is the protocol-layer parallel to BuilderNet's coordination-layer answer. MEV burn proposals continue to circulate. Each of these reshapes what builders can do at all — and to Flashbots' credit, the BuilderNet design is largely compatible with all of them. A shared TEE-attested builder still works under FOCIL-style inclusion lists; it still works under encrypted-mempool decryption; ePBS changes the proposer interface but not the builder interior.

The architecture is layered. BuilderNet sits at the imperative-coordination layer. Declarative intent systems sit at the application-architecture layer. Order-fair consensus and protocol-level MEV interventions sit at the consensus layer. They can coexist; they can even compose; whether they will is downstream of decisions still being made.

## Closing

BuilderNet is serious engineering. The shared-orderflow design is right, the marginal-contribution refund rule is genuinely novel, the operator-pays-nothing model is internally consistent with the public-utility framing, and the reproducible-build discipline against published measurements is the kind of operational rigor that the rest of the MEV ecosystem should be benchmarking against. The TEE bet is load-bearing and the TEE.fail attack is credible — and the Flashbots response (Mind the Gap, DCEA, the push for Intel POE) is the most candid public-facing engagement with TEE limits that any team in this space has produced.

The risk to the ecosystem is treating BuilderNet as *the answer* rather than *a stage*. The 7-day data is the clearest version of this: top-three builders still account for ~90%+ of blocks; Titan still builds half of all MEV-Boost blocks as a standalone operation; Quasar's headline market share is partly funded by subsidies; BuilderNet's number-two slot is genuine but is not by itself evidence of a decentralized block-building *market*. "Decentralized builder network" and "decentralized block building market" are not the same property, and the second is the harder one.

The endgame Flashbots is openly betting on — *the core boilerplate of block building will become a public utility which supports this decentralized network* — is a coherent and serious bet. The evidence so far is partially supportive (Beaverbuild migrated in, Rsync stepped out, the refund rule pays orderflow contributors instead of operators) and partially unresolved (no PMM has committed to operating a node, the top-three concentration has moved sideways rather than down, operator economics in Phase 2 are an open question).

The sharper question worth carrying forward, though, is what comes *after* the building layer commoditizes. If Flashbots' bet lands and the box that builds the blocks becomes a public utility — TEE-attested, shared, refund-aware, operator-cost-only — the rent surface doesn't disappear. It moves. The honest candidates for where it moves next are visible in the architecture already: the measurement registry, whose signer decides what counts as "BuilderNet code"; the operator-admission process, whose criteria decide who gets to participate; and the orderflow-origination layer, where PMMs and intent solvers and frontend operators sit between end-users and the shared builder. Each of those is a candidate next rent surface, and each has its own concentration story that the current "three operators in a shared network" framing doesn't make legible.

That's the open question worth carrying forward. A measurement-registry signer, an operator-admission committee, and an orderflow-origination layer dominated by a handful of PMMs *may or may not* be a better power distribution than "two builders that everyone could name." That's not rhetorical. It's a real question, with a real answer, that depends on how those three surfaces are designed and who participates in them. The decentralized-builder-network property is a meaningful first move. The decentralized-block-building-market property is the harder, more important one, and the most interesting work of the next two years is in surfaces the current architecture has barely started to think about.

Imperative coordination through shared TEE building is one architecture. Declarative intent systems are another. Protocol-level changes — FOCIL, encrypted mempools, MEV burn, order-fair consensus — are a third. The interesting question for the next eighteen months is not which one of these wins, because they're not competing for the same surface. The interesting question is which combination ships, and whether the layering produces a market structure that actually distributes rent and resists concentration, or one where the names of the concentrated entities change while the structure doesn't.

The work BuilderNet has done is the kind of work the ecosystem needs more of: openly engineered, openly attested, openly critiqued, and shipped. That is worth more than a clean answer.

Source material verified by parallel supervisor audits 2026-05-19.
