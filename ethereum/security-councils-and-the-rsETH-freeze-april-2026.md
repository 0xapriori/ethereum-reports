---
title: "Security Councils and the rsETH Freeze: How Much Power Does an L2 Council Actually Have?"
date: 2026-04-27
---

# Security Councils and the rsETH Freeze: How Much Power Does an L2 Council Actually Have?

*A synthesis report written by the apriori-writer agent. | April 2026*

## tl;dr

- **ON APRIL 21, 2026, THE ARBITRUM SECURITY COUNCIL FROZE 30,765.67 ETH ON ARBITRUM ONE BY ATOMICALLY UPGRADING, ACTING AS, AND REVERTING THE L1 DELAYED INBOX** — the first time a production rollup council exercised override authority over individual user assets, and the moment the question "how much power does a security council actually have?" stopped being academic.

- **THE FROZEN FUNDS BELONGED TO AN EXPLOITER, NOT INNOCENT USERS** — the common framing that Arbitrum "froze user funds" is wrong. The freeze targeted the bridged proceeds of an attack that originated on Ethereum mainnet, where a compromised LayerZero 1-of-1 DVN released 116,500 unbacked rsETH (~$292M) via a phantom Unichain burn message; the council's action confiscated the laundering output, not the underlying mint.

- **NO LIVE GENERAL-PURPOSE ROLLUP IS AT L2BEAT STAGE 2 AS OF APRIL 2026** — every production L2 sits at Stage 0 or Stage 1, which means every production L2 has a council with authority to override the rollup's stated trust model under defined conditions. The April 21 freeze did not violate Stage 1; it made visible what Stage 1 always meant.

- **ON ZK ROLLUPS, THE COUNCIL IS THE PROOF SYSTEM** — validity proofs prevent the *sequencer* from committing invalid state, not the *upgrade key holder* from replacing the verifier with a permissive one. Every live ZK rollup's emergency multisig path can swap the verifier with no or short delay. The council's signing threshold is the real security model on these chains, structurally inverted from an OP rollup challenge window.

- **THE SAME ACTION ON OTHER STACKS WOULD HAVE LOOKED MECHANICALLY DIFFERENT BUT POLITICALLY IDENTICAL** — OP Stack chains route emergency upgrades through a 10-of-13 council and a 2-of-2 nested Safe; zkSync's EmergencyUpgradeBoard is 3/3 with zero delay; Linea's 5-of-9 ConsenSys council is the most concentrated control surface among major rollups; Scroll has *proposed* dissolving its council in favor of a smaller "Scroll Admin" multisig as of April 14 2026. The mechanism varies; the legitimacy boundary does not.

- **THE COUNCIL IS A MULTISIG WITH THE TECHNICAL AUTHORITY TO OVERRIDE THE ROLLUP'S STATED TRUST MODEL WHEN IT JUDGES THAT MODEL WOULD OTHERWISE PRODUCE AN ILLEGITIMATE OUTCOME** — Stage 1 explicitly permits this. The deeper truth is that decentralization in 2026 is not a property of any live L2; it is a roadmap. The councils exist precisely because no production rollup has yet built a proof system, governance, and exit mechanism trustworthy enough to operate without one.

- **STAGE 2 DOES NOT ACTUALLY RETIRE INSTANT-ACTION AUTHORITY** — L2Beat's framework explicitly carves out instant upgrades for "adjudicable on-chain bugs," with no timelock and no exit window. Because the trigger is community-defined and the action is instant, the volume of "bug interventions" is structurally likely to rise over the next 5-10 years rather than fall. The honest endpoint, following Polynya's Stage 3 framing, is council dissolution — not council narrowing. High-value applications that depend on credible neutrality belong on a chain whose security council does not exist.

---

## Table of Contents

1. [Premise — The Question That Stopped Being Academic](#1-premise--the-question-that-stopped-being-academic)
2. [How Rollups Work and What "Stage" Means](#2-how-rollups-work-and-what-stage-means)
3. [What Actually Happened — The KelpDAO/LayerZero/Arbitrum Incident](#3-what-actually-happened--the-kelpdaolayerzeroarbitrum-incident)
4. [The Arbitrum Security Council in Practice](#4-the-arbitrum-security-council-in-practice)
5. [The Same Action on Other Stacks — A Counterfactual](#5-the-same-action-on-other-stacks--a-counterfactual)
6. [Closing — What the Council Actually Is](#6-closing--what-the-council-actually-is)
7. [The Adjudicable-Bug Loophole and the Case for Stage 3](#7-the-adjudicable-bug-loophole-and-the-case-for-stage-3)
8. [Sources](#8-sources)

---

# 1. Premise — The Question That Stopped Being Academic

How much power does a security council actually have?

For most of the rollup era the answer was theoretical. Council documents named thresholds and roles. L2Beat's stage framework named the conditions under which a council could and could not act. The councils themselves rotated cohorts, signed routine upgrades, and were the kind of governance surface that crypto Twitter ignored except in audit threads. The question of what they could actually *do* — what an emergency action looked like when it pointed at a specific user's balance instead of a code path — was the kind of question you could let sit, the way you let counterfactuals about deposit insurance sit during a calm decade for banks.

The question stopped being academic on the night of April 21, 2026. At approximately 23:26 ET, the Arbitrum Security Council executed an Emergency Action that froze 30,765.667501709008927568 ETH on Arbitrum One — the laundered proceeds of the rsETH exploit that had drained the LayerZero/Kelp OFT lockbox on Ethereum mainnet three days earlier. The mechanism was an atomic upgrade of the L1 Delayed Inbox to add a function called `sendUnsignedTransactionOverride`, a single use of that function to impersonate the exploiter and send the ETH to the ArbSys precompile burn-style address, and an immediate revert of the Inbox to its prior implementation. The total elapsed time of the new Inbox code being live was the duration of a single L1 transaction.

The action was technically clean, legally unprecedented, and politically loud. Within hours the framing diverged: defenders called it a legitimate use of Stage 1 emergency powers against an attacker; critics called it the moment Arbitrum proved it could freeze any address it wanted. Both readings depend on the same underlying fact, which is that the council did, in production, on a chain with $2.8B in DeFi TVL ([state-of-ethereum-l2-ecosystem-march-2026.md](/ethereum/state-of-ethereum-l2-ecosystem-march-2026.md)), exactly what the council documents have said for years it could do.

This report is about what the councils can do, what they cannot, and how the rsETH freeze exposed the actual shape of authority across the rollup landscape. The freeze is interesting not because of the dollar amount and not because of whether one finds it morally justified. It is interesting because the *mechanism* and the *legitimacy boundary* are the visible edges of a structure that defines every production L2 in the ecosystem — and because the same structure exists, with different thresholds and different delays, on every chain with a live council. The Arbitrum action is one realization of a power that is also held, today, by 13 signers on Optimism, 12 on zkSync, 9 on Linea, and so on through the list.

The deeper thing is that the question has only one honest answer: *the council can override the trust model when the council judges the trust model would otherwise produce an illegitimate outcome*. That is not a workaround or a betrayal; it is the design. Stage 1 is the explicit codification of that design. The work of the next several years is not to pretend the councils are smaller than they are. It is to build the proof systems, exit mechanisms, and governance such that the councils can be retired — Stage 2 — without the chain becoming worse.

# 2. How Rollups Work and What "Stage" Means

A rollup is a blockchain whose security inherits from Ethereum but whose execution sits elsewhere. Vitalik's framing in [*Different types of layer 2s* (2023)](https://vitalik.eth.limo/general/2023/10/31/l2types.html) is the clean version: rollups post their state and the data necessary to reconstruct it to Ethereum L1, and they prove (or, in the optimistic case, *can be challenged to prove*) that the state transitions they claim are valid. The chain that actually runs your transaction is some L2; the chain you trust is Ethereum. The bridge between the two is the proof system — fault proofs for an optimistic rollup, validity proofs for a ZK rollup.

That is the trust model on paper. In practice, every production rollup also has a *governance surface*: the contracts on L1 that hold the rollup's state root, the contracts on L2 that implement the precompiles and the bridge, and the keys that can upgrade either set. If those keys can change the verifier, the dispute logic, or the bridge, then the rollup's actual security depends on those keys as much as on the proof system. This is the gap that L2Beat's stage framework was built to make legible.

The stages, as defined by [L2Beat](https://l2beat.com/scaling/risk):

- **Stage 0** — training wheels. The proof system may not be live, may not be permissionless, or may not be the only path to L1 finality. The council (or the sequencer operator, or both) can override outcomes essentially at will. Users have no exit mechanism that does not depend on operator goodwill. The security model is "trust the team."

- **Stage 1** — fault- or validity-proven, with override authority. The proof system is live and adjudicates the canonical state. Users have a forced exit window if the sequencer becomes hostile or unavailable. *The council retains the authority to override the rollup's normal upgrade cadence in defined emergency conditions* — pause withdrawals, blacklist a dispute game, swap a verifier, or in Arbitrum's case execute an atomic-upgrade Emergency Action on the bridge. Stage 1 is the explicit acknowledgment that the proof system is correct enough to run normally and not yet correct enough to run alone.

- **Stage 2** — full decentralization. The council cannot override the proof system. Upgrades are subject to a delay long enough that users can exit before any change takes effect. The proof system is multi-implementation or otherwise resistant to the failure of any single prover.

**No live general-purpose rollup is at Stage 2 as of April 2026.** Every L2 with meaningful TVL — Arbitrum, Base, OP Mainnet, zkSync, Starknet, Linea, Scroll, Polygon zkEVM, Taiko, and the rest — sits at Stage 0 or Stage 1. ([L2Beat Risk Framework](https://l2beat.com/scaling/risk); [state-of-ethereum-l2-ecosystem-march-2026.md](/ethereum/state-of-ethereum-l2-ecosystem-march-2026.md))

Vitalik's [*Scaling Ethereum L1 and L2s in 2025 and beyond*](https://vitalik.eth.limo/general/2025/01/23/l1l2future.html) frames the same idea from the user's side: the question is not "is this rollup decentralized?" but "what is the worst thing the L2's governance can do to my funds, and how long do I have to exit before they can do it?" Stage 0 is "anything, no exit." Stage 1 is "narrow set of override conditions, with a forced-exit path under most failures." Stage 2 is "no override, exit always available."

Two additional details matter for the rest of this report. The first is that Stage 1 does not mean the council *is supposed to do nothing*. It means the council has authority to act in narrow, pre-defined ways, and the system is designed around the assumption that it sometimes will. The Arbitrum council's `sendUnsignedTransactionOverride` action is a Stage 1 action by construction — Stage 2 would have prohibited it. The second is that the *kind* of override authority varies sharply between optimistic and ZK stacks, and that asymmetry is most of what differentiates the cross-stack response on April 21.

The reader should leave this section with three things: training-wheels period (Stage 0), council can override but users can exit under most conditions (Stage 1), council cannot override the proof system (Stage 2). And the working assumption, true in April 2026, that no production L2 is operating at Stage 2.

# 3. What Actually Happened — The KelpDAO/LayerZero/Arbitrum Incident

There were three actors and three actions, on three days. The public framing has compressed them, and the compression is where the misreadings happen.

**Actor 1: Lazarus Group / TraderTraitor** (state-affiliated North Korean threat actor). On April 18, 2026 at ~17:35 UTC, the group compromised LayerZero Labs' RPC nodes and DDoS'd the external nodes that would have caught the discrepancy. They injected a phantom message into rsETH's LayerZero OFT path: a "burn" of 116,500 rsETH on Unichain that had never happened. Because rsETH used a 1-of-1 DVN on the route — a single verifier network, operated by LayerZero Labs itself — there was no second signature to disagree. The OFT lockbox adapter on Ethereum mainnet accepted the phantom burn as proof of source-chain supply destruction and *released* 116,500 rsETH from the lockbox, valued at ~$292M.

The unbacked mint happened on **Ethereum mainnet** — not on any L2. The Ethereum-side adapter released real, locked rsETH on the basis of a Unichain burn message that was never produced by an actual burn.

**Actor 2: The exploiter.** Within minutes, the attacker supplied ~89,567 rsETH to Aave V3 markets on Ethereum and borrowed ~82,650 WETH and 821 wstETH across seven markets. The borrows were never repaid; bad-debt estimates ran $123.7M to $230.1M depending on markets and price. The attacker moved proceeds across chains. A meaningful portion — the 30,765 ETH that would become the subject of the Arbitrum action — landed on Arbitrum One and was, at the time of the council's action, mid-withdrawal back to L1 via the native ArbSys precompile. The native bridge's 7-day challenge window between L2 initiation and L1 finalization is what made the Arbitrum action technically possible.

**Actor 3a: KelpDAO.** On April 20, the Kelp DAO multisig paused the rsETH OFT contracts on Ethereum and every L2 deployment, blacklisted known attacker addresses at the OFT level, and blocked a 40,000 rsETH follow-up queued in the bridge. Kelp's action did not require a security council; it was the protocol team using its own admin keys.

**Actor 3b: Arbitrum.** On April 21 at approximately 23:26 ET, the Arbitrum Security Council executed an Emergency Action that froze the 30,765.667501709008927568 ETH the exploiter had bridged to Arbitrum One. The mechanism is described in Section 4. The relevant fact for this section is that the Arbitrum freeze did not target the unbacked mint, did not target Kelp, and did not target the OFT path. It targeted the *exploiter's bridged proceeds* during a withdrawal window on Arbitrum's native bridge.

Arbitrum did not freeze users; Arbitrum confiscated an attacker's laundering output. The phantom mint was a LayerZero/Kelp problem on Ethereum mainnet. The bad debt was an Aave problem on Ethereum mainnet. Arbitrum's role in the incident began only when the proceeds touched Arbitrum's bridge.

The freeze transactions and full mechanism are documented in [Arbitrum forum thread #30803](https://forum.arbitrum.foundation/t/security-council-emergency-action-21-04-2026/30803).

# 4. The Arbitrum Security Council in Practice

Composition. The Arbitrum Security Council has 12 members organized in two cohorts of 6, on staggered 6-month election cycles, each member serving a 1-year term. Candidates require ≥0.2% of votable supply to qualify; the top six vote-getters in each election become the cohort. The March 2026 election was active during the rsETH incident. Names that have appeared in candidate lists — gzeon, John Morrow, zachxbt, Griff Green, Bartek Kiepuszewski, Michael Lewellen — are *candidates* and not all are confirmed sitting members; the full current 12-member roster has not been independently verified at the time of writing (per Arbitrum DAO governance documentation at docs.arbitrum.foundation).

Thresholds. Two distinct thresholds operate on different paths. The Emergency Action threshold is **9-of-12** signatures and skips the standard timelock. The non-emergency threshold is 7-of-12 and runs through a delay long enough to permit DAO contestation. The April 21 action used the 9-of-12 emergency path.

What they did, mechanically. The action is documented in Arbitrum forum thread #30803 and is one of the cleaner emergency-mechanism case studies in the rollup literature. The full sequence, in a single L1 transaction:

1. **Upgrade.** The L1 Delayed Inbox implementation was replaced with a modified version that added a single new function: `sendUnsignedTransactionOverride`. The function's distinguishing property, named in the upgrade comments verbatim, is "the ability to impersonate the transaction sender." A normal Inbox cross-chain message is sender-bound to the L1 caller; this function lifts that constraint.

2. **Act.** The council called `sendUnsignedTransactionOverride` to inject an L2 transaction *as if sent by the exploiter's address*, with a payload that sent the 30,765.667501709008927568 ETH on the exploiter's L2 balance to the ArbSys precompile address ending `...DA0`. ArbSys is the precompile through which native L2-to-L1 ETH withdrawals are initiated; sending ETH to the precompile address with the appropriate calldata is the canonical burn-style operation for native withdrawal mechanics.

3. **Revert.** The Inbox implementation was reverted to its prior bytecode in the same atomic transaction. The new function existed on-chain for the duration of one L1 block.

The key technical precondition that made this possible is that **the exploiter had already initiated a native L2→L1 ETH withdrawal**. That initiation had created the 7-day challenge window — the period between L2 finalization of the withdrawal and L1 finalization on the bridge. During that window, the L2 ETH balance is in a transient state where bridge-level intervention is mechanically expressible. Without an open native withdrawal, the same action would have required either a freeze of the entire chain or a more disruptive sequencer-level intervention. The exploiter's choice to use the native bridge, rather than a fast bridge or a same-chain cycle, is what made a clean Emergency Action possible. There is a small irony in this — the attacker chose the most trust-minimized exit path, and that path was also the one most amenable to council override.

Why it was politically possible (in the council's reasoning, as published): the Arbitrum DAO had previously authorized the Security Council to act in cases of exploit and theft; the funds were demonstrably the proceeds of a confirmed exploit; the exploiter was on multiple OFAC-relevant blocklists; a constitutional AIP would be filed, ex post, to release the frozen ETH into a recovery vehicle administered by Aave Labs, Kelp, and Certora as a 2-of-3 Safe. The action did not violate Stage 1; it used Stage 1's emergency surface within its stated bounds.

The Stylus precedent. Six months earlier, on October 13, 2025, the same council had executed a different Emergency Action — a 9-of-12 call to `setWasmMaxStackDepth(22000)` on the ArbOwner precompile, on both Arbitrum One and Arbitrum Nova. That action was triggered by a discovered vulnerability in the Stylus runtime; the council notified the community at 12:23 PM ET and executed the upgrade at 2:30 PM ET — ~2 hours from notification to execution. The Stylus Emergency Action set the precedent for the rsETH freeze. It also set the template: the council acts, then explains, then the DAO ratifies or contests after the fact. ([Arbitrum forum thread #30093](https://forum.arbitrum.foundation/t/security-council-emergency-action-10-13-2025/30093))

Two clarifications that matter for stage-framework consistency: Arbitrum One's BoLD permissionless validation went live in February 2025; L2Beat's Stage 1 designation came in January 2026. These are distinct events. BoLD is a proof-system change; Stage 1 is a holistic assessment that incorporates proof system, exit mechanism, and council authority bounds. The April 21 action took place on a chain that was, at the time, a recently-graduated Stage 1 rollup using a permissionless fault-proof system — and, simultaneously, a chain whose council retained explicit authority to override the proof system's outcome under emergency conditions. Both of those are true. Stage 1 is the conjunction. ([L2Beat Arbitrum project page](https://l2beat.com/scaling/projects/arbitrum); BoLD documentation at docs.arbitrum.io)

# 5. The Same Action on Other Stacks — A Counterfactual

The mechanical question is not whether the Arbitrum action was unique — it was specific to Arbitrum's bridge architecture and to the exploiter's choice of withdrawal path — but whether equivalent override authority exists on other stacks. The answer is that it does, with different mechanisms, different thresholds, and different exposed surfaces. The councils are not equally constituted. They are, however, all real.

## 5.1 OP Stack

The OP Stack — Optimism, Base, Mode, Zora, World Chain, Ink, Soneium, Unichain — runs a layered governance model that sits structurally between a rollup-of-rollups Superchain and a federation of independent chains. The relevant components:

- **Optimism Security Council**: 13 members, **10-of-13** (~77%) emergency threshold. Set under Protocol Upgrade #8, raised from a prior 4-of-13 threshold. The Council holds the **Guardian** role for the Superchain.
- **Optimism Foundation**: separate ~5-of-7 multisig. Holds the **DeputyGuardian** role via the **DeputyGuardianModule**.
- **Standard upgrade path**: 2-of-2 nested Safe — Council and Foundation both must sign. Either can veto.
- **DeputyPauseModule** (added in Upgrade 13, 2025): lives inside the Foundation Safe and allows a designated Pause Deputy key to call `pause()` on the Superchain. The Foundation rotates this key approximately every three months.
- **LivenessGuard / LivenessModule**: prunes inactive Council signers, preserves the 75% threshold; if active Council membership falls below 8, control reverts to the OpFoundationUpgradeSafe.
- **SuperchainConfig**: provides per-chain pause and a global pause (identifier `0x0`). All pauses auto-expire after 7,884,000 seconds (~91 days) — a hard upper bound on how long a Superchain pause can hold without an affirmative re-vote.
- **Powers granted to the Council in emergency**: pause `OptimismPortal` withdrawals, trigger global Superchain pause for ~91 days, blacklist *dispute games* (not output roots directly), swap `DisputeGameFactory`, fall back to `PermissionedDisputeGame`.

Permissionless fault proofs went live on OP Mainnet on June 10, 2024 — that is the change that conferred Stage 1, not 2025. In August 2024 the chain temporarily reverted to a permissioned game after a discovered vulnerability; Fjord (Jul 2024) and Granite (Sep 2024) shipped fixes, after which permissionless operation resumed (per Optimism's published fault-proof timeline). There is no public record of `OptimismPortal.pause()` ever being invoked in production.

Could OP "do an Arbitrum"? Mechanically, yes. The path would be: 10-of-13 Council signatures on an emergency upgrade, routed through the nested Safe, swapping the `OptimismPortal` or `DisputeGameFactory` for a contract that achieves the same effect — for instance, a Portal that explicitly burns or redirects withdrawals from a specified address. The threshold is higher (10-of-13 vs 9-of-12 in raw count, ~77% vs ~75% in proportion). The attack surface — the *bridge contract* — is conceptually equivalent. The political cost of doing it would be substantially higher because the same action would be visible on every Superchain inheritor simultaneously: OP Mainnet, Mode, Zora, World Chain, Ink, Soneium, Unichain.

**Base** is the interesting variant. Base launched its own 9-of-12 Security Council in April 2025 (~75% threshold). On February 18, 2026, Base announced migration off the OP Stack to its own `base/base` codebase, with Sepolia activation on April 20, 2026. The migration severs Base's shared-Guardian dependency on Optimism — an action equivalent to the rsETH freeze, executed against funds on Base, would soon route through Base's own council rather than Optimism's. Base reached Stage 1 on April 29, 2025.

The structural property worth naming is that the OP Stack achieves **separation of powers** in a way Arbitrum's single-council design does not: the standard upgrade path requires both Council and Foundation, and either side can veto. That makes routine coordination harder and emergency action slower, but it also means a hostile council majority cannot act unilaterally on routine upgrades — though it does still hold the Guardian role and can pause unilaterally. Whether that separation would have changed the rsETH outcome is a counterfactual that depends on whether the Foundation would have signed; both bodies are aligned closely enough on attacker-funds confiscation that the answer is almost certainly yes.

## 5.2 ZK Stacks

The ZK rollup landscape is where the answer to "what could the council do?" diverges hardest from the OP Stack picture. The reason is the proof-bypass asymmetry, addressed in 5.3. First, the inventory.

**zkSync** runs a three-body governance: a Token Assembly that performs ZK Token Protocol Proposals (TPPs); a Security Council of 12 members (≥9 of 12 for hard freeze and unfreeze); and a Guardians council of 8 members (≥5 of 8, holding 4/9 veto authority over Token Assembly TPPs). The standard upgrade path runs through a 3-hour batch delay, a 3-day legal veto period, and multiple subsequent timelocks. The fast path is the **EmergencyUpgradeBoard** — a 3/3 multi-body Safe consisting of Security Council, Guardians, and the Foundation, with **zero delay**. TPP-14 / GAP-4 (November 2025) reduced the standard voting delay from 7 days to 3 days. The ZK Stack tenants — Cronos zkEVM, ZERO, GRVT, zkCandy — inherit governance from this structure. zkSync remains at Stage 0 ([L2Beat — ZKsync Era](https://l2beat.com/scaling/projects/zksync-era)).

**Starknet** has a Security Council of 12 members (9-of-12) with **instant** upgrade authority, plus a separate StarkWare Multisig (2/4) with an 8-day delay. The SHARP Verifier — recently transitioned from Stone to **Stwo** on November 5, 2025 — is governed by a *separate* SHARP Multisig (2/4) with an 8-day delay. The SHARP multisig matters out of proportion to its size: it governs the verifier used by *every rollup running on SHARP*, which is a correlated risk surface across multiple StarkWare-based chains. Starknet has been at Stage 1 since May 2025 ([L2Beat — Starknet](https://l2beat.com/scaling/projects/starknet)).

**Linea** (ConsenSys) runs its Security Council under a Zodiac Roles module: 5-of-9, with the first cohort onboarded in April 2025 — operational, not "forming," by April 2026. There is a separate 3/5 multisig path with a 3-month delay module. The sequencer and prover are ConsenSys-operated. The Council can pause for 2 days plus a 2-day cooldown. There is a 6-month inactivity fallback. Linea remains at Stage 0. **Linea's 5-of-9 ConsenSys-controlled multisig is the most concentrated control surface among major ZK rollups**: a 5-of-9 majority that is institutionally aligned with a single corporate parent ([L2Beat — Linea](https://l2beat.com/scaling/projects/linea)).

**Scroll** — first ZK rollup to reach Stage 1, via the Euclid upgrade in April 2025 — had a 12-member Security Council with a 9/12 threshold and 7 independent members. On **April 14, 2026** a governance proposal was filed to *dissolve the Council* and transfer upgrade powers to a smaller "Scroll Admin" multisig within ~10 days. Execution status is unverified at the time of writing; the proposal had not been ratified by April 27. The direction matters: at least one ZK rollup is moving from a larger council toward a smaller multisig, on the stated rationale that the larger council had become a coordination bottleneck. The opposite direction is more typical of rollup decentralization roadmaps. Scroll's pending change is an outlier, and its outcome will be a useful signal ([Scroll governance forum thread on Security Council transition](https://forum.scroll.io/t/governance-update-security-council-transition-contributor-roles-operational-adjustments/1470)).

**Polygon zkEVM** runs a 6-of-8 Security Council with a requirement that ≥4 signatures come from members *external* to Polygon. Standard upgrades use a 10-day timelock with a 2/3 Admin. The emergency state instantly removes the timelock. The chain has a *prior precedent* — on **March 23, 2024**, the emergency state was used after a roughly 10-hour outage caused by a sequencer / Ethereum-reorg interaction. Polygon zkEVM Mainnet Beta has had its sequencer **sunset announced for July 1, 2026** (announced June 2025); Polygon's strategic focus has shifted to AggLayer. Stage 0 ([L2Beat — Polygon zkEVM](https://l2beat.com/scaling/projects/polygonzkevm)).

**Taiko Alethia** is a based rollup with a Security Council launched in April 2025: 8 members, ≥6 of 8 (75%) for emergency. The Based Contestable Rollup (BCR) uses multi-proof validation, which raises the bar for any single proof system being suborned. The combination — multi-proof BCR plus a 75% council — is among the more conservative configurations among live rollups ([L2Beat — Taiko Alethia](https://l2beat.com/scaling/projects/taiko)).

**Other ZK / Validium chains worth naming**: Mantle runs a ~6/14 Security Multisig plus a 3/7 Engineering multisig. Manta Pacific is 5/7, Validium, with no live fault proofs. X Layer (OKX, Polygon CDK) is 5/9 with a permissioned sequencer.

## 5.3 The Proof-Bypass Asymmetry

Here is the property the OP Stack/ZK comparison makes legible, and which the rsETH incident did *not* directly test but which would govern any equivalent ZK incident:

**On every live ZK rollup, the council can swap the verifier for a permissive one.**

A validity proof prevents the *sequencer* from committing invalid state. It does not prevent the *upgrade key holder* from replacing the verifier with a contract that accepts arbitrary inputs. The sequencer can be perfectly honest, the prover can produce a perfectly valid SNARK, and the upgrade key holder can still change the on-chain contract that decides what counts as "valid" on the next state root. The council's signing threshold *is* the real security model on these chains.

This is structurally inverted from an OP rollup. On an OP chain, the council can blacklist a dispute game or revert to a permissioned game, but the proof system itself — the challenge protocol — is harder to circumvent in a single transaction; the council must wait out a forced-exit window or persuade the chain to accept a new game definition. On a ZK chain, the council can replace the verifier in a single upgrade, and any state root submitted afterward will be "valid" by the new verifier's lights. A 9-of-12 council with zero delay can, in principle, finalize any state on the L1 contract.

| Stack | Emergency threshold | Delay | Proof system | Proof bypass via upgrade? |
|---|---|---|---|---|
| Arbitrum One | 9-of-12 | none (atomic) | BoLD permissionless | Bridge-level override demonstrated April 21 2026 |
| OP Stack (Optimism) | 10-of-13 + 2-of-2 nested Safe | minimal (Council + Foundation) | Permissionless fault proofs | Yes, via swap to PermissionedDisputeGame |
| Base | 9-of-12 (own council) | minimal | Inherits via OP, migrating | Yes |
| zkSync Era | EmergencyUpgradeBoard 3/3 | **zero** | Validity (Boojum) | Yes, verifier swap |
| Starknet | 9-of-12 instant + SHARP 2/4 (8d) | instant for SC; 8d for SHARP | Validity (Stwo) | Yes, verifier swap; SHARP risk shared |
| Linea | 5-of-9 (ConsenSys) | minimal | Validity (Plonk) | Yes |
| Scroll | 9-of-12 (proposal: dissolve to "Scroll Admin") | minimal | Validity | Yes |
| Polygon zkEVM | 6-of-8 (≥4 external) | instant in emergency state | Validity | Yes |
| Taiko Alethia | ≥6-of-8 | minimal | Multi-proof BCR | Higher bar (multi-proof) but still upgradeable |

The caveat. This is *not* a universal property of ZK rollups in principle. A ZK rollup with an *immutable* verifier — one whose contract address is hard-coded and whose bytecode cannot be replaced — would not be bypassable by the upgrade key holder. **DeGate V1 historically reached Stage 2 with an immutable verifier**, the only such case in the ZK rollup category; it has since been archived. No live general-purpose ZK rollup has an immutable verifier as of April 2026. For everyone else, the council's signing threshold is the binding security parameter on what the chain can be made to attest.

This is the part of the picture that a cross-stack reading of the rsETH freeze brings into focus. On Arbitrum, the council's bridge-level override authority was the surface used. On a ZK rollup, the equivalent surface — for an attack large enough to motivate one — would have been the verifier itself. The freeze would not have looked the same; it would not have used `sendUnsignedTransactionOverride` because there is no equivalent on a ZK bridge. It would have looked like a verifier swap or a forced-state-root submission, with the same political character and a different mechanical signature. The legitimacy boundary is identical. The technical exposure is, if anything, larger on ZK chains, because the proof bypass is a single contract pointer rather than a multi-week game.

# 6. Closing — What the Council Actually Is

The question this report opened with was: *how much power does a security council actually have?* The honest answer, after the rsETH freeze, is the one the council documents have stated all along.

**The council is a multisig with the technical authority to override the rollup's stated trust model when the council judges that model would otherwise produce an illegitimate outcome.** That sentence carries the entire thing. It is the design, not a deviation from the design. Stage 1 — every production L2's actual classification — explicitly permits override authority within defined emergency conditions. The April 21 freeze did not violate Stage 1 invariants; it *made visible* what Stage 1 always meant.

A few observations follow from this that are worth holding distinct.

The first is that the framing of the freeze as "Arbitrum confiscated user funds" is materially wrong, but the framing of the freeze as "Arbitrum proved it could confiscate any user's funds" is materially right. Both can be true simultaneously, and the discomfort of holding both is the discomfort the stage framework was designed to make legible. The exploiter's funds were targeted; that is the *use case* the council had pre-authorization to act on. The mechanism that targeted them — atomic upgrade-act-revert of the L1 Delayed Inbox, sender impersonation, ETH redirection to ArbSys — is the same mechanism that could target any other address. The council's authority is structural; what it points at on a given day is discretionary. That is the trust assumption, and Stage 1 is the open acknowledgment that the trust assumption is in force.

The second is that the response on other stacks, in a counterfactual rsETH-equivalent incident, would have been *mechanically* different and *politically* equivalent. OP Stack chains would have routed through a 10-of-13 + 2-of-2 nested Safe. zkSync would have used the EmergencyUpgradeBoard 3/3 with zero delay. Linea would have used 5-of-9 with the most concentrated single-organization majority among major rollups. The thresholds vary; the existence of the override authority does not. A reading of the cross-stack picture that searches for which rollup is "actually decentralized enough" to have refused to act ends without a clean answer, because every rollup is engineered to be capable of acting under sufficiently-bad conditions. The councils exist precisely to be capable of it.

The third is that the proof-bypass asymmetry on ZK rollups is the part of this picture that is most under-discussed in mainstream coverage and most consequential for the next several years. *On every live ZK rollup, the council can swap the verifier.* Validity proofs are not a substitute for governance trust; they are a substitute for sequencer trust. The verifier is itself a contract, and on every chain except a hypothetical Stage 2 ZK rollup with an immutable verifier, that contract is an upgrade target. Anyone treating ZK rollup security as *cryptographic* — proof-system-rooted — without reading the proof system as also *governance-rooted* (verifier-upgradeable) is making a claim the contracts do not back.

The fourth is the deepest and the one that connects to the other reports in this series. *Decentralization in 2026 is not a property of any live L2; it is a roadmap.* Vitalik's [*Scaling Ethereum L1 and L2s in 2025 and beyond*](https://vitalik.eth.limo/general/2025/01/23/l1l2future.html) and his earlier [*Different types of layer 2s*](https://vitalik.eth.limo/general/2023/10/31/l2types.html) both name this directly: the question is the trajectory from Stage 0 to Stage 2, with Stage 1 as the period in which the chain is correct enough to run normally and not yet correct enough to run alone. The councils exist because no production rollup has yet built a proof system, governance, and exit mechanism trustworthy enough to operate without one. They are not a betrayal of the rollup vision; they are the artifact the vision *requires* during the period when the proof systems and exit mechanisms are still being made correct. The work of getting to Stage 2 — multi-proof, immutable verifier, sufficiently-long delay, robust forced exit — is the work of making the council retirable without the chain becoming worse.

The corollary is that the legitimacy of any individual emergency action is, at the point of the action, a judgment call. The council ratified itself by being right (about the exploit) and procedurally clean (one atomic transaction, public mechanism, ex post DAO ratification). A council that froze the wrong funds, or a council that froze the right funds with a more disruptive mechanism than necessary, would be a different story. The present body of evidence — Polygon zkEVM's March 2024 emergency, Arbitrum's Stylus precedent in October 2025, the rsETH freeze in April 2026, Optimism's pause-never-invoked record — points to councils that have, so far, acted within the bounds their stated authority defined. The base rate is three emergency actions in roughly thirty months of meaningful Stage 1 operation across the major stacks, each targeting either an attacker-controlled outcome or a chain-level vulnerability, each ratified by the relevant DAO or governance body after the fact. That is a thin record. It is not a guarantee.

The rsETH freeze is not the moment the L2 ecosystem became something other than what it was. It is the moment the public discourse caught up to what the contracts already said. The Arbitrum council had this authority on April 1; it had it on April 21; it has it now. The same is true of every major L2's council. What changes from April 22 forward is not the contracts but the *attention* — the recognition that the override surface is real, that it is not symmetric across stacks, and that the question of when the override is legitimate is the central political question of the rollup ecosystem.

The work, then, is the work the strawmap and the immutable-applications report and the L2 ecosystem reports have all pointed at from different angles. Build the proof systems that don't need a council. Build the exit mechanisms that don't depend on the council's good behavior. Build the application-layer immutability that doesn't inherit the chain's governance. *Stage 2 is not a slogan; it is the binding constraint on whether any of the rollup-centric architecture can credibly be called decentralized in a decade.* The councils exist because the work is not done. The councils will exist for as long as the work is not done.

Whether they will continue to exist after that is a question with one honest answer, which is that no rollup has yet committed to retiring its council on any specific date. Until one does, the question this report opened with — *how much power does a security council actually have?* — has the answer it had on April 22, which is the answer it has had since the first council was constituted: **enough**.

---

# 7. The Adjudicable-Bug Loophole and the Case for Stage 3

The closing argument above lands the report at a comfortable place — Stage 2 as the destination, the council as the artifact of an unfinished journey, the work being to make the council retirable. That framing is incomplete. It is incomplete because Stage 2, as currently defined, does not actually retire the council's instant-action authority. It narrows the *trigger* and constrains the *answer-space*, but the council still holds keys, and those keys can still execute upgrades in zero seconds. The honest reading is harder than the closing pretended, and the harder reading points at a destination beyond Stage 2.

**The Stage 2 carve-out is explicit.** L2Beat's stages framework, in its own published methodology:

> *"One exception that we make is given the existence of a on chain bug detection system (e.g. two valid contradicting zk proofs) instant upgrades are allowed for detected bugs."*

So Stage 2 has two upgrade paths. Normal upgrades require a 30-day exit window — long enough that any user who disagrees with the change can withdraw before it takes effect. The adjudicable-bug path requires no delay at all. ([L2Beat — Stages framework](https://l2beat.com/stages))

There are three problems with this carve-out, and the user-facing impact of each compounds on the others.

**Problem 1: the trigger is community-elastic.** "Adjudicable on-chain bug" is a category whose contents are defined by social consensus, not by the contract. The Polygon zkEVM example — Emergency Mode triggered when two valid proofs can be submitted for the same batch — is the strict reading: a literal cryptographic contradiction, machine-detectable, deniable by no one. North Korea attribution is the loose reading: depends on Chainalysis heuristics, OFAC designations, intelligence-community judgment, and prosecutorial discretion. These are categorically different things. A Stage 2 rollup whose community accepts the loose reading is, functionally, a Stage 1 rollup with a marketing budget. The boundary between the two readings is *not* enforced by the protocol. It is enforced by whatever the council and the surrounding social layer agree counts as a bug at any given moment.

**Problem 2: the action is instant.** Once a "bug" is declared, the council can upgrade contracts in a single block. There is no exit window. There is no period during which a user who disagrees with the bug-classification can withdraw their assets before the override takes effect. Polygon zkEVM's documentation states this directly: *"Instead of employing the 2-out-of-3 Admin Multisig Contract and waiting for the time-delay imposed by the Timelock Contract, the Security Council Multisig may activate the emergency state to bypass such time-delay."* The 10-day standard timelock, gone. This was used in production once already, on March 23, 2024. ([L2Beat — Polygon zkEVM](https://l2beat.com/scaling/projects/polygonzkevm))

**Problem 3: Vitalik's published work is silent on the speed question.** *Different types of layer 2s* (October 2023) constrains the trigger (*"security council can only act in case of provable bugs"*) and the answer-space (*"it can only choose between one of the proposed answers; it cannot answer arbitrarily"*) but says nothing about whether the action itself should be timelocked. *Scaling Ethereum L1 and L2s in 2025 and beyond* (January 2025) does not address Stage 2 at all. *The math of when Stage 1 and Stage 2 make sense* (May 2025) models failure probabilities but specifies no delay parameter for emergency action. So the question of whether the bug-path should be subject to a forced exit window is, in the published literature, unsettled. ([Vitalik, *Different types of layer 2s*, 2023](https://vitalik.eth.limo/general/2023/10/31/l2types.html); [*Scaling Ethereum L1 and L2s in 2025 and beyond*, 2025](https://vitalik.eth.limo/general/2025/01/23/l1l2future.html))

**The drift trajectory follows mechanically from these three together.** If any transaction by any author can be classified as an adjudicable on-chain bug, and the classification is community-defined, and the action is instant, then the volume of "bug interventions" is going to rise over the next 5-10 years rather than fall. The drift is not driven by bad faith; it is driven by the structure of the carve-out:

- Sanction lists expand. Each new entity creates a new class of fund-flows the council has authority over the moment a community accepts those flows as bug conditions.
- Probabilistic attribution gets cheaper. Chain-analytics firms publish more granular heuristics; "likely DPRK" or "likely state actor" becomes a credible bug-trigger; the threshold for action drifts downward because the cost of a false negative feels worse than the cost of a false positive.
- Pre-attribution pressure builds. States, regulators, and stakeholders demand intervention *before* funds settle. The 7-day withdrawal window on a native bridge, or the 30-day exit window on a normal upgrade, becomes a surveillance window in which the council is expected to act.
- Adjacent categories accrete. Mixers, privacy tools, "high-risk" jurisdictions, and eventually any address pattern a sufficiently determined classifier flags. Each precedent makes the next one easier.

The endpoint is not difficult to describe: a Stage 2 rollup whose council adjudicates dozens of "bugs" per year, most of which are not cryptographic contradictions but policy-classifications, is a rollup that has decided to be more like a payment processor than like Ethereum. There is nothing structurally preventing this trajectory under the current framework. The Stage 2 designation does not constrain it.

**Polynya's Stage 3 framing — dissolve the council.** [Polynya](https://polynya.mirror.xyz/) has, across multiple essays, made the argument that high-value applications belong on a chain whose Security Council does not exist at all. The argument is not that councils are illegitimate during the proof-system maturation phase — it is that for assets where the trust assumption needs to be *cryptographic* rather than *political*, the council is a centralization vector that must eventually be removed entirely. A multi-prover setup, an immutable verifier, sufficient delay on every upgrade path, and no privileged signers: that is the destination. The chain becomes, in the limit, a pure execution surface whose state is whatever the proofs say it is, full stop. The Polygon zkEVM Emergency Mode trigger — auto-activation on contradicting proofs, no human in the loop — gestures at the right shape, but the surrounding upgrade authority does not.

A Stage 3 chain, in this framing, is a chain whose properties are *inherited* from Ethereum L1 in a way no Stage 2 chain currently is. Ethereum L1 has no multisig with the authority to redirect ETH on the basis of attribution. That absence is not an oversight; it is the condition that makes ETH a credibly neutral asset. A Stage 2 chain with a council that holds adjudicable-bug authority is not credibly neutral in the same sense. The asset on it inherits the council's discretion, not Ethereum's neutrality.

**The honest restatement of the closing.** The work is to move from Stage 1 to Stage 2, and then from Stage 2 to a state in which the council does not exist. For any application whose value depends on credible neutrality — sanctuary applications, immutable financial primitives, settlement-layer instruments, anything that wants to inherit Ethereum's properties rather than the L2 council's policy stance — Stage 2 is not the destination. Stage 2 is the second-to-last step. The last step is dissolution: no council, no privileged keys, no carve-out. The volume of council interventions across the major rollups will tell us, over the next several years, whether any chain is actually moving toward that endpoint or whether the Stage 2 carve-out has become the new resting place. The base rate so far suggests the latter is the easier institutional path.

The right question to put to every L2 team, in 2026 and after, is the one Polynya has been asking for years: *what is your timeline for retiring the security council, not narrowing its scope?* If the answer is "we don't have one," the chain is not on the Stage 3 trajectory; it is on the trajectory of becoming a more centralized version of itself in the long run, with whatever drift the political weather of the next decade produces. The honest framing of "how much power does a security council actually have" is therefore not the answer the report's closing gave — *enough* — but the harder one: **enough, and likely more over time, until the council is dissolved.**

---

# 8. Sources

**Stage framework and rollup classification:**
- [L2Beat — Stages framework](https://l2beat.com/stages) — Stage 0/1/2 definitions
- [L2Beat — Scaling project list](https://l2beat.com/scaling/summary) — current stage assignments per chain

**Vitalik Buterin essays:**
- [*Different types of layer 2s* (October 31, 2023)](https://vitalik.eth.limo/general/2023/10/31/l2types.html)
- [*Scaling Ethereum L1 and L2s in 2025 and beyond* (January 23, 2025)](https://vitalik.eth.limo/general/2025/01/23/l1l2future.html)
- *The math of when Stage 1 and Stage 2 make sense* (May 2025)

**Polynya — Stage 3 / council-dissolution framing:**
- Polynya's essays on rollup decentralization endpoints, multi-prover requirements, and the case for retiring the Security Council entirely for high-value applications: [polynya.mirror.xyz](https://polynya.mirror.xyz/)

**Arbitrum / rsETH freeze:**
- [Arbitrum forum #30803 — Security Council Emergency Action 21/04/2026](https://forum.arbitrum.foundation/t/security-council-emergency-action-21-04-2026/30803) — primary source on the rsETH freeze, including the `sendUnsignedTransactionOverride` mechanism, transaction hashes, and post-action justification
- [Arbitrum forum #30093 — Security Council Emergency Action 10/13/2025](https://forum.arbitrum.foundation/t/security-council-emergency-action-10-13-2025/30093) — Stylus precedent
- [L2Beat — Arbitrum One](https://l2beat.com/scaling/projects/arbitrum) — Stage 1 assessment, BoLD context, governance assumptions
- Arbitrum DAO governance documentation: docs.arbitrum.foundation
- Arbitrum BoLD technical documentation: docs.arbitrum.io

**rsETH / LayerZero / Kelp / Aave incident** — primary documentation references (no synthesized links):
- LayerZero post-incident communications (LayerZero Labs blog and X)
- Kelp DAO multisig pause and blacklist actions (Kelp DAO public communications)
- Aave bad-debt analysis and recovery thread (Aave governance forum, governance.aave.com)
- Chainalysis attribution to Lazarus Group / TraderTraitor

**OP Stack:**
- [L2Beat — OP Mainnet](https://l2beat.com/scaling/projects/op-mainnet)
- [L2Beat — Base](https://l2beat.com/scaling/projects/base)
- Optimism governance: gov.optimism.io
- Optimism technical specifications: specs.optimism.io
- Base Security Council and security documentation: docs.base.org
- Base codebase migration announcement (February 18, 2026): base.org

**ZK Stacks:**
- [L2Beat — ZKsync Era](https://l2beat.com/scaling/projects/zksync-era)
- [L2Beat — Starknet](https://l2beat.com/scaling/projects/starknet)
- [L2Beat — Linea](https://l2beat.com/scaling/projects/linea)
- [L2Beat — Scroll](https://l2beat.com/scaling/projects/scroll)
- [L2Beat — Polygon zkEVM](https://l2beat.com/scaling/projects/polygonzkevm)
- [L2Beat — Taiko Alethia](https://l2beat.com/scaling/projects/taiko)
- [Scroll governance forum — Security Council Transition (April 2026)](https://forum.scroll.io/t/governance-update-security-council-transition-contributor-roles-operational-adjustments/1470)
- ZK Nation (zkSync) governance documentation: docs.zknation.io
- Starknet governance and SNIPs: starknet.io and github.com/starknet-io/SNIPs
- Stwo verifier announcement (Starkware blog, November 2025)
- Linea decentralization roadmap: docs.linea.build
- Polygon zkEVM Security Council: docs.polygon.technology
- Polygon zkEVM March 23, 2024 incident postmortem: forum.polygon.technology
- Polygon zkEVM sunset announcement (Polygon Community Forum, June 2025)
- Taiko Alethia governance: taiko.xyz

**Companion reports (ethreportseth.xyz):**
- [rseth-layerzero-aave-incident-april-2026.md](/defi/rseth-layerzero-aave-incident-april-2026.md) — primary incident report on the rsETH/LayerZero DVN compromise and Aave contagion
- [state-of-ethereum-l2-ecosystem-march-2026.md](/ethereum/state-of-ethereum-l2-ecosystem-march-2026.md) — L2 governance surface, Stage 2 framework, TVL distribution
- [state-of-ethereum-march-2026.md](/ethereum/state-of-ethereum-march-2026.md) — L1 strawmap, sanctuary technologies
- [immutable-applications-on-ethereum-mainnet-april-2026.md](/ethereum/immutable-applications-on-ethereum-mainnet-april-2026.md) — credible-neutrality framework, application-layer immutability
- [l1-l2-mechanism-design-analysis.md](/ethereum/l1-l2-mechanism-design-analysis.md) — L1/L2 economic and trust-model relationship
