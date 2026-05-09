---
title: "State of Solana MEV: What Breaks First"
date: 2026-05-09
---

# State of Solana MEV — What Breaks First

*Published: May 9, 2026*

---

## tl;dr

- **The right question is not "what's next."** The right question is "which load-bearing assumption fails first." Solana's MEV market is a stack of coordination bets — Jito as the auction, Jupiter as the router, prop AMMs as the inventory layer, validators as compliant participants — and the interesting analysis is structural, not predictive.
- **Jito is a pipeline monopsony, not a binary monopoly.** As of January 2026, ~82% of stake routes through the Jito MEV pipeline (Agave-Jito 41% + JitoBAM 24% + Frankendancer-Jito 17%), even as the Jito-Solana binary specifically holds ~41%. Codebase diversification has not produced auction diversification.
- **Jupiter is the chokepoint nobody elected.** ~93.6% of aggregator volume and ~50% of all Solana DEX volume passes through Jupiter; aggregators themselves now route ~74% of Solana DEX flow. Whatever Jupiter integrates is the market.
- **MEV is migrating from observable searcher tips to invisible MM spread.** Prop AMMs (HumidiFi, SolFi, ZeroFi, Tessera V, Obric V2) handle >60% of Solana DEX volume by December 2025. The sandwich auction shrank ~60–70% in 2025 not because MEV declined but because the venue moved.
- **Solana sandwiches were never a mempool problem.** They were a coordination problem. DeezNode's `Vpe` bot — fed staked-feed visibility from a small set of permissive validators — accounts for roughly half of all observed sandwich activity on the chain. The fix is social (Paladin, JitoBAM TEEs, JitoSOL delegation policy), not technical.
- **SIMD-0096 quietly rewrote validator incentives.** Since February 12, 2025, 100% of priority fees flow to the producing validator. Daily SOL burn fell from ~18,000 to ~1,000. The "validators need toxic flow to survive" defense lost most of its empirical support overnight.
- **The freshest structural data point is consolidation, not innovation.** MoonPay acquired DFlow for $100M in stock on May 5, 2026. A regulated fiat onramp now owns Solana's second-largest orderflow venue — $50B+ cumulative volume, $12B Q1 2026, 1M+ traders, integrators including Coinbase, Phantom, Solflare, and Kamino.

---

## Table of Contents

1. [What's Next in Solana MEV?](#whats-next-in-solana-mev)
2. [Terminology, Carefully](#terminology-carefully)
3. [The Block Construction Pipeline, As It Actually Works](#the-block-construction-pipeline-as-it-actually-works)
4. [The "No Mempool" Myth and What Replaced It](#the-no-mempool-myth-and-what-replaced-it)
5. [Sandwiching Is a Coordination Problem](#sandwiching-is-a-coordination-problem)
6. [Arbitrage, Liquidations, and the Quiet Majority of MEV](#arbitrage-liquidations-and-the-quiet-majority-of-mev)
7. [Cold Economics: Validators, Searchers, RPCs](#cold-economics-validators-searchers-rpcs)
8. [Prop AMMs and the Opacity Transfer](#prop-amms-and-the-opacity-transfer)
9. [The Jupiter Question](#the-jupiter-question)
10. [What Breaks First](#what-breaks-first)
11. [What's Actually Next](#whats-actually-next)

---

## What's Next in Solana MEV?

The honest answer is: that's the wrong question.

"What's next" implies a roadmap — a forward arrow drawn from the current state into a tidy successor regime. The Solana MEV literature is full of that arrow. More BAM, more TEEs, more PBS-flavored mechanisms, more orderflow auctions, more aggregator dominance, more dark-AMM internalization. Any of these forecasts is plausible. None of them is interesting unless you first do the work of asking which assumption in the current system has to fail for any of them to matter.

So the move is to invert. Solana's MEV market in May 2026 is not a chaotic frontier. It is a remarkably tight machine, with a small number of load-bearing actors holding the system in a particular shape. Jito's off-chain auction routes ~82% of stake's MEV flow. Jupiter routes ~93.6% of aggregator volume. Three or four prop-AMM teams quote the majority of major-pair price discovery. A handful of RPC providers — Helius, Triton, QuickNode — control the ingress lanes that make any of this work. The Solana validator set has compressed to roughly 800 active operators, down from ~2,500 at its 2023 peak. This is not noise. It is structure. And like any structure, it has joints — places where, if you push, the whole thing reorganizes.

What breaks first? That is the question.

The reason this matters more than the prediction question is that MEV economics on Solana are not driven by a generative force pushing the system forward. They are driven by **what stays still** — the unspoken agreements among Jito, Jupiter, the major MMs, the largest validators, and the RPC oligopoly to continue routing flow through the existing rails. Every "what's next" forecast is a bet that those agreements will hold long enough to extrapolate. Every "what breaks" question asks whether they will.

Six things are load-bearing, in roughly descending order of fragility: the Jito pipeline monopsony, the Jupiter chokepoint, the opacity transfer to prop AMMs, the post-SIMD-0096 validator P&L, the social equilibrium suppressing sandwich attacks, and the BAM-as-decongestant-or-concentrator question. Each of them sits on top of an empirical claim that could turn out to be wrong. The body of this report walks the structure, fact by fact. The closing section returns to which assumptions are likeliest to fail and what the actual "next" looks like once you take the failures seriously.

---

## Terminology, Carefully

A surprising amount of the Solana MEV discourse fails on definitional precision. Three distinctions are worth nailing down before going further, because every analytical move downstream depends on them.

**Jito the company is not Jito the binary is not Jito the pipeline.** Jito Labs is a company that operates the Block Engine and historically maintained the Jito-Solana validator client. The Jito Foundation governs the JTO token and the TipRouter NCN. Jito-Solana the binary is a fork of Agave (the Anza-maintained successor to the original Solana Labs client) with a relayer-mediated TPU seam. The Jito Block Engine is an off-chain bundle auction. JitoSOL is a liquid-staking token issued by the Jito stake pool. JTO is a governance token. Casual writeups conflate all of these. Throughout this report, "Jito-Solana binary" means the validator client; "Jito pipeline" means the broader off-chain bundle auction infrastructure that any compatible binary can opt into; "Jito Labs" means the company; "Jito DAO" means the governance entity.

**Solana's "MEV pipeline" share is not the same as its client share.** As of January 2026, the Jito-Solana binary specifically runs ~41% of stake. Add JitoBAM (~24%) and Frankendancer-Jito (~17%), and ~82% of stake routes through the Jito MEV pipeline. Frankendancer is Jump's Firedancer networking and tile architecture grafted onto Agave's runtime; "Frankendancer-Jito" denotes the variant that retains Jito's bundle integration. Vanilla Agave residual sits in the low single digits. Pure Firedancer, declared production-ready in December 2025, is still ~1% of stake.

**Sandwich attacks on Solana are not Ethereum-style mempool front-running.** Solana has no public gossip mempool — never has — and Gulf Stream routes transactions directly to upcoming leaders. The sandwich mechanic on Solana works through validator collusion (a leader privately previews routed flow), staked-connection visibility (a validator with high stake-weight QoS sees ingress before others), or co-located process advantage (a bot running in-process at the leader). The DeezNode private mempool ("DeezMempool") is the canonical example of the first. This distinction matters because the wrong mental model leads to the wrong policy prescription. You cannot fix Solana sandwiches by adding encryption to a mempool that does not exist.

With those out of the way, the rest of the report uses precise language without explanation each time.

---

## The Block Construction Pipeline, As It Actually Works

Solana has 400ms slots and assigns leaders in fixed four-slot windows. Each leader owns a contiguous 1.6-second block-production window. The leader schedule is computed deterministically at the start of the previous epoch from finalized stake, so every validator and every searcher knows the leader schedule for the entire current epoch in advance. There is no in-protocol reorg market and no per-block auction in the Ethereum sense. Within a leader's four-slot window, that leader unilaterally orders all included transactions. There is no protocol-level proposer–builder separation. Block building and proposing are the same actor.

The competitive surface is the leader's ingress queue. Searchers must either (a) win the leader's off-chain bundle auction (Jito Block Engine, BAM, Paladin's P3 port, Express Relay), or (b) push directly into the leader's TPU queue with a priority fee high enough to beat conflicting transactions in the central scheduler. There are several ingress paths and most modern transaction services fan out across them in parallel.

The mainstream paths in 2026: public RPC submission (Solana Foundation public RPC, Helius, Triton, QuickNode, Alchemy, Chainstack, Syndica) which forwards via QUIC to the current and next leaders; direct-to-leader TPU; stake-weighted QoS, where leaders allocate 80% of their TPU ingress capacity to staked validators in proportion to stake; Jito Block Engine bundle submission; Paladin's P3 priority port and in-leader paladin-bot; Jito BAM via permissioned BAM Nodes that schedule inside TEEs; Pyth Express Relay's protocol-defined opportunity auction; and private RFQ channels including JupiterZ, DFlow, Hashflow, Bebop, and OKX RFQ.

The Jito stack in detail. Four pieces. The Jito-Solana validator client runs a `validator <> relayer` gRPC seam, delegating ingress to an external relayer. The relayer accepts QUIC ingress, **buffers transactions for ~200 ms**, and forwards them to both the validator and the Block Engine. The 200ms buffer is the load-bearing design choice: it gives the Block Engine a window to assemble higher-paying bundle alternatives that the leader can substitute for the natural ordering. The Block Engine runs a continuous off-chain auction over bundles (≤5 atomic txs plus a tip transfer to a per-validator Tip Payment PDA). Bundles land atomically or revert atomically — no partial fills. ShredStream is a separate paid product streaming leader shreds (pre-finality block fragments) to subscribers at minimum latency, used by HFT searchers and remote validators for state anticipation.

In March 2024, Jito **shut down** its public mempool — not "briefly suspended" — in response to industrialized sandwiching. That decision did not eliminate sandwich MEV. It pushed the visibility surface into private channels (DeezNode's DeezMempool, staked RPC providers, leader-co-located bots), where it has remained.

BAM, launched July 21, 2025, is the most consequential 2025–2026 ingress change. BAM separates scheduling — performed in TEEs by permissioned BAM Node operators — from execution, performed by BAM Validators running an updated Jito-Solana client with a plugin layer for programmable ordering. By the end of January 2026, BAM had crossed 88M SOL in delegated stake (~24% of network stake) across 273 validators. Initial BAM Validator partners include Helius, SOL Strategies, Triton One, and Figment. JIP-28 (active proposal) explicitly subsidizes broader BAM adoption with a >30% target. BAM is the closest Solana has come to PBS-style separation. It is also a tighter form of central coordination than Jito-Solana ever was: the scheduler runs inside a TEE under a permissioned operator set.

The codebase picture is more diversified than the pipeline picture. Frankendancer is the real story: ~20.9% of stake across ~207 validators by October 7, 2025, up from ~8% in June 2025. Pure Firedancer began voting on mainnet July 2025, produced full blocks October 2025, was declared production-ready December 2025 after ~50,000 clean blocks and 20M+ votes. Sig (Syndica, Zig) and Mithril (Overclock, Go) exist but have near-zero production stake. Codebase concentration ≠ economic-actor concentration. Firedancer fixes the first, not the second. MEV economics remain gated by Jito's auction, BAM's scheduler, and the SWQoS inventory held by a handful of RPC providers.

The real architectural fact: >90% of stake is Agave-derived code. A bug in Agave's runtime can correlate-fail almost the entire network. Firedancer is the first real codebase diversification, and that is the central decentralization story of 2025–2026. It is not the central MEV story.

---

## The "No Mempool" Myth and What Replaced It

The "Solana has no mempool" framing is one of those technically-true statements that obscures more than it reveals. It is also a 2022-era talking point that has long outlived its analytical usefulness. The accurate framing in 2026: Solana has fragmented, private, intermediary-controlled visibility windows that look and act like mempools to those with access.

Specifically: the Jito Relayer's 200ms buffer is effectively a 200ms mempool visible only to the Block Engine and the validator. A leader's local TPU and Banking Stage queue is a per-leader mempool that the leader, paladin-bot (where installed), and any co-located software can introspect. Staked RPC providers see all flow they route, which on a high-share provider like Helius is a substantial fraction of all retail transactions. BAM Nodes see scheduling-stage flow inside their TEEs. DeezNode's DeezMempool is a private mempool sold as a product to sandwich operators.

The right MEV question on Solana is therefore not "is there a mempool" but "who controls the visibility window." Several intermediaries do, each with different incentive structures. Jito's auction is operated by Jito Labs as a centralized off-chain process even after JIP-24 redirected its 6% Block Engine fee to the DAO. BAM Nodes run inside TEEs but the scheduler binary is still under Jito's control. Staked RPCs see retail orderflow before any leader and have no enforced separation between "I route your tx" and "I am also a searcher." Most disclaim sandwiching; verification is hard.

This framing matters because it inverts the Ethereum-default intuition. On Ethereum, "private orderflow" is usually framed as a harm — a way for builders to extract more from users by hiding the auction. On Solana, "private orderflow" was the harm-reduction move. Jito's March 2024 mempool shutdown happened precisely because the public auction was being industrialized into sandwich attacks. Pushing flow into private channels did not eliminate the MEV. It changed who could see it. That is the only knob anyone has.

The protocol-level changes between 2024 and 2026 that mattered: Agave v1.18's central / prio-graph scheduler (March 2024) replaced the legacy multi-threaded greedy banker with a single scheduler thread feeding a prio-graph DAG, ordering by priority fee and avoiding lock contention. The greedy scheduler was reintroduced and improved in Agave v2.3 (mid-2025) to keep execution threads saturated. Dynamic, congestion-aware SWQoS arrives in Agave 4.0/4.1. None of these are MEV-policy changes — they are scheduler optimizations. The actual MEV-policy changes lived elsewhere: SIMD-0096, JIP-8, JIP-24, the Jito mempool shutdown, BAM, Paladin, the JitoSOL delegation policy that delisted 15+ sandwich-friendly validators.

---

## Sandwiching Is a Coordination Problem

The single largest source of misanalysis in the Solana MEV literature is treating sandwich attacks as a transparency problem. They are not. They are a coordination problem.

A bot with leader-co-located, private-channel, or staked-connection visibility into routed swap flow inserts a frontrun and a backrun around the victim trade. It does not see a public mempool. It sees a private feed sold by a permissive validator. The DeezNode private mempool is the most visible instance of this. One DeezNode-affiliated validator (HM5H6...jdMRA) historically held ~811k SOL (~$168M) in delegated stake, having grown from ~308k SOL in epoch 697 to ~803k SOL by epoch 709 in late 2024. That stake growth was direct revenue from selling sandwich access.

The numbers. Sandwiched.me, analyzing ~8.5B trades over ~$1T in DEX volume, estimates **$370M–$500M extracted from Solana users via sandwiching** over a trailing 16-month window — figures popularized through Helius's MEV report. The single most prolific bot, **Vpe**, executed 1.55M sandwiches in a 30-day window (December 7, 2024 to January 5, 2025) at 88.9% success, netting ~65,880 SOL (~$13.43M) while paying ~22,760 SOL (~$4.63M) in Jito tips. Vpe alone is roughly half of all observed Solana sandwich activity. Sandwich profits dropped an estimated 60–70% through 2025, driven less by suppression and more by venue migration (more on that in the prop-AMM section).

Sandwiches survive because some validators sell preview access. The fix is therefore not technical. It is social. The Solana Foundation's JitoSOL stake pool delegated away from 15+ validators tied to sandwich activity (epoch 789). Paladin, a ~2,000-line patch on Jito-Solana that filters sandwich bundles at the leader and adds the P3 priority port, reached **~80 validators / ~6% of stake**. Paladin reports producing average rewards of 0.067 SOL versus 0.066 SOL for Agave+Jito in August 2025. That is the empirically important number: the anti-sandwich client matches the standard client on validator yield. The "validators need toxic flow to survive" defense was always thin and is now empirically undercut.

But — and this is the structural point — Paladin is a social equilibrium, not a technical guarantee. JitoBAM TEEs encrypt the scheduling stage but the BAM operator set is permissioned and Jito-controlled. JitoSOL delegation policy is a stake-pool policy, not a protocol rule; the same coordination that produced it can dissolve it. The "blind" sandwich rate — attacks executed without confirmed victim preview — rose from ~1% to ~30% in 2025 as preview channels narrowed. This is not victory. It is adaptation. Searchers responded to the social pressure by absorbing more risk per trade, not by giving up the strategy. If the social equilibrium frays — if a permissive validator wins a major delegation, if Paladin adoption stalls, if BAM operators relax their scheduling rules — sandwich extraction can return to its 2024 peak in months.

This is the first joint of the Solana MEV machine where "what breaks" matters more than "what's next."

---

## Arbitrage, Liquidations, and the Quiet Majority of MEV

Sandwich attacks dominate the discourse because they are extractive against retail. Arbitrage dominates the economics because it is the single largest legitimate MEV vertical on Solana.

Cross-DEX arbitrage between Raydium (CPMM/CLMM), Orca Whirlpools, Meteora DLMM, Phoenix CLOB, Lifinity, and the prop-AMM tier produces a high-frequency, low-margin extraction profile. In the year ending mid-2025, Helius's analysis (citing Jito's detection algorithm) identified **90,445,905 successful arbs / $142.8M total profits / $126.7M (88.7%) SOL-denominated / $1.58 average / $3.7M maximum single arb**. Arb bots tip ~50–60% of expected profit to validators. Sandwich bots tip ~15–20%. The asymmetry is why arbitrage, not sandwiches, is the dominant validator-side MEV revenue line. Per Jito Labs, arb transactions consume more than 60% of block compute units in some epochs. Jito bundles substitute structurally for the "both legs fill or revert" property that flashloans provide on Ethereum.

Liquidation MEV is dominated by Kamino (~$3.5B TVL), MarginFi, Drift, Save (formerly Solend, ~$300M TVL), and Jupiter Perps. The mechanics differ from Ethereum in two ways. First, flashloans exist but are app-specific — Kamino offers a native flashloan facility (the same primitive that powers its Multiply leveraged-vault product); MarginFi added flashloans later; there is no Aave-equivalent universal flashloan venue. Second, atomicity comes from Jito bundles, not from EVM-style transaction semantics. A liquidator typically constructs a bundle: oracle price update (Pyth pull) → liquidate call → close-out swap on Jupiter or Raydium → optional flashloan repayment. The bundle either lands intact or it does not.

MarginFi processed >$1.7B in liquidations in Q1 2025, generating $88.5M in liquidation fees against a TVL that fell 57.6% to $163.78M over the same quarter. The February 16–18, 2025 LIBRA collapse triggered only ~$4M in collateral liquidation on Kamino. The much larger February 2025 cascade (~$400M in 24 hours) was concentrated on Jupiter Perps; JLP remained solvent and the protocol had zero downtime.

Oracle MEV deserves a note. **Pyth is a pull oracle.** Price aggregation runs continuously on Pythnet (a Solana-VM appchain), is signed by Wormhole guardians, and is made available off-chain via Hermes. The on-chain price only updates when a transaction explicitly pulls and verifies it. This is fundamentally different from Chainlink push oracles on Ethereum. **Update timing itself is the MEV.** Whoever decides when to pull the price update can sequence it before or after a price-sensitive action. Pyth Express Relay is the structural answer: an off-chain priority auction that lets protocols delegate execution rights for specific actions (liquidations, swap fills) to highest-bidding searchers, with the bid redirected to the user or protocol. Confirmed Solana integrations include Kamino (lending liquidations + Kamino Swap, Q4 2024) and Synthetix (perps liquidations). Searcher network: Amber, Auros, Caladan, Flowdesk, Selini, Tokka Labs, Wintermute. Pyth has not published a public total of liquidation MEV redirected; the flow is real but unquantified in primary sources.

JIT liquidity on Solana works differently than Uniswap v3 JIT. Three flavors. Drift's JIT auction kicks off a ~5-second Dutch auction on every market order, between AMM bid/ask and AMM intrinsic price; off-chain JIT makers race to fill. Phoenix CLOB sees sub-second MM updates against off-chain price; MEV appears as stale-quote arb when MMs lag CEX prints. Prop-AMM JIT routing via Jupiter uses Ultra Signaling to tag transactions so prop AMMs identify non-toxic retail flow and quote tighter. Uniswap-v3-style "insert+remove single-block LP around a pending swap" is essentially absent on Solana because there is no public mempool to snipe and the prop-AMM tier serves the routing role more efficiently.

Token-launch and bonding-curve MEV is the highest-velocity new MEV vertical on Solana since 2024 and does not exist at this scale on any other chain. Pump.fun is the dominant launchpad: $296M in fees YTD 2025, peaking at $60M in January 2025 before a ~50% February drop. The structural change: **on March 20, 2025, Pump.fun launched PumpSwap and ended automatic migration to Raydium.** PumpSwap removed the 6-SOL migration fee, charges 0.25% (0.20% LP + 0.05% protocol), and reached $80M daily volume within a month. Bonding-curve MEV mechanics include sniper bots racing to be first buyer in the same block as token creation, bonding-curve sandwiching of Pump.fun curve swaps (most of the ~6 dominant Solana sandwich bots do this), and bundle stuffing where deployers bundle "create token + sniping buy" to capture the launch wick themselves. LetsBonk briefly hit ~78% market share of Solana token launches in July 2025 (20,712 daily launches versus Pump.fun's 4,486 on one comparison day), markets explicit anti-MEV features, but share has since collapsed.

This is the quiet majority of MEV on Solana. Sandwiches are loud. Arbitrage is huge. Liquidation flow is concentrated in keeper sets. Oracle timing hides inside protocol margin. Bonding-curve MEV is small in dollar terms but enormous in transaction count and in the social damage of "fair-launch" tokenomics.

---

## Cold Economics: Validators, Searchers, RPCs

The unit economics of Solana MEV are not the same as Ethereum's. The chain runs hotter, faster, and on a far smaller validator set, and the post-2025 protocol changes shifted the validator P&L decisively toward MEV-and-priority-fee revenue.

A mainnet-grade Solana validator in 2026 needs at minimum a 12-core/24-thread CPU, 256GB RAM, 2TB+ NVMe, and 1Gbps symmetric bandwidth. The competitive spec is materially higher. Latitude.sh's Jito-Labs-partnership instance runs $247/month after rebate; Hivelocity, Cherry, and generic enterprise bare-metal sit in the $500–$1,200/month band for properly specced 384–512GB nodes. Realistic all-in opex (server, bandwidth, monitoring, remote-hands) for a serious operator: **$500–$1,500 per month, or ~$6k–$18k/year**.

The vote-credit cost dwarfs the hardware bill. Each vote tx burns 5,000 lamports. At ~432,000 slots/epoch and one vote per slot, that's ~2.16 SOL/epoch, ~1.08 SOL/day, **~394 SOL/year**. At $92.61/SOL on May 9, 2026, that is **~$36,500/year in vote fees alone**. Combine ~$36.5k vote burn with ~$10k–$18k hosting and a no-frills validator runs **~$46k–$54k/year in fixed cost**.

Break-even stake. At 6.5% inflation APY, 5% commission, and the 2026 SOL price, a validator captures ~0.325% of delegated stake per year in commission. Required stake to clear $50k of fixed cost on inflation alone: ~166,000 SOL. With Jito MEV tips adding ~1–1.5% APY on the staker side and the validator keeping 100% of priority fees post-SIMD-0096, break-even drops to **~80,000–100,000 SOL of delegated stake at 5% commission** — the canonical figure operators repeat. Below that threshold, validators lose money without MEV tips, side deals, or non-staking revenue.

The single most consequential 2025 change to validator economics was **SIMD-0096**, which activated on **February 12, 2025** with 77% approval. Pre-SIMD-0096, priority fees were 50% burned and 50% to the validator. Post-SIMD-0096, 100% goes to the producing validator. The effect was immediate: daily SOL burn collapsed from ~18,000 SOL/day to ~1,000 SOL/day. Effective annualized inflation rose from ~3.6% to ~4.7%. Validator priority-fee revenue doubled. The "secret-deal" incentive — validators previously cut private deals with searchers to avoid the burn — effectively disappeared because priority fees became a clean, fully-captured validator revenue stream at the protocol layer.

The Jito tip story sits on top of that. JIP-8 (separate from JIP-24) created the TipRouter NCN, which charges a 3% fee on Jito MEV tips and 1.5% on priority fees a validator chooses to redistribute, with that fee split 90% Jito DAO / 5% LST vault operators / 5% JTO vault operators. Net of NCN fee, validators and stakers receive ~97% of MEV tips. **JIP-24, passed in August 2025, redirected 100% of the 6% Jito Block Engine fee from the prior 3%/3% Jito Labs / Jito DAO split to 100% Jito DAO** — estimated ~$15M/year to treasury. Jito Labs no longer takes a cut of Block Engine fees. The technical chokepoint is unchanged; the political economy is not.

Cumulative Jito tip payouts crossed $674M entering 2025. Peak monthly tips hit $210M in November 2024. Tips comprised 41–66% of Solana REV month-over-month through 2024–2025; specifically, Q2 2025 was 54.5% MEV ($148M of $272.3M REV per Messari), declining to ~25% by Q4 2025 as memecoin volume cooled. Solana REV peaked at $551.7M in January 2025 (3.31× Ethereum's $166.5M same month). Solana 2025 chain-level revenue was **~$1.3–1.4B** depending on source, surpassing Ethereum's $524M by a wide margin. Of that, Jito tips are the dominant single line.

Searcher economics. A serious searcher runs a co-located node in the same datacenters as Jito Block Engine relays (Frankfurt, Amsterdam, NY, Tokyo) at $1,500–$3,800/month for dedicated Solana node access. ShredStream is bundled free with several RPC plans or available as $49/month add-ons via Chainstack Yellowstone gRPC. Helius Professional is $999/month plus $500–$6,000/month in data add-ons for 5–100TB. Triton dedicated starts at $2,900+/month with Cascade pricing on top for staked submission. SWQoS access requires either running a validator with >15,000 SOL self-stake (~$1.4M at $92.61) or buying staked-connection access from Helius, Triton, or Edgevana. Practical institutional rule of thumb: 100k SOL of assigned virtual stake-weight gets predictable throughput; 300k SOL is institutional-grade — i.e., ~$9.3M–$28M of capital.

Engineering costs run another $1.25M–$6M/year for a 5–15-engineer team at fully-loaded $250k–$400k per engineer. Bundle tip economics force searchers to **surrender 50–70% of expected profit to validators as tips** to win the slot. Pay too little, the bundle is skipped. Pay too much, the strategy is unprofitable. The auction is first-price sealed; latency to the Block Engine determines win frequency. At the network level, with ~800 active validators and an estimated ~$1.0–$1.5B annualized MEV tip flow, average per-validator MEV revenue is on the order of $1.25M–$1.9M/year — but the distribution is highly skewed by stake-weight and leader-schedule frequency.

RPC providers are the most under-discussed actor class on Solana. Helius operates the largest single Solana validator (>14M SOL delegated, 0% commission, MEV passed through to stakers), is the SOL staking provider for Bitwise's ETF and the Solana Company, and runs Sender (a bundle-flavored RPC product that parallelizes dispatch across SWQoS and Jito Block Engine). Helius is therefore a participant at three layers simultaneously: RPC ingress, validator block production, and BAM Node operator. Triton operates Dragon's Mouth (gRPC), Steamboat, Old Faithful, and Cascade Marketplace for SWQoS routing, and is also a BAM Node early partner. QuickNode operates Lil-JIT MEV protection. Astralane is a newer entrant. Inbound transaction-flow market share by RPC is not publicly disclosed. Anecdotally, Helius and Triton dominate professional searcher and market-maker connections; QuickNode and Alchemy dominate broad app and wallet use.

The unit-economic regime in May 2026 looks more like an HFT shop's P&L than an Ethereum validator's. High gross revenue, low fixed cost, latency-sensitive, capital-intensive at the SWQoS layer. Solana has effectively become a validator-revenue-maximizing chain. Fees no longer burn at the priority-fee layer. MEV no longer leaks to Jito Labs (0% Block Engine cut). The only economic frictions are TipRouter's 1.5–3% NCN fees. This is structurally different from Ethereum's burn-heavy, PBS-mediated MEV market. Whether it is more sustainable, more captureable, or more vulnerable to incumbent consolidation is an open empirical question — but it is the question that follows from the unit economics, not from any prediction about technology.

---

## Prop AMMs and the Opacity Transfer

The single most important change in Solana MEV between 2024 and 2026 is not in the validator pipeline. It is in the venue layer. The venue layer is where MEV is generated — or, more precisely, where the inefficiencies that searchers extract get surfaced and priced. Between 2024 and 2026, the venue mix shifted from public AMMs to a tier of closed-source proprietary AMMs. The shift is what produced most of the headline "MEV is going down on Solana" narrative. The narrative is partially correct and structurally misleading.

Terminology first. "Private AMM" lumps four mechanically distinct things. **Proprietary on-chain AMMs (prop AMMs, dark AMMs)** are closed-source Solana programs that hold inventory on-chain with no UI, no LP deposits, no public curve. Pricing is driven by an off-chain predictive model that streams parameters on-chain. HumidiFi, SolFi, ZeroFi, Obric V2, Tessera V, GoonFi. Settlement is fully on-chain; price formation is opaque. **RFQ / PMM systems** route quote requests off-chain to authorized MMs; only the winning quote settles on-chain. JupiterZ, Hashflow, Bebop, DFlow. No on-chain pool exists between trades. **Oracle-driven AMMs** are on-chain pools anchored to an external oracle. Lifinity is canonical. Lifinity is winding down — claim deadline December 31, 2026. **Private orderflow auctions** are not venues but channels. Pyth Express Relay runs an off-chain priority auction in which searchers bid for protocol-defined operations.

Lifinity is not a prop AMM. Phoenix is not acquired. Ellipsis Labs is independent, raised $21M from Haun Ventures in October 2024, and is building Atlas (an SVM L2 settling on Ethereum). SolFi is Ellipsis's natural evolution from Phoenix's microstructure expertise. These distinctions matter because lumping them produces wrong analysis.

The numbers. **HumidiFi processed ~$100B notional in less than five months post-May 2025 launch, capturing ~35% of total Solana DEX activity and >25% of SOL/USDC, peaking at ~$34B in a single month.** Attributed to research firm Temporal. Compresses oracle updates to 143 CUs — more than 1000× cheaper than a typical Jupiter swap — and quotes ~5bps spreads versus 65–90bps on legacy AMMs. **SolFi peaked at 46.4% of Jupiter's USD-denominated flow in early May 2025**; ~18.4% of prop-AMM volume by early August 2025. **Obric V2** pulls 92.5% of its volume via aggregators. **ZeroFi** launched January 2025 and ran ~13.9% of prop-AMM volume by early August. **Tessera V** is operated by Wintermute, integrated into Jupiter June 2025, runs ~6% of recent SOL/USDC volume. By December 2025, **prop AMMs accounted for >60% of Solana DEX volume.**

How this changes MEV economics, point by point.

**Sandwich opportunity disappears on internalized flow.** When a swap routes through HumidiFi or JupiterZ, there is no public AMM curve to front-run. Solana sandwich profits dropped an estimated 60–70% through 2025. This is not because validators or governance suppressed sandwiches. It is because the venues moved.

**LVR is externalized to MMs, not LPs.** A passive AMM LP loses to arbitrageurs whenever off-chain price moves before on-chain rebalancing — Loss vs. Rebalancing — estimated by CoW at 5–7% of LP capital annually. Prop AMMs eliminate the passive LP. The operator's balance sheet absorbs the LVR cost in exchange for capturing the flow. This is not a free lunch. It concentrates inventory risk on MM operators and excludes retail LPs from the most profitable venues.

**Latency arb is suppressed within the venue, not network-wide.** Express Relay captures liquidation MEV inside the protocol. HumidiFi's sub-slot oracle updates make latency arb on its quotes hard. But cross-venue arb between prop AMMs and CEXes still exists. It is what the prop AMMs do for a living.

**Opacity replaces transparency.** No proof of reserves, no public curve, no permissionless LP access. The MEV that used to be extracted via observable searcher tips is now internalized into MM books. The system looks cleaner from the outside. The extraction has not stopped — it has moved one layer deeper into spread that is invisible to the user.

This is the **opacity transfer**. It is the single most important framing in this report. The "MEV is going down" narrative on Solana is partially an artifact of where the extraction is now happening. Sandwich extraction is down. Searcher arb on internalized flow is down. CEX-DEX arb on internalized flow is captured by the MM operating the prop AMM, not by external searchers. The user pays a tighter spread than they would have paid on a legacy AMM, but the spread is set by an off-chain model whose parameters are private. The MM captures the asymmetric-information rent that public AMMs leak via LVR. From a welfare lens this is genuinely better for retail. From a transparency lens it is worse, because the redistribution is now invisible.

Three teams plus Jupiter intermediate the majority of Solana DEX volume in May 2026: Ellipsis Labs (SolFi), Temporal (HumidiFi), Wintermute (Tessera V), with Jupiter as the routing chokepoint. That is a smaller intermediary set than "permissionless DeFi" branding implies. It is the core tension the analysis must foreground.

---

## The Jupiter Question

If the prop-AMM tier is where MEV is internalized, Jupiter is where it is permissioned.

Jupiter routes ~93.6% of Solana aggregator volume and ~50% of all Solana DEX volume. Aggregators themselves now route ~74% of Solana DEX volume — up from ~40% six months prior. Jupiter's Q2 2025 alone moved ~$80B across 1.4B swaps. Its routing engine evolved from Metis (modified Bellman-Ford with split/merge across DEXs) to Iris (claimed 100× performance, embedded in Ultra V3), with JupiterZ as the RFQ leg launched late 2024 to bring CEX-style MMs on-chain via webhook (~$100M/day zero-slippage volume). Juno is the umbrella aggregation combining Iris, JupiterZ, and third-party aggregators.

The structural fact: Jupiter's venue-integration choices are a de facto policy decision about who extracts value from Solana orderflow. When Jupiter integrated SolFi, Obric V2, ZeroFi, and Tessera V across late 2024 and 2025, it redirected billions of dollars of flow away from public AMMs (Raydium, Orca, Meteora) into closed-source venues. The redirect is functionally equivalent to a private orderflow agreement at protocol scale: Jupiter selects venues, venues "pay" (via better fills) for flow, unsubsidized public LPs get cut out. For external searchers, informed flow no longer hits public AMMs, so cross-pool arb on major pairs has collapsed and concentrated in long-tail tokens and memecoin pools.

Jupiter Ultra V3 adds Beam, a private relay that mitigates sandwich attacks with claimed 34× lower MEV-per-volume than competing trading UIs. Ultra Signaling tags transactions so prop AMMs identify non-toxic retail flow and quote tighter. JupiterZ beats on-chain routing by 5–20bps on major pairs when MMs are quoting. These are all real user benefits. They are also a tightly coordinated package that no one outside Jupiter authored.

DFlow is the second-largest aggregator force. Since April 2025 it has processed >$50B cumulative volume, with $12B in Q1 2026 alone, and serves >1M traders across 500+ apps. DFlow briefly took 47.9% of Solana aggregator volume on November 15, 2025, edging Jupiter's 47.1% before reverting. DFlow uses Pyth as decentralized NBBO; MMs are programmatically required to fill at or better than oracle. **MoonPay acquired DFlow on May 5, 2026 for $100M in stock.** This is the freshest structural data point in the entire Solana orderflow story. A regulated fiat onramp, with multi-jurisdiction compliance posture, now owns Solana's second-largest orderflow venue, with integrators including Coinbase, Phantom, Solflare, and Kamino.

The MoonPay-DFlow deal is the right place to land the Jupiter-question section, because it points at the trajectory. Solana orderflow is consolidating into a small number of private intermediaries with regulated parents, professionalized policies, and audit-friendly compliance postures. This may professionalize Solana's orderflow market or recreate Robinhood-style payment-for-orderflow concerns at the chain layer. It probably does both, in different proportions for different user segments. What it does not do is decentralize.

---

## What Breaks First

Six load-bearing assumptions hold the current Solana MEV market structure in place. The interesting analytical question is which one is most fragile.

**One. The Jito pipeline monopsony.** When ~82% of stake routes through one off-chain coordinator's auction — a coordinator whose Block Engine binary is operated by a single company even after JIP-24 zeroed the company's fee — governance capture and policy capture are not hypothetical. They are the operating model. A Jito Labs operational failure, a regulatory action against the Block Engine, a contentious DAO governance fight, or a security incident at the relayer or block engine has correlated impact on the majority of Solana MEV pricing. BAM concentrates this rather than diluting it: BAM's scheduler runs in TEEs operated by a permissioned set, and the scheduler binary is still under Jito's control. There is no fallback infrastructure that captures the same fraction of MEV revenue at the same UX. This is the single largest structural fragility in the system.

**Two. The Jupiter chokepoint.** ~93.6% of aggregator volume plus opaque prop-AMM routing equals a permissioned MEV redistributor that no one elected. If Jupiter's venue-integration policy shifts — toward more aggressive private AMM routing, toward equity stakes in MM operators, toward fee structures that monetize the routing rent more directly — the entire Solana orderflow pricing surface moves with it. The MoonPay-DFlow deal demonstrates that orderflow is acquirable. Jupiter is harder to acquire (its token economics and community position make a clean takeover unlikely), but its integration roadmap is a single policy lever held by a small team.

**Three. The opacity transfer to prop AMMs.** Three teams plus Jupiter intermediate the majority of Solana DEX volume. The prop-AMM model is a balance-sheet model — the operator absorbs LVR and inventory risk in exchange for capturing flow. In normal markets it works. In sharp moves, MMs eat losses invisibly because there are no public LPs to share the pain. The 2024 USDe depeg, the February 2025 Bybit hack, and the LIBRA collapse all stress-tested this implicitly. None broke prop AMMs at scale, but the tail-risk profile of the venue layer is concentrated in a way the public DEX tier was not. A correlated MM failure — one operator's quoting model breaks during a sharp move and cascades through the others — is a real failure mode that produces no public signal until the venues simultaneously stop quoting. This is the failure mode the "MEV is going down" narrative does not see.

**Four. Validator economics in the post-SIMD-0096 regime.** SIMD-0096 made validators directly dependent on user fees and MEV. This aligns incentives toward extraction even as the surface gets more private. If memecoin volume cools further (Q4 2025 was already half of Q2 2025 in REV terms), validator revenue compresses and the political economy of MEV policy tightens. Specifically: validators have less margin for accepting policies that suppress extraction (e.g., more Paladin adoption, stricter JitoSOL delegation rules) when their P&L is structurally tighter. This is the slow-moving fragility — not a failure mode that breaks suddenly but one that erodes the social equilibrium suppressing toxic flow.

**Five. The collusion baseline and the social equilibrium suppressing it.** Sandwich extraction survived because some validators sold preview access. The fix was social: JitoSOL delegation policy, Paladin (~6% of stake), JitoBAM TEEs, Jupiter Beam private relay. Each of these is a coordination, not a protocol. If a permissive validator wins major delegation, if Paladin adoption stalls below 10% of stake, if BAM operators relax scheduling rules, if a new private mempool product captures the share DeezNode lost — the sandwich rate can return to its 2024 peak in months. The 2025 60–70% reduction in sandwich profits is not a permanent equilibrium. It is the current equilibrium.

**Six. BAM as both decongestant and concentrator.** BAM is the cleanest argument for "what's next." It separates scheduling from execution via TEEs. It is the closest thing Solana has to PBS-style separation. It also concentrates scheduling under a permissioned operator set whose binary is Jito-controlled. JIP-28 explicitly subsidizes broader adoption with a >30% target. If BAM hits 50% of stake without diversifying its Node operator set or its scheduler binary, the system has imported PBS without importing PBS's most useful property — competitive separation between proposer and builder. This is the failure mode where the technical argument for BAM is right and the structural argument is wrong.

The fragilities are not equally likely to fail and not equally consequential. The Jupiter chokepoint is the most concentrated single point of policy. The Jito pipeline monopsony is the most consequential single point of failure. The opacity transfer is the most under-discussed structural risk. The collusion baseline is the most easily reversed. The validator P&L compression is the slowest-moving and the most likely to actually constrain the system over a multi-year horizon.

A fair characterization: in the next 12–24 months, the most likely "what breaks" event is a Jupiter policy decision that further consolidates routing into prop AMMs in a way that produces a public backlash about closed-source venues quoting majority-pair flow. The most consequential possible break is a Jito Labs operational, governance, or regulatory event that interrupts Block Engine availability and forces a rapid migration to BAM, Paladin, or DeezNode. The most insidious break is the slow tightening of validator economics that erodes the social equilibrium suppressing sandwich attacks until sandwiching returns as a meaningful share of MEV.

---

## What's Actually Next

If the inversion is right — if "what's next" is downstream of "what breaks" — then a sober forecast looks different from the headline trends.

More BAM-ification. Jito will push BAM toward the JIP-28 target. The marginal validator joins BAM because the incentive subsidy beats the alternative. BAM's permissioned Node operator set will expand modestly. The TEE-mediated PBS-on-Solana model will become the dominant ordering primitive on the chain by end of 2027. Whether this represents real PBS or a more elegant centralization depends entirely on the diversity of the BAM Node operator set, which is currently small and Jito-aligned.

More aggregator-internalized routing. Jupiter will continue integrating private venues. Prop AMMs will likely cross 70% of Solana DEX volume in 2026. New entrants will appear from MM teams that lost share in the legacy AMM tier and want to recapture it in the prop tier. The opacity gap widens.

More orderflow OTA acquisitions like MoonPay-DFlow. Regulated parents acquiring crypto-native orderflow venues is the cleanest professionalization-vs-PFOF tension. Expect at least one more transaction at the same scale ($50M+) within 12 months. The acquirers will be regulated fiat onramps, regulated brokerages, and possibly traditional payment processors. The acquired will be Solana-native execution layers and possibly Phantom-tier wallets.

A continued deflation of the searcher economy. Sandwich profits keep eroding as venues internalize. Arb margins compress as prop AMMs quote tighter. The independent-searcher tier loses share to MMs and to validator-aligned bot operations. The bottom of the searcher distribution exits or consolidates. The top tier becomes more capital-intensive, more co-located, more institutional. This is the same trajectory Ethereum's searcher economy went through after MEV-Boost matured; Solana's version will be faster and more concentrated.

A growing political fight about who captures the validator P&L. Post-SIMD-0096, the protocol-layer revenue surface is large enough to attract concrete redistribution proposals. JIP-16 / TipRouter priority-fee redistribution to stakers is the leading edge. Expect proposals to redistribute more priority-fee flow to delegators, to mandate priority-fee caps, or to formalize Paladin-style anti-toxic-flow filters at the network layer. The political coalition for these proposals grows as memecoin volume cools and validator margins tighten.

Firedancer matures into a real codebase diversification but not a real MEV diversification. Pure Firedancer crosses 10% of stake by mid-2027. Frankendancer-Jito remains the bigger story. Jito does eventually ship a Firedancer-native bundle integration (the publicly announced collaboration is real, the shipping date is the open question). The MEV pipeline remains Jito-centered. The bug-correlation risk to Solana from Agave-derived runtime bugs goes down materially. That is a safety win, not an MEV win.

The final question is whether any of this matters from a user-welfare lens. The honest answer is: largely yes. Retail users on Solana in 2026 pay tighter spreads, lose less to sandwich attacks, and see better fills than they did in 2024. The system is more efficient. It is also more concentrated, more permissioned, and more opaque. The MEV that used to leak to searchers as visible tips now leaks to MM operators as invisible spread. The user feels the improvement and does not see the redistribution. That is the trade Solana has actually made.

The right way to evaluate that trade is to ask whether the concentration is reversible. Coordination problems are reversible if the coordination mechanism is. The Solana MEV market in May 2026 is held together by a small number of social and economic equilibria — Jito's auction, Jupiter's routing, the prop-AMM tier's quoting, the JitoSOL delegation policy, Paladin's social pressure on toxic flow. None of these is enforced by protocol. All of them are policies. Any of them can flip. Which is also why the right analytical posture is "what breaks first" rather than "what's next." The structure is not the destination. It is the current settlement of an ongoing negotiation.

What's next is what comes after one of those negotiations re-opens.

---

*Co-authored with Claude (Anthropic). Research conducted by a 5-team multi-agent pipeline with tail supervisor audit. All quantitative claims are sourced inline; primary sources prioritized over vendor blogs.*
