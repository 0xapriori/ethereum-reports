---
title: "State of Ethereum: March 2026"
date: 2026-03-04
---

# State of Ethereum: March 2026

**Report Date**: March 4, 2026
**Data Sources:** DeFiLlama API, L2BEAT, relayscan.io, rated.network, Electric Capital Developer Report, Token Terminal, RWA.xyz, ultrasound.money, Dune Analytics (hildobby), EIP specifications (eips.ethereum.org), strawmap.org, vitalik.eth.limo
**Methodology:** All quantitative data sourced from verified analytics platforms and public API endpoints. Fee revenue and trading volume are clearly distinguished throughout. Figures marked **(unverified)** could not be cross-referenced against a second independent source.

**Companion Reports:**
- [DEX Market Report (2022–2026)](dex-market-report-2022-2026.md)
- [Lending & Borrowing Market Report (2022–2026)](lending-borrowing-market-report-2022-2026.md)
- [State of MEV: March 2026](../state-of-mev-march-2026.md)

---

## Executive Summary

Ethereum enters March 2026 in a paradoxical position: technically, it is executing the most ambitious protocol upgrade roadmap in its history; yet market sentiment sits at "Extreme Fear" (Fear & Greed Index: 10), and ETH trades at ~$2,050-$2,150 — down ~57% from its August 2025 all-time high of ~$4,953.

### Key Stats Snapshot

| Metric | Value | Source |
|--------|-------|--------|
| ETH Price | ~$2,050-$2,150 | CoinMarketCap |
| ETH Market Cap | ~$250-260B | CoinMarketCap |
| ETH Market Dominance | ~9.9% (all-time low) | CoinMarketCap |
| DeFi TVL (Ethereum) | ~$70B (68% of all-chain) | DeFiLlama |
| Daily Active Addresses | ~837K (30d avg) | Etherscan |
| Total ETH Staked | ~35.9M (~29% of supply) | Dune (hildobby) |
| Staking APR | ~2.9-3.3% | Kraken, StakingRewards |
| Developer Count | 31,869 active | Electric Capital |
| Stablecoin Supply (on Ethereum) | ~$175-181B | DeFiLlama |
| RWA TVL (on Ethereum) | $17B+ | rwa.xyz, The Block |
| Net Annual Issuance | ~+0.2-0.3% (inflationary) | ultrasound.money |

### The Tension

Ethereum occupies its strongest technical position in history. The February 2026 "Strawmap" charts seven forks through 2029, targeting sub-second finality, 10,000 TPS on L1, post-quantum security, and protocol-native privacy. Glamsterdam (H1 2026) will enshrine PBS and enable parallel execution. Hegota (H2 2026) will deliver FOCIL for censorship resistance and native account abstraction. Institutional adoption deepens: $17B+ in tokenized RWAs, $175B+ in stablecoins, ETH ETFs with $11.6B in cumulative inflows.

Yet the market perceives a disconnect. L2s paid only ~$10M to L1 in 2025 (down >90% from $113M in 2024). The ETH/BTC ratio has collapsed to ~0.029. Solana processes 6-10x more daily transactions. Solo stakers represent just 0.5% of staked ETH. The "ultrasound money" thesis has been complicated by post-Dencun inflation.

Vitalik Buterin's response — articulated in his March 2026 "sanctuary technologies" thesis — reframes the question entirely: Ethereum's value lies not in token economics but in its structural properties as censorship-resistant, privacy-preserving, decentralized infrastructure for human freedom.

This report examines whether the engineering reality supports that vision.

---

## Section 1: The New Ethereum Roadmap — The Strawmap

### 1.1 What Is the Strawmap?

On February 25-26, 2026, the Ethereum Foundation published a new holistic L1 protocol upgrade roadmap at [strawmap.org](https://strawmap.org/), maintained by the EF Architecture team: Ansgar Dietrichs (@adietrichs), Barnabe Monnot (@barnabemonnot), Francesco D'Amato (@fradamt), and Justin Drake (@drakefjustin). The document — a portmanteau of "strawman" and "roadmap" — originated as a discussion starter at an internal EF workshop in January 2026, with the explicit goal of integrating Ethereum's longer-term "lean Ethereum" vision with shorter-term fork-level planning.

Vitalik Buterin described the strawmap in an X post as "a very important document," signaling his endorsement of its scope and ambition. Unlike a fixed plan, the strawmap is explicitly framed as "living & malleable" — a shared whiteboard that acknowledges uncertainty while mapping dependencies, sequencing constraints, and fork capacity limits visually. The EF Architecture team commits to at least quarterly updates.

The strawmap represents the most comprehensive and internally coherent articulation of Ethereum's L1 development trajectory since the original merge/surge/verge/purge/splurge framing. It is, in many ways, a maturation of how the protocol community communicates about its future.

### 1.2 Five North Star Goals

The strawmap is organized around five long-range objectives that define what Ethereum's L1 should look like by ~2029-2030:

| North Star | Target | Description |
|------------|--------|-------------|
| **Fast L1** | Finality in seconds | Reduce slot times from 12s down to potentially 2s; achieve transaction finality in 6-16 seconds via the Minimmit BFT consensus algorithm (requires 80%+ honest supermajority, vs. traditional 67% BFT threshold). Current finality is ~16 minutes (2 epochs). |
| **Gigagas L1** | ~1 gigagas/sec (~10K TPS) | Scale L1 execution throughput via ZK-EVMs, real-time proving, parallel verification, and gas repricing. Current L1 throughput is roughly 15-30 TPS depending on transaction mix. |
| **Teragas L2** | ~1 GB/sec DA (~10M TPS) | Scale L2 data throughput to approximately 1 gigabyte per second via data availability sampling (DAS), building on PeerDAS (shipped in Fusaka, December 2025). Current blob capacity is ~0.75 MB per block, with ~29% utilization of target. |
| **Post-Quantum L1** | Hash-based cryptography | Replace all quantum-vulnerable cryptographic primitives (BLS signatures, KZG commitments, EOA ECDSA signatures) with hash-based schemes and STARKs, providing centuries-long cryptographic security. |
| **Private L1** | Shielded ETH transfers | Make privacy a first-class protocol citizen through L1-native shielded transfers, rather than relying solely on application-layer privacy protocols. |

These goals are deliberately aspirational. The Fast L1 target (finality in seconds) would represent a ~100x improvement over current finality times. Gigagas L1 would represent a ~300-600x throughput increase. These are not incremental improvements — they represent a fundamental re-engineering of the protocol's performance envelope.

### 1.3 Fork Cadence and Timeline

The strawmap projects **seven forks through 2029** on a roughly **six-month cadence**, starting from the current position:

| Fork | Target | Status |
|------|--------|--------|
| Glamsterdam | H1 2026 (May/June target) | Devnets active; EIPs finalized |
| Hegota | H2 2026 | Headliners selected; specification work ongoing |
| I* | H1 2027 | Placeholder name; early scoping |
| J* | H2 2027 | Placeholder name |
| K* | H1 2028 | Placeholder name |
| L* | H2 2028 | Noted as "unusual & consequential" — two headliners per layer |
| M* | 2029 | Placeholder name |

Prior named forks in the consensus/execution layer lineage: Altair, Bellatrix, Capella, Deneb, Electra, Fulu (consensus); Berlin, London, Shanghai, Cancun, Prague, Osaka (execution). Fusaka (Fulu + Osaka) activated on mainnet December 3, 2025 at 21:49 UTC, delivering PeerDAS for blob scaling and EIP-7918's blob fee floor.

The six-month cadence is a deliberate acceleration from the historical pattern. The Pectra and Fusaka upgrades were completed in 2025, demonstrating that the new cadence is achievable.

### 1.4 Three-Layer Structure

The strawmap organizes all upgrades into three color-coded horizontal layers, reflecting the distinct engineering teams, specification processes, and testing pipelines that govern each:

- **Consensus Layer (CL)**: Protocol consensus mechanisms — validator duties, fork choice, finality, attestation, and committee selection. CL headliners include ePBS (Glamsterdam) and FOCIL (Hegota).
- **Data Layer (DL)**: Data availability, blob handling, erasure coding, and peer-to-peer data propagation. PeerDAS (shipped in Fusaka) is the foundation; future DL work targets higher blob throughput toward the Teragas L2 goal.
- **Execution Layer (EL)**: Transaction execution, the EVM, gas metering, state trees, and account abstraction. EL headliners include BALs (Glamsterdam) and Binary Trees/Frame Transactions (Hegota).

Each fork is capacity-limited to **one headliner per layer** (with the noted exception of L*), reflecting the practical reality that large, cross-cutting protocol changes require concentrated testing and auditing effort. The "headliner" concept is itself an innovation in Ethereum's planning methodology — it forces prioritization and prevents scope creep that plagued earlier upgrade cycles.

### 1.5 How This Differs from the Old Roadmap Framing

The strawmap represents a significant departure from the "merge/surge/verge/purge/splurge" framework that has guided Ethereum's narrative since 2022:

**Old framing (surge/verge/purge/splurge)**:
- Organized by *thematic goals* (scaling, state management, cleanup, miscellaneous)
- Each phase was treated as somewhat independent
- No explicit fork-level sequencing or capacity constraints
- Communicated primarily through Vitalik's annotated roadmap diagrams on X
- Mixed L1 and L2 concerns without clear layer separation

**New framing (strawmap)**:
- Organized by *protocol layers* (consensus, data, execution) and *fork slots*
- Explicit dependency mapping between features
- Capacity-constrained (one headliner per layer per fork)
- Maintained as a living document by a dedicated team with quarterly updates
- Five North Star goals replace the thematic categories, providing clearer success criteria

The strawmap has also reorganized the old thematic stages into three higher-level workstreams: **Scale** (scaling improvements, successor to "the surge"), **Improve UX** (user experience, spanning account abstraction and privacy), and **Harden L1** (security, quantum resistance, and state management — successor to "the verge/purge"). This is a more operationally useful decomposition, as it maps more cleanly to the actual engineering work.

Critically, the strawmap is *not* a Vitalik-authored personal vision document — it is a collective product of the EF Architecture team, subjected to broader community review. This institutional maturation reflects the protocol's transition from a personality-driven roadmap to a more formalized planning process.

*Sources: [strawmap.org](https://strawmap.org/); [CoinDesk](https://www.coindesk.com/tech/2026/02/26/ethereum-foundation-drops-most-ambitious-roadmap-in-years-targets-finality-in-seconds-by-2029); [The Block](https://www.theblock.co/post/391406/ethereum-foundation-researchers-publish-strawmap-outlining-seven-forks-through-2029); [The Defiant](https://thedefiant.io/news/blockchains/ethereum-foundation-outlines-strawmap-through-2029)*

---

## Section 2: Upcoming Hardforks

### 2a. Glamsterdam (Target: H1 2026)

Glamsterdam is the first upgrade in the strawmap era and the most structurally ambitious Ethereum hardfork since the Merge. Developers have whittled the initial list of 50+ proposed features down to approximately 17 high-impact EIPs, with two consensus/execution headliners and a comprehensive gas repricing package.

**Current development status**: Three EIPs have been tested on Devnet-4. BAL-specific devnets (bals-devnet-2, with BAL Devnet 3 imminent) and ePBS-specific devnets (epbs-devnet-0) are active, with cross-client testing ongoing across Geth, Besu, Prysm, and other clients. Public testnets and dual audit phases are planned for early-to-mid 2026, with a **mainnet target of May-June 2026**.

#### Headliner: EIP-7732 — Enshrined Proposer-Builder Separation (ePBS)

**Authors**: Francesco D'Amato, Barnabe Monnot, Michael Neuder, Potuz, Justin Traglia, Terence Tsao
**Status**: Draft (Standards Track: Core)

EIP-7732 moves proposer-builder separation (PBS) from an off-chain middleware system (MEV-Boost) into Ethereum's consensus protocol itself, eliminating the need for trusted relays.

**How it works**: Rather than including a full execution payload in the beacon block, the proposer includes a signed commitment from a builder specifying the blockhash, payment value, and payload details. A new **Payload Timeliness Committee (PTC)** of 512 validators per slot attests to whether the builder revealed their committed payload on time and with correct data availability.

**Key design elements**:
- **Staked builders**: Builders become in-protocol entities with beacon chain balances (minimum 1 ETH stake), enabling trustless payment enforcement.
- **Delayed execution validation**: Proposers gain 6-9 additional seconds for execution validation, making it safe to increase the gas limit.
- **Unconditional payment guarantee**: The proposer receives payment even in "empty" slots, eliminating the "free option problem" in current MEV-Boost.

**Impact on the MEV supply chain**: ePBS fundamentally restructures the block building pipeline. As documented in the companion [State of MEV: March 2026](../state-of-mev-march-2026.md) report, the current builder market is dominated by Titan Builder (~50.2%) and BuilderNet (~27.9%), with 7 active relays mediating ~99% of block production. ePBS eliminates the relay layer entirely for proposer-builder interaction. This does not eliminate builder centralization (exclusive orderflow advantages persist), but it removes a centralized infrastructure dependency.

#### Headliner: EIP-7928 — Block-Level Access Lists (BALs)

EIP-7928 introduces deterministic, per-block state access maps that record every account and storage slot touched during block execution. This enables:

- **Parallel disk reads**: Nodes can prefetch all required state data before execution begins.
- **Parallel transaction validation**: Transactions touching disjoint state can be validated concurrently.
- **Parallel state root computation**: Post-execution values enable state root updates without re-executing transactions.
- **Executionless state updates**: Light clients and fast-sync nodes can update state directly from BALs.

**Block size impact**: The median block produces a compressed BAL of ~42.6 KB; the 95th percentile block produces ~73.7 KB — modest overhead relative to the performance gains.

BALs are the prerequisite for safely increasing the gas limit. Without parallel verification, higher gas limits mean longer block processing times. With BALs, the relationship between gas limit and verification time becomes sublinear, opening the path toward the Gigagas L1 North Star.

#### EIP-7904: Benchmarked Gas Repricing

EIP-7904 revises gas costs for opcodes, precompiles, memory expansion, and data access based on actual client benchmarking data. Many gas costs have remained unchanged since Ethereum's early days. The proposal targets 18 operations benchmarked below 60 Mgas/s (underpriced relative to their actual CPU cost), which represent potential DoS vectors.

#### Gas Repricing Meta-EIP: EIP-8007

EIP-8007 bundles several interconnected gas model reforms:

- **EIP-8011: Multidimensional Gas Metering** — Introduces 6-dimension gas cost vectors at the block level (compute, access, size, memory, state, history) while preserving single-dimensional gas at the transaction level, enabling higher throughput by accounting for per-resource bottlenecks.
- **EIP-8032: Size-Based Storage Gas Pricing** — Scales SSTORE gas costs based on the number of storage slots a contract owns (tracked via a `storage_count` field), making it progressively more expensive to add storage to contracts with large storage footprints.
- **EIP-8037: State Creation Gas Cost Increase** — Introduces a separate `state_gas` dimension, increasing state creation costs by 3x to 9.5x. This decoupling allows Ethereum to scale compute throughput without proportionally increasing state growth.
- **EIP-8038: State-Access Gas Cost Increase** — Adjusts the cost of reading existing state.

Together, these represent the most comprehensive overhaul of Ethereum's gas model since EIP-1559.

#### FOCIL Consideration and Deferral

EIP-7805 (FOCIL) was initially considered for Glamsterdam but was **deferred to Hegota**. The rationale: FOCIL is sufficiently complex to warrant being a dedicated fork headliner rather than a secondary inclusion alongside ePBS. By sequencing ePBS first (Glamsterdam) and FOCIL second (Hegota), each mechanism can be validated independently before they interact in production.

### 2b. Hegota (Target: H2 2026)

Where Glamsterdam focuses on structural efficiency and relay elimination, Hegota targets **censorship resistance, account model reform, and state tree modernization**.

#### Headliner: EIP-7805 — Fork-Choice Enforced Inclusion Lists (FOCIL)

FOCIL is Hegota's consensus layer headliner and the most important censorship resistance mechanism Ethereum has ever proposed. In each slot, **16 randomly-selected attesters** ("includers") each publish a list of transactions they have observed in the mempool. These inclusion lists are enforced by the fork-choice rule: if a block builder ignores valid transactions from these lists, honest validators deprioritize the block (it is not built upon), effectively forcing compliance without making the block invalid in the consensus validation sense.

**Why this matters**: Currently ~50% of Ethereum blocks are produced by a single builder (Titan), and ~94% by three builders. Even with ePBS removing relay trust assumptions, builder concentration means a small number of entities can delay or censor transactions. FOCIL ensures that transactions would still be included within 1-2 slots, because the 16 includers are randomly selected from the full validator set (~1 million validators).

#### EIP-8141: Frame Transactions (Native Account Abstraction)

EIP-8141 restructures the Ethereum transaction model so that a transaction becomes N "frames" (calls) that can read each other's calldata, with the ability to authorize both the sender and a separate gas payer. This enables gas payment in non-ETH tokens, privacy protocol integration, and quantum-resistant key schemes — all without intermediaries.

**FOCIL + EIP-8141 synergy**: With both deployed, smart wallet transactions, gas-sponsored transactions, and privacy protocol transactions can be included through **17 different randomly-chosen actors** (1 proposer + 16 includers) per slot — the same censorship resistance guarantees as simple ETH transfers.

#### EIP-7864: Unified Binary State Tree

EIP-7864 proposes replacing Ethereum's hexary Keccak Merkle Patricia Tree with a unified binary tree using BLAKE3 (with potential future migration to Poseidon2 for SNARK-friendliness).

**Why binary trees over Verkle trees**: Concerns about quantum computing vulnerabilities in the elliptic curve cryptography that Verkle trees rely on prompted a pivot. Binary trees depend only on hash functions, which are quantum-resistant by nature.

**Performance improvements**: ~4x shorter Merkle branches, 3-4x proving efficiency improvement, client-side proving feasibility.

#### State and History Expiry

State expiry mechanisms are under consideration for Hegota, targeting unbounded state growth. The binary tree reform (EIP-7864) is architecturally synergistic with state expiry: metadata bits in the binary tree structure are designed to support state expiry tracking.

### 2c. I* and Beyond (2027-2029)

#### RISC-V VM Transition

The most radical — and still contested — execution layer change on the horizon: replacing the EVM with a RISC-V-based virtual machine. Vitalik estimates this could speed up ZK proof generation by 50x or more (from removing EVM overhead in proving contexts plus native RISC-V efficiency), though the proposal does not yet have broad consensus. Phased deployment: (1) NewVM for precompiles, (2) user-deployed contracts in RISC-V alongside the EVM, (3) EVM retirement (implemented as a smart contract within RISC-V). Expected consideration in the 2027-2028 timeframe.

#### Post-Quantum Cryptography

Four vulnerable areas requiring quantum-resistant replacements: consensus BLS signatures (→ hash-based + STARK aggregation), DA KZG commitments (→ STARKs), EOA ECDSA signatures (→ AA-enabled migration), and application ZK proofs (→ protocol-layer recursive aggregation). Timeline: 2028-2029.

#### Minimmit Consensus and Fast Finality

One-round-finality BFT consensus replacing multi-round Casper FFG. Slot time trajectory: 12s → 8s → 6s → 4s → 3s → 2s (last two speculative). These changes are expected to be bundled with the post-quantum cryptography transition.

#### Encrypted Mempools (LUCID)

LUCID allows transactions to be included in blocks without their contents being visible until after inclusion, preventing front-running and sandwich attacks at the protocol level. More likely to be implemented in I* or later given technical complexity.

The progression Vitalik has outlined: FOCIL → Big FOCIL → encrypted mempools → transaction ingress layer (Tor, mixnets, Flashnet) → distributed block building.

#### Native Rollup Infrastructure (EXECUTE Precompile)

Enables stateless verification of EVM state transitions within L1, allowing rollups to trustlessly settle without fraud proof delay windows. Combined with the Teragas L2 DA target (~1 GB/sec), this creates infrastructure for ~10 million TPS collective rollup throughput. Expected in 2027-2028. **(unverified)**

#### Slot Time Reductions

Progressive slot time reduction: 12s → 8s → 6s → 4s → 3s → 2s. Each reduction improves user experience, reduces MEV extraction windows, and improves bridge security.

*Sources: [EIP-7732](https://eips.ethereum.org/EIPS/eip-7732); [EIP-7928](https://eips.ethereum.org/EIPS/eip-7928); [EIP-7805](https://eips.ethereum.org/EIPS/eip-7805); [EIP-7864](https://eips.ethereum.org/EIPS/eip-7864); [EIP-8141](https://eips.ethereum.org/EIPS/eip-8141); [strawmap.org](https://strawmap.org/); [CoinDesk](https://www.coindesk.com/tech/2026/03/02/vitalik-buterin-unveils-plan-to-curb-ethereum-block-builder-centralization); [Datawallet](https://www.datawallet.com/crypto/ethereum-glamsterdam-upgrade-explained); [RISC-V VM proposal](https://ethereum-magicians.org/t/long-term-l1-execution-layer-proposal-replace-the-evm-with-risc-v/23617)*

---

## Section 3: Application Activity on Ethereum

*Data Sources: DeFiLlama API, CoinGecko, The Block, Etherscan, protocol documentation. Data retrieved March 4, 2026.*

### 3a. DeFi Overview

Ethereum remains the unambiguous center of gravity for decentralized finance, commanding approximately **$70 billion in DeFi TVL** — roughly **68% of all-chain DeFi TVL** (~$93–96B). Some sources report higher all-chain figures ($130–140B); this discrepancy largely reflects whether liquid staking and restaking are double-counted.

| Metric | Value | Source |
|--------|-------|--------|
| Ethereum DeFi TVL | ~$70B | DeFiLlama |
| All-Chain DeFi TVL | ~$93–96B | DeFiLlama |
| Ethereum Share of Total | ~68% | DeFiLlama |
| DeFi Lending TVL (all chains) | $54.13B | DeFiLlama (589 protocols) |
| Total Stablecoin Supply | ~$311B | DeFiLlama |
| Ethereum Daily Active Addresses | ~837,200 (30d avg) | Etherscan |

#### Top Protocols by TVL on Ethereum

| Rank | Protocol | TVL | Category |
|------|----------|-----|----------|
| 1 | **Lido** | ~$27.5B | Liquid Staking |
| 2 | **Aave** | ~$27B ($26.8B V3) | Lending |
| 3 | **EigenLayer** | ~$17-19.5B **(range reflects cross-source variation; depends on inclusion of non-ETH restaked assets)** | Restaking |
| 4 | **Uniswap** | ~$6.8B | DEX |
| 5 | **Morpho** | ~$6.7B | Lending |
| 6 | **Sky (MakerDAO)** | ~$5.4B | CDP/Lending |
| 7 | **Fluid** | ~$2.0B | Unified Liquidity |
| 8 | **Compound V3** | ~$1.3B | Lending |

Outstanding DeFi borrows exceed **$12 billion** across all chains, with Aave commanding ~56.5% of total DeFi debt. Active borrows have grown 37.2% year-over-year through 2025.

> **Cross-reference:** For deeper DeFi analysis, see the companion [Lending & Borrowing Market Report (2022–2026)](lending-borrowing-market-report-2022-2026.md) and [DEX Market Report (2022–2026)](dex-market-report-2022-2026.md).

### 3b. DEX Activity

DEX trading reached unprecedented scale in 2025, with **$4.83 trillion in total spot volume** and **$7.95 trillion in derivatives volume** — on-chain perpetual futures now generate 1.6x the volume of on-chain spot trading.

| Metric | 2024 | 2025 | 2026 YTD (Jan 1 – Mar 3) |
|--------|------|------|--------------------------|
| DEX Spot Volume | $2.63T | $4.83T | $654B |
| Derivatives DEX Volume | $2.62T | $7.95T | $1.80T |
| Aggregator Volume | $781B | $1.62T | $181B |
| DEX Protocols Tracked | ~700 | ~900 | 1,026 |

Ethereum mainnet monthly DEX volume has settled in the **$52–86 billion range**, with February 2026 recording ~$56.5B. **67.5% of Uniswap trading volume** now occurs on Layer 2 networks. The DEX-to-CEX ratio has reached approximately **14%** in early 2026.

Solana emerged as the highest-volume individual chain for DEX activity in early 2025, driven by meme coin speculation — $117.7B in January 2026 alone. However, Ethereum mainnet + L2s collectively exceed Solana's DEX volume.

> **Cross-reference:** For full DEX analysis, see the companion [DEX Market Report (2022–2026)](dex-market-report-2022-2026.md).

### 3c. Lending Activity

Aave's crossing of **$1 trillion in cumulative lending volume** on February 25, 2026 is arguably the most significant operational milestone in DeFi history — a single permissionless smart contract system has intermediated more credit than many national banking systems.

| Metric | Value |
|--------|-------|
| DeFi Lending TVL | $54.13B |
| CDP Protocol TVL | $7.59B |
| Combined Lending + CDP | $61.72B |
| Aave Cumulative Volume | $1T+ |
| Aave 30d Fee Revenue | $83.3M |
| Aave Market Share | ~56.5% of DeFi debt |

By Q2 2025, DeFi commanded **59.83% of all collateralized crypto lending**, a complete reversal from the CeFi-dominated pre-2022 era. The largest DeFi lending incident — Euler Finance ($197M, March 2023) — saw all funds recovered within ~3 weeks. No major DeFi lending protocol has failed due to insolvency.

> **Cross-reference:** For complete lending analysis, see the companion [Lending & Borrowing Market Report (2022–2026)](lending-borrowing-market-report-2022-2026.md).

### 3d. Stablecoin Activity & Payments

Stablecoins are Ethereum's most unambiguous product-market fit. The network settled a record **$8 trillion in stablecoin transfer volume in Q4 2025** — an all-time high for any single blockchain in a single quarter.

| Stablecoin | Market Cap (March 2026) | Notes |
|------------|------------------------|-------|
| USDT (Tether) | ~$183.7B | ~60.7% of total stablecoin market; >$90B on Ethereum |
| USDC (Circle) | ~$76.3B | Primary institutional settlement coin |
| DAI/USDS (Sky) | ~$5.36B | Largest decentralized stablecoin |
| USDe (Ethena) | ~$5-6B | Synthetic dollar |

**Payment infrastructure.** A McKinsey/Artemis Analytics study estimated that **actual stablecoin payment volume reached ~$390 billion in 2025** — approximately 1.1% of the $35 trillion that flowed through stablecoin networks. **B2B payments account for ~$226B (58%)** with 733% year-over-year growth. Base emerged as the primary institutional USDC rail, with Shopify/Stripe integration for merchants across 34 countries. In February 2026, Stripe integrated the **x402 protocol** for autonomous AI agent payments on Base — agents making per-request USDC micropayments ($0.01/request) to APIs.

*Sources: [CoinTelegraph](https://cointelegraph.com/news/stablecoin-transfer-volume-ethereum-tops-8t-q4-ath); [McKinsey](https://www.mckinsey.com/industries/financial-services/our-insights/stablecoins-in-payments-what-the-raw-transaction-numbers-miss); [The Block / Stripe x402](https://www.theblock.co/post/389352/stripe-adds-x402-integration-usdc-agent-payments)*

### 3e. Real-World Assets (RWAs)

Tokenized real-world assets on Ethereum have undergone a phase transition from experimental niche to institutional infrastructure. Ethereum's tokenized RWA market has surpassed **$17 billion**, representing approximately **315% year-over-year growth** from ~$4.1B in early 2025.

| Asset Class | On-Chain Value | Growth |
|-------------|---------------|--------|
| Tokenized US Treasuries | ~$8.7-9B+ | Primary growth engine; BlackRock BUIDL, Ondo OUSG |
| Tokenized Commodities (Gold) | ~$6B+ | 177% growth in 2025 |
| Private Credit | ~$3-4B | Centrifuge, Maple, Goldfinch |
| Tokenized Equities / Funds | Emerging | Expected major growth post-regulatory clarity |

**Key protocols**: Ondo Finance (~$2.7B TVL, 77% on Ethereum), Centrifuge ($1B+ TVL), BlackRock BUIDL.

**Projections**: Standard Chartered projects the total tokenized RWA market will reach **$2 trillion by end of 2028**, with the "vast majority" on Ethereum.

*Sources: [The Block](https://www.theblock.co/post/390130/ethereum-tokenized-rwa-market-jump); [rwa.xyz](https://app.rwa.xyz/treasuries); [Standard Chartered](https://www.theblock.co/post/377059/standard-chartered-tokenized-rwa-2-trillion-2028-ethereum)*

### 3f. Agentic Commerce: ERC-8004 & x402

The infrastructure for autonomous AI agents to transact on-chain is materializing rapidly — and Ethereum is at the center of both the identity and payment layers.

#### ERC-8004: Trustless AI Agent Identity

[ERC-8004](https://eips.ethereum.org/EIPS/eip-8004) ("Trustless Agents") provides a standardized on-chain identity, reputation, and validation framework for AI agents. Proposed by engineers from MetaMask (Marco De Rossi), Ethereum Foundation (Davide Crapis), Google (Jordan Ellis), and Coinbase (Erik Reppel), with contributions from ENS, EigenLayer, The Graph, Taiko, Nethermind, and OpenZeppelin. Created August 13, 2025; reference implementation contracts deployed to **Ethereum mainnet on January 29, 2026**. The ERC remains in **Draft** status.

ERC-8004 introduces three lightweight, composable on-chain registries deployable as per-chain singletons:

| Registry | Function | Key Details |
|----------|----------|-------------|
| **Identity** | ERC-721-based agent registration | Global agent IDs; URI points to registration JSON with service endpoints (web, A2A, MCP, OASF, ENS, DID); includes `x402Support` flag |
| **Reputation** | Standardized on-chain feedback | Fixed-point scoring with tags, off-chain file integrity via KECCAK-256, revocable feedback with response capability |
| **Validation** | Generic validator hooks | Stake-secured re-execution, zkML proofs, TEE oracles; returns 0-100 assessment scores |

**Adoption:** Crypto media (citing 8004 Scan) reports **~45,000 registered agents** across multiple chains within weeks of launch **(unverified — not independently auditable on-chain)**. The October 2025 launch announcement cited over 100 companies building on the protocol, with development groups reportedly attracting 1,000+ builders. The EF's decentralized AI (dAI) team formally unveiled the standard in October 2025 and incorporated it into their 2026 roadmap. Testnet deployments on Ethereum Sepolia, Base Sepolia, Linea Sepolia, and Hedera Testnet preceded mainnet.

#### x402: Machine-to-Machine Payments

The [x402 protocol](https://www.x402.org/) revives the HTTP 402 "Payment Required" status code for instant, automatic stablecoin payments over HTTP. Developed by Coinbase's Developer Platform team and launched in **May 2025** (V2 upgrade December 11, 2025, evolving the protocol into comprehensive payment infrastructure), it enables a simple four-step flow: client requests resource → server responds with HTTP 402 + payment instructions → client sends payment via header → server verifies payment through a facilitator, settles on-chain, delivers resource. Zero protocol fees (only nominal network gas).

**x402 Foundation:** Co-founded by **Coinbase and Cloudflare**, with members including **Google, Visa, AWS, Circle, Anthropic, and Vercel**. Governance details still forthcoming.

| Metric | Value | Source |
|--------|-------|--------|
| Total Transactions | **115M+** (est. March 2026) | CryptoSlate, The Block (100M+ by end 2025; ~15M+ in Jan 2026) |
| Total Volume | **$24M+** | x402.org (late 2025 snapshot; current figure likely higher) |
| Solana Share | ~49% of agent-to-agent payments | ETHNews (week ending Feb 9, 2026) |

**Important nuance for Ethereum:** x402's primary facilitator runs on **Base** (Coinbase's L2) and **Solana**, not Ethereum L1 directly. Ethereum mainnet support exists via the open-source `@x402/evm` package and third-party facilitators (e.g., Primev building sub-second settlement via preconfirmations), but Base is the default settlement layer. x402 is also live on 26+ additional chains via Thirdweb integration.

**Stripe integration (February 11, 2026):** Stripe announced x402 support in **preview** — enabling AI agents to make automated USDC payments on Base. CoinGecko simultaneously activated x402, allowing agents to pay $0.01 USDC per API request. This integration layers stablecoin micropayments onto Stripe's existing sales tax, refund, and reporting infrastructure.

**Visa Trusted Agent Protocol (TAP):** Visa's identity and trust verification protocol for AI agent commerce, built on **RFC 9421** (HTTP Message Signatures) and aligned with web-bot-auth. TAP is designed as **complementary** to x402 — Visa handles identity/trust verification while x402 handles crypto-native payment execution. Visa and Coinbase are collaborating to align the two protocols.

#### The Agentic Commerce Macro Thesis

McKinsey's October 2025 report ["The agentic commerce opportunity"](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-agentic-commerce-opportunity-how-ai-agents-are-ushering-in-a-new-era-for-consumers-and-merchants) projects **$3-5 trillion** in global agentic commerce by 2030 ($1 trillion US retail alone). Morgan Stanley (December 2025) offers a more conservative estimate of **$190-385 billion** in US e-commerce by 2030. Both projections cover all "orchestrated" commerce — revenue influenced or directed by AI agents — including traditional payment rails — not crypto specifically. But the on-chain subset is growing: CoinGecko tracked **550+ AI agent crypto projects** with ~$4.34B combined market cap (October 2025), with a DeFAI-specific subset reported at ~$1.62B (CoinGecko DeFAI category showed ~$601M by March 2026, reflecting broader market declines).

For Ethereum, ERC-8004 and x402 together create the foundation for a new class of autonomous market participants — agents that source liquidity, execute arbitrage, manage LP positions, route orders, and make payments without human intervention. The implications cut across every application category in this report: DEX volume from agent trading bots, lending demand from agents borrowing to fund operations, stablecoin payment volume from machine-to-machine micropayments, and MEV from agent-on-agent competition.

*See companion reports: [DEX Projections (Section 3.4)](dex-projections-2026-2029.md) and [Lending Projections (Section 6)](lending-projections-2026-2029.md) for detailed agentic commerce impact modeling.*

*Sources: [eips.ethereum.org/ERC-8004](https://eips.ethereum.org/EIPS/eip-8004); [x402.org](https://www.x402.org/); [Coinbase x402 launch](https://www.coinbase.com/developer-platform/discover/launches/x402); [Cloudflare x402 Foundation](https://blog.cloudflare.com/x402/); [Stripe x402 preview](https://docs.stripe.com/payments/machine/x402); [Visa TAP](https://investor.visa.com/news/news-details/2025/Visa-Introduces-Trusted-Agent-Protocol-An-Ecosystem-Led-Framework-for-AI-Commerce/default.aspx); [McKinsey](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-agentic-commerce-opportunity-how-ai-agents-are-ushering-in-a-new-era-for-consumers-and-merchants); [CoinDesk ERC-8004](https://www.coindesk.com/markets/2026/01/28/ethereum-s-erc-8004-aims-to-put-identity-and-trust-behind-ai-agents/); [crypto.news](https://crypto.news/ethereum-erc-8004-ai-agents-mainnet-launch-2026/)*

### 3g. Privacy Activity

Privacy on Ethereum today remains **fundamentally inadequate**. Every transaction, balance, and interaction is fully transparent on-chain. As Vitalik Buterin wrote in his April 2025 blog post ["Why I support privacy"](https://vitalik.eth.limo/general/2025/04/14/privacy.html): "Privacy is an important guarantor of decentralization: whoever has the information has the power."

#### Current Privacy Protocols

**Privacy Pools (0xbow)**: Launched March 31, 2025 — uses ZK proofs combined with ASP compliance screening. ~$6M lifetime volume for >1,500 users. Buterin made the first deposit. ($3.5M seed round, November 2025.)

**Railgun**: Longest-running privacy protocol. TVL grew nearly tenfold from $11M (2024) to **~$106-113M** (late 2025). $4.5B cumulative volume. Full DeFi composability from within the shielded set.

**Zama (FHE)**: First deployment of Fully Homomorphic Encryption-based confidential transactions on Ethereum mainnet (December 30-31, 2025). ~$121M TVS. Current throughput ~20 TPS, targeting 500-1,000 TPS.

**Tornado Cash delisting**: US Treasury/OFAC delisted Tornado Cash from sanctions in March 2025 following the Fifth Circuit ruling, removing the chilling effect on privacy tool development.

#### Ecosystem Initiatives

**Vitalik's $45M personal commitment**: 16,384 ETH withdrawn in late January 2026 to fund "an open-source, secure and verifiable full stack."

**Kohaku (EF wallet privacy framework)**: Open-source wallet SDK for building private-by-default wallets. Features private sending/receiving via client-side ZK proofs, IP obfuscation, and separate accounts per dApp.

**Flashnet (Flashbots, February 2026)**: Low-latency anonymous broadcast protocol addressing network-layer deanonymization.

#### Privacy Stack Summary

| Layer | Current State | Near-Term (2026) | Medium-Term (2027-2029) |
|-------|--------------|------------------|------------------------|
| **Application** | Privacy Pools, Railgun, Zama (~$230M+ TVS) | Kohaku private wallet SDK | Private-by-default wallets as standard |
| **Mempool** | Flashbots Protect (trusted relay) | LUCID encrypted mempool (Hegota) | Fully encrypted mempools |
| **Network** | Standard gossip (no anonymity) | Flashnet anonymous broadcast | Tor/mixnet integration |
| **Protocol** | Fully transparent L1 | Privacy-preserving account abstraction | Private L1 with shielded transfers (Strawmap) |

*Sources: [vitalik.eth.limo](https://vitalik.eth.limo/general/2025/04/14/privacy.html); [The Block](https://www.theblock.co/post/387804/vitalik-buterin-commits-roughly-45-million-in-eth-to-open-source-security-and-privacy-projects); [Flashbots Writings](https://writings.flashbots.net/network-anonymized-mempools)*

---

## Section 4: ETH the Asset

### 4a. Price & Market Context

As of March 4, 2026, ETH trades at approximately **$2,050-$2,150**, with a market cap of ~$250-260B. ETH's market dominance has fallen to ~**9.9%** — the lowest in its history. The Crypto Fear & Greed Index stands at **10** ("Extreme Fear").

**ETH/BTC ratio**: Collapsed to ~**0.029**, down from ~0.055 in mid-2025 and well below the 0.08+ levels of 2021-2022. The pair hit a five-year low of 0.01766 in Q1 2025. Whale positioning shows a 7-to-1 short bias ($491M shorts vs. $68M longs).

**ETH ETF landscape**: U.S. spot Ethereum ETFs have accumulated ~$11.6B in cumulative net inflows since inception. February 2026 saw four consecutive weeks of outflows ($2.76B total from peak). Exchange ETH reserves have dropped to 16M ETH (record low). **Staking within ETH ETFs** remains unapproved by the SEC — a significant structural yield disadvantage.

*Sources: [CoinGlass](https://www.coinglass.com/eth-etf); [CoinGecko](https://www.coingecko.com/research/publications/solana-vs-ethereum-2025); [CCN](https://www.ccn.com/analysis/crypto/btc-xrp-eth-crash-chatgpt-claude-feb-2026-price-prediction/)*

### 4b. The Issuance Debate

**Minimum Viable Issuance (MVI)** is the principle that Ethereum should not issue more ETH than necessary for consensus security. Formalized by Anders Elowsson ([notes.ethereum.org](https://notes.ethereum.org/@anderselowsson/MinimumViableIssuance)), MVI argues that over-issuance acts as an implicit tax on non-staking holders, reduces ETH's utility, and drives excessive stake centralization.

**Current staking metrics (March 2026):**
- Total ETH staked: ~35.9M ETH (~29% of supply)
- Staking yield: ~2.9-3.3% APR
- Net staking flows: Turned negative in late 2025, reaching a local low near -600,000 ETH by early January 2026

**No issuance curve change is scheduled for Glamsterdam or Hegota.** This remains a research-stage discussion. The political economy is difficult: liquid staking protocols have strong incentives to oppose reductions.

### 4c. Ultrasound Money Thesis — Complicated

The "ultrasound money" thesis has had a turbulent three years:

- **Post-Merge (Sep 2022 - Mar 2024)**: EIP-1559 burns regularly exceeded issuance. ETH supply declined. Thesis appeared validated.
- **Post-Dencun (Mar 2024 onwards)**: Blobs slashed L2 data costs by 90%+, but also collapsed blob fee burn. Net issuance flipped inflationary.
- **Post-Fusaka (Dec 2025)**: EIP-7918 introduced a blob fee floor tying minimum blob fees to L1 execution costs. Partial correction.

**Current net issuance (March 2026)**: ETH supply is net inflationary at approximately **+0.2-0.3%/year** (~286K ETH net added annually). Lower than Bitcoin's ~0.8%/year, but *not* the deflation the ultrasound thesis promised. Whether EIP-7918 and growing L2 activity will push burn above issuance depends on sustained blob demand growth.

ETH's long-term value does not come from a supply cap or guaranteed deflation. ETH is the native asset of a global, programmable, censorship-resistant settlement layer. Its value comes from *utility* — gas payments, staking collateral, DeFi collateral, and its role as the base-layer asset processing trillions in economic activity. Deflationary dynamics are a nice side effect of high usage, not a design goal.

*Sources: [ultrasound.money](https://ultrasound.money); [CoinLedger](https://coinledger.io/learn/ultrasound-money); [Fidelity Digital Assets](https://www.fidelitydigitalassets.com/research-and-insights/fusaka-upgrade-scaling-meets-value-accrual)*

### 4d. Liquid Staking & Staking Distribution

| Category | ETH Staked (approx.) | Share of Total |
|----------|---------------------|----------------|
| Liquid staking (Lido, Rocket Pool, etc.) | ~10.5M ETH | 31.1% |
| Centralized exchanges (Coinbase, Binance, Kraken) | ~8.1M ETH | 24.0% |
| Staking pools | ~6.0M ETH | 17.7% |
| Liquid restaking | ~2.2M ETH | 6.6% |
| Unidentified | ~6.8M ETH | 20.1% |
| Solo stakers | ~0.18M ETH | **0.5%** |

**Lido**: ~8.7M ETH, ~24.2% market share (down from 32%+ peak). **Binance**: ~3.3M ETH, ~9.1%. **Coinbase**: ~1.8M ETH directly attributed (~5% of staked ETH; some sources report higher figures including all Coinbase-controlled validators). **Rocket Pool**: ~390K rETH, ~4,027 node operators, Saturn I upgrade launched February 18, 2026 with "megapools."

**The solo staker crisis**: At just 0.5% of staked ETH, solo staking is in critical condition. The 32 ETH minimum (~$64K+), technical complexity, and liquid staking alternatives have made solo staking economically irrational for most participants. This is a genuine threat to Ethereum's decentralization thesis.

*Sources: [Datawallet](https://www.datawallet.com/crypto/ethereum-staking-statistics-and-trends); [CoinLaw](https://coinlaw.io/eth-staking-statistics/); [hildobby Dune dashboard](https://dune.com/hildobby/eth2-staking)*

### 4e. Restaking Ecosystem

| Protocol | TVL (approx.) | Market Share |
|----------|--------------|-------------|
| EigenLayer | ~$17-19.5B | ~93-94% |
| Symbiotic | ~$897M-$1.7B | ~5-6% |
| Karak | ~$102M | ~0.6% |

EigenLayer dominates with 4.36M+ ETH restaked. In 2025, it evolved into a broader economic system: EigenDA (100 MB/s data availability), EigenAI (verifiable AI inference), EigenCompute (verifiable off-chain execution), EigenCloud ($70M a16z investment).

**Systemic risk concerns** remain serious: correlated slashing risk across AVSs, leverage compounding (same ETH secures multiple protocols), 93%+ concentration in a single protocol, and Ethereum consensus overload risk. As Vitalik wrote in ["Don't overload Ethereum's consensus"](https://vitalik.eth.limo/general/2023/05/21/dont_overload.html), the total value secured by AVSs must remain well below the total value of restaked ETH for the security model to hold.

*Sources: [DefiLlama](https://defillama.com/protocol/eigenlayer); [P2P.org](https://p2p.org/economy/2025-the-year-eigenlayer-became-an-economic-system/); [LlamaRisk](https://llamarisk.com/research/current-state-of-symbiotic)*

---

## Section 5: The L1/L2 Economic Relationship

### 5a. Historical Fee Revenue Evolution

**IMPORTANT: Fee revenue and trading volume are fundamentally different metrics and must never be conflated.**

| Period | Ethereum L1 Fee Revenue | Context |
|--------|------------------------|---------|
| Q4 2021 | ~$4.3B | All-time high |
| Full Year 2024 | ~$2.48B | Pre/post-Dencun split |
| Q1 2025 | ~$217M | 95% below Q4 2021 peak |

| Year | L2 Total Revenue | Paid to Ethereum L1 | L1 Capture Rate |
|------|-----------------|---------------------|-----------------|
| 2024 | ~$277M | ~$113M | ~41% |
| 2025 | ~$129M | ~$10M | ~8% |

The >90% year-over-year decline in L1 value capture from L2s is the most important economic data point in Ethereum's recent history. **Base case study**: ~$92M revenue in 2024, only $4.9M to L1 in blob fees (~5% capture rate). Data posting costs fell from $9.34M (Q1 2024, pre-Dencun) to $42K (Q3 2024).

*Sources: [Token Terminal](https://tokenterminal.com/explorer/projects/ethereum/metrics/fees); [CryptoSlate](https://cryptoslate.com/insights/ethereum-layer-2-revenue-hits-277-million-in-2024-spearheaded-by-bases-92-million/)*

### 5b. Blob Fee Market Reforms

| Upgrade | Date | Blob Target / Max | Key Change |
|---------|------|-------------------|------------|
| Dencun (EIP-4844) | March 13, 2024 | 3 / 6 | Blobs introduced; separate fee market |
| Pectra (EIP-7691) | May 7, 2025 | 6 / 9 | Target doubled |
| Fusaka (PeerDAS + EIP-7918) | December 3, 2025 | 6 / 9 (PeerDAS-enabled) | Data availability sampling; fee floor |
| BPO1 | December 9, 2025 | 10 / 15 | First parameter increase post-PeerDAS |
| BPO2 | January 7, 2026 | 14 / 21 | Current parameters |

**EIP-7918** establishes a floor price for blob gas tied to L1 execution gas costs. Post-Fusaka activation, blob base fees jumped ~15 million-fold. Fidelity Digital Assets estimates an additional **~$78.6M** in blob revenue had the floor been active since Dencun.

**Current utilization**: ~29% of 14-blob target. The blob market remains in massive surplus.

> **Cross-reference:** For detailed blob fee market analysis, see *State of MEV: March 2026*, Section 7e.

### 5c. L2 Ecosystem Landscape

**Market concentration: Base + Arbitrum = 77% of L2 DeFi TVL.**

| L2 Network | Share of L2 DeFi TVL | Key Metrics |
|------------|---------------------|-------------|
| Base | ~46.6% | ~596K DAU; only consistently profitable L2 |
| Arbitrum One | ~30.9% | Largest by total TVL; Stylus (WASM VM) upgrade |
| OP Mainnet | ~6% | Superchain hub (34+ OP Stack chains) |
| Starknet | ~2-3% | Grinta upgrade, sequencer decentralization |
| Scroll | ~2-3% | ~$748M TVL **(unverified)** |

**Enterprise rollups**: Base (Coinbase), INK (Kraken), Unichain (Uniswap), Soneium (Sony), Robinhood Chain (testnet). All chose OP Stack or Arbitrum Orbit.

**Zombie chain problem**: 21Shares projects most small L2s will not survive 2026. Base, Arbitrum, and Optimism process nearly 90% of all L2 transactions. Kinto shut down; Loopring shut down its wallet app (L2 protocol remains operational); Blast TVL collapsed 97%.

**L2BEAT Stages**: No major rollup has reached Stage 2 (fully permissionless, no governance override). Top 3 are at Stage 1.

*Note: Individual L2 deep-dives to be covered in separate reports.*

### 5d. Based Rollups

Based rollups delegate transaction ordering to Ethereum L1 validators, inheriting full decentralization and censorship resistance. Revenue flows directly to L1 validators rather than centralized sequencer operators.

**Taiko**: Flagship based ZK rollup. Mainnet since May 2024, 50M+ transactions, zero downtime. **ENS Namechain** adopted Taiko stack (testnet Q2 2026). Other projects: Puffer UniFi, Spire Labs (Based Stack), RISE. **FABRIC** initiative standardizes preconfirmation infrastructure.

Primary tradeoff: latency bounded by L1 slot times (12s, improving to 2-4s per roadmap).

### 5e. Native Rollups and the Post-Rollup-Centric Roadmap

**Vitalik Buterin (February 3, 2026)**: *"The original vision of L2s and their role in Ethereum no longer makes sense, and we need a new path."* Two driving factors: L2 decentralization has lagged (no major rollup at Stage 2), and L1 is scaling directly (low fees, gas limits expected to reach 100M-200M).

The **EXECUTE precompile** would enable native rollups with automatic EVM upgrade inheritance, no security councils, and hard-fork protection. Major L2 teams (Arbitrum, Base, Optimism, Scroll) have expressed interest in "going native." Timeline: 2027-2028+.

*Sources: [The Block](https://www.theblock.co/post/388285/vitalik-buterin-reevaluates-rollup-centric-roadmap-arguing-l2s-decentralized-far-slower-while-ethereum-base-layer-advanced); [Decrypt](https://decrypt.co/356841/we-need-new-path-ethereum-founder-vitalik-buterin-rips-up-l2-focused-roadmap)*

### 5f. The Value Accrual Question

**Paths to value accrual:**
1. **Blob demand growth**: Utilization approaching target triggers congestion pricing
2. **Based rollups**: Sequencing revenue flows directly to L1 validators
3. **Native rollups**: L1 as indispensable settlement layer, not merely DA provider
4. **L1 scaling**: Cheaper, faster L1 → more direct L1 DeFi execution
5. **ETH as money**: Persistent demand as collateral, gas, and settlement asset across all L2s

The structural tension: cheap L2 DA is essential for scaling but reduces L1 revenue. EIP-7918 partially addresses this, but the long-term solution requires demand growth, architectural alignment, and ETH's base-asset role.

---

## Section 6: Developer Activity

The [Electric Capital Developer Report (2025)](https://www.developerreport.com) confirms Ethereum's position as the largest developer ecosystem:

| Ecosystem | Total Active Developers | New Developers (Jan-Sep 2025) | FT Dev YoY Growth |
|-----------|------------------------|-------------------------------|-------------------|
| **Ethereum** | **31,869** | **16,181** | **+5.8%** |
| Solana | 17,708 | 11,534 | +29.1% |
| Bitcoin | 11,036 | ~7,494 | Moderate |
| Polkadot | ~2,400 monthly active | ~1,000 (2024) | Stabilizing |

Ethereum's 31,869 total active developers is nearly **double** Solana's 17,708. Ethereum maintained #1 on **every continent**. However, Solana's 29.1% growth rate vs. Ethereum's 5.8% reflects the classic large-base/low-rate pattern.

**L2 developer shift**: More than half of Ethereum developers are now committed to L2 development. Base contributed **42% of new code** in the Ethereum ecosystem in 2024.

**Multi-chain**: 1 in 3 developers (34%) now work across multiple chains. **74% of multi-chain developers work on EVM chains**. Since 2021, the proportion of EVM cross-chain deployers has **quadrupled**.

**AI-assisted development**: AI tools have seen rapid adoption — AI-powered auditing, Solidity-focused code generation, and formal verification acceleration. Projects that would have taken 3-6 months are being completed in weeks, though security implications of AI-generated smart contract code remain an active concern. **(The specific "85% adoption surge" figure could not be independently verified.)**

*Sources: [Electric Capital](https://www.developerreport.com); [CryptoNinjas](https://www.cryptoninjas.net/news/ethereum-dominates-2025-developer-landscape-with-over-16k-new-builders/); [Messari State of Polkadot Q1 2025](https://messari.io/report/state-of-polkadot-q1-2025)*

---

## Section 7: Ethereum vs. Competition (Solana Focus)

### 7.1 The Activity Gap: Raw Numbers Favor Solana

| Metric | Ethereum Mainnet | Ethereum + L2s | Solana |
|--------|-----------------|----------------|--------|
| Daily Active Addresses | ~400-500K | ~2.5-3M | ~3.25-5M |
| Daily Transactions | ~1.1M | ~2.88M+ | ~62-148M |
| Observed TPS | ~15-30 | Varies by L2 | ~1,500-4,000 |
| Block Time | 12 seconds | 2s (some L2s) | ~400ms |

**Important caveats**: Solana's low costs (~$0.00025/tx) inflate DAU via sybil/bot activity. A single Ethereum transaction often represents complex DeFi operations requiring multiple Solana transactions. L2 fragmentation means Ethereum's activity is spread across dozens of rollups.

### 7.2 Where Ethereum Dominates: Value, Not Volume

| Metric | Ethereum | Solana | Ratio |
|--------|----------|--------|-------|
| DeFi TVL | ~$70B | ~$9.2B | ~7.6x |
| Stablecoin Supply | ~$175B | ~$14B | ~12.5x |
| RWA Value On-Chain | ~$17B+ (~34% of total on-chain RWA) | Growing but small | >10x |

BlackRock's 2026 Thematic Outlook named Ethereum as underpinning **65% of tokenized assets**. The institutional preference reflects Ethereum's battle-tested security, deep liquidity, and regulatory familiarity.

### 7.3 The "Digital Oil" Framing

**Ethereum** functions as a **settlement and security layer** — optimized for high-value transactions, complex financial instruments, and institutional DeFi. **Solana** functions as a **high-throughput execution layer** — optimized for consumer applications, payments, and speed-sensitive use cases.

The stablecoin data illustrates this: Ethereum holds ~12.5x more stablecoin supply (stock/trust), while Solana processed $650B in stablecoin transaction volume in February 2026 (flow/activity). Both matter; they measure different things.

### 7.4 Competitive Risks

1. **User experience gap**: Solana's sub-second finality and sub-cent fees create dramatically better consumer UX
2. **Ecosystem fragmentation**: Multi-L2 architecture introduces bridging friction
3. **Developer momentum**: Solana's 29.1% growth rate narrows the gap within 2-3 years if sustained
4. **Narrative capture**: Solana more effective at consumer-facing mindshare
5. **DEX volume**: Solana overtook Ethereum in DEX volume in early 2026 ($117B)

Ethereum's roadmap directly addresses several gaps: faster slots (12s → 2s target, via incremental reductions), scaling improvements (Glamsterdam), account abstraction (EIP-8141), and FOCIL for guaranteed inclusion — a property Solana's single-leader model cannot match.

*Sources: [CoinLedger](https://coinledger.io/tools/solana-vs-ethereum); [DefiLlama](https://defillama.com); [CoinDesk / BlackRock](https://www.coindesk.com/business/2026/01/21/blackrock-names-crypto-and-tokenization-as-themes-driving-markets-in-2026)*

---

## Section 8: Sanctuary Technologies & Vitalik's Vision

### 8a. The Sanctuary Technologies Thesis

On March 3-4, 2026, Vitalik Buterin published a series of posts articulating what may be the most significant reframing of Ethereum's purpose since its founding:

> "Free open-source technologies that let people live, work, talk to each other, manage risk and build wealth, and collaborate on shared goals, in a way that optimizes for robustness to outside pressures."

Buterin described Ethereum as the **"wrong-shaped tool"** for "fixing the world," arguing that beyond a certain point, that mission "implies a form of power projection that is more like a centralized political entity than like a decentralized technology community." Instead, Ethereum's role is to create **"digital space"** — persistent shared objects representing social arrangements. Money is the obvious example. Multisignature wallets, governance structures, identity systems are others.

### 8b. De-Totalization

The intellectual core is **"de-totalization"**: preventing any single actor from achieving total control while also preventing any loser from suffering total defeat. In Buterin's words, the aim is "reducing the stakes of the war in heaven."

This builds on two December 2025 blog posts:
- **["Balance of Power"](https://vitalik.eth.limo/general/2025/12/30/balance_of_power.html)** — Urging crypto projects to develop "decentralization models" alongside business models
- **["Let a Thousand Societies Bloom"](https://vitalik.eth.limo/general/2025/12/17/societies.html)** — Arguing that community membership should be a matter of choice, not accident of birth

### 8c. Not Apple, Not Google

Buterin urged developers to "not try to be Apple or Google, seeing crypto as a tech sector that enables efficiency or shininess." Ethereum is a different kind of thing: a **shared digital space with no owner**. The value proposition is structural — persistent digital objects that no single party can censor, freeze, or unilaterally modify.

Under the platform model, the priority is UX polish. Under the sanctuary model, the priority is robustness, privacy, and censorship resistance.

### 8d. The Full-Stack Vision

Buterin called for building toward a full-stack ecosystem: applications → wallets → protocols → operating systems → hardware → security infrastructure. If users access Ethereum through closed-source wallets on surveilled operating systems on locked-down hardware, protocol-level guarantees are undermined.

### 8e. Allies Beyond Crypto

Buterin identified non-crypto allies: **Starlink** (connectivity resilience), **locally-running open-weights LLMs** (AI independence), **Signal** (communications privacy), and **Community Notes** (distributed fact-checking). The inclusion of Starlink drew controversy; Buterin's response was characteristically precise: evaluate technologies on structural properties, not creator politics.

### 8f. The Cypherpunk Lineage

The trajectory: **"Make Ethereum Cypherpunk Again"** (December 2023) → **defensive acceleration (d/acc)** (November 2023, expanded through 2024) → **privacy as fundamental right** (2025) → **sanctuary technologies** (2026). Each iteration broadens scope.

### 8g. The Narrative Shift

| Era | Dominant Narrative | Core Metric | Failure Mode |
|-----|--------------------|-------------|--------------|
| 2015-2017 | World Computer | dApp count | Overpromised on generality |
| 2017-2021 | DeFi/NFT Platform | TVL, floor prices | Financialization crowded out utility |
| 2021-2023 | Ultrasound Money | ETH burn rate, supply deflation | Tied value to fee revenue |
| 2023-2025 | Cypherpunk/d/acc | Privacy features, censorship resistance | Narrowly technical |
| 2026- | Sanctuary Technologies | Robustness to outside pressures | TBD |

The sanctuary framing redirects attention from token economics to *structural properties*. The question shifts from "Is ETH deflationary?" to "Does this technology reduce the ability of powerful actors to control, surveil, or exclude people?"

### 8h. Assessment

The sanctuary thesis resolves tensions in earlier framings: "world computer" was too ambitious, "ultrasound money" too narrow, "cypherpunk" too subcultural. "Sanctuary" evokes safety and protection — concepts with universal appeal. The risk is abstraction; the challenge is translating philosophy into products people use.

*Sources: [The Block](https://www.theblock.co/post/392079/ethereum-vitalik-buterin-sanctuary-tech-forget-emulating-apple-or-google); [vitalik.eth.limo](https://vitalik.eth.limo/general/2025/04/14/privacy.html); [Decrypt](https://decrypt.co/359895/vitalik-buterin-ethereum-broaden-mission-beyond-finance)*

---

## Section 9: Account Abstraction & Quantum Resistance

### 9a. Account Abstraction

Account abstraction is proceeding along three converging tracks:

**ERC-4337 (Live since March 2023)**: Over **40 million smart accounts** deployed. 132M+ UserOperations processed. ~$5.7M in gas sponsored by paymasters. Limitation: separate mempool, extra bundler layer, cannot interact with FOCIL inclusion lists.

**EIP-7702 (Live since Pectra, May 2025)**: EOA delegation — any existing EOA can temporarily delegate to a smart contract implementation, enabling batching, gas sponsorship, social recovery, and alternative signature schemes. MetaMask and Ledger have shipped support.

**EIP-8141 — Frame Transactions (Draft, targeting Hegota)**: Native AA eliminating bundlers entirely. A transaction becomes N programmable "frames" with independent sender and gas payer authorization. Combined with FOCIL, creates **17 independent randomly-chosen actors per slot** for censorship-resistant inclusion of AA transactions. Buterin confirmed it is expected to ship as part of Hegota.

*Sources: [Alchemy ERC-4337 Statistics](https://www.alchemy.com/blog/erc-4337-statistics-q3-2023); [EIP-8141 Specification](https://eips.ethereum.org/EIPS/eip-8141); [Circle: Pectra & EIP-7702](https://www.circle.com/blog/how-the-pectra-upgrade-is-unlocking-gasless-usdc-transactions-with-eip-7702)*

### 9b. Quantum Resistance

**Threat**: Buterin warned at Devconnect that quantum computers could break Ethereum's elliptic curve cryptography "before the 2028 U.S. presidential election" (20% probability estimate from Metaculus for cryptographically relevant QCs before 2030).

**EF response (January 2026)**: Post-quantum security elevated to **top strategic priority**. Dedicated Post-Quantum team formed. **$2M in research prizes**: $1M Poseidon Prize + $1M cryptographic proximity prize. Multi-client PQ consensus devnets already running.

| Component | Vulnerability | Migration Path | Timeline |
|-----------|--------------|----------------|----------|
| Consensus BLS signatures | Shor's algorithm | Hash-based signatures + STARK aggregation | 2027-2028 |
| Data availability (KZG) | Shor's algorithm | Replace with STARKs | 2027-2028 |
| EOA signatures (ECDSA) | Shor's algorithm | AA-enabled migration to quantum-safe schemes | 2026-2027 (begins with AA) |
| Application ZK proofs | Hash function security | Protocol-layer recursive aggregation | 2028-2029 |

**Critical coupling**: AA must ship before the quantum threat materializes, or users face forced migration. EIP-7702 and EIP-8141 are the primary mechanisms for quantum-safe user migration — once accounts use arbitrary signature verification, users upgrade to lattice-based or hash-based schemes without changing addresses.

*Sources: [The Block](https://www.theblock.co/post/386938/ethereum-foundation-forms-post-quantum-security-team-adds-1-million-research-prize); [CoinDesk](https://www.coindesk.com/tech/2026/01/24/ethereum-foundation-makes-post-quantum-security-a-top-priority-as-new-team-forms)*

---

## Section 10: Ethereum Governance & Community

### 10a. The EF Leadership Transition

**March 2025**: Aya Miyaguchi transitioned from Executive Director to President. Two co-Executive Directors appointed: **Hsiao-Wei Wang** (7-year EF researcher, beacon chain contributor) and **Tomasz Stanczak** (Nethermind founder).

**February 2026**: Stanczak stepped down after less than a year. **Bastian Aue** appointed as interim co-Executive Director alongside Wang. The rapid turnover has drawn mixed reactions — supporters see iteration, critics worry about instability at a critical moment.

### 10b. The "Protocol Priorities" Framework

On February 18, 2026, the EF published its [Protocol Priorities Update](https://blog.ethereum.org/2026/02/18/protocol-priorities-update-2026):

- **Track 1 — Scale**: Gas limit toward 100M+, BALs, ePBS, gas repricing, multidimensional gas, blob scaling
- **Track 2 — Improve UX**: Native AA (EIP-8141), cross-L2 interoperability
- **Track 3 — Harden the L1**: "Trillion Dollar Security Initiative," post-quantum readiness, multi-client diversity

### 10c. Community Dynamics

**Issuance debate**: The most politically charged question — whether to modify issuance to disincentivize excessive staking. Critics argue insufficient "rough consensus" with stakeholders; proponents counter the current curve was never permanent.

**L2 alignment tensions**: Base's dominance raises concerns about corporate-backed concentration. Specific criticisms: centralized sequencers, MEV extraction, disproportionate value capture.

**Sanctuary tech narrative**: Vitalik's January 2026 criticism of L2s as "centralized databases dressed in blockchain clothing" is directly consistent with the sanctuary thesis — centralized sequencers are chokepoints antithetical to sanctuary properties.

### 10d. EIP Process and ACD Cadence

ACDE (Execution Layer) and ACDC (Consensus Layer) calls run on alternating weeks. Upgrade cadence stabilized at **two hard forks per year**. Headliner EIPs selected first; non-headliner proposals open only after headliners are locked.

*Sources: [EF Blog](https://blog.ethereum.org/2025/03/01/leadership-announcement); [The Block](https://www.theblock.co/post/389875/tomasz-stanczak-to-step-down-as-ethereum-foundation-co-executive-director-bastian-aue-to-take-interim-role); [EF Protocol Priorities](https://blog.ethereum.org/2026/02/18/protocol-priorities-update-2026)*

---

## Section 11: MEV & Block Production

*For comprehensive analysis, see the companion [State of MEV: March 2026](../state-of-mev-march-2026.md) report (v6.0).*

### 11a. Builder Market Concentration

**Titan Builder** holds ~50% of block production. **BuilderNet** ~28%, **Quasar** ~16%. Top three = ~94% of blocks. The Titan/Beaverbuild duopoly broke in May 2025 when Beaverbuild migrated fully to BuilderNet (decentralized, TEE-based). Builder profitability is asymmetric: Titan earned 764.94 ETH/7d vs. BuilderNet's 106.01 ETH/7d. MEV-Boost adoption: 90-96%.

### 11b. MEV Extraction

Total MEV extraction: ~$250-300M annually (extrapolated from late 2025). **CEX-DEX arbitrage** has replaced sandwich attacks as the dominant strategy — three searchers (Wintermute, SCP, Kayle) capture ~73% of identified value. Sandwich attack profitability declined ~75% since late 2024, driven by MEV Blocker (4.5M+ wallets).

### 11c. The "Holy Trinity of Censorship Resistance"

1. **ePBS (EIP-7732) — Glamsterdam, H1 2026**: Relay elimination, first-price blind auction
2. **FOCIL (EIP-7805) — Hegota, H2 2026**: 16 randomly-selected validators enforce inclusion. Combined with EIP-8141, 17 independent actors per slot guarantee inclusion
3. **Encrypted Mempools (LUCID/EIP-8105) — Hegota**: Content-based censorship prevention. Complemented by Flashnet anonymous broadcast

**"Big FOCIL"** (March 2, 2026): Buterin proposed inclusion lists covering essentially *all* transactions, reducing the builder's role to only MEV-relevant ordering and state computation. This would transform builders from general transaction inclusion gatekeepers into specialized MEV optimizers — a fundamentally more commoditized role.

### 11d. Long-Term Vision

FOCIL → Big FOCIL → Encrypted mempools → Transaction ingress layer (Tor, mixnets, Flashnet) → Distributed block building. BuilderNet currently in Phase 1 (replicated privacy, permissioned operators), targeting Phase 2 (modular, permissionless).

---

## Section 12: Outlook & Key Risks

### 12a. The Bull Case

- **Technical acceleration via AI**: Development velocity increasing, potentially finishing the roadmap sooner at higher security. "Bug-free code may transition from idealistic delusion to basic expectation."
- **Sanctuary tech narrative**: Resonates in a moment of rising authoritarianism and AI surveillance, positioning Ethereum as essential infrastructure
- **Institutional adoption**: ~$11.6B in cumulative ETH ETF net inflows, $17B+ in RWAs, GENIUS Act reinforcement
- **Based/native rollups**: Close the value-alignment gap between L1 and L2s
- **Glamsterdam + Hegota**: More protocol-level change in 2026 than any year since the Merge

### 12b. The Bear Case

- **ETH price disconnect**: ~57% below ATH at "Extreme Fear" levels despite strong fundamentals
- **L2 centralization**: Centralized sequencers, upgrade keys, and Base concentration
- **Solana competition**: 50%+ of global DEX volume, 3.2M DAU, Firedancer upgrade pending
- **Quantum risk**: Possible materialization before 2028, requiring emergency migration

### 12c. Key Risks

- **EigenLayer systemic risk**: Correlated slashing across 4.36M+ ETH, untested in major market crash
- **Base concentration**: Single corporate-backed L2 capturing majority of user activity
- **Issuance politics**: Potential governance crisis if changes pushed without community buy-in
- **Regulatory uncertainty**: CLARITY Act awaits Senate action; global approaches fragmented

### 12d. 2026 Milestones to Watch

| Milestone | Expected Timing | Significance |
|-----------|----------------|--------------|
| Glamsterdam activation | H1 2026 (May-June) | ePBS + BALs + gas repricings |
| Hegota headliner finalization | Q1 2026 | FOCIL (CL) + EIP-8141 (EL) |
| Native rollup specifications | 2026, initial drafts | ZK-EVM proving at L1 level |
| Blob utilization growth | Ongoing | Whether rollups fill PeerDAS capacity |
| Post-quantum devnet milestones | H1-H2 2026 | Multi-client PQ consensus |
| ETF staking approvals | 2026 | ETH ETF yield inclusion |
| GENIUS Act implementation | 2026 | Stablecoin regulatory framework |
| Firedancer on Solana | 2026 (if on schedule) | Competitive performance benchmark |

### 12e. Summary Assessment

Ethereum enters mid-2026 in paradox: the most ambitious and well-executed technical roadmap in its history, all-time-high institutional adoption, and accelerating development cadence — yet its token trades at extreme-fear levels, its L2 ecosystem raises centralization concerns, and competitors gain ground in retail applications.

Resolution depends on three factors:
1. **Execution**: Whether Glamsterdam and Hegota ship on schedule
2. **Narrative coherence**: Whether "sanctuary technology" resonates beyond the existing community
3. **Value alignment**: Whether native rollups and improved L1 economics close the L1/L2 value gap

The technical building blocks are falling into place. The open question is whether 2026 is the year the market recognizes it.

---

## Appendix: Key Data Sources

| Source | Use |
|--------|-----|
| [strawmap.org](https://strawmap.org/) | Protocol roadmap through 2029 |
| [vitalik.eth.limo](https://vitalik.eth.limo) | Historical context, blog posts, vision |
| [DeFiLlama API](https://defillama.com) | TVL, DEX volume, lending data, stablecoin supply |
| [L2BEAT](https://l2beat.com) | L2 TVL, stages, risk analysis |
| [relayscan.io](https://relayscan.io) / [rated.network](https://rated.network) | Relay and builder market share |
| [Electric Capital Developer Report](https://www.developerreport.com) | Developer activity and growth |
| [Token Terminal](https://tokenterminal.com) | Fee revenue, protocol financials |
| [rwa.xyz](https://rwa.xyz) | Tokenized RWA data |
| [ultrasound.money](https://ultrasound.money) | ETH supply, burn rate, issuance |
| [Dune Analytics (hildobby)](https://dune.com/hildobby/eth2-staking) | Staking distribution |
| [EIP specifications](https://eips.ethereum.org) | Technical specifications |
| [CoinGlass](https://www.coinglass.com) | ETF flows, market data |

---

*Published March 4, 2026. All figures as of late February/early March 2026 unless otherwise noted. This report references three companion reports for detailed coverage: [DEX Market Report](dex-market-report-2022-2026.md), [Lending & Borrowing Market Report](lending-borrowing-market-report-2022-2026.md), and [State of MEV: March 2026](../state-of-mev-march-2026.md).*
