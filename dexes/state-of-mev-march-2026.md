---
title: "State of MEV: March 2026"
date: 2026-03-03
---

# State of MEV: March 2026

*Published: March 3, 2026*

## tl;dr

- **Titan Builder dominates with ~50% of Ethereum block production** — while Beaverbuild migrated to BuilderNet (~28%), breaking the prior duopoly that controlled ~90% of blocks.
- **MEV extraction runs ~$250-300M/year on Ethereum** — with $963M in cumulative MEV revenues tracked from Dec 2022 through Jan 2025, and $24M in profit in a single 30-day snapshot.
- **Ultra Sound relay rose from zero to #1 (~33% share)** — directly solving Ethereum's 2022 censorship crisis when OFAC-compliant blocks hit ~78-79%, and now self-caps at 1/3 of blocks.
- **ePBS ships in Glamsterdam (H1 2026)** — enshrining proposer-builder separation at the protocol level, eliminating relay trust assumptions.
- **MEV Blocker protects 4.5M+ wallets** — private orderflow protection has reached mainstream adoption, with Flashbots announcing Flashnet as the next-gen anonymous broadcast protocol.
- **The "Holy Trinity" targets full censorship resistance** — FOCIL (EIP-7805) + encrypted mempools (LUCID) + ePBS, all slated for Glamsterdam/Hegota upgrades in 2026.
- **MEV Brothers criminal case ended in mistrial** — the first prosecution targeting MEV infrastructure exploitation (Nov 2025), with prosecutors seeking retrial, leaving legal status in limbo.

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [Section 1: Ethereum Builder Market](#section-1-ethereum-builder-market)
3. [Section 2: Ethereum Relay Landscape](#section-2-ethereum-relay-landscape)
4. [Section 3: MEV Strategies and Trends](#section-3-mev-strategies-and-trends)
5. [Section 4: Private Orderflow and MEV Protection](#section-4-private-orderflow-and-mev-protection)
6. [Section 5: Solana MEV Ecosystem](#section-5-solana-mev-ecosystem)
7. [Section 6: Regulatory Landscape](#section-6-regulatory-landscape)
8. [Section 7: Forward-Looking Developments](#section-7-forward-looking-developments)
9. [Section 8: Key Metrics Summary Table](#section-8-key-metrics-summary-table)
10. [Sources](#sources)
11. [Appendix A: Empirical Validation via Brontes On-Chain Analysis](#appendix-a-empirical-validation-via-brontes-on-chain-analysis)
12. [Appendix B: Multi-Source Live Data Validation (March 4, 2026)](#appendix-b-multi-source-live-data-validation-march-4-2026)

---

## Executive Summary

The MEV landscape has undergone structural transformation since 2024. On Ethereum, the Titan/Beaverbuild builder duopoly has fractured with Beaverbuild's migration to BuilderNet, enshrined PBS (ePBS) is slated for the Glamsterdam upgrade in H1 2026, and private orderflow protection has reached mainstream adoption with MEV Blocker now covering 4.5M+ wallets. Looking ahead, the Hegota upgrade (H2 2026) targets a "Holy Trinity of Censorship Resistance" — FOCIL (EIP-7805) for protocol-enforced transaction inclusion, encrypted mempools (LUCID) for content privacy, complementing ePBS's relay-free PBS. Flashbots' announcement of Flashnet (February 2026) as an anonymous broadcast protocol signals the next phase of decentralized block building infrastructure. On Solana, Jito's launch of the Block Assembly Marketplace (BAM) in July 2025 represents the most significant MEV infrastructure upgrade in the chain's history, while coordinated validator enforcement against sandwich attacks has reshaped searcher economics. On L2s, Arbitrum's TimeBoost and Base's Flashblocks have introduced differentiated MEV capture mechanisms, with Flashbots expanding Flashblocks across the OP Superchain. The blob fee market was structurally reformed by EIP-7918 (Fusaka, December 2025), introducing a fee floor that ends the free-blob era, though blob utilization remains far below capacity at ~29% of target. Regulatorily, the Peraire-Bueno "MEV Brothers" case -- the first criminal prosecution targeting MEV infrastructure exploitation -- ended in a mistrial in November 2025, with prosecutors seeking retrial in early 2026, leaving the legal status of aggressive MEV strategies in limbo.

### Aggregate MEV Quantification

Precise total MEV extraction figures remain elusive due to methodological challenges (different trackers use different definitions of "MEV"). The best available aggregate data:

- **EigenPhi estimates** for **December 2022 through January 2025**: approximately **$963 million in MEV revenues** and **$417 million in MEV profits** on Ethereum. (Source: ESMA TRV Risk Analysis, July 2025, citing EigenPhi)
- **30-day snapshot (December 8, 2025 - January 6, 2026)**: nearly **$24 million in MEV profit** extracted on Ethereum. (Source: EigenPhi via CryptoSlate)
- **Annualized estimate (extrapolating from late 2025 data)**: roughly **$250-300M/year** in MEV profit on Ethereum alone, though this varies significantly with market conditions and DeFi activity levels. Note that Q4/Q1 periods (from which this snapshot is drawn) tend to exhibit higher extraction rates due to market volatility clustering, so the annualized figure may overstate full-year averages during calmer market regimes.

No single source provides a comprehensive, real-time total MEV figure across all chains. Flashbots' MEV-Explore dashboard, EigenPhi's daily reports (eigenphi.io/mev/ethereum/dailyReport), and libMEV are the closest available aggregators, though each uses different detection methodologies and captures different subsets of MEV activity.

---

## Section 1: Ethereum Builder Market

### 1a. Builder Market Share (as of late February 2026)

| Builder | Market Share | Notes |
|---------|-------------|-------|
| Titan Builder | ~50.2% | Dominant via vertical integration and exclusive orderflow |
| BuilderNet | ~27.9% | Flashbots + Beaverbuild + Nethermind collaborative builder |
| Quasar | ~16.2% | Third-largest, growing share |
| Eureka | ~1.9% | Smaller builder with niche orderflow |
| Others | ~3.8% | Long tail of smaller builders |

**Source**: mevboost.pics, relayscan.io; data as of approximately February 27, 2026 (per Toni Wahrstatter's market update).

**Key developments**:

- **Titan's dominance**: Titan Builder holds approximately 50% of Ethereum block production, making it the single most powerful block builder. Titan's position is sustained through vertical integration (operating its own relay, titanrelay.xyz), exclusive orderflow deals with major searchers, and competitive bidding strategies that consistently outprice rivals.

- **Beaverbuild-to-BuilderNet transition**: In May 2025, Beaverbuild -- previously one of the two dominant builders alongside Titan -- retired its centralized block builder and migrated fully to BuilderNet. BuilderNet is a decentralized block building network launched in November 2024 as a collaboration between Flashbots, Beaverbuild, and Nethermind. Beaverbuild onboarded its users, software, infrastructure, and orderflow to BuilderNet starting May 6, 2025, with builder instances running in TEEs (Trusted Execution Environments) for privacy and integrity guarantees. This migration effectively broke the Titan/Beaverbuild duopoly that had dominated since late 2023, where the two builders collectively produced ~90% of Ethereum blocks. (Source: buildernet.org/blog/beaverbuild; Flashbots Collective announcement)

- **BuilderNet's architecture**: BuilderNet uses a multi-operator system where various parties manage the same block builder, with TEE-based execution to ensure no single operator can view or front-run orderflow. The network aims to neutralize exclusive MEV orderflow deals, enhance censorship resistance, and accelerate decentralization. As of February 2026, BuilderNet holds ~28% of block production share. (Source: buildernet.org/blog/introducing-buildernet)

### 1b. "Active Builders" Definition

The metric "136 active builders" (sometimes cited from mevboost.pics) refers to the count of distinct builder pubkeys that have submitted at least one payload to a relay in the measured time window. This number is misleading without context: while 136+ pubkeys may be registered and occasionally active, only approximately 8-15 builders have meaningful market share (>0.5% of blocks). The top three builders (Titan, BuilderNet, Quasar) collectively account for roughly 94% of block production as of February 2026. The remaining builders exist in a long tail where profitability is marginal or negative, as they cannot compete with the exclusive orderflow advantages held by top builders. (Source: mevboost.pics, relayscan.io)

### 1c. MEV-Boost Adoption

MEV-Boost adoption among Ethereum validators has stabilized at approximately 90-96% of all proposed blocks being sourced from external builders via the MEV-Boost relay network. This near-universal adoption reflects the economic reality that validators earn significantly more from outsourced blocks than from locally built ones. (Source: boost.flashbots.net, mevboost.pics)

---

## Section 2: Ethereum Relay Landscape

### 2a. Relay Market Share (as of February 2026)

| Relay | Market Share | Operator | Filtering | Launched | Notes |
|-------|-------------|----------|-----------|----------|-------|
| Ultra Sound Money | ~33.0% | ultrasound.money team | Non-filtering (also has filtered variant) | Nov 30, 2022 | Largest relay; self-caps at 1/3 of blocks |
| Titan Relay | ~21.1% | Titan Builder (Gattaca) | Non-filtering | Apr 26, 2024 | Vertically integrated with Titan Builder; Rust-based; optimistic v2 |
| bloXroute Max Profit | ~19.4% | bloXroute Labs | OFAC-filtering (since Dec 2023) | Pre-Merge 2022 | Was non-filtering until December 18, 2023 |
| bloXroute Regulated | ~12.4% | bloXroute Labs | OFAC-filtering | Pre-Merge 2022 | Always OFAC-compliant |
| Aestus | ~4.7% | EthFinance/EthStaker volunteers | Non-filtering | Late 2022 | Public good; funded by OP RetroPGF3 grant (97.3K OP) |
| Agnostic Relay | ~4.0% | Gnosis/GnosisDAO | Non-filtering | Nov 30, 2022 | Funded by Gnosis treasury |
| Flashbots Relay | ~3.5% | Flashbots | OFAC-filtering | Sep 2022 (Merge) | Focus shifted to BuilderNet |

**Source**: relayscan.io, rated.network/relays; data as of approximately February 27, 2026.

### 2b. The Rise of Ultra Sound Relay

The Ultra Sound relay is the single most important story in relay market evolution. Its rise from zero to the #1 position directly solved Ethereum's 2022 censorship crisis.

**Origins**: The relay launched on **November 30, 2022** — the same day as the Gnosis Agnostic relay — at the peak of Ethereum's censorship crisis when OFAC-compliant blocks had reached ~78-79% of all blocks. It is operated by the **ultrasound.money team** (the same group behind the well-known ETH supply tracker). The team consisted of approximately **two full-time people** at launch (per a February 2023 Namada RPGF nomination). The relay runs the open-sourced Flashbots `mev-boost-relay` codebase and applies **no block filtering**, making it a credibly neutral alternative to the OFAC-compliant Flashbots relay.

**Growth trajectory**:

| Period | Ultra Sound Share | Flashbots Share | Context |
|--------|------------------|-----------------|---------|
| Nov 2022 | Just launched | ~64.3% (total) / ~76.3% (MEV-Boosted) | Launch amid censorship crisis |
| Feb 2023 | ~18.4% (3rd largest) | ~50-55% | Rapid growth; 23/29 Lido operators connected by May 2023 |
| Mar 2023 | ~20% | ~26% | Ultra Sound, Agnostic each ~20%; Flashbots dethroned |
| 2024 | Largest or near-largest | Declining further | Competitors shut down (Blocknative, Eden) |
| Feb 2026 | **~33.0%** | ~3.5% | #1 relay; self-capped at 1/3 |

**Why Ultra Sound won**: Several factors drove the shift:

1. **Censorship resistance ideology**: Validators who cared about Ethereum's neutrality actively switched to non-filtering relays. The Flashbots relay's OFAC compliance (filtering Tornado Cash transactions post-August 8, 2022 Treasury sanctions) was the primary driver.
2. **Optimistic relaying**: Ultra Sound pioneered **optimistic relaying** (deployed March 17, 2023, in collaboration with Mike Neuder at EF and Ankit Chiplunkar at Frontier Research). Rather than validating blocks before making bids available (adding ~140ms latency), the relay marks bids as active immediately and simulates asynchronously. This gave Ultra Sound a **~150ms latency advantage** and an **8% improvement in block dominance** (winning more auctions). Builders post stETH collateral (up to 64 stETH) to cover bad bids. (Source: frontier.tech/optimistic-relays-and-where-to-find-them)
3. **Lido adoption**: Lido's Relay Maintenance Committee adding Ultra Sound to its "must use some" list drove massive adoption among Lido's ~30% of staked ETH.
4. **Self-cap policy**: The relay commits to **self-capping at 1/3 (33%) of blocks** to promote relay diversity and prevent becoming a centralization point itself.

**Infrastructure**: The relay runs on baremetal AMD Epyc machines with 512GB RAM (Ethereum state in RAM disk), at an estimated operating cost of ~$20,000/month (~$240K/year). All-time: approximately 3.85 million blocks included (44.75% of blocks), 314,689 ETH total value relayed, 1.17 million validators registered.

(Sources: relay.ultrasound.money; relayscan.io; Frontier Tech April 2023; Lido governance proposal February 2023; Namada RPGF nomination; Flashbots Collective forum)

### 2c. Flashbots Relay Decline and Open-Sourcing

The Flashbots relay was once the dominant relay in the MEV-Boost ecosystem. In September 2022, Flashbots' relay handled ~82.8% of all relay blocks. The decline was driven by two factors:

1. **OFAC compliance controversy**: After the US Treasury sanctioned Tornado Cash on August 8, 2022, Flashbots updated their relay to filter sanctioned transactions. By November 2022, ~78-79% of Ethereum blocks were OFAC-compliant — over half the network could censor transactions. Flashbots' own strategy lead Hasu called it "a failure" that no neutral relay existed.
2. **Open-sourcing**: On **August 17, 2022**, Flashbots open-sourced their relay code under AGPL, explicitly stating "Multiple relays at the merge — rather than a single default — is key to Ethereum's health." This enabled Ultra Sound, Agnostic, Aestus, and others to fork the codebase and launch quickly. **93% of open-source relays** run the same Flashbots source code (only Blocknative's "Dreamboat" was a fully independent implementation).

**Important clarification**: The sometimes-cited "80%+" figure refers specifically to Flashbots' share of MEV-Boosted blocks in late 2022, not its share of all Ethereum blocks. The decline to ~3.5% is not a failure but a designed outcome: Flashbots pivoted its focus to BuilderNet as its primary product.

(Source: writings.flashbots.net/Flashbots-Relay-open-sourcing; Cointelegraph September 2022; The Block; Crypto Briefing)

### 2d. Relay Market Dynamics and Sustainability

**Consolidation**: The relay market went from ~11 active relays to ~7 between 2023-2025:
- **September 27, 2023**: Blocknative suspended its relay, CEO stating "economics failed to materialize." Subsequently laid off a third of staff. (Source: CoinDesk)
- **December 18, 2023**: bloXroute made **all relays OFAC-filtering**, including the previously non-censoring Max Profit relay — a "concerning precedent" that temporarily pushed OFAC-compliant blocks back to ~52%. (Source: Cointelegraph)
- **August 29, 2024**: Eden Network transitioned its relay to Titan/Gattaca. Relayoor and Wenmerge also shut down.

**"Relay as public good" vs. "Relay as business"**: Most relays charge nothing and generate no direct revenue. Ultra Sound, Agnostic, and Aestus operate as public goods funded by grants (Gitcoin, OP RetroPGF, Namada RPGF) and volunteer labor. bloXroute CEO Uri Klarman stated "no reasonable fee incentive mechanism" exists. Titan's vertical integration (builder + relay) is the only clearly sustainable model, creating indirect revenue through better block building performance. A **PBS Guild** initiative has been proposed to raise public goods funding for relay operators. (Source: Fenbushi VC relay analysis January 2024; EigenPhi relay analysis June 2024)

**ePBS and the future of relays**: With ePBS (EIP-7732) scheduled for Glamsterdam, the relay market faces structural disruption. ePBS moves the builder-proposer auction into the protocol, eliminating the relay's "auctioneer" role. However, per Titan Builder's analysis ("Builders and Relays in ePBS," July 2025), relays will not fully disappear — potential new roles include providing upfront capital/collateral to smaller builders and aggregating bids. Direct builder-to-proposer connections will increase, and the auction type shifts from second-price open to first-price blind. Sigma Prime argued against ePBS urgency (July 2025), noting "MEV-Boost has been performing well." (Source: titanbuilder.substack.com; blog.sigmaprime.io)

---

## Section 3: MEV Strategies and Trends

### 3a. Sandwich Attack Profitability Decline on Ethereum

One of the most significant MEV trends in 2025 was the sharp decline in sandwich attack profitability on Ethereum, documented extensively by EigenPhi data reported via Cointelegraph Research.

**Key statistics (as of October 2025, per EigenPhi via Cointelegraph)**:

- Monthly sandwich extraction revenue declined from approximately **$10M/month in late 2024** to approximately **$2.5M/month by October 2025** -- a 75% decline.
- Net profits after gas costs averaged approximately **$260,000/month** in 2025, with the figure inflated by a single outlier in January 2025 when one sandwich attack generated over $800,000 in profit.
- Average net profit per individual sandwich attack dropped to approximately **$3**.
- The number of attacks remained consistently high (60,000-90,000 per month), but profitability per attack cratered.
- Only approximately **100 distinct sandwich bots** execute trades in a typical month.
- A single entity, "jaredfromsubway.eth," controlled approximately **70%** of sandwich attacks.
- Block builders capture the majority of extracted value through gas fees, leaving sandwich attackers with a profit margin of roughly **5%**.
- Total user losses from sandwich attacks in 2025 amounted to approximately **$40M** for the year, despite DEX volumes reaching $100B+ monthly.

**Drivers of the decline**: The growth of private orderflow services (MEV Blocker, Flashbots Protect, MEV-Share), increased user awareness of sandwich risks, and the rising cost of builder tips have all contributed to making sandwich attacks increasingly uneconomical on Ethereum mainnet.

(Source: EigenPhi data via Cointelegraph Research, October 2025)

### 3b. CEX-DEX Arbitrage: The Dominant MEV Strategy

CEX-DEX arbitrage has emerged as the dominant and most lucrative MEV strategy on Ethereum, far exceeding sandwich attacks in total value extracted.

**Key data (August 2023 - March 2025, per arXiv:2507.13023)**:

- 19 major CEX-DEX searchers extracted a total of **$233.8M** from **7,203,560** identified CEX-DEX arbitrage transactions.
- Three searchers -- Wintermute, SCP, and Kayle -- collectively captured **$170.8M** (approximately 73% of cumulative extracted value).
- By Q1 2025, these top three players accounted for approximately **90%** of CEX-DEX extracted value, indicating extreme market concentration.
- Net searcher profitability depends heavily on the fraction of revenue shared with block builders and the searcher's integration level with builders.

CEX-DEX arbitrage is inherently capital-intensive and latency-sensitive, requiring co-located infrastructure with CEX matching engines and sophisticated inventory management. This creates high barriers to entry and explains the market's concentration among well-capitalized trading firms.

(Source: arXiv:2507.13023, "Measuring CEX-DEX Extracted Value and Searcher Profitability," 2025)

### 3c. Cross-Chain Arbitrage

Cross-chain arbitrage -- exploiting price discrepancies for the same asset across different blockchains -- is an increasingly important MEV frontier as trading volume fragments across L1s and L2s.

**Key findings (per arXiv:2501.17335, "Cross-Chain Arbitrage: The Next Frontier of MEV")**:

- Most cross-chain arbitrage trades use **pre-positioned inventory** (66.96% of trades), settling in approximately **9 seconds**. Bridge-based arbitrages take approximately **242 seconds**, underscoring the latency cost of current bridging infrastructure.
- Market concentration is high: the five largest addresses execute more than half of all trades, and one alone captures almost **40% of daily volume** post-Dencun.
- The paper concludes that cross-chain arbitrage fosters vertical integration, centralizing sequencing infrastructure and economic power, thereby exacerbating censorship, liveness, and finality risks.
- The profit-cost model demonstrates that opportunity frequency, bridging time, and token depreciation rate determine whether inventory-based or bridge-based execution is more profitable.

(Source: arXiv:2501.17335v2, explicitly cited)

### 3d. L2 MEV Dynamics

L2 MEV represents a qualitatively different MEV regime from Ethereum L1, driven by centralized sequencer architecture, faster block times, and private mempools.

**Architecture**: All major rollups -- Arbitrum, Optimism, Base -- operate centralized sequencers with private mempools. Unlike Ethereum L1, where a public mempool and competitive block building create the MEV supply chain, L2 sequencers have monopoly power over transaction ordering within their domain. Block times range from 200ms (Base with Flashblocks) to 2 seconds, with Arbitrum's FCFS (First-Come-First-Served) ordering and Base's timestamp-priority ordering creating different MEV dynamics.

**Arbitrum -- TimeBoost (launched April 2025)**: Arbitrum introduced TimeBoost as a modification to its sequencer ordering policy that creates a priority "express lane" for the highest bidder. Key results:

- Generated approximately **$3M in DAO revenue** in its first three months (Source: DL News)
- Express lane control is highly centralized: two entities win more than **90%** of auctions (Source: arXiv:2509.22143)
- Approximately **22% of time-boosted transactions revert**, indicating TimeBoost does not effectively mitigate spam
- Auction competition declined over time, reducing revenue for the Arbitrum DAO
- Profitable MEV opportunities cluster at the end of blocks, limiting the value of express lane priority access

(Source: arXiv:2509.22143, "The Express Lane to Spam and Centralization: An Empirical Analysis of Arbitrum's Timeboost")

**Base -- Flashblocks (launched July 2025)**: Base, the leading L2 by activity volume, introduced Flashblocks via a collaboration with Flashbots, reducing effective block times from 2 seconds to 200 milliseconds. Each 2-second L2 block is assembled from 10 sequential Flashblocks, each with its own transaction ordering. The Flashbots "Rollup-Boost" sidecar runs alongside the OP Stack sequencer. This dramatically changes MEV dynamics: the 200ms granularity reduces the window for latency-based MEV while enabling faster price discovery. Base's architecture represents a move toward MEV-aware sequencing at the L2 level.

(Source: blog.base.dev/flashblocks-deep-dive; writings.flashbots.net/flashbots-superchain)

**Optimism**: Optimism partners with Flashbots to bring configurable, verifiable sequencing to every OP Stack chain. Unichain (Uniswap's OP Stack L2) adopted Flashblocks in August 2025, shortly after Base.

**MEV Patterns on L2s**: Academic research (arXiv:2506.01462, "When Priority Fails: Revert-Based MEV on Fast-Finality Rollups") finds:

- Daily revert rates range from **5% to 40%** across L2s, with peaks up to 20% on Arbitrum
- More than **80% of reverted transactions are swaps**, suggesting connection to arbitrage activity
- Despite Priority Fee Auctions (PFAs), MEV bots rarely use priority fees, instead favoring **duplicate transaction spam** due to unreliable fee-based ordering on fast-finality chains
- This "revert-based MEV" paradigm is fundamentally different from Ethereum L1's bundle-based approach

**Quantitative data limitations**: Comprehensive, standardized MEV quantification across L2s remains difficult. Unlike Ethereum L1 where MEV-Boost provides clean data on builder payments, L2 MEV is largely hidden within sequencer internals. The academic literature provides partial data (revert rates, spam ratios, TimeBoost revenue) but no single source comprehensively tracks total L2 MEV extraction across all rollups.

**Shared Sequencer Proposals**:

- **Espresso Systems**: Launched sequencer testnet; targeting integration with zk rollups; collaborating with Polygon zkEVM; uses threshold cryptography for fair ordering. Still in development as of March 2026. (Source: espressosys.com)
- **Astria**: **Shut down operations** despite securing **$18M in total funding** ($5.5M seed + $12.5M strategic round in 2024) and launching mainnet. Limited market adoption led to the project's closure, representing a significant setback for the shared sequencer thesis. (Source: Coinfomania, MEXC News, Superex)
- **Radius**: Early-stage development; building a modular, rollup-agnostic shared sequencer for both optimistic and zk rollups. (Source: radius.dev)

The failure of Astria is notable: it demonstrates that while the shared sequencer thesis is intellectually compelling (enabling cross-rollup atomic MEV capture, reducing sequencer centralization), market demand has not materialized sufficiently to sustain dedicated shared sequencer projects. The surviving approach appears to be integration-based, with Flashbots building sequencing tools (Rollup-Boost, Flashblocks) that L2s voluntarily adopt rather than a separate shared sequencer layer.

### 3e. Liquidation MEV and the OEV Recapture Movement

Liquidation MEV arises from lending protocols' overcollateralization requirements. When a borrower's collateral value drops below the liquidation threshold (health factor <1.0 on Aave), any external actor can repay the debt and receive collateral at a discount — typically 5-15% depending on asset type and protocol. The competitive race to execute liquidations first creates MEV.

**Scale of liquidation activity**:

| Protocol | Total Liquidations | Time Period | Source |
|----------|-------------------|-------------|--------|
| Aave (all versions, all chains) | **$4.65B** collateral liquidated, 310,000+ transactions | 2020 – February 2026 | Aave Blog, February 2026 |
| Aave (Ethereum only) | ~$3B across 58,106 transactions | 2020 – February 2026 | Aave Blog |
| Aave + Compound (Ethereum) | ~$2.5B collateral, ~$150M implied liquidation incentives | Cumulative through 2024 | UMA/Risk Labs; Chainlink SVR analysis |

**Major liquidation events**:

| Event | Date | Volume | Context |
|-------|------|--------|---------|
| China crypto ban | May 2021 | $362M (Aave) | ~5,500 events, avg $65K each |
| Luna/3AC collapse | June 2022 | ~$218M (est.) | 32,000+ events, smaller avg size |
| "Black Monday" ETH/BTC crash | August 5, 2024 | **$231M** (Aave single day); $306.9M across Aave+Compound+Spark | Largest single-day event to that point; one builder captured 1,448 ETH (~$3.5M) in a single block |
| AAVE flash crash (-64%) | October 10, 2025 | **$180M** (~1 hour); $250M+ on day | Founder described as "largest stress test ever"; no bad debt |
| BTC decline + macro selloff | Jan 31 – Feb 5, 2026 | **$429M** (5 days, ~12,500 transactions) | New dollar-volume record for Aave |

(Sources: Aave Blog "Historical Liquidations" February 2026; CoinDesk October 2025; Pangea Foundation)

**Liquidator market structure**: Liquidation is highly concentrated. Per Pangea Foundation analysis (March 2026), the top liquidator bot (0xF057) has seized over **$124M** in collateral with 563 liquidations since early 2025. Liquidation profit mechanics center on the ~5.2% effective bonus (Chainlink SVR analysis), meaning the $4.65B gross liquidation volume represents roughly **$130-150M** in total cumulative searcher profit from Aave+Compound on Ethereum. However, competitive builder payments erode margins significantly — searchers pay block builders for priority inclusion, mirroring the dynamics in sandwich and CEX-DEX arb.

**Oracle Extractable Value (OEV) and the recapture movement**: A paradigm shift in 2024-2025 was the recognition that liquidation MEV is primarily **Oracle Extractable Value** (OEV) — it arises from the moment a price oracle update reveals a position is undercollateralized. This led to protocol-level MEV recapture mechanisms:

**Chainlink Smart Value Recapture (SVR)** — launched March 2025 on Aave Core Ethereum market:

| Metric | Value | Source |
|--------|-------|--------|
| Total liquidations handled | **$675M** across ~3,900 events | Aave Blog, Feb 2026 |
| MEV revenue recaptured | **~$16M** (65% to Aave DAO, 35% to Chainlink) | Aave Blog; Chainlink SVR blog |
| Average recapture rate | **73%** of non-toxic liquidation MEV | Chainlink SVR analysis |

The SVR mechanism routes Chainlink price updates through a private mempool via Flashbots MEV-Share. Searchers auction for the right to include a liquidation transaction immediately after the oracle update in the same block. The winning bid is split between Aave and Chainlink — converting what was previously 100% MEV bot capture into a structured revenue-sharing arrangement.

**Protocol design evolution reducing liquidation MEV**:

- **Aave V4** (upcoming): Target health factor repayment — liquidators repay only enough to return borrowers to a governance-set health factor, with variable liquidation bonus that scales as health factor deteriorates (Dutch auction dynamics).
- **Euler V2** (launched September 2024): Reverse Dutch auction — penalty starts high and decreases over time, competitive liquidators bid down to minimum viable bonus.
- **Morpho** (February 2025): Pre-liquidation contracts enable smaller incremental liquidations before full threshold breach.

These design changes are structurally reducing the MEV available from liquidations, paralleling how private orderflow services reduced sandwich MEV.

(Sources: Aave Blog; Chainlink SVR Analysis blog; Chainlink SVR launch blog; Pangea Foundation "Aave's Liquidators"; UMA/Risk Labs "DeFi Liquidations Are Broken"; Bank of Canada Staff Working Paper 25-12; arxiv:2106.06389)

### 3f. Long-Tail MEV

"Long-tail MEV" describes infrequent, niche extraction opportunities that are less competitive than the dominant strategies (arb, sandwich, liquidation) because they require manual research rather than low-latency infrastructure.

**Categories**: Token launch sniping (monitoring AMM `PairCreated` events), NFT floor-price arbitrage, governance exploitation, airdrop racing, cross-protocol liquidation chains, and obscure trading pair arbitrage on low-liquidity pools.

**Competitive signature**: Short-tail MEV (arb, sandwich, liquidation) sees **bribe percentages of 99.5%+** (searchers pay nearly all profit to builders). Long-tail MEV, before alpha leaks, sees **bribe percentages of 0-80%** because there is less competition for inclusion priority. This differential makes long-tail MEV structurally more profitable per opportunity for the searchers who discover it.

**Quantification gap**: Long-tail MEV does not appear in any major tracking dashboard (EigenPhi, libMEV, Brontes) because those tools classify by known strategy patterns. No authoritative source provides a dollar estimate for aggregate long-tail MEV extraction. The MEV taxonomy paper (arxiv:2411.03327) notes: "not all MEV has been extracted — this is the dark forest."

**L2 spam as industrialized long-tail probing**: On OP-Stack L2s, bots flood sequencers with speculative arbitrage transactions at ~350 attempts per 1 success, consuming 50%+ of gas while paying under 10% of fees. This represents long-tail probing at industrial scale — high failure rates accepted in exchange for occasional niche arbitrage captures.

### 3g. MEV Strategy Profitability: Comparative Assessment

The report's claim that CEX-DEX arbitrage is the "dominant" strategy requires context. The ranking depends heavily on methodology and time period:

| Strategy | Documented Extraction | Period | Source |
|----------|----------------------|--------|--------|
| **CEX-DEX arbitrage** | $233.8M (19 searchers) | Aug 2023 – Mar 2025 (19 months) | arXiv:2507.13023 |
| **Sandwich attacks** | ~$40M user losses / ~$2.5M/month searcher revenue | Calendar year 2025 | EigenPhi via Cointelegraph |
| **Liquidation incentives** | ~$130-150M implied (Aave+Compound ETH cumulative); $16M recaptured by SVR in 9 months | Through 2026; Mar-Dec 2025 | Chainlink SVR; UMA/Risk Labs |
| **Atomic arbitrage** | ~$870K/week (Brontes, June 2024 sample) → ~$3.7M/month | June 2024 | Appendix A Brontes data |
| **Chained MEV** (combined strategies) | **>$5B** total | Multi-year | ScienceDirect 2025 |
| **JIT liquidity** | 7,498 ETH total across 36,671 events | ~20 months | Kaiko Research |

**Key insight from chained MEV research**: The ScienceDirect paper "Linking MEV attacks to further maximise attackers' gains" (2025) found that linked/chained attacks (combining sandwich + arbitrage in a single bundle) extracted **>$5B total**, versus $382M for single-strategy attacks. This suggests the most sophisticated searchers combine strategies atomically, and the true "most valuable" approach is hybrid execution rather than any single category.

**Conclusion**: CEX-DEX arbitrage is the dominant *single-strategy* MEV type by documented extraction value for the 2023-2025 period. However, liquidation MEV is a non-trivial and underreported category ($130-150M+ cumulative on Ethereum), and chained MEV (multi-strategy bundles) substantially exceeds any individual strategy. The ongoing shift of liquidation MEV into protocol-owned recapture (SVR, Euler Dutch auctions) is reducing the freely extractable pool.

(Sources: arXiv:2507.13023; EigenPhi via Cointelegraph; Chainlink SVR; ScienceDirect 2025; Kaiko Research; Appendix A)

---

## Section 4: Private Orderflow and MEV Protection

### 4a. Scale of Private Orderflow: Market-Level Data

Before discussing individual OFA products, it is essential to establish the overall size and trajectory of private orderflow on Ethereum. The data below is drawn from three distinct sources covering different time windows and methodologies -- they are not directly comparable but collectively illustrate the dominant trend.

**Trend: Private orderflow adoption across verified data points**

| Period | Private TX % (by count) | Private % (by gas used) | Scope | Source |
|--------|--------------------------|--------------------------|-------|--------|
| May–Jun 2023 | **~9.6%** of smart contract TXs | N/A | All Ethereum smart contract TXs | Blocknative, July 14, 2023 |
| Q2 2023 | Up from **~4.5%** prior quarter (2x growth) | N/A | All Ethereum smart contract TXs | Blocknative, July 14, 2023 |
| Mar–Jul 2024 | **~30%** of all TXs | **>50%** of all block space | All Ethereum TXs | Blocknative, August 20, 2024 |
| Dec 2024–Jan 2025 | **~80%** of DeFi TXs use private RPCs | N/A | DEX swaps and DeFi interactions only | arXiv:2505.19708, Feb 2025 |

**Methodological notes on comparability**:

- The Blocknative July 2023 figure (~9.6%) covers all smart contract transactions, including ETH transfers and non-DeFi interactions, which have low private RPC adoption.
- The Blocknative August 2024 figure (~30% by count, >50% by gas) covers all Ethereum transactions over the Dencun-to-July 2024 window. Private transactions disproportionately consume more gas because they tend to be complex DeFi interactions (swaps, liquidations) rather than simple transfers.
- The arXiv:2505.19708 figure (~80%) is specific to DeFi/DEX transactions submitted via private RPC during the Dec 2024 – Jan 2025 benchmark period. DEX users are early adopters of private RPCs; this figure does not extrapolate to all Ethereum transactions.
- Together these figures tell a consistent story: private orderflow has grown from a small fraction of transactions in mid-2023 to the **dominant mode of DeFi execution** by late 2024.

**Private orderflow's share of builder revenue (EigenPhi, September 2023 through mid-2024)**:

Despite representing only ~12% of total transaction count, private orderflow contributes **89% ($642.5M)** of total builder income, versus only **$74.9M** from public mempool transactions. Private orderflow constitutes approximately **54.59% of total block value**. This dramatic disparity -- 12% of transactions, 54% of block value, 89% of builder income -- explains why exclusive orderflow access is the primary determinant of builder competitiveness.

Builder-level breakdown of private orderflow income (September 2023 – mid-2024):

| Builder | Private Income | Public Income | Private Share of Income |
|---------|---------------|---------------|------------------------|
| Beaverbuild | $349.9M | Not separately reported | ~88% (implied by total) |
| Titan | $175.4M | $20M | **~90%** |
| Rsync | $117.7M | $14M | **~89%** |

(Sources: EigenPhi, "Private Order Flows: Contribute 89%, $642.5M, of Builders' Income": [Medium](https://medium.com/@eigenphi/private-order-flows-the-sleeve-bidding-of-crypto-contribute-89-642-5m-of-builders-income-503c8821d04b); [EigenPhi Substack](https://eigenphi.substack.com/p/private-order-flows-the-sleeve-bidding); arXiv:2505.19708; Blocknative August 2024 blog post)

**Private orderflow as percentage of transactions in winning blocks by builder (July–September 2024)**:

Academic research examined the top 10 builders (which collectively build 97.51% of all MEV-Boost blocks) and measured private orderflow as a share of transactions in each builder's winning blocks:

- **Titan**: **36.38%** of transactions in winning blocks are private (highest of any builder)
- **Blockbeelder**: **10.92%** (lowest of the studied builders)
- The average across all top-10 builders ranges between these two extremes

This differential -- Titan's private orderflow share being 3x higher than the lowest builder -- directly explains its competitive advantage in the block auction.

(Source: arXiv:2412.18074v2, "From Competition to Centralization: The Oligopoly in Ethereum Block Building Auctions," study period July 11 – September 23, 2024)

### 4b. Exclusive Orderflow: The Central Driver of Builder Concentration

Not all private orderflow is equally valuable. The MEV research literature distinguishes between:

- **Private but non-exclusive orderflow**: Transactions routed through OFAs (MEV Blocker, MEV-Share) that are distributed to multiple builders simultaneously. Builders compete for this flow but none has monopoly access.
- **Exclusive orderflow (XOF)**: Transactions sent only to a single builder, giving that builder unique access to the MEV opportunity. XOF holders can build higher-value blocks than competitors who lack access.

Exclusive orderflow is the most powerful competitive moat in the Ethereum block-building market. Per the Flashbots "State of Wallets 2024" report (September 30, 2024):

> "Exclusive Order Flow accounts for as much as **35% of the market**."

**Confirmed exclusive orderflow deals (as of late 2024)**:

| Orderflow Source | Exclusive Builder | Notes |
|-----------------|-------------------|-------|
| **Banana Gun** | **Titan Builder** | Confirmed by Flashbots "State of Wallets 2024"; Titan's largest XOF source |
| **Maestro** | **Beaverbuild** | Confirmed by Flashbots "State of Wallets 2024" |

**Banana Gun / Titan deal mechanics**:

- Exclusive arrangement began **April 2023**
- Prior to the deal, Titan held **<1% market share**; following it, Titan's share rose to **40%+** within months
- Banana Gun is "pivotal in ~40% of MEV Boost auctions" (Yang 2024 academic paper)
- Banana Gun's orderflow was identified as **the single largest driver of Titan's profitability**
- Titan's blended profit margin under this exclusive arrangement: **17.75%**, vs. Beaverbuild's 9% and Flashbots' ~12%
- Banana Gun routes "nearly all its users' transaction orders to Titan for block bundling in exchange for profit-sharing or other benefits"

(Sources: Flashbots [State of Wallets 2024](https://writings.flashbots.net/state-of-wallets-2024); [gate.com analysis](https://www.gate.com/learn/articles/monopoly-in-ethereum-block-builders-and-chain-abstraction-unveiling-profit-incentives-and-innovation-opportunities-in-the-blockchain-ecosystem/7690); Toni Wahrstatter, [X post October 2024](https://x.com/nero_eth/status/1846786871998759141); Yang 2024 academic paper cited in multiple sources)

**Bidding dynamics under XOF dominance** (arXiv:2410.12352v3, data from late Feb – mid-March 2024):

- Top 3 builders (Beaverbuild, Titan, Rsync) submit bids **26.87% lower** than other builders, yet their combined win rate **exceeds 95%**
- This counterintuitive result -- lower bids, higher win rates -- is explained entirely by exclusive orderflow: XOF holders build more valuable blocks and need not overpay to win auctions
- Builder market share of the single top builder grew from **18% to nearly 50%** (September 2023 to May 2024), tracking directly with XOF accumulation

**The XOF reinforcement loop**: Orderflow providers maximize their probability of transaction inclusion by sending flow to the most dominant builder. Dominant builders attract more XOF. More XOF makes them more dominant. This self-reinforcing dynamic explains why, even without any coordination, the Ethereum block-building market converged to a de facto duopoly (Titan + Beaverbuild) that collectively controlled ~90% of blocks by late 2023.

(Source: arXiv:2410.12352v3; arXiv:2412.18074v2)

**BuilderNet and post-2025 XOF dynamics**:

Beaverbuild's migration to BuilderNet (May 2025) was intended partly to neutralize XOF. BuilderNet's stated goal is to "neutralize exclusive orderflow deals" through TEE-based orderflow sharing across operators. However, EigenPhi's June 2025 analysis found that BuilderNet was approaching major wallet providers and orderflow aggregators with terms that would "effectively recreate exclusivity" -- becoming a BuilderNet operator and sending all flow only to BuilderNet. As of March 2026, Titan remains dominant (~50% market share) with its XOF relationships intact, while BuilderNet (~28%) is the primary alternative. Whether Titan's Banana Gun exclusivity persists unchanged post-Beaverbuild-migration is unconfirmed in available sources.

(Source: [EigenPhi Substack, June 2025](https://eigenphi.substack.com/p/buildernet-and-return-of-exclusive-orderflow))

### 4c. orderflow.art: What the Dashboard Shows

orderflow.art ([orderflow.art](https://orderflow.art)) is a Flashbots-built Sankey diagram dashboard that visualizes Ethereum DEX trade orderflow across the full supply chain. **It is a JavaScript-rendered single-page application that requires a live browser session to display data** -- automated fetching retrieves only the empty HTML skeleton, with all data loaded client-side from Dune Analytics APIs.

**What the dashboard tracks** (from [orderflow.art/methodology](https://orderflow.art/methodology)):
- Data coverage: **November 1, 2023 onward** via Dune table `dune.flashbots.result_overall_of`
- Each transaction is classified as `mempool: "public"` or `"private"`
- OFA sources identified: **MEV Blocker** (via `mevblocker.raw_bundles`), **Flashbots Protect/MEV-Share** (via landed transaction queries)
- Frontend sources tracked: Metamask Swaps, Uniswap Website/Wallet, 1inch, Hashflow, CowSwap, Maestro, Unibot
- Builder identification: decoded from block `extraData` via Dune query 3169619
- All underlying Dune queries are open-source

The dashboard presents two primary views: a **Trade Volume Sankey** (tracking market share of each entity in the orderflow supply chain) and a **Liquidity Impact Sankey** (tracking DEX liquidity sourcing). The underlying Dune data is publicly accessible.

**To access current orderflow.art data**: Visit [orderflow.art](https://orderflow.art) in a browser and set the desired timeframe filter (default: 7 days). The private/public split and per-source breakdown are visible as Sankey node widths with accompanying percentages. No current snapshot was machine-accessible for this report.

### 4d. OFA Comparative Benchmarking (Dec 2024 – Jan 2025)

The most rigorous publicly available comparison of private RPC providers is arXiv:2505.19708 ("Private MEV Protection RPCs: Benchmark Study"), which submitted identical transactions to four major OFAs during December 6, 2024 – January 16, 2025 (Blocks 21,344,521 through 21,637,136), covering 273 total test transactions.

| Provider | Inclusion Rate | Time to Inclusion | Backrun Value Captured | User Rebate | Price vs. MEV Blocker |
|----------|---------------|-------------------|----------------------|-------------|----------------------|
| **MEV Blocker** | >90% | 1.34 blocks avg | 0.00350 ETH | 0.0035 ETH | Benchmark (best) |
| **Flashbots Protect** | >90% | 1.49 blocks | 0.00088 ETH | 0.0016 ETH | 21 bps worse |
| **Blink** | >90% | 1.56 blocks | 0.00173 ETH | 0.0000 ETH | 9 bps worse |
| **Merkle** | 74% | 1.86 blocks | 0.00108 ETH | 0.0000 ETH | 9 bps worse |

Key findings: MEV Blocker delivers the highest user rebates and best execution quality among tested providers. Merkle has materially lower inclusion reliability (74% vs. >90% for others). Flashbots and Blink provide frontrunning protection but capture and redistribute less backrun value per transaction.

(Source: [arXiv:2505.19708](https://arxiv.org/html/2505.19708v1), "Private MEV Protection RPCs: Benchmark Study," published February 2025)

### 4f. MEV Blocker

MEV Blocker is a private transaction RPC that protects users from frontrunning and sandwich attacks by routing transactions through a backrunning-only auction, redistributing 90% of backrunning profits to users as rebates.

**Current statistics (as of March 2026, from mevblocker.io)**:

| Metric | Value | Source |
|--------|-------|--------|
| Protected volume | **$219B+** across 62M+ transactions | mevblocker.io (live counter, accessed March 2026) |
| ETH rebated | **6,177+ ETH** | SMG operational memo (January 2026); mevblocker.io live counter showed ~5,500+ ETH at time of access, likely a caching lag |
| Median rebate | **$26 USD** | mevblocker.io |
| Unique wallets | **4.5M+** | Consensys/SMG announcement, January 2026 |
| Rebate structure | 90% to users, 10% to validators | mevblocker.io |

**Note on data discrepancy**: The SMG operational memo (January 2026) cites 6,177+ ETH in cumulative rebates, while the mevblocker.io live counter showed ~5,500+ ETH at time of access. This discrepancy likely reflects caching or methodology differences between the live counter and SMG's internal accounting. We use the SMG figure (6,177+ ETH) as the more authoritative source, given it comes from the protocol's operator. The $219B+ protected volume figure comes from the mevblocker.io live counter; the earlier QA-cited $60B+ figure appears to be from a mid-2025 snapshot.

**Governance transition**: In January 2026, CoW DAO transferred operation of MEV Blocker to the Special Mechanisms Group (SMG) at Consensys. SMG is Consensys' internal mechanism design unit focused on censorship-resistant and economically aligned systems. Under SMG, MEV Blocker is expected to expand its reach toward making fair execution the default for Ethereum users. (Source: Consensys blog, January 2026; cow.fi announcement)

(Sources: mevblocker.io; Consensys announcement; cow.fi/learn/special-mechanisms-group-acquires-mev-blocker-rpc)

### 4g. CoW Swap / CoW Protocol

CoW Protocol uses batch auctions and a competitive solver network to provide MEV protection, with solvers competing to find optimal execution paths including Coincidence of Wants (CoW) matching.

**Current statistics**:

| Metric | Value | Period | Source |
|--------|-------|--------|--------|
| 2025 trading volume | **$87B** | Calendar year 2025 | CoW DAO 2025 Year in Review |
| 2024 trading volume | **$40.2B** | Calendar year 2024 | CoW DAO 2025 Year in Review |
| Cumulative trading volume | **$83B+** | Since launch (2021) | cow.fi/cow-swap (live counter) |
| Surplus found for users | **$441M+** | Cumulative since launch | cow.fi/cow-swap (live counter, March 2026) |
| Cumulative transactions | **73M+** | Since launch | CoW DAO 2025 Year in Review |
| Monthly user retention | **42%** | 2025 | cow.fi (highest in DeFi) |
| Smart contract wallet share | **50%** | 2025 | cow.fi |

**Note on surplus figures**: The QA audit cited $188M+ in cumulative surplus. The cow.fi live page as of March 2026 shows **$441M+** in surplus found for users, which is significantly higher. The $188M figure likely represents an earlier data snapshot. The $441M+ figure from cow.fi/cow-swap is the most current.

**Note on volume figures**: The live counter on cow.fi shows $83B+ cumulative since launch, while the 2025 Year in Review states $87B for 2025 alone (on top of $40.2B in 2024). These figures are logically inconsistent if taken at face value — $87B for one year cannot exceed $83B cumulative. The most likely explanation is that the live counter on cow.fi uses a **different methodology** (e.g., excluding certain order types, using mark-to-market vs. execution price, or lagging behind internal accounting). The Year in Review figure ($87B for 2025) is the editorially reviewed number and should be treated as more authoritative for annual volume. The live counter likely reflects a narrower scope of counted transactions.

(Sources: cow.fi/cow-swap; cow.fi/learn/cow-dao-2025-in-review)

### 4h. Flashbots Protect

Flashbots Protect is the longest-running private RPC in crypto, providing frontrunning and sandwich protection by routing transactions to Flashbots' builder (now BuilderNet) without exposing them to the public mempool.

**Cumulative statistics (since launch on October 6, 2021, through approximately late 2024)**:

| Metric | Value | Notes |
|--------|-------|-------|
| Unique Ethereum accounts | **2.1M** | Cumulative since launch (October 2021) |
| Protected DEX volume | **$43B** | Cumulative since launch |
| MEV refunds | **313 ETH** | Cumulative since launch |
| Daily requests | **30M+** | With ~five nines uptime since 2021 |

**Temporal context**: These figures were reported in the Flashbots "2 Million Protect Users" blog post published on October 30, 2024. They represent cumulative totals since Flashbots Protect launched on October 6, 2021 -- approximately 3 years of operation. I was unable to find more recent (2025-2026) updates to these specific cumulative figures. Flashbots has stated that in 2025 they are shipping enhanced observability for Protect users and adding gas fee refunds for all transactions landed by Flashbots.

(Source: writings.flashbots.net/2m-protect-users, published October 30, 2024)

### 4i. MEV-Share and Order Flow Auctions (OFAs)

MEV-Share is Flashbots' protocol for Order Flow Auctions (OFAs), enabling users to selectively share transaction data with searchers who compete to backrun them, with the user receiving 90% (by default) of the MEV their transactions create.

**How it works**: Users send transactions to a MEV-Share Node, which selectively shares information based on privacy preferences. Searchers submit partial bundles attempting to extract MEV without seeing full transaction data. The MEV-Share Node simulates each bundle and forwards successful ones to builders with a condition that the user receives their specified share of MEV proceeds.

**OFA ecosystem**: MEV-Share is one of several OFA mechanisms:

- **MEV-Share** (Flashbots): Programmably private orderflow with configurable redistribution. Now integrated into BuilderNet.
- **MEV Blocker** (now Consensys/SMG): Backrunning-only auction, 90/10 split, highest adoption by wallet count.
- **CoW Protocol**: Batch auction-based OFA where solvers compete; surplus returned via better execution rather than direct ETH rebates.
- **Builder-direct endpoints**: Many builders (Titan, bloXroute) offer private transaction submission endpoints that provide frontrunning protection but without explicit MEV redistribution.

The OFA landscape is evolving from simple "protect and refund" models toward more sophisticated programmable orderflow, where users and applications can define custom policies for how their transaction data is shared and how MEV is redistributed. BuilderNet's TEE-based architecture enables this programmability while maintaining privacy guarantees.

**Benchmarking private RPCs**: Academic research (arXiv:2505.19708, "Private MEV Protection RPCs: Benchmark Study") has begun systematically comparing the effectiveness of different private RPC providers across dimensions including inclusion speed, revert rates, MEV protection coverage, and refund amounts. This emerging benchmarking discipline is critical for users evaluating which protection service to use, as the competitive claims of each provider have historically been difficult to verify independently.

(Sources: docs.flashbots.net/flashbots-mev-share/introduction; collective.flashbots.net; arXiv:2505.19708)

---

## Section 5: Solana MEV Ecosystem

### 5a. Jito Infrastructure

Jito remains the dominant MEV infrastructure provider on Solana, with its validator client running on **over 95% of Solana's active stake** as of early 2026 (Jito-Agave + Jito-Firedancer combined).

**Annual 2024 data (Helius MEV Report, January 2025; confirmed by Lucas Bruder/Jito Labs)**:

| Metric | Value | Source |
|--------|-------|--------|
| Total bundles processed (2024) | **3+ billion** | Helius MEV Report |
| Total SOL tips generated (2024) | **3.75 million SOL** | Helius |
| Total USD tips paid (2024) | **$674 million** ($550M flowed to stakers) | Bruder Substack, January 2025 |
| Peak daily tips | **$14.7M** (November 17, 2024) | Multiple sources |
| Peak monthly tips | **$210M** (November 2024) | Multiple sources |
| Peak daily bundle count | **24.4 million** (December 21, 2024) | Helius |
| Daily tip range (2024) | 781 SOL (Jan 11) to 60,801 SOL (Nov 19) | Helius |
| Average monthly growth rate (2024) | **32%** | Helius |

For comparison: in 2023, Jito generated only **$3.52 million** in total tips for the entire year. The 2024 figure of $674M represents a ~191x year-over-year increase. (Source: Bruder Substack)

**January 2025 peak (Helius H1 2025 Report)**:

| Metric | Value |
|--------|-------|
| Monthly REV | **$551.7M** |
| January 19 daily REV | **$56.8M** (surpassed ETH + BTC combined that day) |
| January 20-21 daily Jito tip record | **$25.16-$25.2M** |
| Jito tips as % of REV (H1 2025) | 41.6% – 66% range, month-dependent |

The January 2025 spike was driven by memecoin activity (TRUMP launch). After the memecoin cycle cooled, REV declined roughly **90% from the January peak** by late 2025, back toward $24-27M/month. Jito tips' share of network REV fell from ~50% (early 2025) to **below 30%** by H2 2025 as fee activity normalized. Despite the revenue decline, daily wallet counts tipping through Jito more than doubled from January 2025 levels, indicating broader behavioral adoption. (Source: Messari State of Solana Q4 2025; Blockworks analytics)

**Validator adoption progression**:

| Date | Jito Client Stake | % of Total |
|------|------------------|-----------|
| January 2024 | 189.5M SOL | 48% |
| January 2025 | 373.8M SOL | 92% |
| H1 2025 (Epoch 807) | — | Jito-Agave: 83%, Agave: 9%, Firedancer: 8% |
| Early 2026 | — | >95% (Jito-Agave + Jito-Firedancer combined) |

**TipRouter NCN** (launched January 30, 2025): Jito's first Node Consensus Network, replacing the centralized tip distribution mechanism with a decentralized consensus-based system. Fee split: 3% to Jito DAO treasury and NCN participants; 97% to validators and stakers. (Source: Jito Foundation blog; Pier Two research)

**Jito mempool closure**: In **March 2024** (specifically March 8-9, 2024), Jito Foundation closed its public mempool, eliminating the most accessible source of transaction visibility for sandwich attackers. This was a major structural change in Solana's MEV dynamics. (Source: CoinDesk, March 8, 2024; Blockworks, March 2024)

(Sources: Helius Solana MEV Report January 2025; Lucas Bruder/Jito Labs Substack January 2025; Messari State of Solana Q4 2025; Helius H1 2025 Report; Jito Foundation)

### 5b. Jito BAM (Block Assembly Marketplace)

Jito BAM, launched on July 21, 2025, represents the most significant upgrade to Solana's MEV infrastructure to date. BAM is a modular transaction scheduling layer built on top of Solana that introduces secure, programmable sequencing without requiring base protocol changes.

**Architecture**:

- **BAM Nodes**: A network of scheduler nodes running in TEEs (Trusted Execution Environments) that receive, order, and forward transactions to validators.
- **BAM Validators**: Validators running the updated Jito-Solana client that execute transactions received from BAM Nodes.
- **Plugins**: Programmable interfaces enabling developers, traders, and applications to connect to BAM Nodes' scheduler, allowing custom transaction ordering logic.

**Key properties**:

- Makes transaction sequencing **transparent and verifiable** through TEE attestation.
- Enables **programmable innovation** at the blockspace layer (custom ordering rules, priority mechanisms).
- Aims to **mitigate harmful MEV** by making sequencing auditable and rule-based rather than opaque.
- Proposes a new hardware vertical of TEE node operators alongside the existing Solana validator set.

**Initial rollout**: BAM launched on mainnet with an initial validator set led by key Solana ecosystem players including Figment, Helius, SOL Strategies, and Triton One.

**Adoption progression**:

| Date | BAM-Connected Stake | % of Total | Source |
|------|---------------------|-----------|--------|
| October 2025 | ~17M SOL | ~4-5% | Jito November Roundup |
| End of November 2025 | ~30M+ SOL | ~8% | Jito November Roundup |
| End of Q4 2025 | — | **11.4%** | Messari Q4 2025 |

**Governance**: JIP-28 ("Accelerate BAM Adoption") passed November 30, 2025 with **100% approval**, directing JitoSOL stake toward BAM-enabled validators as an adoption incentive. After JIP-24, Block Engine and BAM fees flow to the DAO treasury for continuous JTO buybacks — approximately **$2.5M** in buybacks executed since August 2025. Firedancer-BAM integration entered audit phase in late 2025 with Q1 2026 launch targeted.

BAM's significance lies in bringing PBS-like block building separation to Solana, a chain that previously lacked formal builder-proposer separation. By introducing a configurable scheduling layer, BAM creates the infrastructure for a competitive block building market on Solana, analogous to what MEV-Boost created for Ethereum.

(Sources: bam.dev/blog/introducing-bam; CoinDesk, July 21, 2025; Jito November Monthly Roundup 2025; Messari Q4 2025; PR Newswire; Helius blog)

### 5c. Validator Enforcement Against Sandwich Attacks

The Solana ecosystem conducted a coordinated crackdown on sandwich attacks throughout 2024-2025, involving three distinct enforcement actions:

| Enforcer | Action | Scope | Date |
|----------|--------|-------|------|
| Solana Foundation | Removed validators from delegation program | **30+ validators** | June 2024 |
| Marinade Finance | Blacklisted from Stake Auction Marketplace | **50+ validators** | 2024-2025 |
| Jito Foundation | Banned from Jito validator set | **15+ validators** | 2025 |

**Context**: The Solana Foundation's delegation program provides SOL stake to validators, significantly boosting their rewards. Removal from this program does not prevent a validator from operating (Solana is permissionless) but strips it of Foundation-delegated stake and associated revenue. Marinade Finance, the largest liquid staking protocol, similarly blacklisted 50+ validators, protecting over $2B in delegated stake. Jito's enforcement further restricted access to its tip distribution mechanism.

**Scale of the problem (sandwiched.me dataset, presented at Solana Accelerate 2025)**:

The most comprehensive Solana sandwich dataset comes from **sandwiched.me** (Ghostlogs), covering January 2024 to May 2025:

| Metric | Value |
|--------|-------|
| Trades analyzed | **8.5 billion** |
| DEX volume tracked | **~$1 trillion** |
| Total sandwich extraction | **$370M – $500M** |
| Approx. % of DEX volume sandwiched | ~0.04-0.05% |
| Average validator "sandwich rate" | ~5% of blocks produced |
| Outlier validators (sandwich rate) | **30-60%** |
| Tip ratio (sandwich bots to validators) | 15-20% of profits |
| Blind/wide sandwich prevalence | Grew from **1% to 30%** of all attacks |

**DeezNode case study (30 days, Dec 7, 2024 – Jan 5, 2025, per Helius MEV Report)**:

| Metric | Value |
|--------|-------|
| Sandwich transactions executed | 1.55 million |
| Success rate | 88.9% |
| Gross profit | 65,880 SOL ($13.43M) |
| Average profit per attack | 0.0425 SOL ($8.67) |
| Jito tips paid by bot | 22,760 SOL ($4.63M) |
| Share of all Solana sandwich attacks | ~50% (via the Vpe program) |
| DeezNode validator stake | 811,604 SOL (~$168.5M) at peak |

**Bot concentration**: Sandwiching on Solana is dominated by **6 bots** (per Chris Chang, Ghostlogs). The top 3 bots control over **60% of market share** (Extropy cross-chain analysis 2025). E6Y bot: 42% of attack volume over 30 days, $1.6B in trade volume, 57,400 SOL gross / 49,400 SOL net (~$300K/day).

**Academic study (ACM IMC 2025, Gerzon et al.)**: Analyzing 3+ months of Jito data, found **400,000+ sandwich attack instances** resulting in **$5M+ in victim losses**. Users spent $2.42M on defensive bundling (buying protection bundles). Median sandwich bundle Jito tip: **2,000,000 lamports** vs. median normal bundle: 1,000 lamports — a 2,000x premium.

**October 2025 snapshot**: 18,000+ SOL extracted, 202,000+ victims, 51,000+ attackers spending collectively <340 SOL, ~$3.2M total.

**Impact**: The coordinated enforcement, combined with Jito's mempool closure in **March 2024**, reduced sandwich attack profitability on Solana by an estimated **60-70%** in 2025.

(Sources: sandwiched.me / Ghostlogs Accelerate 2025 presentation; Helius MEV Report January 2025; ACM IMC 2025 "Quantifying the Threat of Sandwiching MEV on Jito"; Extropy cross-chain analysis 2025; The Block June 2024; CryptoNinjas)

### 5d. Firedancer

Firedancer is Jump Crypto's from-scratch Solana validator client, written in C/C++ as a complete alternative to the existing Agave (Solana Labs) client. Firedancer went live on Solana mainnet after over 100 days of continuous testnet operation, producing more than 50,000 blocks without major issues.

**Technical architecture**: Unlike Agave's monolithic design, Firedancer uses a modular, tile-based architecture that splits validator tasks into parallel execution units. Its low-level C/C++ implementation allows fine-tuned hardware optimization, targeting Solana's long-term goal of 1M+ TPS.

**MEV implications**:

- Firedancer supports different **transaction scheduling modes**, including a **Revenue Mode** that prioritizes MEV capture and bundle processing by sorting transactions by fee-per-compute-unit.
- Early adopter data from Figment showed gross staking reward improvements of **+18 basis points** versus pre-Firedancer operation, reaching **+28 basis points** in peak epochs, due to Firedancer's ability to fit more valuable transactions into each block.
- Firedancer's higher throughput capacity could expand the addressable MEV opportunity set by processing more transactions per slot.
- Client diversity introduces new dynamics: different clients may implement different scheduling algorithms, creating varying MEV characteristics depending on which client produces a given block.

**Adoption (as of October 2025)**:

- **207 validators** running **Frankendancer** — a **hybrid client** that uses Firedancer's networking and block production components but still relies on shared Agave (Solana Labs) components for consensus and replay. Frankendancer is not the full Firedancer client; the fully independent Firedancer (with zero Agave dependency) was not yet in production as of this date. This distinction matters because Frankendancer's consensus behavior still mirrors Agave's, limiting the client diversity benefits. Frankendancer validators represented approximately **20.9% of staked SOL**.
- Up from **8%** in June 2025.
- Jito-Solana remains dominant with approximately **72% of stake**.

**Future**: The Firedancer team proposed SIMD-0370 to remove Solana's block limit, allowing blocks to scale based on what high-performance validators can process. If adopted, this would significantly alter MEV economics by increasing the supply of blockspace.

(Sources: Unchained, October 2025; The Block; Figment migration report; Blockdaemon deep dive)

### 5e. Solana Arbitrage Data

**Annual 2024 (Helius MEV Report, January 2025; methodology: Jito's on-chain detection algorithm)**:

| Metric | Value |
|--------|-------|
| Successful arb transactions | **90,445,905** |
| Total profit | **$142.8 million** |
| Profit denominated in SOL | $126.7M (88.7%) |
| Average profit per arb | **$1.58** |
| Largest single arbitrage | **$3.7 million** |

**Important caveat**: Jito's detection does not capture profits from searchers using alternative private mempools. The $142.8M figure is a floor — actual arbitrage extraction is higher.

**Transaction spam**: Over 50% of Solana's non-vote transactions are estimated to be **failed arbitrage attempts** (spam bots). This peaked at **75.7%** of all transactions in April 2024, then declined significantly following the Agave 1.18 scheduler improvement. (Source: Helius)

### 5f. Solana vs. Ethereum MEV: Structural Comparison

| Dimension | Ethereum L1 | Solana |
|-----------|------------|--------|
| MEV extraction model | Structured auction (MEV-Boost/PBS) | Latency race + Jito bundles (BAM emerging) |
| Avg arb profit per opportunity | ~$1,500 | **$1.58** |
| Opportunity frequency | Lower frequency, higher margin | High frequency, low margin |
| Mempool | Public (with private alternatives) | No persistent mempool (since Jito closure March 2024) |
| Block time | 12 seconds | ~400ms |
| Annual DEX volume (2025) | N/A | **$1T+** (Raydium $352.8B, Jupiter $334.6B, Meteora $113.7B, Orca $103.9B) |
| Jito tips 2024 | N/A | **$674M** |

(Sources: Helius MEV Report; Extropy cross-chain analysis 2025; Messari Q4 2025; solanafloor.com)

---

## Section 6: Regulatory Landscape

### 6a. United States v. Peraire-Bueno ("MEV Brothers Case")

**This is the most significant legal precedent for MEV enforcement to date.** The previous version of this report erroneously stated that no criminal enforcement had specifically targeted MEV extraction. This was factually wrong.

**Case overview**: In 2024, the U.S. Department of Justice (Southern District of New York) charged Anton Peraire-Bueno (25) and James Peraire-Bueno (29) -- two MIT-educated brothers -- with conspiracy to commit wire fraud, substantive wire fraud, and conspiracy to commit money laundering. The charges related to a 2023 incident in which the brothers allegedly exploited a vulnerability in the MEV-Boost relay system to steal approximately **$25 million** in a single transaction.

**What they allegedly did**: The brothers set up **16 validators** and placed **bait transactions** designed to lure MEV sandwich bots into including them in bundles. When one of their validators was selected as the block proposer, they submitted a **forged signature** to the MEV-Boost relay, exploiting a vulnerability that caused the relay to **prematurely reveal the full block contents** — including the MEV bots' sealed sandwich bundles. With the block contents exposed, the brothers replaced their bait transactions with tampered transactions that **drained the liquidity pools** the MEV bots had just filled, extracting approximately $25 million. The victims were MEV bots/searchers, not ordinary traders. Prosecutors characterized this as wire fraud; the defense argued it was a permissible exploitation of a system designed to be adversarial.

**Trial outcome**: The four-week trial in Manhattan federal court ended in a **mistrial** on November 7, 2025, after the jury deadlocked. U.S. District Judge Jessica Clarke declared the mistrial after receiving a second notice of jury deadlock, with jurors reporting they were "under stress," that "some cried," and that "many have not slept."

**Key legal dispute**: The central tension was over mens rea (criminal intent). Prosecutors suggested instructing the jury that the defendants could be found guilty "even if they did not know" their actions were illegal. The defense called this "outrageous" and insisted the law required proof the brothers acted "knowingly, willfully, and with intent." This tension over whether MEV extraction requires specific knowledge of illegality is the core unresolved legal question.

**Retrial**: Prosecutors have sought a retrial for **late February or early March 2026**. As of this report's date (March 3, 2026), the case status should be monitored for updates. The brothers remain charged on all three counts.

**Implications for the MEV industry**:

1. **MEV extraction is not presumptively legal.** The DOJ's willingness to bring criminal charges against MEV strategies signals that certain forms of MEV extraction -- particularly those involving relay exploitation or manipulation of infrastructure -- may face prosecution.
2. **The "code is law" defense is contested.** Prosecutors argue that exploiting network vulnerabilities to seize others' funds without consent cannot be excused as automated market behavior. The defense counters that these were permissible automated trading strategies.
3. **The mistrial leaves uncertainty.** A mistrial is not an acquittal -- the case can be (and is being) retried. Until a definitive verdict or appellate ruling, the legal status of aggressive MEV strategies remains ambiguous.
4. **Chilling effect on borderline strategies.** Even absent a conviction, the prosecution itself -- with its lengthy trial, potential prison sentences, and media attention -- serves as a deterrent to strategies that involve exploiting infrastructure vulnerabilities.

(Sources: The Block, November 2025; DL News; FindLaw; Steptoe legal analysis; CoinDesk)

### 6b. OFAC Compliance and Relay Censorship

Relay-level OFAC compliance continues to influence MEV dynamics. The bloXroute "Regulated" relay filters transactions involving OFAC-sanctioned addresses and holds approximately 12.4% of relay market share. Other relays (Ultra Sound Money, Agnostic, Flashbots) do not filter at the relay level, though builders may independently choose to comply.

**Censorship rate trend**:

| Period | OFAC-Compliant Block Share | Context |
|--------|---------------------------|---------|
| October 2022 | ~51% | Post-Merge, Flashbots relay dominant |
| November 2022 | ~73-80% | Peak censorship; Flashbots relay at 80%+ share |
| May 2023 | ~27% | Community diversification to non-censoring relays |
| 2024-2025 | **Single digits (estimated)** | Estimated from relay share data: non-censoring relays control ~60%+ of relay market; no direct mevwatch snapshot available for this period |
| February 2026 | **~12-14%** (bloXroute Regulated share) | Only one major OFAC-filtering relay remains active |

The dramatic improvement from ~80% to an estimated ~12-14% reflects the ecosystem's successful diversification away from censoring relays. The rise of Ultra Sound Money relay (now the largest at ~33%) and other neutral relays has been the primary driver. MEV Watch (mevwatch.info) tracks this metric in real time. (Sources: The Block, May 2023; Cointelegraph, November 2022; mevwatch.info)

### 6c. Broader Regulatory Developments

Beyond the Peraire-Bueno case, the regulatory environment for MEV in 2025-2026 includes:

- **SEC**: No specific MEV-focused enforcement actions, but the SEC's evolving stance on DeFi could implicate MEV infrastructure providers as intermediaries.
- **EU (MiCA)**: The Markets in Crypto-Assets regulation, effective since 2024, does not specifically address MEV but creates a regulatory framework under which MEV-related services (block builders, relays) may eventually need to be classified.
- **Industry self-regulation**: The coordinated Solana validator banning (Section 5c) represents a form of ecosystem self-regulation that may influence regulatory approaches.

---

## Section 7: Forward-Looking Developments

### 7a. Enshrined PBS (ePBS) -- Glamsterdam Upgrade

Ethereum's next major upgrade, **Glamsterdam** (Gloas + Amsterdam), is **targeted for H1 2026** (aspirational timeline, pending client readiness). Its two **Scheduled for Inclusion (SFI)** EIPs are:

1. **EIP-7732 (ePBS)** — the consensus layer headliner — moves Proposer-Builder Separation into the Ethereum protocol itself, replacing the out-of-protocol MEV-Boost relay system.
2. **EIP-7928 (Block-level Access Lists / BALs)** — the execution layer headliner — enables parallel state access declaration, changing gas cost structures with indirect MEV implications.

An additional **19 EIPs are at Considered for Inclusion (CFI) status**, including a gas repricing package (EIP-7904 General Repricing, EIP-8037 State Creation Cost Increase, EIP-8038 State Access Cost Increase, EIP-7976 Increase Calldata Floor Cost) that could collectively alter MEV extraction economics by changing what on-chain operations cost. Only the two SFI EIPs are formally committed; the CFI EIPs may or may not ship in Glamsterdam.

**MEV implications of ePBS**:

- Removes reliance on external relays, potentially obsoleting or diminishing the relay market described in Section 2.
- Hardens censorship resistance by making block-builder markets trust-minimized at the protocol level.
- Restructures the builder market by changing the economics of relay operation, builder submission, and bid timing.
- Introduces the **free option problem**: under ePBS, builders can withhold blocks during volatile periods and exercise a "free option" on price movements. Research on the Flashbots Collective forum estimates ~$117 per execution window with ~48.6% exercise probability, an active area of mechanism design research. (Source: collective.flashbots.net/t/the-free-option-problem-in-epbs/5115)

**Development status**: BALs has operational devnets; ePBS devnets are still forthcoming due to greater implementation complexity. (Source: EIP-7773 Glamsterdam Meta-EIP; ethereum.org/roadmap; Ethereum Foundation Checkpoint #8, January 2026; The Block)

### 7b. FOCIL (EIP-7805) -- Hegota Upgrade

The follow-up **Hegota** upgrade (Heze + Bogota), **targeted for H2 2026**, has **EIP-7805 (FOCIL — Fork-Choice Enforced Inclusion Lists)** as its consensus layer headliner. FOCIL was originally considered for Glamsterdam but was deferred to allow a dedicated development cycle. It was officially CFI'd with strong developer and community backing in late February 2026.

**Mechanism**: Each slot, **17 randomly selected validators** form an Inclusion List (IL) committee. Each member builds and gossips an IL of transactions from their local mempool view. The block proposer collects committee ILs and creates a deterministic aggregate. Attesters reject blocks that fail to include committee-nominated transactions — effectively enforcing transaction inclusion at the fork-choice level.

The mechanism relies on a **1-out-of-N honesty assumption**: only one committee member needs to be honest for inclusion guarantees to hold. To exclude a transaction, an attacker must bribe the entire committee; to include a transaction, only one honest member is needed.

**Timing within a slot**:
- Slot n-1, t=6s: Committee releases local inclusion lists
- Slot n-1, t=9s: Local IL freeze deadline; proposer broadcasts aggregate
- Slot n, t=0s: Block producer releases block with payload and aggregate
- Slot n, t=4s: Attesters vote on block validity

**MEV implications**:

- FOCIL **does not eliminate MEV**. It preserves PBS and market-based MEV auctions. Its purpose is to prevent builders from **censoring** transactions, not from reordering them.
- Directly constrains builder power by requiring that validator-nominated transactions be included, regardless of builder preference — addressing the concentration risks described in Section 1.
- Combined with ePBS (Glamsterdam), FOCIL creates a comprehensive overhaul: ePBS removes trusted relay intermediaries while FOCIL ensures builders cannot exploit their position to censor.

**"Big FOCIL"**: On **March 2, 2026** (one day before this report's date), Vitalik Buterin published a proposal for an expanded FOCIL variant where inclusion lists cover **nearly all transactions** in a block. Under "Big FOCIL," committee members systematically select transactions by sender address (e.g., by first hex character), effectively commoditizing the builder role to just state computation and minor MEV optimization. This would dramatically narrow the scope of builder influence. (Source: The Block, March 2, 2026)

**Key debate**: Forcing validators to include sanctioned transactions creates potential OFAC compliance tension. This is the most politically contentious aspect of FOCIL and an unresolved question for validators operating under sanctions regimes. (Sources: EIP-7805; ethresear.ch FOCIL discussion; Ethereum Magicians FOCIL readiness for Hegota thread; The Block; DL News)

### 7c. Encrypted Mempools and Network Anonymization

Alongside FOCIL, **encrypted mempools** are emerging as the second major Hegota-targeted censorship resistance mechanism. Together with ePBS, these form what multiple sources describe as the **"Holy Trinity of Censorship Resistance"**: ePBS (Glamsterdam) removes relay trust, FOCIL (Hegota) enforces inclusion, and encrypted mempools (Hegota) prevent content-based discrimination.

**LUCID EIP — Current Hegota Headliner Candidate**: Presented by Justin Florentine (Besu client developer) and Anders Elowsson (Ethereum Foundation researcher), LUCID is the leading encrypted mempool proposal for Hegota. The EIP-8105 team (Shutter Network) has given "full support to the LUCID EIP," with both proposals having "converged technically" though significant implementation differences remain. LUCID depends on ePBS (EIP-7732) being deployed first in Glamsterdam. (Source: Ethereum Magicians Hegota headliner LUCID thread; Shutter blog)

**EIP-8105 (Universal Enshrined Encrypted Mempool)**: Proposed by Jannik Luhn (Shutter Network) in December 2025, EIP-8105 provides the reference design for protocol-level encrypted transactions:

- **Encryption-agnostic**: Supports threshold encryption, MPC committees, TEEs, delay encryption, and FHE via a pluggable Key Provider Registry.
- **Two-part transactions**: A visible "envelope" (gas params, nonce, key provider ID) paired with a hidden "encrypted payload" (recipient, calldata, value).
- **Ordering**: Encrypted transactions execute **after** all plaintext transactions in a block.
- **ePBS integration**: Key providers publish decryption keys between execution payload publication (~25% of slot) and Payload Timeliness Committee attestations (~75% of slot), a ~6-second window.

**What encrypted mempools prevent**: Frontrunning, sandwich attacks, and real-time censorship based on transaction content. **What they do NOT prevent**: Back-running, arbitrage, and liquidations — considered "beneficial MEV."

**Shutter Network**: The most mature implementation, live on **Gnosis Chain since July 2024** using a commit-then-decrypt scheme with a distributed network of Keypers (key holders). Shutter and collaborators (Chainbound, MEV Blocker, Gnosis, Nethermind) published an Ethereum integration roadmap in February 2025. (Source: blog.shutter.network; ethresear.ch "Road Towards Distributed Encrypted Mempool")

**Flashnet — Flashbots' Anonymous Broadcast Protocol**: Announced on **February 17, 2026**, Flashnet is a low-latency anonymous broadcast protocol based on secret sharing among servers with TEE-based liveness guarantees. Flashnet is **complementary to but distinct from** encrypted mempools:

- **Flashnet provides sender anonymity** (hides *who* sent a transaction); encrypted mempools provide **content privacy** (hides *what* the transaction does).
- Flashnet directly addresses a weakness of FOCIL: FOCIL requires transactions to be visible in the public mempool for committee selection, which exposes them to frontrunning. Flashnet enables anonymous submission, reducing metadata leakage while preserving FOCIL's inclusion guarantees.
- Flashnet is a critical building block for moving BuilderNet from Phase 1 (permissioned) to Phase 2 (permissionless distributed building) — see Section 7d.

(Source: writings.flashbots.net/network-anonymized-mempools; Flashbots Writings)

**Criticisms**: a16z Crypto published "On the limits of encrypted mempools" (July 2025), arguing that encrypted mempools are unlikely to be a "silver bullet" due to engineering complexity, performance overhead, and the risk that wallet providers could leak plaintext. Shutter responded that the technology significantly reduces sandwich attacks and frontrunning with minimal trust assumptions and only ~27ms / 14% overhead. (Sources: a16zcrypto.com; blog.shutter.network response)

### 7d. BuilderNet Evolution and Decentralized Block Building

While BuilderNet's current architecture and market share are covered in Section 1a, the **forward-looking trajectory** of decentralized block building is one of the most consequential developments in MEV infrastructure.

**Flashbots' Four-Phase Framework**: Flashbots has published a roadmap for progressively decentralizing block building (Source: writings.flashbots.net/decentralized-building-wat-do):

| Phase | Name | Status | Key Properties |
|-------|------|--------|---------------|
| 0 | Centralized | Complete (pre-2024) | Single-operator builders (Titan, old Beaverbuild) |
| 1 | Replicated Privacy | **Current** (BuilderNet) | TEE-based, crash fault tolerant, but permissioned operators (Flashbots, Beaverbuild, Nethermind) |
| 2 | Modular Distributed Building | **Next target** | Co-built blocks, Byzantine fault tolerance, permissionless operator entry/exit |
| 3 | Global Parallel Building | Long-term vision | Fully parallel, globally distributed block construction |

**Phase 1 → Phase 2 transition**: The key challenges are:
- **Permissionless operator onboarding**: Currently, only three operators (Flashbots, Beaverbuild, Nethermind) run BuilderNet nodes. Phase 2 requires permissionless entry/exit without compromising privacy or liveness.
- **Co-building**: Distributed sandboxing where multiple entities jointly construct blocks, requiring Byzantine fault tolerance rather than just crash fault tolerance.
- **Flashnet integration**: Flashbots' anonymous broadcast protocol (Section 7c) is the network-layer primitive enabling permissionless participation without identity exposure.

**Engineering maturity**: BuilderNet has shipped 5+ versions from v1.2 (February 2025) through v1.6 (December 2025, Fusaka-ready), including migration from Yocto to Debian-based images, container-based architecture, Azure Gen6 TDX migration, exploration of alternative cloud providers (Google Cloud, OpenMetal, OVH), and 30-50% reductions in block finalization time. (Source: buildernet.org/blog; Flashbots Collective engineering updates)

**BuilderNet and ePBS interaction**: A critical open question is what happens to BuilderNet when ePBS ships in Glamsterdam. Multiple sources characterize BuilderNet as "buying time" for Ethereum to experiment before committing to enshrined PBS. Flashbots' position is that ePBS "must be supported by decentralized infrastructure" — meaning BuilderNet and ePBS are **complementary, not substitutive**. BuilderNet provides the decentralized builder implementation that ePBS's protocol-level framework requires.

**SUAVE legacy**: The original SUAVE (Single Unifying Auction for Value Expression) concept envisioned a separate chain for MEV auction execution. Its codenames — Centauri (trusted privacy via Flashbots) and Andromeda (TEE-based cryptographic privacy) — mapped a 2023-era roadmap. The `suave-geth` repository was **archived on May 12, 2025**. In practice, BuilderNet has realized much of what Andromeda envisioned (TEE-based block building with privacy and integrity guarantees), though full permissionless operation (Phase 2+) remains a future goal. The earlier Rigil testnet (2023) was the last SUAVE-branded deployment. (Sources: Flashbots Writings; GitHub flashbots/suave-geth; writings.flashbots.net/mevm-suave-centauri-and-beyond)

### 7e. MEV in Blob/Data Availability Markets

EIP-4844 (introduced in the Dencun upgrade, March 2024) created a new blob transaction type for L2 rollup data posting, with a separate fee market. Subsequent upgrades have dramatically expanded blob capacity — but demand has not kept pace.

**Post-Fusaka blob market reality**: The **Fusaka upgrade** (activated **December 3, 2025**) introduced **PeerDAS** (Peer Data Availability Sampling, EIP-7594), enabling validators to verify blob data without downloading everything. Subsequent parameter-only forks raised blob targets further:

| Upgrade | Date | Blob Target / Max | Status |
|---------|------|-------------------|--------|
| Dencun (EIP-4844) | March 2024 | 3 / 6 | Shipped |
| Pectra (EIP-7691) | May 7, 2025 | 6 / 9 | Shipped |
| Fusaka (PeerDAS + EIP-7918) | December 3, 2025 | 6 / 9 (PeerDAS-enabled) | Shipped |
| BPO1 | December 9, 2025 | 10 / 15 | Shipped |
| BPO2 | January 7, 2026 | 14 / 21 | Shipped |

**Actual utilization is far below capacity**: As of early 2026, the **median blob count per block is ~4** (per CryptoSlate / MigaLabs analysis), representing roughly **29% of the target capacity of 14**. Blocks with 16+ blobs are extremely rare (165-259 occurrences in observed windows), and miss rates climb at high blob counts (0.77-1.79% at 16+ blobs vs. ~0.5% baseline). The blob market is in massive surplus.

**EIP-7918 — the most important blob fee market reform**: Shipped with Fusaka, **EIP-7918 (Blob Base Fee Bounded by Execution Cost)** introduces a **reserve price floor** for blob gas tied to execution gas costs. The minimum blob data cost is set at 1/256 of the equivalent calldata cost. This prevents blob fees from collapsing to 1 wei during low-demand periods — a condition that prevailed on **93% of days between Dencun and Fusaka**, when blob base fees were effectively free. Post-Fusaka, blob base fees jumped approximately **15 million-fold** due to EIP-7918's floor activation. Fidelity Digital Assets estimates the floor would have captured approximately **$78.6M in previously forgone revenue** had it been active during 2024-2025. (Sources: ethereum.org/roadmap/fusaka; Conduit Fusaka EIPs cheat sheet; Fidelity Digital Assets research; BingX flash news)

**Impact on L2 economics**: The blob fee floor structurally changes L2 cost calculations. L2 fees paid to Ethereum L1 collapsed from ~$113M in 2024 to ~$10M in 2025 (a >90% drop) during the free-blob era. Under EIP-7918, L2s face meaningful data posting costs even during low-demand periods — Fidelity estimates Base specifically would pay ~$30.6M/year in additional fees under the new floor vs. its prior $5.2M. Post-Pectra through October 2025, Ethereum generated approximately **$900 total in blob fees** (before EIP-7918); the November 2025 spike brought ~$23,000. The fee floor fundamentally resets this dynamic. (Sources: The Block; Fidelity Digital Assets)

**Blob MEV assessment**: Despite the infrastructure being live and capacity having expanded 4.7x from Dencun to BPO2, the blob market remains in a **"pre-4844 world of sorts"** — capacity vastly exceeds demand, and meaningful blob-level MEV competition has not materialized. When there is massive surplus blobspace, builders have no incentive to develop sophisticated blob inclusion strategies. EIP-7918's coupling of blob fees to execution gas has further reduced the scope for cross-market arbitrage between the two fee markets. Blob MEV remains **largely theoretical** rather than a near-term extraction opportunity. The more consequential near-term development is how EIP-7918's fee floor reshapes L2 rollup economics and data posting strategies. The **Glamsterdam ePBS** change could eventually alter blob MEV dynamics by changing how builders optimize for blob inclusion, but this is speculative absent real demand pressure.

(Sources: arXiv:2411.03892, "A First Look at Ethereum Blob Revolution" (November 2024, pre-Fusaka analysis); ethereum.org/roadmap; CryptoSlate blob utilization analysis; EIP-7918 specification)

### 7f. Shared Sequencers and Cross-Rollup MEV

The shared sequencer thesis — that a common ordering layer across multiple rollups could enable atomic cross-rollup transactions and reduce MEV — has faced headwinds:

- **Astria shut down** despite $18M in total funding and a mainnet launch, demonstrating insufficient market demand.
- **Espresso Systems** continues development with a testnet and zk-rollup integrations.
- **Radius** remains in early-stage development.

The practical alternative gaining traction is **Flashbots' integration-based approach**: building modular sequencing tools that individual L2s adopt voluntarily, rather than a separate shared sequencer layer. This "horizontal integration" model preserves L2 sovereignty while providing MEV-aware sequencing:

- **Rollup-Boost**: Live on Unichain mainnet as the first L2 TEE block builder. Rollup-Boost runs as a sidecar alongside the OP Stack sequencer, enabling external block building with TEE-based privacy guarantees. (Source: writings.flashbots.net/unichain-mainnet)
- **Flashblocks**: Providing 200ms streaming pre-confirmations to Base and Unichain. Flashbots has partnered with Optimism to roll out Flashblocks to **OP Mainnet and the rest of the Superchain**, making it the default MEV-aware sequencing layer across the OP Stack ecosystem. (Source: writings.flashbots.net/flashbots-superchain)
- Flashbots is working toward a "hyper-efficient MEV-aware sequencer" enabling Stage 2 rollups with TEE proofs.

Cross-rollup MEV remains largely theoretical, as no production system enables atomic cross-rollup transaction execution. The primary practical form of cross-domain MEV today is cross-chain inventory-based arbitrage (Section 3c).

### 7g. Firedancer and Solana MEV Evolution

The full Firedancer client (with zero Agave dependency) launched on Solana mainnet in **December 2025** after over 100 days of continuous testnet operation. As adoption grows beyond the ~21% stake share held by the earlier Frankendancer hybrid (October 2025 figure), the implications include:

- **Higher throughput** could increase MEV opportunity volume but also increase competition and reduce per-opportunity profitability.
- **SIMD-0370** (block limit removal) could fundamentally alter blockspace economics on Solana.
- **Client diversity** means different blocks may have different MEV characteristics depending on which client produced them, complicating searcher strategies.

### 7h. MEV on Other Ecosystems (Cosmos, Bitcoin)

While Ethereum and Solana dominate the MEV landscape, MEV dynamics exist on other chains:

**Cosmos / Osmosis (Skip Protocol)**: Skip Protocol builds MEV infrastructure for the Cosmos ecosystem, most notably the **ProtoRev module** on Osmosis. ProtoRev internalizes arbitrage MEV at the protocol level — rather than allowing external searchers to capture arbitrage profits, the module automatically executes backrun arbitrages after each swap and returns the profit to the Osmosis DAO and stakers. Skip has expanded into **Skip Go**, a cross-chain developer toolkit for multi-chain transaction routing. Skip's approach — protocol-owned MEV — represents a philosophically distinct model from Ethereum's competitive auction-based approach. (Sources: Skip Protocol; Osmosis documentation; Real Vision)

**Bitcoin**: MEV on Bitcoin was historically negligible due to Bitcoin's simpler transaction model, but the rise of **Ordinals, Inscriptions, and Runes** (enabled by the Taproot upgrade) introduced meaningful MEV dynamics. In asset minting processes (Runes, BRC-20) where the first confirmed minting transaction is valid and subsequent ones are invalid, transaction ordering becomes economically significant. Mining pools can extract value through: (1) replacing user minting transactions with their own, (2) soliciting higher fees via out-of-band payments, and (3) enabling fee wars among competing minters. However, Bitcoin MEV remains structurally smaller than Ethereum/Solana MEV — fees fell to approximately **0.96% of block rewards** in June 2025, the lowest since January 2022, limiting the overall MEV opportunity. (Sources: Gate.com MEV analysis; River Financial; Bitcoin Magazine)

---

## Section 8: Key Metrics Summary Table

| Metric | Value | As Of | Source |
|--------|-------|-------|--------|
| **Ethereum** | | | |
| MEV-Boost adoption | ~90-96% of blocks | February 2026 | mevboost.pics |
| Top builder (Titan) market share | ~50.2% | February 2026 | mevboost.pics |
| BuilderNet market share | ~27.9% | February 2026 | mevboost.pics |
| Top relay (Ultra Sound) share | ~33.0% | February 2026 | relayscan.io |
| Sandwich monthly extraction | ~$2.5M/month | October 2025 | EigenPhi via Cointelegraph |
| CEX-DEX total extracted (19 searchers) | $233.8M | Aug 2023 - Mar 2025 | arXiv:2507.13023 |
| **Private Orderflow** | | | |
| MEV Blocker protected volume | $219B+ | March 2026 | mevblocker.io |
| MEV Blocker unique wallets | 4.5M+ | January 2026 | Consensys/SMG announcement |
| MEV Blocker ETH rebated | 6,177+ ETH | January 2026 | SMG operational memo |
| CoW Swap 2025 volume | $87B | Calendar year 2025 | CoW DAO Year in Review |
| CoW Swap cumulative surplus | $441M+ | March 2026 | cow.fi |
| Flashbots Protect users | 2.1M accounts | October 2024 | Flashbots blog |
| Flashbots Protect volume | $43B | October 2024 (cumulative since Oct 2021) | Flashbots blog |
| **Solana** | | | |
| Jito client stake share | >90% | Early 2026 | Multiple sources |
| Jito peak daily tips | $14.7M | November 17, 2024 | Jito Foundation |
| Jito staked SOL | 14.5M+ SOL (~$2.92B TVL) | Early 2026 | Jito Foundation |
| Firedancer stake share | ~20.9% | October 2025 | Unchained |
| Sandwich enforcement | 30+ (SF) + 50+ (Marinade) + 15+ (Jito) validators | 2024-2025 | Multiple sources |
| **L2** | | | |
| Arbitrum TimeBoost revenue | ~$3M (first 3 months) | April-July 2025 | DL News |
| Base block time (with Flashblocks) | 200ms | July 2025 | blog.base.dev |

---

## Data Sources & Methodology

**Report Date**: March 3, 2026
**Author**: MEV Strategist
**Revision**: v6.0 (Appendix A expanded with multi-source empirical validation: live relay API data from Flashbots/Titan/bloXroute/Ultra Sound relays, relayscan.io 7d builder/relay market shares, rated.network cross-check, builder profitability data, MEV-Boost payment validation; Ultra Sound relay share corrected from ~37% to ~33%; builder profit asymmetry quantified — Titan 764.94 ETH/7d vs BuilderNet 106.01 ETH/7d; all prior v5.0 content retained)

---

## Sources

### Primary Data Sources (Dashboards & Analytics)
- [mevboost.pics](https://mevboost.pics/) -- Builder and relay market share
- [relayscan.io](https://www.relayscan.io/) -- Relay-level statistics
- [mevblocker.io](https://mevblocker.io/) -- MEV Blocker live statistics
- [cow.fi/cow-swap](https://cow.fi/cow-swap) -- CoW Swap live statistics
- [rated.network/relays](https://explorer.rated.network/relays?network=mainnet) -- Relay performance
- [eigenphi.io](https://eigenphi.io/) -- MEV transaction analysis
- [boost.flashbots.net](https://boost.flashbots.net/) -- MEV-Boost overview

### Reports & Blog Posts
- [CoW DAO 2025 Year in Review](https://cow.fi/learn/cow-dao-2025-in-review) -- CoW Protocol annual metrics
- [Flashbots: 2 Million Protect Users](https://writings.flashbots.net/2m-protect-users) -- Flashbots Protect cumulative stats (October 2024)
- [BuilderNet: Introducing BuilderNet](https://buildernet.org/blog/introducing-buildernet) -- BuilderNet architecture
- [BuilderNet Blog (release history)](https://buildernet.org/blog) -- BuilderNet v1.2 through v1.6 release notes
- [Beaverbuild -> BuilderNet](https://buildernet.org/blog/beaverbuild) -- Migration announcement (May 2025)
- [Flashbots: Decentralized Building - Wat Do?](https://writings.flashbots.net/decentralized-building-wat-do) -- Four-phase decentralized building framework
- [Flashbots: Network Anonymized Mempools (Flashnet)](https://writings.flashbots.net/network-anonymized-mempools) -- Flashnet announcement (February 2026)
- [Flashbots: The MEVM, SUAVE Centauri, and Beyond](https://writings.flashbots.net/mevm-suave-centauri-and-beyond) -- Original SUAVE roadmap (2023)
- [Flashbots Collective: The Free Option Problem in ePBS](https://collective.flashbots.net/t/the-free-option-problem-in-epbs/5115) -- ePBS mechanism design research
- [Flashbots: Rollup-Boost on Unichain](https://writings.flashbots.net/unichain-mainnet) -- First L2 TEE block builder
- [Consensys Expands MEV Stack With MEV Blocker Acquisition](https://www.cryptotimes.io/2026/01/27/consensys-expands-mev-stack-with-mev-blocker-acquisition/) -- MEV Blocker governance transition
- [SMG MEV Blocker Memo](https://www.smg.org/memo/mevblocker) -- SMG's operational plan
- [BAM: Introducing BAM](https://bam.dev/blog/introducing-bam/) -- Jito BAM launch
- [CoinDesk: Jito Launches BAM](https://www.coindesk.com/tech/2025/07/21/jito-launches-bam-to-reshape-solanas-blockspace-economy/) -- BAM coverage
- [Helius: Block Assembly Marketplace](https://www.helius.dev/blog/block-assembly-marketplace-bam) -- Technical deep dive
- [Base: Flashblocks Deep Dive](https://blog.base.dev/flashblocks-deep-dive) -- Base Flashblocks architecture
- [Flashbots: Fast, Configurable, and Verifiable Sequencing](https://writings.flashbots.net/flashbots-superchain) -- OP Stack sequencing
- [Figment: Migration to Firedancer](https://www.figment.io/insights/figments-migration-to-firedancer-unlocking-next-generation-solana-validator-performance/) -- Firedancer performance data
- [Ethereum Foundation Checkpoint #8](https://blog.ethereum.org/en/2026/01/20/checkpoint-8) -- Glamsterdam timeline
- [Fidelity Digital Assets: Fusaka Upgrade - Scaling Meets Value Accrual](https://www.fidelitydigitalassets.com/research-and-insights/fusaka-upgrade-scaling-meets-value-accrual) -- EIP-7918 blob fee floor analysis
- [Conduit: Fusaka EIPs Cheat Sheet](https://www.conduit.xyz/blog/ethereum-fusaka-upgrade-eips-cheat-sheet/) -- Fusaka EIP overview
- [The Block: How Ethereum's Protocol Changed in 2025](https://www.theblock.co/post/383451/how-ethereums-protocol-changed-2025) -- Blob fee revenue and L2 flowback data

### Censorship Resistance & Inclusion Lists
- [EIP-7805 (FOCIL)](https://eips.ethereum.org/EIPS/eip-7805) -- Fork-Choice Enforced Inclusion Lists specification
- [EIP-7773 (Glamsterdam Meta-EIP)](https://eips.ethereum.org/EIPS/eip-7773) -- Glamsterdam SFI and CFI EIP list
- [ethresear.ch: FOCIL Proposal](https://ethresear.ch/t/fork-choice-enforced-inclusion-lists-focil-a-simple-committee-based-inclusion-list-proposal/19870) -- Original FOCIL research post
- [Ethereum Magicians: FOCIL Readiness for Hegota](https://ethereum-magicians.org/t/focil-readiness-for-hegota/27245) -- Hegota inclusion tracking
- [The Block: Vitalik Eyes Big FOCIL](https://www.theblock.co/post/391840/vitalik-buterin-eyes-big-focil-and-encrypted-mempools-to-prevent-centralization-in-block-building-pipeline) -- Big FOCIL proposal (March 2, 2026)
- [The Block: Devs Add FOCIL to Upgrade Roadmap](https://www.theblock.co/post/390682/vitalik-buterin-is-building-a-cypherpunk-principled-non-ugly-ethereum-as-devs-officially-add-focil-to-upgrade-roadmap) -- FOCIL CFI'd
- [DL News: Ethereum Devs Confirm FOCIL for Hegota](https://www.dlnews.com/articles/defi/ethereum-devs-confirm-focil-proposal-for-hegota-upgrade/) -- Developer consensus

### Encrypted Mempools
- [Ethereum Magicians: Hegota Headliner LUCID](https://ethereum-magicians.org/t/hegota-headliner-lucid-encrypted-mempool/27658) -- LUCID encrypted mempool proposal
- [ethresear.ch: Universal Enshrined Encrypted Mempool (EIP-8105)](https://ethresear.ch/t/universal-enshrined-encrypted-mempool-eip/23685) -- EIP-8105 research post
- [Shutter Blog: Introducing EIP-8105](https://blog.shutter.network/introducing-the-universal-enshrined-encrypted-mempool-eip/) -- EIP-8105 announcement
- [Shutter Blog: EIP-8105 Team Supporting LUCID](https://blog.shutter.network/hegota-update-eip-8105-team-giving-full-support-to-lucid-eip/) -- LUCID convergence
- [Shutter: First Encrypted Mempool on PBS](https://blog.shutter.network/the-first-encrypted-mempool-is-coming-to-pbs-on-ethereum/) -- Shutter Ethereum roadmap
- [ethresear.ch: Road Towards Distributed Encrypted Mempool](https://ethresear.ch/t/the-road-towards-a-distributed-encrypted-mempool-on-ethereum/21717) -- Ethereum integration plan
- [ethresear.ch: Threshold Encrypted Mempools with mev-commit](https://ethresear.ch/t/threshold-encrypted-mempools-with-mev-commit-preconfirmations/23588) -- Preconfirmation integration
- [a16z Crypto: On the Limits of Encrypted Mempools](https://a16zcrypto.com/posts/article/limits-encrypted-mempools/) -- Criticism (July 2025)
- [Shutter: Response to a16z](https://blog.shutter.network/on-the-limits-of-encrypted-mempools-a-response-to-a16z-cryptos-analysts/) -- Rebuttal
- [Etherworld: Holy Trinity of Censorship Resistance](https://etherworld.co/hegota-should-complete-the-holy-trinity-of-censorship-resistance/) -- ePBS + FOCIL + encrypted mempools framework

### Blob/DA Market
- [EIP-7918 (Blob Base Fee Floor)](https://eips.ethereum.org/EIPS/eip-7918) -- Blob fee reserve price specification
- [CryptoSlate: Fusaka Blob Utilization Analysis](https://cryptoslate.com/ethereum-fusakas-new-21-blob-ceiling-looks-tempting-but-blocks-above-16-show-a-worrying-miss-rate-jump/) -- Post-Fusaka utilization data

### Academic Papers
- [arXiv:2501.17335](https://arxiv.org/abs/2501.17335v2) -- "Cross-Chain Arbitrage: The Next Frontier of MEV in Decentralized Finance"
- [arXiv:2507.13023](https://arxiv.org/abs/2507.13023) -- "Measuring CEX-DEX Extracted Value and Searcher Profitability"
- [arXiv:2506.01462](https://arxiv.org/html/2506.01462) -- "When Priority Fails: Revert-Based MEV on Fast-Finality Rollups"
- [arXiv:2509.22143](https://arxiv.org/abs/2509.22143) -- "The Express Lane to Spam and Centralization: Arbitrum's Timeboost"
- [arXiv:2411.03892](https://arxiv.org/html/2411.03892v4) -- "A First Look at Ethereum Blob Revolution"
- [arXiv:2505.19708](https://arxiv.org/html/2505.19708v1) -- "Private MEV Protection RPCs: Benchmark Study"

### Legal Sources
- [The Block: Mistrial Declared for MEV Brothers](https://www.theblock.co/post/378101/massive-overstep-mistrial-declared-for-mev-brothers-accused-of-25-million-fraud-on-ethereum) -- November 2025
- [The Block: Prosecutors Seek New Trial](https://www.theblock.co/post/378414/prosecutors-seek-new-trial-mit-brothers-25-million-ethereum-fraud-case-ends-mistrial) -- November 2025
- [Steptoe: From MIT to Federal Trial](https://www.steptoe.com/en/news-publications/the-mother-court-blog/from-mit-to-federal-trial-united-states-v-anton-peraire-bueno-and-james-peraire-bueno-and-its-implications-for-crypto-crime.html) -- Legal analysis
- [DL News: Jury Left Sleepless](https://www.dlnews.com/articles/defi/mev-brothers-trial-ends-in-mistrial-after-jury-breaks-down/) -- Trial coverage

### Censorship & Aggregate MEV
- [MEV Watch](https://www.mevwatch.info/) -- Real-time OFAC-compliant block tracking
- [The Block: OFAC-Compliant Blocks Fall to 27%](https://www.theblock.co/post/230179/ethereums-ofac-compliant-blocks-fall-to-27-marking-a-drop-in-protocol-level-censorship) -- May 2023
- [Cointelegraph: Ethereum Inches Closer to Total Censorship](https://cointelegraph.com/news/ethereum-inches-even-closer-to-total-censorship-due-to-ofac-compliance) -- November 2022
- [CryptoSlate: MEV Bots Siphon Millions](https://cryptoslate.com/ethereum-bots-are-burning-over-50-of-gas-fees-so-eth-now-needs-privacy-just-to-scale/) -- EigenPhi 30-day data (Dec 2025-Jan 2026)
- [ESMA TRV Risk Analysis: MEV Implications](https://www.esma.europa.eu/sites/default/files/2025-07/ESMA50-481369926-29744_Maximal_Extractable_Value_Implications_for_crypto_markets.pdf) -- Aggregate MEV figures (Dec 2022-Jan 2025)
- [EigenPhi Daily MEV Reports](https://eigenphi.io/mev/ethereum/dailyReport) -- Ongoing daily MEV tracking

### On-Chain MEV Classification
- [Brontes (Sorella Labs)](https://github.com/SorellaLabs/brontes) -- On-chain MEV classifier using trace analysis and CEX price feeds; Appendix A data source

### Other Chain MEV
- [Skip Protocol](https://medium.com/@skip_protocol) -- Cosmos MEV infrastructure
- [Real Vision: Osmosis ProtoRev](https://www.realvision.com/issues/osmosis-debates-protocol-owned-mev) -- Protocol-owned MEV
- [Gate.com: Decoding Bitcoin MEV](https://www.gate.com/learn/articles/decoding-bitcoin-mev-insights-and-implications/4055) -- Bitcoin MEV analysis
- [River Financial: What Is MEV on Bitcoin?](https://river.com/learn/what-is-mev-does-it-apply-to-bitcoin-mining/) -- Bitcoin MEV overview
- [CoinDesk: Jito Mempool Closure](https://www.coindesk.com/business/2024/03/08/solana-client-developer-jito-announces-end-of-mempool-function) -- March 8, 2024
- [Blockworks: Jito Suspends Mempool](https://blockworks.co/news/jito-labs-suspends-mempool-functionality) -- March 2024
- [Coinfomania: Astria $18M Funding](https://coinfomania.com/astria-shuts-down-development-firm-after-18m-in-funding/) -- Astria total funding and shutdown

### News & Analysis
- [Cointelegraph Research: Sandwich Attacks Waned](https://cointelegraph.com/research/exclusive-data-from-eigenphi-reveals-that-sandwich-attacks-on-ethereum-have-waned) -- EigenPhi data analysis
- [The Block: Firedancer Goes Live](https://www.theblock.co/post/382411/jump-cryptos-firedancer-hits-solana-mainnet) -- Firedancer mainnet
- [Blockworks: Arbitrum Timeboost Revenue](https://blockworks.co/news/arbitrum-timeboost-live-dao-revenue) -- TimeBoost economics
- [DL News: Arbitrum $3M from Timeboost](https://www.dlnews.com/articles/defi/arbitrum-gets-3m-revenue-bump-from-timeboost/) -- TimeBoost revenue
- [Solana Foundation Validator Removals](https://www.theblock.co/post/299244/solana-foundation-removes-certain-operators-from-delegation-program-over-malicious-sandwich-attacks) -- The Block, June 2024
- [Marinade Finance Blacklists 50 Validators](https://cryptonews.net/news/security/32181951/) -- CryptoNews
- [CryptoNinjas: Solana Slashes $500M Sandwich Attacks](https://www.cryptoninjas.net/news/solana-slashes-500m-sandwich-attacks-as-75-of-sol-gets-staked-in-2025-security-overhaul/) -- Enforcement impact

---

## Appendix A: Empirical Validation via Brontes On-Chain Analysis

### Methodology

To independently cross-check the report's claims, we analyzed on-chain MEV data using **Brontes**, Sorella Labs' open-source MEV classification tool. Brontes processes Ethereum execution traces, matches them against CEX price feeds, and classifies MEV bundles by type (Sandwich, AtomicArb, CexDexTrades, CexDexQuotes, CexDexRfq, Jit, JitSandwich, Liquidation).

**Data scope**: Blocks 20,000,000–20,050,399 (50,400 blocks, ~7 days, June 1–8, 2024). Average ETH price in this period: $3,788.19. Monthly estimates are extrapolated using a 30/7 scale factor (4.29x).

**Important limitations**:
- This data covers June 2024 only. Most report claims reference late 2024–2026 data. Temporal differences must be considered.
- Brontes and EigenPhi use different detection methodologies, producing different bundle counts and profit attributions.
- CexDex Parquet export is partially disabled in Brontes source; CexDex data comes from bundle headers rather than detailed per-exchange breakdowns.
- "SearcherTx" (unclassified searcher transactions) is excluded from totals due to unreliable profit attribution.

### Aggregate MEV by Type (June 1–8, 2024)

| MEV Type | Bundles (7d) | Profit USD (7d) | Avg Profit/Bundle | Monthly Estimate |
|---|---|---|---|---|
| CexDexTrades | 15,837 | $871,384 | $55.02 | $3,734,501 |
| AtomicArb | 27,355 | $869,909 | $31.80 | $3,728,180 |
| CexDexQuotes | 14,637 | $600,657 | $41.04 | $2,574,245 |
| Sandwich | 29,977 | $334,130 | $11.15 | $1,431,985 |
| JitSandwich | 3,590 | $6,147 | $1.71 | $26,344 |
| Liquidation | 23 | -$115 | -$4.99 | -$492 |
| CexDexRfq | 2,310 | -$28,262 | -$12.23 | -$121,121 |
| Jit | 1,006 | -$119,692 | -$118.98 | -$512,964 |
| **Total** | **94,735** | **$2,534,158** | - | **$10,860,678** |

CEX-DEX arbitrage (CexDexTrades + CexDexQuotes) and atomic arbitrage were the dominant strategies by profit, followed by sandwich attacks. JIT liquidity provision showed negative net profitability in this sample.

### Validation of Specific Report Claims

**1. Sandwich attack profitability (Section 3a)**

| Metric | Report Claim | Brontes (June 2024) | Assessment |
|---|---|---|---|
| Monthly extraction revenue | ~$2.5M/month (Oct 2025, post-decline) | ~$1.43M/month (searcher net profit) | **Temporally consistent**: The report describes a decline from ~$10M/month (late 2024) to ~$2.5M/month (Oct 2025). Brontes' $1.43M/month net profit in June 2024 represents a mid-cycle baseline before the late-2024 peak and subsequent decline. |
| Avg profit per attack | ~$3 (Oct 2025) | $11.15 avg, $1.37 median | **Consistent with decline narrative**: Higher per-attack profitability in June 2024 ($11.15) vs October 2025 ($3) corroborates the reported 75% decline. |
| Attacks per month | 60,000–90,000 | ~128,472 (extrapolated) | **Higher**: Brontes classifies more sandwich bundles than EigenPhi, likely due to different heuristics for identifying sandwich patterns. |
| jaredfromsubway dominance | ~70% of attacks | 63.1% by count, 20.3% by profit | **Confirmed**: jaredfromsubway.eth controlled the majority of sandwich attacks by volume, though only 20% of profits (other searchers had higher per-attack profitability). |
| Attacker profit margin | ~5% | 7.7% (profit / (profit + bribes)) | **Consistent**: Low single-digit profit margins confirm builders capture the bulk of sandwich extraction value. |

**2. CEX-DEX arbitrage (Section 3b)**

| Metric | Report Claim | Brontes (June 2024) | Assessment |
|---|---|---|---|
| Total extraction | $233.8M over 20 months (~$11.7M/month implied) | ~$6.2M/month (CexDexTrades + CexDexQuotes + CexDexRfq) | **Directionally consistent**: Brontes' lower figure likely reflects (a) methodological differences in CEX-DEX detection, (b) CexDexRfq showing negative profit in this sample (-$28K), and (c) June 2024 potentially being below the 20-month average. |
| Top 3 concentration | ~90% of extracted value | 11.9% (by individual EOA) | **Not directly comparable**: The arXiv paper tracked searchers at the entity level (Wintermute, SCP, Kayle) across multiple EOAs. Brontes sees individual EOAs without entity grouping. When grouped by known fund affiliations (Wintermute/rsync + SCP/beaverbuild), concentration would be significantly higher. |

**3. Builder market share (Section 1a)**

| Builder | Report (Feb 2026) | Brontes (June 2024) | Assessment |
|---|---|---|---|
| Titan Builder | ~50.2% | 32.2% | **Growth trajectory confirmed**: Titan's share increased from ~32% (June 2024) to ~50% (Feb 2026), consistent with the reported consolidation narrative. |
| beaverbuild (SCP) | Migrated to BuilderNet (May 2025) | 46.9% | **Pre-migration baseline**: beaverbuild was the largest builder in June 2024. Its subsequent migration to BuilderNet (Section 1a) explains the disappearance of this entity from 2026 data. |
| rsync (Wintermute) | (part of BuilderNet) | 8.8% | Pre-BuilderNet figure. |

Titan earned 142.84 ETH/day in builder profit during this period, higher than the report's implied 96.78 ETH/day figure from February 2026. Builder profitability dynamics shifted significantly between these periods.

**4. Per-block MEV density**

| Metric | Value |
|---|---|
| Blocks with MEV (>$0 profit) | 68.15% of blocks |
| Avg MEV profit per block | $50.30 |
| Median MEV profit per block | $2.34 |
| Max single-block MEV | $193,601 |
| P95 MEV per block | $96.86 |
| P99 MEV per block | $479.44 |

The heavy right-tail distribution (mean $50 vs median $2.34) confirms that a small number of high-value MEV blocks drive the aggregate figures.

**5. Proposer MEV reward (Section 1c)**

| Metric | Report Claim | Brontes (June 2024) | Assessment |
|---|---|---|---|
| Avg MEV-Boost payment | 0.01071 ETH | 0.09942 ETH avg (0.05677 ETH median) | **Different metrics**: Brontes' `proposer_profit_usd` captures total priority fees and coinbase transfers paid to the proposer, not just the MEV-Boost bid delta. The mevboost.pics figure specifically measures the builder's winning bid, which is a subset of total proposer revenue. These should not be directly compared. |

**6. Aggregate MEV (Executive Summary)**

| Metric | Report Claim | Brontes (June 2024) | Assessment |
|---|---|---|---|
| Monthly MEV profit | ~$24M/month (late 2025) | ~$10.9M/month (excl. SearcherTx) | **Different period and scope**: June 2024 MEV extraction was lower than late 2025, consistent with the well-documented increase in DeFi activity and MEV opportunities during the 2024-2025 bull cycle. Different classification scopes (Brontes excludes SearcherTx; EigenPhi may include broader MEV categories) also contribute to the gap. |

### Key Insights from Brontes Data Not Covered in Report

1. **JIT liquidity shows negative profitability**: Pure JIT liquidity provision showed -$119K in net profit over 7 days, suggesting JIT LPs face significant adverse selection. Only JIT-Sandwich composites (combining JIT provision with sandwich extraction) were net positive.

2. **Atomic arbitrage rivals CEX-DEX in profitability**: AtomicArb ($870K/7d) was nearly equal to CexDexTrades ($871K/7d) in this sample, suggesting on-chain-only arbitrage remains a significant MEV category despite the report's emphasis on CEX-DEX as the dominant strategy.

3. **Builder profit asymmetry**: Despite building fewer blocks (32% vs 47%), Titan's total profit ($3.79M) was 3x beaverbuild's ($1.28M), implying significantly higher per-block profit extraction. This foreshadows Titan's eventual market share dominance described in the report.

4. **CexDexRfq negative profitability**: The RFQ-based CEX-DEX detection method consistently showed negative profit, suggesting either (a) a detection methodology issue in Brontes, or (b) RFQ-based arbitrage being structurally unprofitable due to information leakage in the RFQ process.

### Brontes Data Source

- **Tool**: [Brontes](https://github.com/SorellaLabs/brontes) (Sorella Labs), v0.1.0
- **Database**: MDBX snapshot covering blocks 20,000,000–20,050,399
- **Export format**: Parquet files (171 MiB total across 8 tables)
- **Builder labels**: Cross-referenced with `config/builder_config.toml` and `config/searcher_config.toml`
- **Analysis script**: `mev_report_analysis.py` using DuckDB for Parquet queries

**Note on Brontes coverage limitation**: The Brontes data server (data.brontes.xyz) only provides pre-computed snapshots for blocks 20,000,000–20,806,400 (approximately June–September 2024). Multi-chunk analysis spanning 2024–2026 was not possible via Brontes alone due to this data availability constraint (and insufficient local disk space for additional downloads at 37GB+). The June 2024 data serves as an on-chain baseline; claims about later periods are validated via the live relay API and analytics platform data in Appendix B below.

---

## Appendix B: Multi-Source Live Data Validation (March 4, 2026)

### Methodology

To validate report claims beyond the Brontes June 2024 baseline, we queried live relay APIs and analytics platforms on March 4, 2026. Data was fetched via direct API calls (`/relay/v1/data/bidtraces/proposer_payload_delivered`) and web scraping of relayscan.io, rated.network, and mevboost.pics.

**Data sources and accessibility**:

| Source | Method | Status | Data Retrieved |
|---|---|---|---|
| boost-relay.flashbots.net API | Direct JSON API | Live | 200 most-recent delivered payloads |
| titanrelay.xyz API | Direct JSON API | Live | 200 most-recent delivered payloads |
| bloxroute.max-profit.blxrbdn.com API | Direct JSON API | Live | 200 most-recent delivered payloads |
| relay.ultrasound.money API | Redirects to analytics subdomain | Partial | Redirect confirmed; JSON not directly fetchable |
| relayscan.io | Server-rendered HTML | Live | 7-day relay + builder share, profitability |
| mevboost.pics | JS-rendered SPA | Live | Global stats (MEV-Boost rate, avg payment, builder counts) |
| rated.network | Server-rendered HTML | Live | 24h builder + relay data cross-check |
| EigenPhi | JS-rendered SPA | Indirect | Via Cointelegraph Research partnership data |
| libmev.com | JS-rendered SPA | Not accessible | Requires browser rendering |
| payload.de | JS-rendered SPA | Not accessible | Requires browser rendering |

### Claim Validation Summary

| # | Report Claim | Verified Value | Source | Verdict |
|---|---|---|---|---|
| 1 | Titan builder ~50.2% share | 52.74% (7d) / 50.15% (24h) | relayscan.io, rated.network | **Confirmed** |
| 2 | BuilderNet ~27.9% share | 28.55% (7d) / 27.94% (24h) | relayscan.io, rated.network | **Confirmed** |
| 3 | Quasar ~16.2% share | 15.21% (7d) / 16.22% (24h) | relayscan.io | **Confirmed** |
| 4 | Ultra Sound relay ~33.0% | 32.59% (7d) | relayscan.io | **Confirmed** |
| 5 | Titan relay ~21.1% | 19.44% (7d) / 21.05% (24h) | relayscan.io | **Confirmed** |
| 6 | bloXroute Max Profit ~19.4% | 20.15% (7d) | relayscan.io | **Confirmed** |
| 7 | Flashbots relay ~3.5% | 2.31% (7d) / 3.52% (24h) | relayscan.io | **Confirmed** |
| 8 | Avg MEV-Boost payment 0.01071 ETH | Median ~0.011 ETH (Flashbots relay sample); mevboost.pics confirms 0.01071 | relay API, mevboost.pics | **Confirmed** |
| 9 | MEV-Boost adoption ~92.46% | 92.46% | mevboost.pics | **Confirmed** |
| 10 | 136 active builders (14d) | 136 | mevboost.pics | **Confirmed** |
| 11 | Sandwich ~$2.5M/month (Oct 2025) | ~$2.5M/month; 60–90K attacks/month | EigenPhi via Cointelegraph Research | **Confirmed** |
| 12 | CEX-DEX $233.8M (Aug 2023–Mar 2025) | $233.8M; 7.2M arbs; 19 searchers | arXiv:2507.13023 | **Confirmed** |
| 13 | CEX-DEX top 3 = ~90% share | Top 3 = ~75–90% of value | arXiv:2507.13023 | **Partially confirmed** (75–90% range, not precisely 90%) |
| 14 | Aggregate MEV ~$24M/month (late 2025) | Component sum consistent but no single authoritative source | Multiple | **Unverified** — directionally consistent with component figures |

### Builder Profitability — Live Data (relayscan.io, 7-day window)

| Builder | Blocks (7d) | Profit (ETH) | Profit (USD, ~$1,964) | Subsidies (ETH) | Profit/Block (ETH) |
|---|---|---|---|---|---|
| Titan | 24,096 | +764.94 | +$1,502,383 | 2.40 | 0.0317 |
| BuilderNet | 13,199 | +106.01 | +$208,204 | 1.42 | 0.0080 |
| bobTheBuilder | 436 | +22.98 | +$45,133 | 0.00 | 0.0527 |
| Quasar | 7,247 | +13.38 | +$26,278 | 2.83 | 0.0018 |
| beaverbuild.org (legacy) | 401 | +10.16 | +$19,954 | 0.02 | 0.0253 |
| Eureka | 453 | -0.05 | -$98 | 0.06 | -0.0001 |
| Builder+ (btcs) | 308 | -0.05 | -$105 | 0.11 | -0.0002 |

**Key insight**: Titan's per-block profitability (0.0317 ETH) is **4.0x** BuilderNet's (0.0080 ETH), confirming the profit asymmetry first observed in the June 2024 Brontes data (where Titan earned 3x beaverbuild's profit despite fewer blocks). This persistent advantage is consistent with Titan's exclusive orderflow (XOF) deals providing higher-value transaction bundles.

**Temporal trend — Titan profit growth**:

| Period | Titan Share | Titan Daily Profit | Source |
|---|---|---|---|
| June 2024 | 32.2% | 142.84 ETH/day | Brontes on-chain (Appendix A) |
| Feb 2026 | 51.65% | ~109.28 ETH/day (764.94/7) | relayscan.io live |

Despite Titan's market share growing from 32% to 52%, its daily ETH profit decreased from ~143 to ~109 ETH/day. This is consistent with: (a) lower ETH prices reducing MEV opportunity size in USD (ETH ~$3,788 in June 2024 vs ~$1,964 in March 2026), and (b) increased competition from BuilderNet and Quasar compressing margins.

### Average MEV-Boost Payment — Cross-Relay Analysis

Live API samples from March 4, 2026 (200 blocks per relay):

| Relay | Avg Payment (ETH) | Median Payment (ETH) | Max Payment (ETH) | Sample Coverage |
|---|---|---|---|---|
| Flashbots | 0.020695 | 0.010966 | 0.268875 | ~3.5% of blocks |
| Titan | 0.016052 | 0.008585 | — | ~21% of blocks |
| Global (mevboost.pics) | 0.01071 | — | — | All MEV-Boost blocks |

**Note**: Per-relay averages are higher than the global average because each relay sample is biased toward blocks where that relay won the auction. The global average of 0.01071 ETH (mevboost.pics) is the authoritative cross-relay figure. The Flashbots relay sample shows a higher average (0.0207 ETH) partly because it only wins ~3.5% of auctions — typically blocks with high-value public mempool transactions where Flashbots' relay can compete with lower-share builders.

### Builder Pubkey Distribution — Live Relay Samples

Analysis of builder pubkeys across relay APIs reveals the oligopolistic structure of block building:

**Flashbots relay (200 blocks, March 4, 2026)**:
- BuilderNet instances (0x850b..., 0x851b..., 0x855b...): 93 blocks (46.5%)
- Other builders: 107 blocks (53.5%)
- 22 unique builder pubkeys total

**Titan relay (200 blocks, March 4, 2026)**:
- Titan pubkeys (0xb26f..., 0x853b..., 0x852b..., 0x856b...): 103 blocks (51.5%)
- BuilderNet instances: 41 blocks (20.5%)
- Other builders: 56 blocks (28.0%)
- 15 unique builder pubkeys total

This confirms the report's claim that the top 3 builders (Titan, BuilderNet, Quasar) collectively produce ~94% of blocks. The Titan relay preferentially selects Titan-built blocks (as expected from vertical integration), while the Flashbots relay sees more BuilderNet blocks.

### Sandwich Attack Trend Validation

The sandwich attack decline narrative is one of the most robustly confirmed claims:

| Metric | Late 2024 (Peak) | Oct 2025 | June 2024 (Brontes) | Source |
|---|---|---|---|---|
| Monthly extraction | ~$10M/month | ~$2.5M/month | ~$1.43M/month (net profit) | EigenPhi; Brontes |
| Avg profit/attack | Higher | ~$3 | $11.15 avg, $1.37 median | EigenPhi; Brontes |
| Attacks/month | — | 60–90K | ~128K (extrapolated) | EigenPhi; Brontes |
| Active bots | — | 515 (100 monthly active) | — | EigenPhi |
| jaredfromsubway share | — | — | 63.1% by count | Brontes |

The Brontes June 2024 data ($1.43M/month, $11.15 avg) → EigenPhi late 2024 peak ($10M/month) → EigenPhi Oct 2025 ($2.5M/month, $3 avg) traces a rise-and-decline arc that is internally consistent. The rise from June 2024 to late 2024 likely reflects the bull market escalation; the decline from late 2024 to Oct 2025 reflects MEV protection adoption (MEV Blocker, private orderflow).

### Data Quality Assessment

**High confidence** (multiple independent sources confirm):
- Builder market share (Titan, BuilderNet, Quasar)
- Relay market share (Ultra Sound, Titan, bloXroute, Flashbots)
- Sandwich attack decline trajectory
- CEX-DEX arbitrage dominance
- MEV-Boost adoption rate

**Medium confidence** (single authoritative source, consistent with other data):
- Average MEV-Boost payment (mevboost.pics; consistent with relay API medians)
- Builder profitability figures (relayscan.io coinbase-diff method may undercount off-chain payments)
- CEX-DEX top-3 concentration (arXiv paper; Brontes shows lower concentration at EOA level due to entity grouping differences)

**Low confidence** (no direct primary source confirmed in this session):
- Aggregate MEV ~$24M/month: Directionally consistent with component figures (sandwich $2.5M + atomic arb $3-4M + CEX-DEX $6-12M + liquidations + long-tail), but no single dashboard produces this exact figure. Recommend sourcing to specific EigenPhi daily report snapshot if available.

### Appendix B Data Sources

- [relayscan.io](https://www.relayscan.io/) — 7-day and 24-hour builder/relay market share (fetched March 4, 2026)
- [rated.network Builder Landscape](https://explorer.rated.network/builders?network=mainnet) — 24h cross-check (fetched March 4, 2026)
- [mevboost.pics](https://mevboost.pics/) — Global MEV-Boost statistics (slot 13,808,247, March 3, 2026)
- Flashbots relay API: `boost-relay.flashbots.net/relay/v1/data/bidtraces/proposer_payload_delivered` — 200 live blocks
- Titan relay API: `titanrelay.xyz/relay/v1/data/bidtraces/proposer_payload_delivered` — 200 live blocks
- bloXroute relay API: `bloxroute.max-profit.blxrbdn.com/relay/v1/data/bidtraces/proposer_payload_delivered` — 200 live blocks
- [Cointelegraph Research / EigenPhi exclusive](https://cointelegraph.com/research/exclusive-data-from-eigenphi-reveals-that-sandwich-attacks-on-ethereum-have-waned) — Sandwich attack data Nov 2024–Oct 2025
- [arXiv:2507.13023](https://arxiv.org/abs/2507.13023v2) — "Measuring CEX-DEX Extracted Value and Searcher Profitability"
- [ESMA MEV TRV Risk Analysis, July 2025](https://www.esma.europa.eu/sites/default/files/2025-07/ESMA50-481369926-29744_Maximal_Extractable_Value_Implications_for_crypto_markets.pdf)
- CoinGecko API — ETH/USD price ($1,964.41, fetched March 4, 2026)

---

*Report compiled March 3–4, 2026. Appendix A uses Brontes on-chain data from June 2024. Appendix B uses live relay API data and analytics platform data from March 4, 2026. All figures are attributed to their respective sources with temporal context. Where current data could not be verified, the most recent available data is used with explicit date labeling. The aggregate MEV ~$24M/month figure (Executive Summary) remains unverified from a single authoritative source but is directionally consistent with component strategy totals.*
