---
title: "Audit Report: Stablecoin Privacy Technology Research Brief"
date: 2026-03-31
auditor: "Independent QA Auditor"
---

# Audit Report: Stablecoin Privacy Technology & Confidential Settlement Research Brief

*Audit Date: March 31, 2026*

---

## tl;dr

- **The research brief is generally well-sourced and accurate on its major factual claims, but contains several imprecisions, one meaningful conflation error, and one outright mislabeling that require correction.** The 84% illicit volume figure, Payy's funding details, Railgun's metrics, Aleo's funding, Aztec's vulnerability, Zama's funding, and the CROPS mandate are all verifiable. However, the brief mislabels ERC-7984's scope and conflates raw on-chain stablecoin volume with payment volume when arguing for enterprise adoption blockers.
- **The brief's weakest area is source quality for Payy-specific performance claims.** The sub-0.5s proving time and user/volume metrics are company-reported or sourced from Aztec's marketing blog (which has an interest in promoting Noir adoption). No independent benchmarks or third-party analytics verify these figures.
- **Overall grade: 3.5 out of 5.** Solid directional research with good competitive mapping, but the brief would not pass rigorous institutional due diligence without corrections to the items identified below.

---

## Table of Contents

1. [Verified Claims](#1-verified-claims)
2. [Unverified or Suspect Claims](#2-unverified-or-suspect-claims)
3. [Corrections Needed](#3-corrections-needed)
4. [Gaps Identified](#4-gaps-identified)
5. [Conflation Errors](#5-conflation-errors)
6. [Grades](#6-grades)
7. [Sources Used in Audit](#7-sources-used-in-audit)

---

## 1. Verified Claims

The following claims were verified against independent or primary sources:

### Payy

| Claim | Status | Source |
|-------|--------|--------|
| $6M seed round led by FirstMark Capital (March 2026) | VERIFIED | Multiple outlets: The Block, FinSMEs, CrowdFund Insider, FFNews |
| Additional investors: Robot Ventures, DBA Crypto | VERIFIED | FinSMEs, FFNews |
| 100K+ users across 120 countries | VERIFIED (company-reported, not independently auditable) | Press coverage of seed round; company claims in multiple outlets |
| ~$130M annualized transaction volume | VERIFIED (company-reported, not independently auditable) | Same as above |
| Founded by Sid Gandhi (CEO) and Calum Moore (CTO) | VERIFIED | CoinDesk, FinSMEs |
| Visa non-custodial stablecoin card launched August 2025 | VERIFIED | CoinDesk (Aug 6, 2025) |
| Originally Polybase; pivoted to payments | VERIFIED | Payy docs, GitHub (polybase/payy), Protocol Labs directory |
| Migration from Halo2 to Noir | VERIFIED | Aztec blog post ("Just write if: Why Payy left Halo2 for Noir") |
| Codebase shrank from thousands of lines to 250 | VERIFIED | Same Aztec blog post |
| UTXO model, HotStuff PoS consensus, validium rollup | VERIFIED | Payy docs, whitepaper at polybase.github.io |

### Railgun

| Claim | Status | Source |
|-------|--------|--------|
| $4.5B cumulative volume | VERIFIED (self-reported; privacy protocol limits independent verification) | Railgun community analytics, Messari |
| $108M TVL | VERIFIED (approximately; DeFiLlama shows ~$106M) | DeFiLlama |
| TVL grew ~10x from $11M | VERIFIED | DeFiLlama historical data; Railgun X/Twitter post |
| Live on Ethereum, Arbitrum, Polygon, BNB | VERIFIED | DeFiLlama, Railgun official site |
| Vitalik Buterin has personally used Railgun | VERIFIED | Multiple CoinDesk, CryptoTimes articles documenting ETH transfers |
| Ethereum Foundation staked 50,000 RAIL | VERIFIED | Messari, 99Bitcoins, Bankless |
| DCG investment | VERIFIED but amount differs from brief | CoinDesk (Jan 2022): DCG invested $10M+, not the $7M stated in the brief |

### Aleo

| Claim | Status | Source |
|-------|--------|--------|
| $228M total funding | VERIFIED | Tracxn ($200M Series B Feb 2022, SoftBank led) |
| Circle confidential USDC (USDCx) partnership | VERIFIED | Fortune (Dec 2025), Circle blog, BusinessWire |
| Paxos USAD launched on Aleo | VERIFIED | CoinDesk (Oct 2025) |
| Toku payroll integration (Jan 2026) | VERIFIED | BusinessWire (Jan 29, 2026) |

### Aztec

| Claim | Status | Source |
|-------|--------|--------|
| $100M Series B (Dec 2022, a16z led) | VERIFIED | TechCrunch, CoinTelegraph, Medium |
| $17M Series A (Paradigm led) | VERIFIED | CoinDesk (Dec 2021) |
| Critical vulnerability disclosed March 2026 | VERIFIED but date differs | HackMD disclosure: discovered March 17, 2026, not March 27 as stated in the brief |
| Vulnerability affects proving system, could enable theft | VERIFIED | HackMD disclosure |
| v5 fix targeted for July 2026 | VERIFIED | HackMD disclosure |
| Alpha testnet May 2025 | VERIFIED | The Defiant |

### Zama

| Claim | Status | Source |
|-------|--------|--------|
| $150M+ total funding | VERIFIED | Tracxn ($73M Series A Mar 2024 + $57M Series B Jun 2025 + earlier rounds) |
| $1B valuation | VERIFIED | Tracxn, CoinDesk, MEXC analysis |
| 11K+ bidders in sealed auction, $118.5M committed | VERIFIED | AInvest, Zama community forum |

### Market and Ecosystem Claims

| Claim | Status | Source |
|-------|--------|--------|
| Stablecoins = 84% of illicit transaction volume | VERIFIED | Chainalysis 2026 Crypto Crime Report |
| $154B received by illicit addresses in 2025 | VERIFIED | Chainalysis 2026 Crypto Crime Report |
| 162% increase from 2024 | VERIFIED | Chainalysis 2026 report |
| A7A5 ruble stablecoin >$93B in <1 year | VERIFIED | Chainalysis 2026 report |
| Ethereum Foundation CROPS mandate (March 2026) | VERIFIED | EF blog (March 13, 2026), CoinDesk, Unchained, The Defiant |
| CROPS = Censorship Resistance, Open Source, Privacy, Security | VERIFIED | EF blog, EF X/Twitter |
| Less than 1% of businesses use crypto for payroll | VERIFIED (sourced from Aleo/Toku press release) | BusinessWire (Jan 2026) |
| Stablecoins processed $8.9T in H1 2025 | VERIFIED as raw on-chain volume (but see conflation section) | TRM Labs, multiple outlets |
| ERC-7984 exists | VERIFIED | eips.ethereum.org, OpenZeppelin docs, Zama docs |
| GENIUS Act enacted 2025 | VERIFIED | Multiple legislative coverage outlets |

---

## 2. Unverified or Suspect Claims

### 2a. Claims That Could Not Be Independently Verified

1. **Payy's 100K users and $130M annualized volume**: These are company-reported figures from seed round press coverage. No third-party analytics or on-chain data independently confirms them. The brief itself flags this in Section 5, which is commendable, but then uses these figures without qualification throughout the body of the report.

2. **Sub-0.5s proof generation on iPhones**: The sole source is the Aztec blog post, which is a promotional piece for Noir (Aztec's own DSL). No independent benchmark, no specification of iPhone model, no circuit complexity details, no measurement methodology disclosed. Aztec has a direct commercial interest in advertising Noir's performance.

3. **"2x proving speed improvement without optimization"**: Same source concern as above.

4. **"95% of code auditable by auditors without Noir experience"**: This is a qualitative claim from the Aztec blog with no methodology. What does "95%" mean operationally? Who measured this?

5. **326 daily shields (Railgun)**: Sourced from community analytics. Not verified against an independent data provider.

6. **PLONK "2.4s" proving time**: No specific benchmark source cited. Proving times vary enormously by circuit size, hardware, and implementation.

### 2b. Claims With Potential Source Bias

1. **Payy's "demand-first" framing as unique**: The brief presents this as established fact, but it is essentially Payy's own marketing narrative. Whether a Visa card integration constitutes genuine "demand" for private L2 infrastructure (as opposed to demand for a convenient fiat off-ramp) is an analytical question the brief does not interrogate.

2. **"Every stablecoin transaction is a permanent data leak"**: This is a quote attributed to Protocol Labs, which is an investor in Payy. It is advocacy, not analysis.

3. **"Privacy being the primary blocker" for enterprise adoption**: This comes from the Aleo/Toku/Paxos press release -- companies selling privacy solutions. The claim that privacy (rather than, say, regulatory uncertainty, accounting treatment, or integration complexity) is THE primary blocker deserves skepticism.

---

## 3. Corrections Needed

### 3a. Factual Errors

1. **Aztec vulnerability disclosure date**: The brief states "March 27, 2026." The HackMD disclosure document states the vulnerability was discovered on **March 17, 2026**. The brief should correct this date.

2. **ERC-7984 mislabeled**: The brief describes ERC-7984 as a standard "for confidential smart contracts." It is actually **ERC-7984: Confidential Fungible Token** -- a token standard (analogous to ERC-20 with encrypted balances), not a general-purpose confidential smart contract standard. This is a meaningful mislabeling that overstates the standard's scope.

3. **Railgun funding**: The brief states "$7M private token sale (July 2021, DCG led)." DCG's involvement was actually a **$10M+ strategic investment announced January 2022**, consisting of $7.2M to the DAO treasury and $3M+ in governance token purchases. The brief conflates the July 2021 token sale with the later DCG investment and understates the total.

4. **Calum Moore described as "ex-Apple engineer"**: Multiple sources (CoinDesk, Aug 2025) describe the "ex-Apple engineer" as the person who "unveils" the Visa card, and other sources indicate Sid Gandhi previously worked in engineering at Apple. The brief should verify and correctly attribute the Apple background -- some sources attribute it to Gandhi, not Moore.

5. **Aztec total funding**: The brief states "$119M+" but notes uncertainty. Tracxn confirms $119M across 3 rounds, while some sources report higher figures ($178M including earlier grants and rounds). The brief's figure is defensible but the "+" qualifier should be expanded with a note about the discrepancy.

6. **Payy's relationship to Protocol Labs**: The brief states Payy "launched as a project from Polybase Labs, a team within Protocol Labs." The actual relationship is more nuanced: Polybase Labs appears in the Protocol Labs directory as a network team, and Protocol Labs is an investor. Calling it "a team within Protocol Labs" implies an employment/subsidiary relationship that may overstate the connection.

### 3b. Imprecisions

1. **"Stablecoins represent 84% of all illicit transaction volume tracking"**: The original Chainalysis finding is that stablecoins represent 84% of illicit *crypto* transaction volume (some TRM Labs data shows 86%). The phrase "illicit transaction volume tracking" is awkward and could imply 84% of all surveillance activity, which is not what the data says.

2. **Vitalik's statement about "backsliding"**: The brief claims "Vitalik stated Ethereum will reverse a decade of 'backsliding' on self-sovereignty." The CROPS mandate is an Ethereum Foundation document. Attributing it solely to Vitalik as a personal statement may be imprecise; it is an institutional position.

---

## 4. Gaps Identified

### 4a. Major Omissions

1. **No discussion of actual payment volume vs. raw on-chain volume**: The brief cites $8.9 trillion in H1 2025 stablecoin volume alongside arguments about enterprise payment adoption. McKinsey/Artemis data (published early 2026) shows that actual stablecoin *payment* volume is approximately $390 billion annually -- roughly 1% of the $33+ trillion in total on-chain stablecoin transaction volume for 2025. The remaining 99% is trading, internal transfers, and protocol-level activity. This distinction is critical for sizing the actual addressable market for private payment infrastructure and the brief completely ignores it.

2. **No discussion of regulatory risks to privacy projects**: The brief covers privacy as a desirable feature but does not address the regulatory headwinds. FATF's March 2026 report specifically warned about stablecoins in illicit finance. The tension between privacy features and regulatory compliance obligations (travel rule, sanctions screening) is acknowledged only superficially through mentions of "selective disclosure" and "Proofs of Innocence."

3. **No discussion of Tornado Cash precedent**: The largest regulatory action against a privacy protocol (Tornado Cash sanctions) is not mentioned. This is a significant omission given that it directly bears on the legal risks facing every project in this space.

4. **Missing competitive entrants**: The brief does not cover:
   - **Secret Network** (privacy-focused L1 in the Cosmos ecosystem with significant history)
   - **Mina Protocol** (ZK-native L1 with succinct blockchain design)
   - **Solana-native privacy solutions** mentioned in passing (Tempo, Arc, RCM) but not analyzed

5. **No fee revenue or economic model analysis**: The brief discusses TVL and volume for Railgun but does not examine protocol fee revenue, which is the metric that determines economic sustainability. Per CLAUDE.md domain knowledge rules, fee revenue and trading volume are fundamentally different metrics and should not be substituted for each other.

6. **No analysis of anonymity set sizes**: For privacy protocols, the anonymity set (the pool of users among whom a transaction can hide) is a fundamental security metric. Larger anonymity sets = stronger privacy. The brief mentions Namada's MASP advantage but does not provide any actual anonymity set data for any project.

### 4b. Missing Context

1. **Payy's current chain**: The Payy docs indicate the current validium rollup runs on Polygon, with Ethereum planned. The brief does not mention Polygon at all, characterizing Payy only as an "Ethereum L2."

2. **No token information for Payy**: The brief does not discuss whether Payy has a token, plans for one, or how the network's economic incentives will work.

3. **No discussion of Noir's backend**: The brief correctly notes Noir is a DSL, not a proof system, but does not specify that Payy uses Barretenberg (UltraHonk) as the proving backend. This matters because the proving time is a function of the backend, not Noir itself.

---

## 5. Conflation Errors

### 5a. Raw On-Chain Volume vs. Payment Volume (SIGNIFICANT)

**Location**: Section 3c, paragraph beginning "Stablecoins processed over $8.9 trillion in H1 2025 alone, but less than 1% of businesses use crypto for payroll..."

**The problem**: This sentence juxtaposes $8.9 trillion in raw on-chain volume with enterprise payroll adoption as if they are related metrics. The $8.9 trillion figure is dominated by trading activity, DeFi interactions, and protocol-level transfers -- not payments. McKinsey/Artemis data shows actual stablecoin payment volume is approximately $390 billion annually (roughly 1% of total on-chain volume). Using the $8.9 trillion figure alongside enterprise payment adoption statistics creates a misleading impression of the addressable market for private payment infrastructure.

**Per CLAUDE.md domain rules**: "In DeFi analytics, distinguish between fee revenue (protocol earnings) and trading volume (total value of trades). These are fundamentally different metrics and must never be conflated or substituted for each other." While this specific instance is about payment volume vs. trading volume rather than fee revenue vs. trading volume, the same principle applies: different volume categories must not be conflated.

### 5b. Payy's Consumer Volume vs. L2 Network Volume

**Location**: Throughout, particularly Section 4b.

**The problem**: The brief uses Payy's $130M annualized consumer card volume as evidence of demand for its privacy L2. However, the consumer card product (a Visa card for spending stablecoins) and the planned L2 validium rollup are different products serving different markets. Card volume demonstrates demand for convenient stablecoin spending, not necessarily demand for on-chain private settlement infrastructure. The brief does not interrogate whether Payy's consumer users would migrate to, or need, the L2.

---

## 6. Grades

### Accuracy: 3.5 / 5

**Rationale**: The major factual claims are verifiable and largely correct. However, the Aztec vulnerability date is wrong, ERC-7984 is mislabeled, the Railgun funding details are inaccurate, the Apple engineer attribution is unclear, and the Protocol Labs relationship is imprecise. None of these errors are catastrophic, but in aggregate they indicate insufficient fact-checking rigor for a research brief that could inform investment or strategic decisions.

### Completeness: 3 / 5

**Rationale**: The competitive landscape mapping is solid and the project-by-project breakdown is useful. However, the omission of the payment volume vs. on-chain volume distinction is a serious analytical gap. The absence of Tornado Cash regulatory context, anonymity set analysis, fee revenue analysis, and several competitive projects (Secret Network, Mina) leaves meaningful holes. The brief also does not adequately stress-test Payy's narrative (demand-first, Protocol Labs relationship) despite being positioned as a deep dive.

### Source Quality: 3 / 5

**Rationale**: The brief cites a good range of sources and commendably flags what it could not verify in Section 5. However, several key claims rely on first-party or commercially interested sources: Payy's metrics come from its own press coverage, the sub-0.5s proving claim comes from Aztec's marketing blog, and the "privacy as primary blocker" framing comes from companies selling privacy solutions. The brief does not sufficiently distinguish between independently verified data (Chainalysis reports, DeFiLlama TVL, Tracxn funding data) and company-reported or promotional claims.

### Overall: 3.5 / 5

The brief is a competent first-pass survey of the stablecoin privacy landscape with genuine analytical value in its competitive mapping and technology taxonomy. It falls short of institutional-grade research due to the conflation errors, factual imprecisions, and insufficient critical examination of source bias, particularly around Payy-specific claims.

---

## 7. Sources Used in Audit

- [Payy Network Raises $6M in Seed Funding - FinSMEs](https://www.finsmes.com/2026/03/payy-network-raises-6m-in-seed-funding.html)
- [Stablecoin startup Payy raises $6 million - The Block](https://www.theblock.co/post/395106/stablecoin-startup-payy-funding-private-transactions)
- [FirstMark Capital Leads $6M Round - FFNews](https://ffnews.com/newsarticle/firstmark-capital-leads-6m-round-in-payy-to-bring-private-transactions-to-stablecoins/)
- [Just write "if": Why Payy left Halo2 for Noir - Aztec Blog](https://aztec.network/blog/just-write-if-why-payy-left-halo2-for-noir)
- [Payy ZK Proofs Documentation](https://docs.payy.network/payy-network/07_zk_proofs)
- [Railgun TVL - DeFiLlama](https://defillama.com/protocol/railgun)
- [Railgun $10M DCG Investment - CoinDesk](https://www.coindesk.com/business/2022/01/26/railgun-aims-for-private-defi-with-10m-backing-from-dcg)
- [Aleo Funding - Tracxn](https://tracxn.com/d/companies/aleo/__RacRfD0m2MMCL1W0r84AYItrD3voiquV86feVcv6RwI/funding-and-investors)
- [Circle USDCx on Aleo - Fortune](https://fortune.com/2025/12/09/circle-privacy-stablecoin-aleo-udsc-udscx/)
- [Aztec Funding - Tracxn](https://tracxn.com/d/companies/aztec/__LE23OLSU3tSwcB512mod1tOqusASlza36dQp2aaFw6k/funding-and-investors)
- [Aztec Vulnerability Disclosure - HackMD](https://hackmd.io/@aztec-network/disclosure-of-recent-vulnerabilities)
- [Aztec $100M Series B - TechCrunch](https://techcrunch.com/2022/12/15/aztec-network-takes-on-encrypted-blockchains-with-100m-round-led-by-a16z/)
- [Zama Funding - Tracxn](https://tracxn.com/d/companies/zama/__5Wsps5czqkAfDYHYlSjucnQDaYuHTgfxaxaBuYO_Cgc/funding-and-investors)
- [Zama $57M Raise - CoinDesk](https://www.coindesk.com/tech/2025/06/25/zama-raises-57m-becomes-first-unicorn-involved-with-fully-homomorphic-encryption)
- [EF Mandate Blog Post](https://blog.ethereum.org/2026/03/13/ef-mandate)
- [EF CROPS Mandate - Unchained](https://unchainedcrypto.com/ethereum-foundation-publishes-crops-mandate-for-network-stewardship/)
- [ERC-7984: Confidential Fungible Token - eips.ethereum.org](https://eips.ethereum.org/EIPS/eip-7984)
- [ERC-7984 Explained - Zama](https://www.zama.org/post/erc-7984-the-confidential-token-standard-explained)
- [ERC-7984 - OpenZeppelin Docs](https://docs.openzeppelin.com/confidential-contracts/token)
- [Chainalysis 2026 Crypto Crime Report](https://www.chainalysis.com/blog/2026-crypto-crime-report-introduction/)
- [TRM Labs 2026 Crypto Crime Report](https://www.trmlabs.com/reports-and-whitepapers/2026-crypto-crime-report)
- [Aleo/Toku/Paxos Payroll Launch - BusinessWire](https://www.businesswire.com/news/home/20260129619369/en/Aleo-Toku-and-Paxos-Labs-Launch-First-Private-Stablecoin-Payroll-Solution-Removing-the-Final-Barrier-to-Enterprise-Stablecoin-Adoption)
- [Stablecoins in payments: What the raw transaction numbers miss - McKinsey](https://www.mckinsey.com/industries/financial-services/our-insights/stablecoins-in-payments-what-the-raw-transaction-numbers-miss)
- [Stablecoins moved $35T but only 1% real payments - CoinDesk](https://www.coindesk.com/business/2026/01/23/stablecoins-moved-usd35-trillion-last-year-but-only-1-of-it-was-for-real-world-payments)
- [Ex-Apple Engineer Unveils Privacy-Focused Crypto Visa Card - CoinDesk](https://www.coindesk.com/business/2025/08/06/ex-apple-engineer-unveils-privacy-focused-crypto-visa-card)
- [Payy and Privacy for Stablecoins - Protocol Labs](https://www.protocol.ai/blog/payy-and-privacy-for-stablecoins/)
- [Payy in Protocol Labs Directory](https://directory.plnetwork.io/teams/cldvnxu4101lfu21kxoix4epq)
- [FATF Stablecoin Warning - CoinDesk](https://www.coindesk.com/policy/2026/03/03/international-financial-watchdog-warns-stablecoins-are-increasingly-used-in-sanctions-evasion-and-money-laundering)
