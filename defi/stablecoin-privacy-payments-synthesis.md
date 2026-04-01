---
title: "Synthesis: Stablecoin Privacy & Payments Infrastructure"
subtitle: "Consolidated findings across technology, market, regulatory, and commercial research -- with audit corrections applied"
date: 2026-03-31
status: Phase 3 Synthesis
---

# Stablecoin Privacy & Payments Infrastructure: Research Synthesis

*Consolidated from 4 research briefs and 4 audit reports | March 31, 2026*

---

## tl;dr

- **The actual stablecoin payment market is $390B annually, not $33T.** McKinsey/Artemis found that ~1% of raw on-chain stablecoin volume represents real-world payments. B2B payments ($226B, 58% of that $390B) grew 733% YoY and are the breakout vertical. All market sizing in this document uses the corrected $390B baseline.
- **Privacy is a verified blocker for enterprise adoption, but "the primary blocker" framing comes from companies selling privacy solutions.** The 7 specific CFO/treasurer objections are real and documented. Privacy solves objection #1 (competitive intelligence exposure) definitively, but objections #2-7 (accounting treatment, counterparty risk, Travel Rule, KYC, custody, ERP integration) require separate infrastructure that does not yet exist at scale.
- **No regulator has endorsed ZKP-based compliance.** This is the single most important unresolved question. The technical architecture for compliant privacy exists in academic papers and early deployments, but FATF has not opined, no jurisdiction has formally accepted ZK proofs as Travel Rule compliant, and the "privacy is infrastructure" argument has never been tested in enforcement.
- **The competitive landscape is fragmented with no clear winner.** Payy has the most consumer traction ($130M annualized, 100K users -- self-reported) on the least capital ($6M). Aleo has the most funding ($228M) and major issuer partnerships (Circle, Paxos). Aztec has the largest war chest among privacy L2s ($119M+) but a critical vulnerability delays its mainnet. Railgun has the most organic DeFi traction ($4.5B cumulative volume, $108M TVL).
- **The regulatory window is real but fragile.** The GENIUS Act, Tornado Cash delisting, and pro-crypto administration create a narrow opening. But administrations change, the GENIUS Act does not address privacy, and the FinCEN BSA/AML rulemaking (not yet issued) will define whether ZKP-based compliance is viable.

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [The Privacy Gap Thesis](#2-the-privacy-gap-thesis)
3. [Market Opportunity Map](#3-market-opportunity-map)
4. [Competitive Landscape (Corrected)](#4-competitive-landscape-corrected)
5. [Regulatory Reality Check](#5-regulatory-reality-check)
6. [Enterprise Adoption Barriers (Verified)](#6-enterprise-adoption-barriers-verified)
7. [Strategic Questions Worth Addressing](#7-strategic-questions-worth-addressing)
8. [Data Gaps & Unverified Claims](#8-data-gaps--unverified-claims)

---

## 1. Executive Summary

The following 15 findings represent the most important verified conclusions across all four research areas, incorporating audit corrections.

**Market:**

1. **The stablecoin market cap reached ~$320B by March 2026**, growing 55%+ YoY from $205B. USDT holds ~60% market share (~$184B, corrected down from $187B due to $6.5B in early 2026 burns). USDC holds ~24% (~$77-78B, corrected up from $75.7B). [Verified: DefiLlama, CoinMarketCap]

2. **Actual stablecoin payment volume was ~$390B in 2025, doubling from 2024.** This is ~1% of the $33-35T in raw on-chain volume (the discrepancy between Bloomberg's $33T and McKinsey's $35T is unresolved but immaterial to the ~1% ratio). B2B payments at $226B (58% of adjusted total, corrected from 60%) grew 733% YoY. [Verified: McKinsey/Artemis]

3. **Infrastructure M&A confirms strategic value of stablecoin settlement rails.** Stripe acquired Bridge for $1.1B (closed Feb 2025). Mastercard agreed to acquire BVNK for up to $1.8B ($1.5B base + $300M earnouts; deal pending regulatory approval as of March 2026). [Verified: CNBC, Mastercard IR]

4. **BlackRock's BUIDL fund reached approximately $18B in AUM by February 2026** -- not the "$1B by early 2026" stated in the original payments research. The $1B milestone was actually reached in March 2025. This 18x understatement in the original brief actually strengthens the case for institutional RWA adoption being further along than assumed. [Corrected per audit; source: BlockEden, Securitize]

**Technology:**

5. **ZK proof technology is approaching payment-speed readiness.** Payy claims sub-0.5s proof generation on iPhones using Noir (source: Aztec marketing blog -- not independently benchmarked). PLONK-based systems are at ~2.4s. Groth16 is fastest but requires per-circuit trusted setup. FHE (Zama, Fhenix) is not yet suitable for real-time payment settlement. [Partially verified; Payy proving times are self-reported]

6. **The EVM-compatible privacy design space is where stablecoin liquidity lives.** Ethereum holds ~56% of stablecoin supply ($184.8B). The four leading EVM privacy approaches are: Payy (L2 validium rollup), Aztec (programmable privacy L2), Railgun (on-chain smart contract privacy), and Fhenix/Zama (FHE coprocessor). [Verified: DefiLlama]

7. **Aztec disclosed a critical vulnerability on March 17, 2026** (corrected from March 27 in the original brief) that could have enabled theft via the proving system. The fix is targeted for v5 in July 2026. This delays the most well-funded privacy L2's mainnet. [Verified: HackMD disclosure]

**Regulatory:**

8. **The GENIUS Act (signed July 18, 2025) is the first U.S. federal stablecoin framework**, requiring 1:1 reserves, BSA/AML compliance, and prohibiting yield on stablecoins. OCC proposed implementing regulations February 25, 2026, with final rules due July 2026. The Act does not address transaction-level privacy or ZKP-based compliance. [Verified: Congress.gov, OCC, White House]

9. **The Tornado Cash legal precedent has two distinct events.** The Fifth Circuit ruled on November 26, 2024 (not March 2025 as originally stated) that immutable smart contracts cannot be sanctioned under IEEPA. OFAC delisted Tornado Cash on March 21, 2025. These are legally and chronologically separate. Roman Storm received a mixed verdict August 6, 2025: guilty on unlicensed money transmitting conspiracy, deadlocked on money laundering and sanctions conspiracy. [Corrected per audit; sources: Fifth Circuit opinion, OFAC SDN list]

10. **FATF Travel Rule adoption is at 52%, not 73%.** The original brief stated "85 of 117 jurisdictions (73%)" but the FATF's own 2025 Targeted Update surveys 163 jurisdictions, making it 85 of 163 (52%). The 85 jurisdictions figure is correct; the denominator and percentage were wrong. [Corrected per audit]

**Commercial:**

11. **The 7 specific enterprise adoption barriers are documented and real**: (1) on-chain transparency exposing competitive intelligence, (2) accounting treatment uncertainty, (3) counterparty risk on stablecoin issuers, (4) Travel Rule compliance gaps, (5) KYC/AML on counterparty wallets, (6) custody and key management, (7) ERP/TMS integration. Privacy infrastructure directly addresses #1; the rest require separate solutions. [Verified across multiple industry sources]

12. **Cross-border stablecoin payments offer "up to 80%" cost advantage** over traditional remittance in high-cost corridors (corrected from "80-90%" -- the 90% figure was not independently confirmed). The advantage narrows significantly against digital competitors like Wise. World Bank average remittance cost is 6.62%, not 6.2% as originally stated. [Corrected per audit; source: World Bank RPW Q3 2024]

13. **Marqeta's revenue is $507M (FY2024 net revenue), not "$700M+"** as originally stated. The overstatement resulted from not accounting for the Cash App contract renegotiation that changed revenue presentation. [Corrected per audit; source: Marqeta 10-K]

**Unresolved:**

14. **Payy's traction metrics (100K users, $130M annualized volume, 120 countries) are company-reported and cannot be independently verified.** The sub-0.5s mobile proving claim comes from an Aztec marketing blog post, which has a commercial interest in promoting Noir. No independent benchmarks exist. [Flagged across all audits]

15. **The GENIUS Act yield prohibition creates a structural opportunity** for yield aggregation layers and bank-issued deposit tokens (which can bear interest). JPMorgan's JPMD on Base and Canton Network is a deposit token, not a stablecoin, specifically because deposit tokens can offer yield. [Verified: JPMorgan, GENIUS Act text]

---

## 2. The Privacy Gap Thesis

### The Strongest Version of the Argument

Privacy is not a feature but a legal requirement for institutional stablecoin adoption. The evidence:

**Legal pillars:**
- **GLBA/Reg P** requires financial institutions to safeguard customer financial data. A public blockchain that exposes customer transaction data to anyone who reads the chain arguably violates the Safeguards Rule.
- **GDPR** (EDPB Guidelines 02/2025, adopted for public consultation April 2025) confirms blockchain receives no exemption from data minimization, storage limitation, and right-to-erasure requirements. Transparent blockchains may be inherently non-compliant.
- **The Fifth Circuit's Tornado Cash ruling** (November 2024) implicitly recognized that privacy in financial transactions has legal protection -- the government cannot ban tools that enable it.

**Commercial reality:**
- Every stablecoin transaction on a public blockchain is permanently visible. Blockchain analytics firms (Chainalysis, TRM Labs, Elliptic) trace across 25+ chains with "one-click tracing."
- AI-accelerated de-anonymization has destroyed practical pseudonymity on transparent chains.
- No CFO will accept a treasury where competitors, suppliers, and adversaries can track balances, vendor relationships, and payment timing in real time.

**Market signal:**
- Vitalik Buterin has personally used Railgun. The Ethereum Foundation staked 50,000 RAIL tokens.
- The EF's March 2026 CROPS Mandate explicitly prioritizes privacy alongside censorship resistance, open source, and security.
- Circle launched confidential USDC on Aleo. Paxos launched USAD on Aleo. These are major issuers investing in private stablecoin infrastructure.
- Aleo, Toku, and Paxos launched the first private stablecoin payroll solution (January 2026).

**The synthesis:** If transparent blockchains violate existing privacy law (GLBA, GDPR), and if institutions will not move meaningful capital on-chain without confidentiality, then privacy is not a feature layer -- it is an infrastructure prerequisite. Without it, stablecoins remain a retail/trading phenomenon. With it, they can absorb institutional payment flows.

### The Weakest Version of the Argument

Several critical vulnerabilities in the thesis:

1. **No regulator has endorsed ZKP-based compliance.** The entire "privacy is infrastructure" argument assumes regulators will accept zero-knowledge proofs as satisfying BSA, Travel Rule, and AML obligations. No jurisdiction has formally done so. FATF has not issued guidance. This is not a minor gap -- it is the central unresolved question.

2. **"Privacy as primary blocker" is an interested-party claim.** The framing that privacy is THE primary enterprise adoption barrier comes from Aleo/Toku press releases, Protocol Labs (Payy investor), and Payy itself. Other credible analyses identify regulatory uncertainty, accounting treatment, and integration complexity as co-equal or larger barriers.

3. **The GLBA/GDPR arguments have never been tested in enforcement.** No regulator has brought an action against a transparent blockchain for violating customer data protections. The argument is legally plausible but untested.

4. **Institutions may choose TEE-based privacy over ZK privacy.** Zaki Manian's "Tier 1" framework (Tempo, Circle Arc, RCM on Solana) offers invisible, fast privacy with hardware trust assumptions and backdoors. This is less sovereign but more palatable to compliance teams. Most institutions may adopt Tier 1 privacy, not Tier 2.

5. **The Tornado Cash precedent is narrower than presented.** The Fifth Circuit ruled specifically that immutable smart contracts are not "property" under IEEPA. It did not broadly protect anonymity-enabling technologies. Future regulatory action could use different legal theories or new legislation.

6. **"Privacy enables compliance" is a positioning statement, not a proven fact.** The five regulatory framings presented in the regulatory brief ("privacy enables compliance, not evasion"; "we provide more to regulators than transparent chains"; etc.) are untested marketing arguments, not established regulatory positions.

---

## 3. Market Opportunity Map

### Sizing the Addressable Market (Corrected Figures)

| Market Segment | 2025 Size | Privacy Requirement | Notes |
|---|---|---|---|
| **Total stablecoin payment volume** | $390B | Varies by segment | McKinsey/Artemis adjusted figure, not the $33-35T raw volume |
| **B2B payments** | $226B (58% of payments) | Must-have for large enterprises | Fastest-growing segment (733% YoY); supplier relationships, contract terms all exposed on transparent chains |
| **Cross-border remittances** | Portion of $390B; US-Mexico alone ~$63B corridor | Must-have in capital-control jurisdictions | Privacy is safety-critical in Nigeria, Argentina, Turkey where balances create physical risk |
| **Card settlement** | $18B annualized crypto card spend; $4.5B stablecoin-linked specifically | Nice-to-have currently; must-have for stablecoin-native settlement | Current crypto cards settle in fiat; privacy matters only if/when settlement moves on-chain |
| **Payroll** | Nascent (<1% of businesses use crypto for payroll) | Must-have | Compensation data cannot be public; Aleo/Toku launched first private stablecoin payroll Jan 2026 |
| **RWA settlement** | BUIDL at ~$18B AUM (corrected from $1B); total tokenized Treasuries >$5B | Must-have for institutional participants | Investor positions, trading strategies, allocation decisions all exposed on transparent chains |
| **Treasury management** | Yield-bearing stablecoins grew from $9.5B to $20B+ | Must-have | Corporate treasury balances are among the most sensitive financial data |

### Where Privacy Is "Must-Have" vs. "Nice-to-Have"

**Must-have (deal-breaker without privacy):**
- Enterprise treasury management (balance visibility)
- B2B settlement above $100K (supplier relationship exposure)
- Payroll (compensation confidentiality)
- Institutional RWA trading (position and strategy exposure)
- Cross-border payments in capital-control jurisdictions (personal safety)

**Nice-to-have (adoption possible without privacy, improved with it):**
- Retail consumer payments (most consumers do not understand or care about on-chain transparency)
- Small-ticket remittances under $1,000 (risk/reward of exposure is lower)
- DeFi interactions (power users already use Railgun; mass market does not demand it yet)
- Card settlement (currently fiat-settled; privacy matters only for future on-chain settlement)

### Vertical Deep Dives

**Card Settlement:** The most misleading opportunity in stablecoin payments. Current crypto card programs (Coinbase Card, Crypto.com Visa) convert crypto to fiat at point of sale and settle entirely through traditional Visa/Mastercard rails. The "crypto" part is the funding source only. For privacy to matter here, settlement itself must move on-chain, which requires Visa/Mastercard to accept stablecoin settlement at scale. Visa's USDC pilot (Crypto.com on Ethereum, Worldpay and Nuvei on Solana -- corrected to include Nuvei) is directional but limited in scope.

**Cross-Border B2B:** The clearest near-term opportunity. $226B growing 733% YoY, dominated by Asia-Pacific ($245B, 60% of payment volume). Companies settling invoices on-chain expose supplier relationships, payment terms, and volumes. Privacy-preserving settlement directly addresses this.

**RWA Tokenization:** The highest-value long-term opportunity. With BUIDL at $18B (corrected) and the total tokenized RWA market projected at $5-16T by 2030 (BCG/McKinsey -- treat with skepticism), the intersection of private stablecoins and tokenized assets is a complete institutional financial system on-chain. Investor privacy, NAV protection, and compliant transfer restrictions all require privacy infrastructure.

---

## 4. Competitive Landscape (Corrected)

### Project Comparison Table (Audit-Corrected)

| Project | Funding | Traction | Approach | Key Strength | Key Risk |
|---|---|---|---|---|---|
| **Payy** | $6M seed (FirstMark, Robot Ventures, DBA Crypto) | 100K users, $130M annualized (self-reported) | EVM L2 validium rollup, Noir, UTXO model | Only project with live consumer traction; sub-0.5s mobile proving (unverified) | Dramatically underfunded vs. competitors; L2 not yet live; traction metrics unaudited |
| **Aztec** | $119M+ (3 rounds; some sources report up to $178M including grants) | Alpha testnet; TGE Feb 2026 | Programmable privacy L2, Noir DSL, client-side proving | Largest war chest among privacy L2s; Noir becoming standard ZK DSL | Critical vulnerability (discovered March 17, 2026, corrected from March 27); v5 fix not until July 2026; no consumer product after 8 years of development |
| **Railgun** | DCG invested $10M+ (Jan 2022; corrected from "$7M private token sale"), including $7.2M to DAO treasury + $3M+ in governance tokens. EF staked 50K RAIL. | $4.5B cumulative volume (self-reported); ~$106-108M TVL (DeFiLlama); 326 daily shields | On-chain smart contract privacy on existing EVM chains; "Private Proofs of Innocence" | Live and growing (TVL 10x from $11M); no bridge risk; Vitalik endorsement; compliance innovation | Gas costs for on-chain proof verification; constrained by L1 throughput; limited to EVM chains |
| **Aleo** | $228M ($200M Series B, SoftBank led) | Mainnet live; confidential USDC (Circle) + USAD (Paxos) launched; Toku payroll | Privacy-native L1, Leo language, zk-SNARKs (Marlin) | Largest total funding; Circle and Paxos partnerships give institutional credibility | Separate L1 (not EVM-native); must bootstrap liquidity/ecosystem; token performance uncertain |
| **Zama** | $150M+ ($73M Series A + $57M Series B + earlier); $1B valuation | Mainnet on Ethereum (Dec 2025); sealed auction with 11K bidders, $118.5M committed | FHE coprocessor for encrypted computation on existing chains | FHE enables fundamentally different use cases (sealed auctions, encrypted state) that ZK cannot | FHE too computationally expensive for real-time payments; developer complexity |
| **Namada** | $60M+ (Anoma Foundation) | Mainnet Dec 2024; NAM token live June 2025 | Multi-Asset Shielded Pool (MASP), Cosmos ecosystem | All assets share one anonymity set (stronger privacy) | Not EVM-native; Cosmos ecosystem smaller than Ethereum; adoption metrics unclear |

### Who Has the Best Positioning and Why

**For institutional payments/settlement:** Aleo, due to Circle and Paxos partnerships. When the two largest regulated stablecoin issuers choose your platform for confidential stablecoins, that is a powerful signal. However, being a separate L1 (not EVM-native) means Aleo must bootstrap its own ecosystem.

**For DeFi privacy:** Railgun, due to live production deployment on existing EVM chains with no bridge risk. Railgun does not require users to migrate to a new chain -- privacy operates as a layer on existing infrastructure. The $108M TVL and Vitalik's personal usage are strong organic signals.

**For developer ecosystem:** Aztec, because Noir is becoming the standard ZK domain-specific language used by multiple projects (including Payy). Aztec's risk is execution -- 8 years of development, a critical vulnerability, and still no consumer product.

**For "demand-first" consumer adoption:** Payy. The 100K users and Visa card are unique traction metrics no other privacy project has. The question is whether consumer card users (who want a convenient spending product) translate into demand for an on-chain privacy L2 (a fundamentally different product). This conflation was flagged by auditors.

**For long-term encrypted computation:** Zama. FHE enables things ZK cannot (computation on encrypted data, not just proving statements about it). But FHE is not payment-speed ready and may not be for years.

### Risks for Each Approach

- **Payy:** $6M is a very small seed relative to the infrastructure required to launch and secure an L2. Single-team risk is high. The "demand-first" thesis assumes consumer card users will adopt the L2 -- this is unproven. The Protocol Labs relationship is more nuanced than "a team within Protocol Labs" (corrected per audit).
- **Aztec:** The March 2026 vulnerability undermines confidence in the proving system. v5 in July 2026 is the critical milestone. The project has raised $119M+ but has not shipped a consumer product in 8 years.
- **Railgun:** Privacy protocols with growing TVL attract regulatory attention. Railgun's "Proofs of Innocence" are innovative but untested with regulators. Gas costs limit throughput.
- **Aleo:** The existential risk of non-EVM L1s: ecosystem bootstrapping. If developers and liquidity do not come, partnerships with Circle and Paxos are insufficient.
- **Zama:** FHE's computational overhead may never reach payment-speed performance. The use case may be limited to specific applications (auctions, voting, encrypted state) rather than general payment settlement.

---

## 5. Regulatory Reality Check

### What Is Actually Settled

| Item | Status | Confidence |
|---|---|---|
| GENIUS Act is law | Signed July 18, 2025 | Certain |
| Stablecoin issuers must comply with BSA/AML | Required by GENIUS Act | Certain |
| 1:1 reserve backing required | Required by GENIUS Act | Certain |
| Stablecoin issuers cannot pay yield | Prohibited by GENIUS Act (but third-party platforms can -- loophole exists) | Certain |
| OCC can charter non-bank stablecoin issuers | Conditional approvals granted (Circle, Paxos, Bridge, Ripple, BitGo, Fidelity) | Certain |
| MiCA is fully in force in the EU | All transitional deadlines by July 2026 | Certain |
| Immutable smart contracts cannot be sanctioned under IEEPA | Fifth Circuit ruling, Nov 26, 2024 (corrected from "March 2025") | Settled law (subject to potential SCOTUS review) |
| Operating a financial service without registration is prosecutable | Roman Storm conviction, Aug 6, 2025 | Established (appeal pending) |

### What Is Still Uncertain

| Item | Status | Risk Level |
|---|---|---|
| **Whether ZKP-based compliance satisfies BSA requirements** | No regulator has opined | HIGH -- this is the central question |
| **Whether ZK proofs can satisfy Travel Rule obligations** | FATF has not issued guidance | HIGH |
| **FinCEN's implementing regulations for GENIUS Act BSA/AML** | Not yet issued; this rulemaking will define the specific requirements privacy systems must meet | HIGH |
| **Whether transparent blockchains violate GLBA/GDPR** | Never tested in enforcement | MEDIUM |
| **EDPB blockchain guidelines finalized** | Adopted for public consultation April 2025; final status unclear | MEDIUM |
| **Roman Storm appeal outcome** | Expected; could reshape developer liability | MEDIUM |
| **Whether the "tool vs. service" distinction holds** | Interpretive framework from legal analysts, not binding precedent | MEDIUM |
| **Hong Kong first stablecoin licenses** | Expected "early 2026" but specific date unverified | LOW |
| **UK stablecoin framework** | FCA consultation closed; final rules expected 2026 | LOW |

### Corrected Tornado Cash Timeline

- **August 2022:** OFAC designates Tornado Cash smart contract addresses under IEEPA.
- **May 2024:** Alexey Pertsev convicted in the Netherlands for money laundering (64-month sentence). This international dimension was omitted from the original regulatory brief.
- **November 26, 2024:** Fifth Circuit rules in *Van Loon v. Dep't of the Treasury* that immutable smart contracts are not "property" under IEEPA. OFAC exceeded its authority.
- **March 21, 2025:** OFAC officially delists Tornado Cash from the SDN list.
- **August 6, 2025:** Roman Storm mixed verdict -- guilty on unlicensed money transmitting conspiracy, deadlocked on money laundering and sanctions conspiracy.

The original regulatory brief conflated the November 2024 court ruling with the March 2025 administrative delisting into a single "March 2025" event. These are legally and chronologically distinct.

### Corrected FATF Statistics

- **Original claim:** "73% of countries (85 of 117 jurisdictions surveyed)" have Travel Rule legislation.
- **Corrected:** 85 of **163** jurisdictions surveyed (**52%**, not 73%). The 85 jurisdictions figure is correct; the denominator and percentage were wrong. Source: FATF's 2025 Targeted Update.

### The Genuine Open Question

No regulator anywhere in the world has formally endorsed zero-knowledge proof-based compliance as satisfying Travel Rule, BSA, or AML requirements. The technical architecture exists. Academic papers describe it. The IMF has published on it. Companies are building it (Payy, Midnight, Taurus). But the actual regulatory acceptance is theoretical.

This creates a catch-22: institutions will not adopt private stablecoin infrastructure without regulatory endorsement, and regulators will not endorse it without institutional adoption demonstrating it works. Breaking this deadlock requires either a regulatory sandbox success, a FinCEN rulemaking that explicitly accommodates ZKP compliance, or an institution willing to go first and establish precedent.

---

## 6. Enterprise Adoption Barriers (Verified)

### The 7 Specific CFO/Treasurer Objections

These are documented across industry analysis, enterprise surveys, and public statements from corporate treasury professionals.

| # | Objection | Privacy Solves It? | What Else Is Needed? |
|---|---|---|---|
| 1 | **On-chain transparency exposes competitive intelligence** (treasury balances, supplier relationships, payment terms, M&A signals) | YES -- this is the core use case for private stablecoin settlement | Nothing additional; ZK-based privacy directly addresses this |
| 2 | **Accounting treatment uncertainty** (how to classify stablecoin holdings; FASB ASU 2023-08 helped but questions remain for cash-equivalent treatment) | NO | Clear FASB/IASB guidance on stablecoin classification; auditor consensus |
| 3 | **Counterparty risk on stablecoin issuers** (what if Circle fails? SVB depeg event in March 2023 saw USDC trade as low as ~$0.87) | NO | FDIC-like insurance or resolution framework for stablecoin issuers; GENIUS Act partially addresses this through reserve requirements |
| 4 | **Travel Rule compliance gaps** (FATF requires originator/beneficiary data for transfers; infrastructure is fragmented) | PARTIALLY -- ZKP compliance could satisfy Travel Rule intent while preserving privacy, but this is unproven with regulators | Standardized Travel Rule infrastructure; regulatory acceptance of ZKP attestations |
| 5 | **KYC/AML on counterparty wallets** (who is responsible for KYC on the receiving wallet?) | PARTIALLY -- zkKYC proves regulatory eligibility without revealing identity, but not yet productized at scale | Clear regulatory guidance on wallet-to-wallet KYC obligations; productized zkKYC |
| 6 | **Custody and key management** (enterprise signing authority, key compromise, insurance) | NO | Enterprise custody solutions integrated with corporate governance; better insurance coverage |
| 7 | **ERP/TMS integration** (SAP, Oracle, Kyriba not designed for blockchain payments; reconciliation, approval workflows, audit trails) | NO | Integration middleware; enterprise blockchain adapters; standardized APIs |

### Net Assessment

Privacy infrastructure (specifically ZK-based confidential settlement) definitively solves the #1 barrier -- competitive intelligence exposure -- and partially addresses #4 and #5 through zkKYC and ZKP-based compliance attestations. However, barriers #2, #3, #6, and #7 require entirely separate infrastructure categories (accounting standards, issuer regulation, custody solutions, and enterprise software integration).

The claim that "privacy is the primary blocker" is defensible only if barrier #1 is treated as a hard prerequisite that gates everything else. The argument: even if you solve #2-7, enterprises still will not use transparent chains because of #1. Therefore, privacy is the first thing that must be solved, even though it is not the only thing. This is a reasonable analytical position but should be distinguished from the stronger claim that privacy alone unlocks enterprise adoption.

---

## 7. Strategic Questions Worth Addressing

The following questions emerge from synthesizing all four research areas and their audits. They are ordered roughly by importance and tractability.

### Regulatory Viability

1. **Can you be compliant AND private? What is the actual evidence?** Academic papers and IMF publications describe the architecture. Payy, Midnight, and Taurus are building it. But no deployment has been formally audited and accepted by a regulator. What is the path from "theoretically possible" to "regulatory sign-off"?

2. **What happens if regulators explicitly prohibit private stablecoin transactions?** The GENIUS Act does not address privacy. FinCEN's forthcoming BSA/AML rulemaking could either accommodate or foreclose ZKP-based compliance. What is the contingency plan if the rulemaking goes hostile?

3. **Is the regulatory window actually narrowing, or is that urgency framing?** The "narrow window" argument appears in multiple briefs. But the Tornado Cash ruling provides durable legal protection for privacy tools, and GENIUS Act implementation runs through January 2027. Is the urgency real or manufactured?

4. **How does the Pertsev conviction (Netherlands, 64 months) affect the "tool vs. service" distinction?** The U.S. Fifth Circuit said code is not property. A Dutch court convicted a developer for money laundering. The international inconsistency creates jurisdictional risk for any privacy protocol with global users.

### Market and Competitive

5. **Is Payy's "demand-first" approach actually defensible with $6M?** Building an L2 validium rollup requires significant engineering, security auditing, and ecosystem bootstrapping. Competitors have 20-40x more capital. Can Payy ship testnet and mainnet on $6M, or is a Series A required before the L2 is viable?

6. **Does Payy's consumer card traction translate to L2 demand?** The audit correctly flags that a Visa card for spending stablecoins and a privacy-preserving L2 rollup are different products serving different markets. How many of the 100K card users need or want on-chain private settlement?

7. **Why hasn't Railgun -- live, growing, Vitalik-endorsed, on existing EVM chains -- already won this market?** If privacy infrastructure is the primary blocker, and Railgun provides it today, why is enterprise adoption not already happening through Railgun? The answer may reveal the limits of the privacy-as-primary-blocker thesis.

8. **Is Aleo's non-EVM approach a fatal flaw or a competitive advantage?** Circle and Paxos chose Aleo for confidential stablecoins despite it not being EVM-compatible. Does the stablecoin liquidity argument (most supply is on Ethereum) override the issuer partnership argument?

### Technical

9. **Is "privacy as infrastructure" positioning or reality?** Privacy could be: (a) a base layer that all stablecoin activity must run on, (b) a feature that specific applications add as needed (Railgun model), or (c) a specialized product for specific verticals (payroll, treasury). Which of these is correct has massive implications for total addressable market and competitive dynamics.

10. **Can FHE catch up to ZK for payment use cases?** Zama's $150M+ and $1B valuation bet on FHE's long-term superiority. If FHE computational overhead drops by 10-100x in the next 3-5 years, it could leapfrog ZK for certain applications. How likely is this?

11. **What are the actual anonymity set sizes for each project?** No brief provided this data. For privacy protocols, the anonymity set (pool of users among whom a transaction hides) is a fundamental security metric. Namada's MASP shares one set across all assets. Railgun's depends on its TVL. Payy's depends on its (future) user base. Without this data, comparative privacy claims are unsubstantiated.

### Commercial

12. **How does the GENIUS Act yield prohibition create opportunities?** Stablecoin issuers cannot pay yield; bank deposit tokens can. This creates demand for (a) yield aggregation layers, (b) bank-issued deposit tokens like JPMD, and (c) DeFi yield products built on stablecoins. How does private settlement infrastructure interact with this yield gap?

13. **What is the realistic TAM for private stablecoin settlement?** The $390B in stablecoin payments is the upper bound. But not all of it needs privacy. How much of the $226B B2B segment is between parties that would pay for privacy? What is the fee that can be charged for private settlement, and does it support a venture-scale business?

14. **Is the Synapse/BaaS crisis actually an opening for on-chain accounts?** The briefs argue that the BaaS middleware collapse creates an opportunity for stablecoin-native accounts. But the regulatory response has been to tighten bank oversight, not embrace new models. Is the "opening" real?

15. **Who are the actual first customers for private stablecoin settlement?** Not "enterprises" in the abstract -- which specific companies or categories of companies would pay for this today? Crypto-native companies (exchanges, DeFi protocols, token treasuries) are the obvious early adopters. When do traditional enterprises become reachable?

### Existential

16. **What if TEE-based privacy (Tier 1) is "good enough" for most institutions?** If Tempo, Circle Arc, and RCM on Solana provide privacy that satisfies compliance teams (even with backdoors and hardware trust assumptions), the market for ZK-based self-sovereign privacy may be niche rather than foundational.

17. **What if stablecoins remain primarily a trading/DeFi instrument?** The $390B in payments is real but small relative to the $33T in total on-chain volume. What if the 99% (trading/DeFi) remains the dominant use case, and payment adoption stays at ~1% of volume? The privacy thesis depends on payment/enterprise adoption growing dramatically.

18. **What is the risk that a major privacy protocol is used for a high-profile illicit transaction, triggering a regulatory backlash?** Stablecoins already represent 84% of illicit crypto transaction volume (Chainalysis). The A7A5 ruble stablecoin processed $93B in sanctions evasion. If a privacy protocol becomes associated with a major illicit finance event, the entire "privacy is compliance" narrative could collapse.

---

## 8. Data Gaps & Unverified Claims

### Consolidated List Across All 4 Research Areas

**Cannot be independently verified:**

| Claim | Source | Why It Cannot Be Verified |
|---|---|---|
| Payy 100K users, $130M annualized volume, 120 countries | Company press coverage of seed round | Self-reported; no third-party analytics or on-chain data confirms |
| Payy sub-0.5s proof generation on iPhones | Aztec marketing blog | Single commercially interested source; no independent benchmark; no iPhone model, circuit complexity, or methodology specified |
| "2x proving speed improvement without optimization" (Payy/Noir) | Same Aztec blog | Same source concern |
| "95% of code auditable by non-Noir auditors" | Same Aztec blog | Qualitative claim with no methodology |
| Railgun 326 daily shields | Community analytics | Not verified against independent provider |
| PLONK "2.4s" proving time | No specific benchmark cited | Varies enormously by circuit size, hardware, implementation |
| Railgun $4.5B cumulative volume | Railgun community analytics, Messari | Privacy protocol limits independent verification by design |
| Specific corporate incidents of competitive harm from stablecoin transparency | Risk is well-documented in principle | No named incidents found; companies have no incentive to publicize |
| ERC-7984 adoption status | Exists on eips.ethereum.org | It is a Confidential Fungible Token standard (corrected from "confidential smart contract standard"), but adoption breadth unclear |
| PYUSD market cap (Q1 2026) | Not found | Multiple sources mention PYUSD without specific numbers |
| Conduit and Brale volumes | Not found | Stablecoin issuance-as-a-service platforms without public metrics |
| Wise stablecoin routing volume | Confirmed qualitatively | Volume not publicly disclosed |
| Bitso exact market share of US-Mexico corridor | Company-reported as "material share" | No independent verification |
| Total global stablecoin cross-border volume | No single authoritative source | Multiple sources with different methodologies |
| ZKP compliance performance claims (97% data reduction, 96.7% fraud accuracy, 28% cost reduction) | Academic papers (SSRN) | Lab results, not production; should not be cited without heavy caveats |

**Factual discrepancies not fully resolved:**

| Item | Discrepancy | Resolution |
|---|---|---|
| $33T vs $35T raw stablecoin volume (2025) | Bloomberg reports $33T; McKinsey reports $35T | Likely different data cutoffs or methodology; immaterial to ~1% payment ratio |
| Aztec total funding | Brief states $119M+; some sources report up to $178M including grants | $119M across 3 verified rounds; higher figures may include grants and earlier rounds |
| Payy's Protocol Labs relationship | Brief states "a team within Protocol Labs"; audit says this overstates the connection | Polybase Labs appears in Protocol Labs directory; Protocol Labs is an investor; not a subsidiary |
| "Ex-Apple engineer" attribution | Brief attributes to Calum Moore (CTO); some sources attribute Apple background to Sid Gandhi (CEO) | Needs primary source verification |
| Circle IPO valuation vs market cap | Brief tl;dr says "IPO'd at $32B market cap" | Circle IPO'd at ~$6.9B valuation (priced at $31/share, June 5, 2025); $31-32B is the March 2026 market cap after significant appreciation |

**Major structural gaps across all research:**

1. **No anonymity set analysis for any privacy project.** This is a fundamental privacy metric that none of the briefs addresses.
2. **No fee revenue or economic model analysis for privacy protocols.** TVL and volume are cited but not revenue, which determines economic sustainability.
3. **No analysis of FinCEN's forthcoming BSA/AML rulemaking** -- the single most consequential upcoming regulatory event for private stablecoins.
4. **No analysis of the Federal Reserve's role** in GENIUS Act implementation (alongside OCC and FDIC).
5. **Missing competitive entrants:** Secret Network (privacy L1, Cosmos), Mina Protocol (ZK-native L1), and Solana-native privacy solutions (Tempo, Arc, RCM) mentioned in passing but not analyzed.
6. **No discussion of Payy's token plans or network economic incentives.**
7. **Euro stablecoin consortium understated:** The Qivalis venture involves 9 banks from 8 countries (ING, UniCredit, KBC, DekaBank, Banca Sella, CaixaBank, Danske Bank, Raiffeisen Bank International, SEB), not 4 as originally reported.
8. **BENJI chain coverage outdated:** Franklin Templeton's FOBXX is now on 8+ chains (Ethereum, Solana, Base, Arbitrum, Avalanche, Aptos, BNB Chain, Stellar, Polygon), not just Stellar and Polygon.
9. **Mastercard/BVNK deal status:** Not yet closed as of March 2026 (pending regulatory approvals); original brief implies completion.

---

*Synthesis completed March 31, 2026. All figures use audit-corrected values. Claims marked as unverified or self-reported should be treated accordingly. This document is analytical research, not investment advice or marketing material.*
