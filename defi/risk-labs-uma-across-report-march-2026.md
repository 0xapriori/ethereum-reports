---
title: "Risk Labs Comprehensive Report: UMA Protocol, Across Protocol & ACX-to-Equity Conversion"
date: 2026-03-16
---

# Risk Labs Comprehensive Report
### UMA Protocol | Across Protocol | Corporate Overview
**Published:** March 16, 2026 | **Research conducted by multi-agent DeFi analysis team with independent QA verification**

---

## Table of Contents
1. [Risk Labs: Company Overview](#1-risk-labs-company-overview)
2. [UMA Protocol & Polymarket](#2-uma-protocol--polymarket)
3. [Across Protocol: Data & Competitive Position](#3-across-protocol-data--competitive-position)
4. [The ACX-to-Equity Conversion Proposal](#4-the-acx-to-equity-conversion-proposal)
5. [Key Risks & Open Questions](#5-key-risks--open-questions)
6. [Timeline of Key Events](#6-timeline-of-key-events)
7. [Sources](#7-sources)

---

## 1. Risk Labs: Company Overview

### Founding & Leadership
**Risk Labs Foundation** is a Cayman Islands-registered nonprofit foundation that serves as the core development and operational entity behind two DeFi protocols: UMA and Across.

- **Co-Founders:** Hart Lambur (CEO) and Allison Lu
- **Hart Lambur:** Columbia CS (Magna Cum Laude, 2005). Interest rate trader at Goldman Sachs (2005–2013), lived through the 2008 crisis on the trading desk. Co-founded Openfolio (personal finance platform, acquired 2017). Founded UMA in 2018.
- **Allison Lu:** Economics & Management, MIT. VP at Goldman Sachs for six years, managed a $30B balance sheet and $100M+ in annual revenue. Elevated desk to #1 in U.S. Treasuries (primary and secondary). Has since stepped back from day-to-day operations; involved with Founders Pledge.
- **Nick Pai:** Tech Lead, Across Protocol. Joined Risk Labs in 2020.
- **Kevin Chan:** Project Lead, Across Protocol. Central figure in the DAO treasury controversy (see Section 5).

**Structure:** Remote-first organization. Cayman Islands nonprofit. Lean leadership team with a "Chief of Staff to Founder" role in job postings. Exact headcount not publicly disclosed.

### Funding History

| Round | Date | Amount | Lead | Valuation | Notable Participants |
|-------|------|--------|------|-----------|---------------------|
| Seed (UMA) | ~2018-2019 | $4M | Placeholder | — | Bain Capital Ventures, Blockchain Capital, Box Group, Coinbase Ventures, Dragonfly Capital, FinTech Collective, Two Sigma Ventures |
| IDO (UMA) | April 2021 | Undisclosed | Public (Uniswap) | — | First-ever IDO on Uniswap |
| Token Round (Across) | Nov 2022 | $10M | — | $200M | Multiple participants |
| Token Sale (Across) | Q2+Q4 2024 | $41M | Paradigm | Not disclosed | Bain Capital Crypto, Coinbase Ventures, Multicoin Capital, Sina Habinian (angel) |
| **Total Raised** | | **$51M** | | | |

### Strategic Architecture: The Two-Protocol Flywheel
Risk Labs operates a vertically integrated infrastructure stack:

1. **UMA (oracle layer):** Provides trust and verification infrastructure. The optimistic oracle is a general-purpose system for asserting and verifying data on-chain — the "source of truth" layer.
2. **Across (application layer):** Uses UMA's oracle as its security backbone for cross-chain bridging. Across is the highest-profile commercial application built on UMA's infrastructure.

**The flywheel:** Across drives demand for UMA's oracle services, and UMA's security model gives Across a differentiated trust architecture vs. competing bridges.

### Strategic Evolution

| Phase | Era | Focus |
|-------|-----|-------|
| UMA Phase | 2018–2020 | "Universal Market Access" — synthetic assets, financial contracts |
| Pivot | 2021–2022 | Repositioned UMA as general-purpose oracle; launched Across bridge |
| Intents | 2023–2024 | Across embraced intents-based architecture; became key differentiator |
| Institutional Pivot | 2025–2026 | Proposing DAO → C-corp conversion; pursuing enterprise partnerships |

---

## 2. UMA Protocol & Polymarket

### How UMA's Optimistic Oracle Works

UMA's oracle operates on two layers:

**Layer 1 — The Optimistic Oracle (OO):**
1. **Request:** A data consumer (e.g., Polymarket) requests information
2. **Propose:** A proposer submits an answer + posts a bond as collateral
3. **Challenge Period:** Opens (typically 2 hours for Polymarket). If unchallenged, the answer is accepted as truth.
4. **Dispute:** If challenged, the disputer posts their own bond and it escalates to Layer 2.

**Layer 2 — The Data Verification Mechanism (DVM):**
- UMA token holders vote on the correct answer over 48–96 hours
- Token-weighted voting: voters who vote with the majority are rewarded; incorrect/absent voters are slashed
- This is the ultimate backstop for all UMA-secured protocols

**DVM 2.0 Staking Economics:**
- Staking required to vote (minimum raised from 500 to 1,000 UMA in Nov 2025)
- Emissions: 0.05% of UMA supply per voting round to active voters
- Estimated staking APY: ~30% for consistent voters
- 7-day unstake cooldown
- Slashing redistributes tokens from inactive/incorrect voters to correct voters

### Polymarket Integration: The Exact Mechanism

Polymarket uses a **UMA CTF (Conditional Token Framework) Adapter** — a smart contract sitting between Polymarket's markets and UMA's oracle:

1. **Market hits resolution conditions** → Adapter sends price request to UMA's OO
2. **Proposer submits answer** (Yes/No) with bond
3. **2-hour liveness window** opens for disputes
4. **Critical design feature — Two-round dispute mechanism:** The *first* dispute is ignored and a new identical request is created (prevents malicious single disputes from slowing resolution). Only if the *second* request is also disputed does it escalate to full DVM vote.
5. **DVM token-holder vote** (if escalated) → 48–96 hours → majority determines outcome
6. **Finalization** → Polymarket conditional tokens settle accordingly

**Polymarket actually uses three oracle systems:**
- **UMA Protocol** — most markets, especially subjective judgment calls
- **Chainlink** — markets with clearly defined, objective data feeds
- **Markets Team** — Polymarket's internal team for certain market types

### Major Controversies & Oracle Failures

#### 1. The Barron Trump DJT Meme Coin Override (June 2024)
- **Market:** "Was Barron Trump involved in creating the $DJT token?"
- **UMA's DVM repeatedly resolved:** "No"
- **Polymarket's response:** Overrode the oracle, stating it was "conclusive" that Barron Trump was "in fact, involved in some way"
- **Significance:** First public demonstration that Polymarket retains centralized override power over its "decentralized" oracle. Raises fundamental questions about actual decentralization.

#### 2. The $7M Ukraine Mineral Deal Governance Attack (March 2025)
The single most significant oracle failure in UMA/Polymarket history:

- **Market:** "Will Ukraine agree to Trump's mineral deal before April?"
- **The attacker:** An Ethereum wallet identified as **BornTooLate.eth** accumulated ~1.3 million UMA tokens over the preceding year (~$2M+ cost), becoming a **top-5 governance staker**
- **The attack:** Cast ~5 million UMA tokens across three accounts (25% of total votes) to force a "Yes" resolution
- **The result:** Market moved from 9% → 100% despite no official agreement existing
- **Polymarket's response:** Confirmed the oracle reached the "incorrect outcome" but called it "unprecedented" and issued no refunds ("this wasn't a market failure")
- **Financial impact:** Largest winner gained ~$55,000; largest loser lost ~$73,000
- **Core issue exposed:** 95% of UMA tokens held by large holders (per IntoTheBlock). Token-weighted voting fundamentally favors capital over truth.

**Verified across:** CoinDesk, The Defiant, CoinTelegraph, Yahoo Finance, The Block, CoinMarketCap

#### 3. The Zelenskyy Suit Controversy (July 2025)
- **Market:** "Will Zelenskyy wear a suit before July?" — **$237M in volume**
- **What happened:** On June 24, Zelenskyy appeared at a NATO event wearing a black jacket, matching trousers, and a collared shirt. BBC and New York Post described it as a suit.
- **Resolution:** Initially proposed as "Yes," then over nine days of disputes and re-proposals, **flipped to "No"** by UMA whale voters
- **The whale problem (quantified):** Top 10 UMA voters held ~6.5M UMA tokens (~30% of average vote participation). Allegations emerged that large UMA holders also held positions on the Polymarket market itself — a **direct conflict of interest**.
- **THE KEY INSIGHT — Scale Mismatch:** UMA's total market cap was ~$95M. This single Polymarket market had $237M in volume. **The economic value at stake exceeded the entire market cap of the oracle securing it** — a fundamental security model failure.

#### 4. The UFO Declassification Market
- Polymarket resolved "YES" on a $16M market asking whether Trump would declassify UFO files in 2025, despite no documents being released
- Users labeled the outcome a "scam" and criticized the "proof-of-whales" model

#### 5. Other Notable Disputes
- **Venezuela's contested election** — Criticized for subjective decision-making
- **OceanGate submarine** — Disputed over interpretation of "finding" wreckage vs. the vessel

### Systemic Response: UMIP-189 and the MOOV2 Upgrade (August 2025)

In response to the governance attacks, UMA passed **UMIP-189** on August 6, 2025:

| Change | Detail |
|--------|--------|
| New system | Managed Optimistic Oracle V2 (MOOV2) |
| Proposer restriction | Whitelist of **37 addresses** only |
| Who's on the whitelist | Risk Labs employees, Polymarket employees, and high-accuracy proposers |
| Qualification criteria | 20+ proposals in past 3 months with >95% accuracy |
| Whitelist stats | These 37 addresses account for 96% of all historical proposals with 99.7% cumulative accuracy |
| Dispute rights | **Remain open to anyone** |
| AI integration | AI bots now propose and dispute data for speed/cost (July 2025) |

**The tradeoff:** MOOV2 trades decentralization for reliability. Critics call it centralization of the oracle around a small pre-approved group. Supporters argue it's pragmatic given demonstrated vulnerability.

### The Fundamental Security Model Problem

UMA's security relies on a game-theoretic assumption: **the cost of corrupting the oracle should exceed the profit from corruption**. This is the "cost-of-corruption" model.

**Why it's broken at scale:**
- When Polymarket markets grow large enough, the profit from manipulating a resolution can exceed the cost of acquiring enough UMA tokens to control the vote
- The Zelenskyy market ($237M volume) vs. UMA market cap (~$95M) is the clearest demonstration
- MOOV2 fixes the proposal side but **does not fix the dispute-resolution voting problem**
- Just four whales reportedly control over 40% of UMA supply; the largest single whale holds ~25%

### UMA's AI Oracle Initiative: The Optimistic Truth Bot

UMA launched the **Optimistic Truth Bot (OTB)** in **March 2025** as an experimental AI-powered proposer for the Optimistic Oracle. As of March 2026, the bot runs in **shadow mode** — processing real Polymarket markets in parallel with human proposers but **not** submitting proposals on-chain. It publishes recommendations publicly on X at **@OOTruthBot**.

#### Architecture: Mixture of Experts

The OTB uses a three-layer design:

| Layer | Function |
|-------|----------|
| **Router** | Lightweight classifier (regex + embeddings) that categorizes queries (price, sports, general) and routes to appropriate solver |
| **Solvers** | Two types: (1) **Perplexity Solver** — uses Perplexity's research API for open-ended fact-based questions; (2) **Code Runner Solver** — writes/executes sandboxed Python scripts to query deterministic APIs (Binance, SportsData IO) with 60-second timeouts |
| **Overseer** | Quality control — validates JSON/date syntax, checks semantic coherence, compares answers against live Polymarket pricing (flags if confidence mismatch ≥85%), can restart entire workflow |

UMA's blog references GPT-4, Claude 3, and Llama 4 as enabling models, though the specific model(s) powering the current deployment are not confirmed. Perplexity's API is explicitly used for the web-search solver.

#### Accuracy Metrics

| Market Type | Accuracy |
|-------------|----------|
| Overall (all types) | **78%** |
| Clean binary Yes/No markets | **95%** |
| Sports and asset pricing | **99.3%** |
| "Mention markets" (word count, social media) | **72%** |

For comparison, human proposers achieve a 98.2% assertion acceptance rate with only 0.4% facing genuine accuracy disputes.

#### Current Status
The OTB is **not yet making on-chain proposals**. UMA's plan is to integrate it once accuracy is sufficiently high, subject to the same dispute process as human proposers. The vision: **"AI proposes, humans decide"** — AI handles routine cases, human oversight remains the backstop.

#### Critical Limitation: AI Does NOT Solve the Whale Problem

**The OTB addresses the proposal layer, NOT the voting/dispute layer.** If a proposal is disputed, it still goes to UMA token-weighted voting — where the whale concentration problem exists. AI-based proposals make the system faster and cheaper, but do not reform the dispute resolution mechanism that has been exploited.

#### EigenLayer Next-Gen Oracle (Research Phase)

The more promising initiative for the whale problem is UMA's collaboration with **Polymarket and EigenLayer** on a next-generation oracle:
- **Multi-token dispute resolution** (not just UMA tokens)
- EigenLayer's **intersubjective security model** to mitigate economic bribery attacks
- Prediction markets could stake **their own native assets** instead of relying solely on UMA
- **Dynamic bonding system** conducive to AI agent participation
- Could dilute whale influence by diversifying the voting base beyond UMA holders

This is still in the **research phase** — no deployed system yet. But it represents the most credible path to solving the structural security model failure.

#### Connection to UMIP-189 / Managed Proposers

The AI oracle and managed proposers (UMIP-189) are currently **parallel tracks**:
- Managed proposers: Human-curated whitelist addressing proposal quality (177 addresses as of Nov 2025, up from initial 37)
- OTB: AI-assisted proposals as separate R&D effort
- **Logical convergence:** If the OTB achieves sufficient accuracy, it could become a whitelisted managed proposer — but this hasn't happened yet

### Existential Threat: The POLY Token

Polymarket has teased a **POLY token** (reported October 2025) that could potentially replace UMA for dispute resolution. No tokenomics have been disclosed. If Polymarket internalizes its oracle function, UMA loses its most important use case.

### UMA Token Metrics (March 2026, approximate)

| Metric | Value | Notes |
|--------|-------|-------|
| Price | ~$0.42–$0.68 | Discrepancy between CoinGecko (~$0.68) and CoinMarketCap (~$0.42) |
| Market Cap | ~$38–39M | |
| CoinGecko Rank | #549 | |
| 24h Volume | ~$7M | |
| 7-Day Performance | -15.7% | Per CoinGecko |
| Circulating Supply | ~90–92M UMA | |
| Max Supply | ~114.6–130M UMA | Discrepancy across sources |

**Data caveat:** Price data shows significant discrepancy between aggregators. TVL data is also inconsistent ($1.5M vs. $18M across sources). These figures should be verified on live dashboards.

---

## 3. Across Protocol: Data & Competitive Position

### How Across Works (Technical Detail)

Across's intents-based architecture decouples user experience from backend settlement:

1. **User submits intent:** Calls `deposit` on the SpokePool contract on the origin chain, escrowing assets with metadata (recipient, destination chain, relayer fee)
2. **Relayer competition:** Bonded relayers compete to fill the intent on the destination chain using their own capital → user receives funds in seconds to <1 minute
3. **Bundling:** A "dataworker" aggregates fulfilled intents into repayment bundles
4. **Optimistic settlement:** Bundles submitted to UMA's oracle on Ethereum mainnet with a challenge period
5. **Dispute resolution:** If challenged, UMA token holders vote. Incorrect proposers are slashed.

**Key difference from competitors:** No multisigs, no centralized validators, no lock-and-mint. The optimistic oracle model is architecturally distinct from the validator/multisig approaches that have been exploited for billions (Ronin, Wormhole pre-upgrade, Nomad).

### Security Track Record
- **18 comprehensive audits** (primarily OpenZeppelin)
- **232 issues identified and addressed** across all audits
- **Zero security exploits in production** — ever
- No multisigs or centralized validators

### Supported Chains (15+)
Ethereum, Arbitrum, Optimism, Base, zkSync, Blast, Scroll, Zora, World Chain, Linea, Polygon, BNB Chain, Solana, Hyperliquid, and others being onboarded.

Smaller coverage than Stargate (~80 chains) or Wormhole (~30+), but focused on high-volume Ethereum + L2 corridors.

### Bridge Data & Metrics

| Metric | Value | Source/Notes |
|--------|-------|--------------|
| Lifetime Volume | **~$35B** | Across blog + OpenZeppelin case study (earlier reference: $19B+ at Mar 2025 funding announcement; $20.7B at an earlier date) |
| Total Transfers | **14.4M+** | Protocol statistics |
| TVL | **~$52M** | DeFiLlama (Across agent found $51.89M; supervisor found ~$32M — discrepancy may reflect timing) |
| Protocol Revenue | **$0** | Fees go to LPs/relayers, not the protocol |
| Daily Fees (user-paid) | **~$1,778** | Recent 24h snapshot from DeFiLlama |

**Critical distinction:** Across generates fees from bridge transactions, but these fees flow to relayers and liquidity providers. The protocol itself captures **zero revenue**. Fee revenue ≠ protocol revenue. This is a key motivation behind the corporate restructuring.

**Data caveat:** The $35B lifetime volume figure comes from protocol and partner marketing materials. The most recent independently verifiable data point found was $20.7B at an earlier date. DeFiLlama (defillama.com/bridge/across) is the authoritative neutral source.

### Competitive Landscape (Detailed)

| Feature | Across | Stargate (LayerZero) | Wormhole/Portal | deBridge |
|---------|--------|---------------------|-----------------|----------|
| **Architecture** | Intents + Optimistic Oracle | Unified liquidity pools + LayerZero messaging | Guardian network (13/19 multisig) + lock-and-mint | Intent-based |
| **Speed** | <1 minute typical | Sub-2-second (V2 Bus mode) | Fast finality | ~2 seconds |
| **Chains** | 15+ | ~80+ | 30+ | Multiple EVM + Solana |
| **TVL** | ~$52M | ~$216M | Varies | N/A |
| **Lifetime Volume** | ~$35B | N/A | $60B+ | $9B+ |
| **Security Model** | UMA optimistic oracle, no multisig | LayerZero validator-oracle | 19-node Guardian network | DLN validation |
| **Exploit History** | **Zero exploits** | No major exploits | $320M exploit (Feb 2022, pre-upgrade) | Zero exploits |
| **Key Advantage** | Uniswap/ERC-7683 ecosystem, capital efficiency | Chain coverage, LayerZero integration | Chain coverage | Speed, institutional focus |

**Note:** Stargate/LayerZero merger in 2025 ($110–120M acquisition) creates a formidable integrated competitor.

### Where Across Wins
1. **Ethereum + L2 routes:** Consistently rated fastest and cheapest for transfers under $1,000 (often <$1 per ETH bridged)
2. **Institutional distribution:** Uniswap and MetaMask integrations give unmatched retail access — every Uniswap bridge transaction flows through Across
3. **Standards ownership:** Co-authored ERC-7683 with Uniswap (see below)
4. **Security architecture:** Zero exploits, no multisig attack surface

### Where Across Loses
1. **Chain coverage:** 15+ vs. 80+ (Stargate) — not competitive for non-EVM bridging beyond Solana
2. **Speed on some routes:** deBridge and Stargate V2 achieve sub-2-second on some routes
3. **Fee competitiveness:** Synapse reportedly offered lower fees on 45/60 tested routes in one benchmark
4. **Revenue capture:** $0 protocol revenue signals no fee switch activated

### Key Partnerships & Integrations

#### Uniswap (Flagship)
- Across powers **in-app bridging** directly within the Uniswap interface
- Supported chains: Ethereum, Base, Arbitrum, Polygon, OP Mainnet, Zora, Blast, World Chain, zkSync
- Co-authored **ERC-7683** (cross-chain intent standard) with Uniswap Labs

#### ERC-7683: The Strategic Moat
- Universal framework for intent-based cross-chain systems
- **35+ L2s and sidechains** have adopted/expressed support (including Arbitrum, Optimism, Base)
- As the standard gains adoption, Across's relayer network becomes the default fulfillment layer
- This positions Across not just as a bridge, but as **core infrastructure for cross-chain intent settlement**

#### MetaMask
- Integrated into MetaMask for cross-chain bridging (massive user base access)

#### PancakeSwap
- Cross-chain swaps powered by Across via ERC-7683 (June 2025)
- Supports BNB Chain ↔ Arbitrum ↔ Base in a single transaction
- Combines bridging + swapping into one action

#### Other: ZeroDev (account abstraction), LI.FI (aggregation), various bridge aggregators

### ACX Token Metrics (March 2026, approximate)

| Metric | Value | Notes |
|--------|-------|-------|
| Price | ~$0.042–$0.063 | Highly volatile post-proposal |
| Market Cap | ~$30–45M | Range reflects volatility |
| CoinMarketCap Rank | ~#486–543 | Fluctuating |
| Total Supply | 1,000,000,000 ACX | Fixed, no inflation |
| Circulating Supply | ~658–702M ACX | Discrepancy across sources; fully unlocked as of 2025 |
| Pre-announcement 30-day avg | ~$0.035 | Implied by buyout premium |
| Post-announcement surge | **+80–85%** | To ~$0.06 peak |

**Token Distribution:**

| Allocation | Amount |
|------------|--------|
| Risk Labs Treasury | 195M ACX (~150M team vesting, now complete) |
| Across Success Token investors | 110M ACX (expired June 30, 2025) |
| Other investors (lockup) | 110.6M ACX |
| Community/DAO/liquidity | ~584.4M ACX |

---

## 4. The ACX-to-Equity Conversion Proposal

### "The Bridge Across" — The Most Significant Development at Risk Labs

**Proposal Name:** "The Bridge Across"
**Posted:** March 11, 2026, on the Across governance forum (forum.across.to/t/the-bridge-across/2097)
**Proposed by:** Risk Labs

### What Is Being Proposed

Risk Labs proposes converting Across from a DAO + token governance structure to a **U.S. C-corporation** called **"AcrossCo."** The new entity would:
- Take over development, partnerships, and commercialization
- Hold the protocol's intellectual property
- Enter enforceable contracts and structured revenue agreements
- The underlying bridge infrastructure would remain open and permissionless

### Exact Terms (verified across 5+ independent sources)

#### Option A: Equity Exchange
- Convert ACX tokens to equity in AcrossCo at **1:1 token-to-share ratio**
- Holders with **>5 million ACX** can convert directly to equity
- Smaller holders (minimum 250,000 ACX, ~$10,000) participate through a **no-fee Special Purpose Vehicle (SPV)**

#### Option B: USDC Buyout
- Sell tokens for USDC at **$0.04375 per token**
- Represents a **25% premium** over the trailing 30-day average price
- Protocol's liquid assets (~equivalent to market cap) finance the buyout
- **Six-month redemption window** opens within three months of proposal passing

### Governance Timeline

| Date | Event | Status |
|------|-------|--------|
| March 11, 2026 | Proposal posted to forum | Complete |
| March 18, 2026 | Community call | Upcoming |
| March 25, 2026 | End of formal discussion period | Upcoming |
| **March 26, 2026** | **Snapshot vote** | **NOT YET OCCURRED** |
| Early April 2026 | Conversion begins (if approved) | Pending vote |

**CRITICAL: As of March 16, 2026, the binding Snapshot vote has NOT yet taken place. The proposal is in discussion/temperature-check phase.**

### Risk Labs' Stated Rationale (Direct Quotes)

> "As Across deepens our work with institutional and enterprise partners, the token and DAO structure has materially impacted our ability to close partnerships and integrations."

> "Enterprise partners need enforceable contracts. Revenue agreements need a legal counterparty. The kinds of deals that would drive the next phase of growth require a structure that a DAO, today, simply can't provide."

> "Transitioning to a traditional legal entity would meaningfully improve our ability to enter enforceable contracts, structure revenue agreements, and deliver more value to Across stakeholders."

### Market & Community Reaction
- **Price:** ACX surged 80–85% within 24 hours; trading volume hit ~3.5x market cap
- **Market cap:** Briefly reached ~$45M
- **Sentiment:** Market is pricing this as value-accretive
- **The 25% premium** buyout provides a price floor for dissenters
- **Community discussion ongoing** — formal vote hasn't happened yet

### Supportive Arguments
- Provides legal clarity for institutional partnerships
- Offers tangible equity stake with potential for dividends, M&A, or IPO
- Addresses real limitations of DAO structures for commercial operations
- Market price reaction strongly positive

### Critical Arguments
- Philosophical retreat from decentralization
- One of the first major protocols to argue DAOs are inferior to traditional corporate structures
- Equity in a private company is illiquid vs. freely tradeable token
- SPV structure introduces intermediation for smaller holders
- Regulatory implications of token-to-equity conversion are untested
- **Critics could argue Risk Labs is dissolving the DAO partly to escape governance accountability** (see $23M controversy below)

### Legal & Regulatory Implications
- Converting governance token to C-corp equity explicitly makes the asset a security under U.S. law
- Token-to-equity swap and USDC buyout both likely trigger taxable events
- Direct equity conversion for 5M+ ACX holders may require accredited investor verification
- Paradigm's support (as $41M lead investor) suggests comfort with the regulatory path
- **This is precedent-setting** — how regulators respond could create a template for or against similar future conversions

### Broader Industry Implications
1. **DAO disillusionment signal:** If successful, sets precedent that DAOs aren't the default organizational structure
2. **Token-to-equity template:** Creates a mechanism for how protocols might "re-incorporate"
3. **Regulatory pragmatism:** Acknowledges that operating in a regulatory gray zone as a DAO may be costlier than incorporating traditionally
4. **VC alignment:** Paradigm would gain traditional equity rights (board seats, liquidation preferences) instead of governance tokens with unclear legal standing

---

## 5. Key Risks & Open Questions

### The $23 Million DAO Treasury Scandal

This is a critical piece of context for evaluating Risk Labs' governance credibility:

**Allegations (surfaced 2025, raised by Ogle, founder of Glue):**
- Kevin Chan (Across project lead) submitted a governance proposal in October 2023 to transfer **100M ACX tokens (~$15M)** from the DAO treasury to Risk Labs
- On-chain analysis allegedly showed Kevin used a second wallet (**"maxodds.eth"**) to secretly cast "yes" votes on his own proposal
- Without the team's own votes, the proposal allegedly would not have passed
- A second "retrospective funding" proposal for **50M ACX (~$7.5M)** followed. "maxodds.eth" and a new wallet funded by it allegedly contributed **44% of "yes" votes**
- **Total allegedly directed to Risk Labs: ~$23M in ACX tokens**
- No public audit or transparent accounting of how funds were used

**Risk Labs' Response (Hart Lambur):**
- Called allegations "completely untrue"
- Stated Risk Labs is a nonprofit Cayman foundation with fiduciary responsibilities
- Claimed team members purchased tokens independently and voted openly
- Stated all wallet addresses are publicly disclosed
- Asserted funds used for protocol development and team expansion
- Questioned accuser's credibility (ties to competing projects)

**Status:** Unresolved. No legal proceedings filed. Absence of transparent accounting remains a legitimate concern.

**Relevance to equity conversion:** Critics argue Risk Labs may be dissolving the DAO partly to escape governance accountability. Supporters argue a C-corp with board oversight and auditing would actually improve accountability.

### For UMA Protocol

| Risk | Severity | Detail |
|------|----------|--------|
| Security model failure at scale | **Critical** | When market volume exceeds oracle market cap, cost-of-corruption assumption breaks. Demonstrated empirically. |
| Token concentration | **High** | 95% in large wallets; 4 whales control 40%+; largest holds ~25%. Governance capturable. |
| Polymarket dependency | **High** | If Polymarket launches POLY token and internalizes resolution, UMA loses primary use case. |
| MOOV2 centralization | **Medium** | Band-aid that trades decentralization for security. Doesn't fix DVM voting problem. |
| Reputational damage | **Medium** | Ukraine deal, Zelenskyy suit, UFO market controversies have damaged trust. |
| Conflict of interest | **Medium** | Whale voters accused of holding Polymarket positions on markets they vote to resolve. No enforcement mechanism. |

### For Across Protocol

| Risk | Severity | Detail |
|------|----------|--------|
| Zero protocol revenue | **High** | Sustainability depends on fee switch or equity conversion enabling fundraising |
| Equity conversion execution | **High** | Legally novel, untested regulatory territory, vote hasn't happened yet |
| Bridge competition | **High** | Intensely competitive; Stargate/LayerZero merger, Wormhole, native chain bridging |
| TVL gap | **Medium** | ~$52M vs. Stargate ~$216M despite architectural advantages |
| UMA dependency | **Medium** | Across security model entirely depends on UMA oracle functioning correctly |
| Relayer concentration | **Medium** | Insufficient relayer decentralization creates liveness/censorship risk |
| Token-to-equity liquidity | **Medium** | Private company equity is illiquid vs. freely tradeable token |

### For Risk Labs Corporate

| Risk | Severity | Detail |
|------|----------|--------|
| Regulatory risk | **High** | SEC treatment of token-to-equity is unknown |
| Governance credibility | **High** | $23M treasury scandal + oracle manipulation create trust deficit |
| Two-product spillover | **Medium** | UMA controversies could damage Across reputation, and vice versa |
| Key-person risk | **Medium** | Hart Lambur is dominant public figure; lean leadership team |
| Valuation uncertainty | **Medium** | Last known valuation ($200M) from Nov 2022; likely outdated |

### Open Questions to Watch

1. **March 26 vote:** Does the community approve DAO → C-corp conversion?
2. **SEC response:** How do regulators view the token-to-equity conversion?
3. **Institutional deals:** Does corporatization actually unlock enterprise partnerships?
4. **POLY token:** Does Polymarket internalize its oracle, killing UMA's primary use case?
5. **UMA security model:** Can it be redesigned to handle markets exceeding its market cap?
6. **Precedent effect:** Do other protocols follow Across's lead on DAO → C-corp?
7. **$23M accountability:** Will the treasury controversy be transparently resolved?

---

## 6. Timeline of Key Events

| Date | Event |
|------|-------|
| 2018 | Hart Lambur and Allison Lu found UMA Protocol |
| ~2018–2019 | $4M seed round led by Placeholder |
| April 2021 | First-ever IDO on Uniswap for UMA token |
| November 2022 | Across Protocol raises $10M at $200M valuation |
| October 2023 | First controversial DAO treasury proposal (100M ACX to Risk Labs) |
| June 2024 | Barron Trump DJT market — UMA resolves "No," Polymarket overrides to "Yes" |
| Q2+Q4 2024 | Paradigm leads $41M token round for Across ($51M total) |
| 2024 | UMA oracle processes 34,000+ assertions (98.6% dispute-free) |
| March 2025 | BornTooLate.eth governance attack on $7M Ukraine mineral deal market |
| March 2025 | Across announces $41M raise publicly; Optimistic Truth Bot launched (shadow mode) |
| June 2025 | $23M DAO treasury scandal allegations surface; ACX drops 10% |
| June 2025 | PancakeSwap launches cross-chain swaps via Across/ERC-7683 |
| July 2025 | $237M Zelenskyy suit market controversy |
| July 2025 | UMIP-189 proposed; AI bot integration for oracle |
| August 6, 2025 | UMIP-189 passed — MOOV2 deployed (37 whitelisted proposers) |
| October 2025 | Reports of Polymarket POLY token potentially replacing UMA |
| November 2025 | UMA staking minimum raised to 1,000 UMA; managed proposer whitelist grows to 177 |
| March 6, 2026 | UMA tweets: "Prediction markets continue to scale, so does UMA" |
| March 11, 2026 | "The Bridge Across" proposal posted — ACX surges 80–85% |
| March 18, 2026 | Community call (upcoming) |
| March 26, 2026 | Snapshot vote (upcoming) |

---

## 7. Sources

All findings were cross-verified by an independent QA supervisor agent. Full source list below.

### UMA / Polymarket Sources
- [UMA Docs: How does UMA's Oracle work?](https://docs.uma.xyz/protocol-overview/how-does-umas-oracle-work)
- [UMA Docs: DVM 2.0](https://docs.uma.xyz/protocol-overview/dvm-2.0)
- [UMA Blog: What is UMA's Optimistic Oracle?](https://blog.uma.xyz/articles/what-is-umas-optimistic-oracle)
- [UMA Blog: Improving Oracle Efficiency with Managed Proposers](https://blog.uma.xyz/articles/managed-proposers)
- [UMA Discourse: UMIP-189](https://discourse.uma.xyz/t/umip-189-approve-new-managedoptimisticoraclev2-deployment/2229)
- [Polymarket Docs: Resolution via UMA](https://docs.polymarket.com/developers/resolution/UMA)
- [Polymarket Legacy Docs: Polymarket + UMA](https://legacy-docs.polymarket.com/polymarket-+-uma)
- [GitHub: Polymarket UMA CTF Adapter](https://github.com/Polymarket/uma-ctf-adapter)
- [CoinDesk: Polymarket Suffers UMA Governance Attack](https://www.coindesk.com/markets/2025/03/26/polymarket-suffers-uma-governance-attack-after-rouge-actor-becomes-top-5-token-staker)
- [CoinDesk: $160M Controversy Over Zelenskyy Suit](https://www.coindesk.com/markets/2025/07/07/polymarket-embroiled-in-usd160m-controversy-over-whether-zelensky-wore-a-suit-at-nato)
- [CoinDesk: Polymarket's POLY Could Bring Oracle's Home](https://www.coindesk.com/markets/2025/10/10/asia-morning-briefing-polymarket-s-poly-could-bring-oracle-s-home)
- [The Block: Governance Attack 'Unprecedented'](https://www.theblock.co/post/348171/polymarket-says-governance-attack-by-uma-whale-to-hijack-a-bets-resolution-is-unprecedented)
- [The Block: UMA Oracle Update to Limit Proposals](https://www.theblock.co/post/366507/polymarket-uma-oracle-update)
- [The Block: Polymarket Contradicts UMA on Barron Trump](https://www.theblock.co/post/302171/polymarket-contradicts-umas-resolution-on-barron-trumps-involvement-with-djt-token)
- [The Defiant: $7M Ukraine Mineral Deal Debacle](https://thedefiant.io/news/defi/polymarket-s-usd7m-ukraine-mineral-deal-debacle-traced-to-oracle-whale)
- [The Defiant: Zelenskyy Suit Market Resolves](https://thedefiant.io/news/nfts-and-web3/polymarket-controversy-heats-up-after-the-zelenskyy-suit-market-resolves)
- [Decrypt: Polymarket Rules 'No' on $237M Bet](https://decrypt.co/329210/polymarket-rules-no-237m-bet-zelenskyys)
- [Yahoo Finance: 'This Isn't Decentralized'](https://finance.yahoo.com/news/isn-t-decentralized-says-polymarket-111359338.html)
- [Webopedia: Polymarket's UMA Controversial?](https://www.webopedia.com/crypto/learn/polymarkets-uma-oracle-controversy/)
- [Orochi Network: Oracle Manipulation in Polymarket 2025](https://orochi.network/blog/oracle-manipulation-in-polymarket-2025)
- [Cryptopolitan: Community Protests Oracle Vote](https://www.cryptopolitan.com/polymarket-community-protests-oracle-vote-by-uma-whales-claims-market-manipulation/)
- [CryptoSlate: Whales Forced UFO Vote](https://cryptoslate.com/polymarket-faces-major-credibility-crisis-after-whales-forced-a-yes-ufo-vote-without-evidence/)

### UMA AI Oracle Sources
- [UMA's AI Experiment: Can AI Agents Enhance the Optimistic Oracle?](https://blog.uma.xyz/articles/experiment-can-ai-agents-enhance-uma-oracle)
- [Inside UMA's Optimistic Truth Bot](https://blog.uma.xyz/articles/inside-umas-optimistic-truth-bot)
- [AI Is Helping Us Find the Truth — UMA Blog](https://blog.uma.xyz/articles/ai-is-helping-us-find-the-truth)
- [Managed Proposers Update — UMA Blog](https://blog.uma.xyz/articles/managed-proposers-update)
- [UMA, Polymarket, and EigenLayer Research a Next-Gen Prediction Market Oracle](https://blog.uma.xyz/articles/uma-polymarket-and-eigenlayer-research-a-next-gen-prediction-market-oracle)
- [UMA's Truth Bot Joins Polymarket, Accuracy Hits 95% — Stocktwits](https://stocktwits.com/news-articles/markets/cryptocurrency/uma-truth-bot-polymarket/chkuj5XRbwC)
- [AI Agents Are Quietly Rewriting Prediction Market Trading — CoinDesk](https://www.coindesk.com/tech/2026/03/15/ai-agents-are-quietly-rewriting-prediction-market-trading)

### Across Protocol Sources
- [Across Protocol Official](https://across.to/)
- [Across Documentation](https://docs.across.to/)
- [Across V3: First Intents-Based Protocol](https://across.to/blog/Across-V3-Introducing-the-First-Intents-Based-Interoperability-Protocol)
- [The Bridge Across — Governance Forum](https://forum.across.to/t/the-bridge-across/2097)
- [CoinDesk: ACX Rockets 80%](https://www.coindesk.com/markets/2026/03/12/across-s-acx-rockets-80-massively-beating-bitcoin-on-plans-to-dump-its-dao-structure)
- [The Block: ACX-to-Equity Exchange](https://www.theblock.co/post/393192/paradigm-backed-across-protocol-acx-token-equity-exchange)
- [Crypto.news: ACX Jumps 85%](https://crypto.news/acx-price-across-protocol-token-equity-proposal-2026/)
- [The Defiant: DAO to Private Company](https://thedefiant.io/news/defi/across-protocol-proposes-shift-from-dao-to-private-company)
- [Crypto.news: Token-to-Equity Shift](https://crypto.news/across-protocol-weighs-token-to-equity-shift-in-bid-for-legal-clarity-and-institutional-capital/)
- [DL News: DAO-to-Company Proposal](https://www.dlnews.com/articles/defi/across-soars-as-foundation-proposes-turning-dao-into-company/)
- [Odaily: Across Leads the "Rebellion"](https://www.odaily.news/en/post/5209712)
- [Across Raises $41M — Official Blog](https://across.to/blog/across-raises-41m-to-unify-ethereum-ecosystem)
- [The Block: Paradigm Leads $41M Round](https://www.theblock.co/post/344470/paradigm-leads-token-round-across-protocol-ethereum)
- [CryptoNews: Across Raises $41M](https://cryptonews.com/news/across-raises-41-million-in-a-strategic-token-sale-led-by-paradigm-to-expand-its-intents-based-technology/)
- [Uniswap Labs Integrates Across](https://across.to/blog/Uniswap-Labs-Integrates-Across-for-In-App-Bridging)
- [ERC-7683 Standard Proposal](https://blog.uniswap.org/uniswap-labs-and-across-propose-standard-for-cross-chain-intents)
- [PancakeSwap Cross-Chain via Across](https://across.to/blog/pancakeswap-crosschain-swaps)
- [ERC-7683 Official](https://www.erc7683.org/)
- [OpenZeppelin: How Across Secured $30B+](https://www.openzeppelin.com/customer-stories/across)
- [Across: Why We've Never Been Hacked](https://across.to/blog/why-across-has-never-been-hacked)
- [UMA: Case Study Securing Across](https://blog.uma.xyz/articles/case-study-how-uma-secures-across-protocol)
- [DeFiLlama: Across Protocol](https://defillama.com/protocol/across)
- [DeFiLlama: Bridge Rankings](https://defillama.com/bridges)
- [Dune: Across Bridge Stats](https://dune.com/sandman2797/across-bridge-stats)

### $23M Treasury Scandal Sources
- [Protos: Across Accused of Looting DAO Treasury](https://protos.com/across-protocol-accused-of-looting-dao-treasury-of-23m/)
- [CoinTelegraph: Founders Accused of Funneling $23M](https://cointelegraph.com/news/across-protocol-founders-accused-of-funneling-23m-to-own-company)
- [CryptoPotato: Co-Founder Responds](https://cryptopotato.com/across-protocol-team-accused-of-a-23m-grab-co-founder-responds/)
- [The Block: ACX Drops 10% Amid Allegations](https://www.theblock.co/post/360075/across-protocol-founder-refutes-allegations)
- [Bitget News: Governance Scandal](https://www.bitget.com/news/detail/12560604837743)

### Risk Labs Corporate Sources
- [Risk Labs Foundation](https://risklabs.foundation/)
- [Risk Labs — Crunchbase](https://www.crunchbase.com/organization/risk-labs)
- [Hart Lambur — IQ.wiki](https://iq.wiki/wiki/hart-lambur)
- [Allison Lu — IQ.wiki](https://iq.wiki/wiki/allison-lu)
- [UMA 2024 Year in Review](https://blog.uma.xyz/articles/2024-in-review-truth-trust-and-innovation-at-uma-protocol)
- [CoinGecko: UMA](https://www.coingecko.com/en/coins/uma)
- [CoinMarketCap: UMA](https://coinmarketcap.com/currencies/uma/)
- [CoinGecko: Across Protocol](https://www.coingecko.com/en/coins/across-protocol)
- [CoinMarketCap: Across Protocol](https://coinmarketcap.com/currencies/across-protocol/)

---

## QA Verification Notes

This report was produced by a **4-agent research team:**
- **3 DeFi Market Analyzer agents** conducted parallel research on UMA/Polymarket, Across Protocol, and Risk Labs corporate
- **1 DeFi Agent Supervisor** independently verified all findings and flagged potential fabrication risks

**Confidence Levels:**
- **HIGH confidence:** Founding team, funding history, ACX equity proposal terms ($0.04375 buyout, 1:1 equity, 5M threshold, SPV structure, March 26 vote), UMA governance attack details (BornTooLate.eth, 5M UMA, March 2025), UMIP-189/MOOV2 changes (37 addresses, August 6, 2025), $23M treasury allegations, Zelenskyy suit $237M market, security audit record (18 audits, zero exploits), OTB accuracy metrics — all verified across 3+ independent sources
- **MEDIUM confidence:** TVL figures (~$32–52M range across sources), lifetime volume ($19B–$35B range depending on date), UMA token price (~$0.42–0.68 discrepancy), ACX circulating supply (~658–702M discrepancy)
- **DATA GAPS (explicitly noted):** Current daily bridge volumes, precise market share %, specific fee revenue breakdowns, Risk Labs current valuation, exact employee count, Risk Labs treasury balance, detailed fund deployment accounting for $23M controversy

**No data was fabricated in this report.** Where specific numbers could not be independently verified or showed discrepancies across sources, this is explicitly noted with ranges and caveats.
