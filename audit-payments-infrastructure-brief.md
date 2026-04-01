---
title: "Audit Report: Payments Infrastructure, Card Programs & Embedded Finance Brief"
date: 2026-03-31
auditor: "Independent Auditor -- Research Team 3"
---

# Audit Report: Payments Infrastructure, Card Programs & Embedded Finance Brief

**Subject:** `/Users/apriori/ethereum-reports/payments-infrastructure-card-programs-embedded-finance-brief.md`
**Date:** March 31, 2026

---

## tl;dr

- **The brief is substantively accurate on its core factual claims, with one significant error on Marqeta revenue and one understated figure on BlackRock BUIDL AUM.** Most sourced data points were confirmed through independent verification. The brief's own flagging of unverified figures is honest and appropriate.
- **The "80-90% cost advantage" claim for stablecoin cross-border payments is directionally supported but presented without sufficient nuance** -- third-party sources confirm "up to 80%" savings, but the range depends heavily on corridor, last-mile costs, and whether the comparison baseline is traditional remittance operators or cheaper digital alternatives like Wise.
- **No conflation errors were found between fee revenue and trading volume, or between other commonly confused metrics.** The brief is disciplined in distinguishing settlement mechanics from marketing claims.
- **Overall grade: 3.8 / 5** -- strong accuracy and analytical rigor, weakened by a few numeric errors and reliance on unverifiable forward-looking claims.

---

## Table of Contents

1. [Claim-by-Claim Verification](#1-claim-by-claim-verification)
2. [Identified Gaps and Unsupported Claims](#2-identified-gaps-and-unsupported-claims)
3. [Conflation Error Check](#3-conflation-error-check)
4. [Grading](#4-grading)

---

## 1. Claim-by-Claim Verification

### 1.1 Visa USDC Settlement Pilot (2023, Ethereum and Solana, Crypto.com and Worldpay)

**Verdict: CONFIRMED with minor correction needed.**

Visa's September 2023 press release confirms:
- Expansion of stablecoin settlement capabilities using Circle's USDC.
- Initial pilot with **Crypto.com** on Ethereum for cross-border settlement of their Australian card program.
- Expansion to **Solana** blockchain with merchant acquirers **Worldpay and Nuvei**.

**Correction:** The brief states the pilot partners were "Crypto.com, Worldpay." The actual expansion announcement named **Worldpay and Nuvei** as the new merchant acquirer partners. Crypto.com was the original issuer-side pilot partner. The brief omits Nuvei and slightly conflates the issuer-side and acquirer-side pilots into one statement. This is a minor inaccuracy but should be corrected for precision.

Sources: [Visa Press Release](https://usa.visa.com/about-visa/newsroom/press-releases.releaseId.19881.html), [Fortune](https://fortune.com/crypto/2023/09/05/visa-stablecoin-usdc-solana-worldpay-nuvei-merchants-payments/)

---

### 1.2 Marqeta Powering Coinbase Card, Cash App -- Revenue ~$700M+

**Verdict: CLIENT LIST CONFIRMED. Revenue figure is INCORRECT.**

- **Coinbase Card:** Confirmed. Marqeta's own case study and press materials confirm it powers the Coinbase Card in the U.S. using its Just-in-Time funding model.
- **Cash App (Block):** Confirmed. Block (parent of Cash App) was Marqeta's single largest customer, accounting for 45% of revenue in 2024.
- **Revenue ~$700M+:** **INCORRECT.** Marqeta reported full-year 2024 net revenue of **$507 million**, a 25% decrease year-over-year due to the Cash App contract renegotiation in July 2023 which changed how certain fees are netted against revenue. Even under the prior revenue presentation methodology, 2023 full-year revenue was approximately $675 million. The "$700M+" figure does not match either year and appears to be an overstatement.

**Required correction:** The revenue figure should be updated to $507M (2024 net revenue) or $675M (2023 net revenue under prior presentation), with a note about the Cash App contract impact.

Sources: [Marqeta Q4/FY2024 Results](https://investors.marqeta.com/news-releases/news-release-details/marqeta-reports-fourth-quarter-and-full-year-2024-financial/), [Marqeta-Coinbase Case Study](https://www.marqeta.com/blog/coinbase-case-study)

---

### 1.3 Galileo Acquired by SoFi for $1.2B in 2020

**Verdict: CONFIRMED.**

SoFi announced the definitive agreement to acquire Galileo Financial Technologies on April 7, 2020, for total consideration of $1.2 billion (comprising $75M cash, $250M seller financing debt, and $875M in company stock). Confirmed across CNBC, TechCrunch, Axios, and SoFi's own press release.

Sources: [CNBC](https://www.cnbc.com/2020/04/07/sofi-to-acquire-payment-software-company-galileo-for-1point2-billion.html), [TechCrunch](https://techcrunch.com/2020/04/07/another-major-fintech-exit-as-sofi-acquires-banking-and-payments-platform-galileo-for-1-2b/)

---

### 1.4 Synapse Collapse in 2024 and Its Impact on BaaS

**Verdict: CONFIRMED.**

Synapse Financial Technologies filed for Chapter 11 bankruptcy in April 2024. Key verified facts:
- Over 100,000 customers lost access to more than $265 million.
- Approximately $85 million in customer funds were trapped in reconciliation disputes between Synapse's ledger and partner banks (primarily Evolve Bank & Trust).
- The FDIC proposed a new rule (the "Synapse rule") in October 2024 requiring banks to maintain accurate beneficial ownership records in custodial accounts.
- The collapse triggered broad regulatory scrutiny of the BaaS middleware model.

The brief's characterization of the collapse and its regulatory fallout is accurate.

Sources: [CNBC - Synapse Bankruptcy](https://www.cnbc.com/2024/05/22/synapse-bankruptcy-customer-funds.html), [CNBC - FDIC Rule](https://www.cnbc.com/2024/09/17/fdic-banks-fintech-customer-data-synapse.html), [Consumer Federation of America](https://consumerfed.org/the-synapse-crisis-reveals-the-urgent-need-for-supervision-of-baas/)

---

### 1.5 Card Settlement Timing (T+1 to T+2 for Visa/Mastercard)

**Verdict: CONFIRMED (general range).**

Industry sources confirm that merchant settlement for card transactions typically occurs within 1-3 business days, with most standard domestic transactions settling in 1-2 business days. The brief's characterization of "T+1 to T+2" as the standard Visa/Mastercard settlement window is consistent with industry practice, though it should be noted the range can extend to T+3 in certain circumstances (cross-border, weekends/holidays).

Sources: [ClearlyPayments](https://www.clearlypayments.com/blog/how-long-do-credit-card-payments-take-to-settle/)

---

### 1.6 Interchange Fees (1.5-3.5% US, additional 1-3% cross-border)

**Verdict: PARTIALLY CONFIRMED -- range is reasonable but slightly oversimplified.**

- **US interchange fees:** Verified range is approximately 0.05% + $0.21 (regulated debit under Durbin Amendment) up to 3.15-3.30% + $0.10-$0.30 (premium rewards credit cards). The US average is approximately 2%. The brief's cited range of "1.5-3.5%" is reasonable for credit card transactions specifically but does not account for the lower end of debit card transactions.
- **Cross-border fees:** Visa charges approximately 0.80% for international dollar-denominated transactions, in addition to higher interchange rates for interregional transactions. The "additional 1-3%" cited in the brief is a reasonable range when combining the international assessment fee, higher interchange, and currency conversion markups. However, regulated interregional caps in some markets are lower (1.15% debit, 1.50% credit).

The figures are directionally correct and reasonable for the context of the argument being made, but could benefit from noting that debit interchange is significantly lower.

Sources: [Visa Interchange](https://usa.visa.com/support/small-business/regulations-fees.html), [Wikipedia - Interchange Fee](https://en.wikipedia.org/wiki/Interchange_fee)

---

### 1.7 Stripe Acquired Bridge for $1.1B

**Verdict: CONFIRMED.**

Stripe announced the acquisition of Bridge, a stablecoin orchestration platform, in October 2024 for $1.1 billion. The deal closed in February 2025. Confirmed as the largest crypto acquisition at the time of announcement. Bridge's prior valuation was approximately $200 million.

Sources: [Stripe Newsroom](https://stripe.com/newsroom/news/stripe-completes-bridge-acquisition), [CNBC](https://www.cnbc.com/2024/10/23/stripes-1point1-billion-deal-for-bridge-marks-much-needed-win-for-vc.html), [Fortune](https://fortune.com/crypto/2024/10/22/stripe-announces-1-1-billion-acquisition-of-stablecoin-start-up-bridge/)

---

### 1.8 BlackRock BUIDL Exceeded $1B AUM by Early 2026

**Verdict: CONFIRMED but SIGNIFICANTLY UNDERSTATED.**

BUIDL surpassed $1 billion in AUM in March 2025 -- not "early 2026." By the brief's own date (March 2026), BUIDL had reportedly reached approximately **$18 billion** in AUM across nine blockchain networks, according to February 2026 reporting. The brief's claim that BUIDL "exceeded $1 billion in AUM by early 2026" is technically true but dramatically understates the fund's growth. By early 2026, the fund was closer to $18 billion.

**Required correction:** The $1B milestone was reached approximately one year earlier than stated (March 2025, not early 2026). The current AUM as of the brief's date should be updated to reflect the actual figure, which appears to be in the $18B range based on available reporting.

Sources: [Securitize/BlackRock Press Release - $1B milestone](https://www.prnewswire.com/news-releases/blackrock-usd-institutional-digital-liquidity-fund-buidl-tokenized-by-securitize-surpasses-1b-in-aum-302401480.html), [BlockEden - $18B by Feb 2026](https://blockeden.xyz/blog/2026/02/24/blackrock-buidl-uniswap-defi-integration/), [The Defiant - $2B milestone](https://thedefiant.io/news/tradfi-and-fintech/blackrock-s-buidl-fund-hits-2-billion-aum-pays-record-4-17m-dividends-3-5m-on-295ae8a5)

---

### 1.9 Franklin Templeton BENJI on Stellar and Polygon

**Verdict: CONFIRMED but INCOMPLETE.**

Franklin Templeton's BENJI (FOBXX) is confirmed to operate on Stellar and Polygon, and launched publicly in 2021. However, by 2025-2026, the fund has expanded to additional blockchains including **Ethereum, Solana, Base, Arbitrum, Avalanche, Aptos, and BNB Chain**. The brief's statement is not wrong but is outdated -- BENJI is now a multi-chain product far beyond just Stellar and Polygon.

The total locked value is approximately $732 million, with nearly $480 million on Stellar.

Sources: [Franklin Templeton - Benji](https://digitalassets.franklintempleton.com/benji/), [RWA.xyz - BENJI](https://app.rwa.xyz/assets/BENJI), [Stellar.org](https://stellar.org/press/franklin-templeton-announces-the-franklin-onchain-u-s-government-money-fund-surpasses-270-million-in-assets-under-management)

---

### 1.10 Cross-Border Cost Comparisons (SWIFT vs Stablecoin vs Traditional Remittance)

**Verdict: CONFIRMED (directionally accurate).**

- **Traditional remittance 5-10%:** The World Bank's Q3 2024 data shows a global average of 6.62%, with Sub-Saharan Africa at 8.45%. The 5-10% range is confirmed.
- **World Bank ~6.2% for $200 (Q3 2024):** The actual Q3 2024 figure was **6.62%**, not 6.2%. Minor but notable inaccuracy.
- **SWIFT 2-5%:** Reasonable estimate given the compounding of fees through correspondent banking chains, though highly corridor-dependent.
- **Stablecoin 0.1-1%:** Confirmed as directionally correct for the onchain transfer component. The brief correctly notes that onramp/offramp costs add 0.5-2%.

**Required correction:** The World Bank figure should be updated from "approximately 6.2%" to 6.62% (Q3 2024).

Sources: [World Bank Remittance Prices Worldwide](https://remittanceprices.worldbank.org/), [Border Report - Mexico $63B](https://www.borderreport.com/news/trade/63-billion-in-remittances-sent-to-mexico-mostly-from-us-in-2023/)

---

### 1.11 The "80-90% Cost Advantage" Claim for Stablecoin Cross-Border Payments

**Verdict: PARTIALLY SUPPORTED -- high end of the range is aggressive.**

Third-party sources (Stripe, BVNK, Circle) confirm cost reductions of "up to 80%" compared to traditional remittance channels. This supports the lower bound of the brief's "80-90%" claim. However:

- The 80% figure applies when comparing stablecoin transfers to the most expensive traditional channels (Western Union/MoneyGram at 6-10%) in the most expensive corridors (Sub-Saharan Africa at 8.45%).
- In corridors where digital remittance services like Wise already operate (0.5-2%), the cost advantage narrows dramatically.
- The "90%" figure was not independently confirmed in any source reviewed.
- The brief's own cost comparison table shows stablecoin total costs (including onramp/offramp) at 0.3-1.5%, compared to traditional remittance at 5-10%. The maximum saving is approximately 97% (0.3% vs 10%) and the minimum is approximately 70% (1.5% vs 5%). This means the actual range should be stated as "70-97%" with heavy caveats, or more conservatively "up to 80%" as other sources state.

**Recommendation:** Revise to "up to 80% cost advantage over traditional remittance operators in high-cost corridors" with a note that the advantage is narrower in corridors served by digital-first competitors.

Sources: [Stripe - Stablecoin Cross-Border](https://stripe.com/resources/more/stablecoin-cross-border-payments), [BVNK](https://bvnk.com/blog/blockchain-cross-border-payments)

---

### 1.12 Securitize $47M Round Led by BlackRock (2024)

**Verdict: CONFIRMED.**

Securitize announced the completion of a $47 million strategic funding round led by BlackRock on May 1, 2024. Additional investors included Hamilton Lane, ParaFi Capital, Tradeweb Markets, Aptos Labs, Circle, and Paxos.

Sources: [Securitize Press Release](https://securitize.io/learn/press/securitize-announces-strategic-funding-round-led-by-blackrock), [CoinDesk](https://www.coindesk.com/business/2024/05/01/rwa-tokenization-firm-securitize-raises-47m-led-by-fund-partner-blackrock)

---

### 1.13 US-Mexico Remittance Corridor (~$63B in 2023)

**Verdict: CONFIRMED.**

Mexico received $63.31 billion in remittances in 2023 according to Bank of Mexico data, a 7.6% increase over 2022. Approximately 96% originated from the United States. This is confirmed as one of the world's largest remittance corridors.

Sources: [Border Report](https://www.borderreport.com/news/trade/63-billion-in-remittances-sent-to-mexico-mostly-from-us-in-2023/), [BBVA Research](https://www.bbvaresearch.com/en/publicaciones/mexico-remittances-accumulate-10-years-of-increase-and-break-record-633bn-in-2023/)

---

## 2. Identified Gaps and Unsupported Claims

### 2.1 Material Gaps

1. **BUIDL AUM is dramatically understated.** The brief claims ">$1B by early 2026" when the actual figure appears to be approximately $18B by February 2026. This is not a minor discrepancy -- it is an 18x understatement that weakens the brief's own argument about RWA tokenization reaching production scale.

2. **Marqeta revenue is overstated.** The "$700M+" figure does not match any verified annual figure. The 2024 net revenue was $507M. This should be corrected.

3. **No mention of USDC depeg magnitude.** The brief references the March 2023 SVB event and states USDC "briefly depegged" but does not quantify the depeg (USDC traded as low as ~$0.87). For a research brief focused on enterprise adoption barriers, the magnitude of the depeg is relevant context.

4. **Missing competitor context on BENJI.** The brief lists BENJI on Stellar and Polygon but does not reflect its expansion to 8+ chains. More importantly, it does not mention that BENJI's AUM (~$732M) is significantly smaller than BUIDL's, which affects the competitive landscape analysis.

5. **No discussion of USDT dominance in cross-border.** While the brief mentions USDT is "the dominant stablecoin for cross-border transfers," it does not quantify the USDC vs USDT market share split, which is essential context for understanding which stablecoin infrastructure matters most in the corridors discussed.

### 2.2 Unsupported or Unfalsifiable Claims

1. **"The protocol or platform that becomes the 'SWIFT of onchain RWA settlement' captures a structural position in the financial system."** This is a thesis statement, not a verifiable claim. It is appropriately framed as an argument rather than a fact, but it should be acknowledged that multiple prior crypto projects have claimed to be "the new SWIFT" without achieving that position.

2. **"US stablecoin legislation appears likely to pass in this Congressional session."** This is a prediction. As of March 2026, I cannot verify whether this has occurred. The brief should either cite specific legislative status or more clearly flag this as speculative.

3. **"The convergence of regulatory clarity, institutional adoption, privacy technology maturity, and infrastructure disruption creates a window."** This is a narrative thesis. While each component is individually supported, the convergence argument is the author's interpretation and cannot be independently verified.

### 2.3 Appropriately Flagged Uncertainties

To the brief's credit, the following items are explicitly flagged as unverified:
- Exact BUIDL, BENJI, and Ondo AUM figures (though BUIDL should have been verifiable)
- PayPal PYUSD market cap
- Bitso's market share of US-Mexico remittances
- Total global stablecoin cross-border volume
- BCG/McKinsey RWA projections
- Stablecoin legislation passage

This level of intellectual honesty is commendable and raises the overall credibility of the brief.

---

## 3. Conflation Error Check

### 3.1 Fee Revenue vs Trading Volume

**No conflation detected.** The brief is careful to distinguish between:
- Interchange fees (revenue extracted per transaction)
- Transaction volume (total value processed)
- Marqeta's revenue (platform fees) vs its TPV ($291B in 2024)

The brief does not substitute volume metrics for revenue metrics or vice versa.

### 3.2 Settlement vs Payment

**No conflation detected.** The brief makes a clear and correct distinction between the authorization flow (real-time) and the settlement flow (T+1 to T+2), and correctly identifies that crypto card programs settle in fiat, not crypto.

### 3.3 Onchain Transfer Volume vs Cross-Border Payment Volume

**Correctly handled.** The brief cites Circle's $197B onchain transfer volume but explicitly notes "Not all of this is cross-border" and declines to extrapolate a cross-border figure.

### 3.4 Stablecoin Gas Costs vs Total Transfer Costs

**Correctly handled.** The brief explicitly separates onchain gas fees (fractions of a cent) from total transfer costs including onramp/offramp (0.5-2% additional). This is a common conflation point that the brief avoids.

---

## 4. Grading

### Accuracy: 3.5 / 5

**Rationale:**
- 11 of 13 major claims verified as accurate or directionally correct.
- Two significant numeric errors: Marqeta revenue overstated (~$700M+ vs actual $507M), BlackRock BUIDL AUM dramatically understated ($1B vs ~$18B by early 2026).
- World Bank remittance cost slightly misstated (6.2% vs 6.62%).
- The "80-90% cost advantage" claim is at the aggressive end of supportable range.
- Visa pilot partner list is incomplete (omits Nuvei).
- The brief's self-flagging of unverified data points partially compensates, but the errors that do exist are in figures that were presented as verified.

### Completeness: 4.0 / 5

**Rationale:**
- Comprehensive coverage of the five major topic areas (card programs, embedded finance, cross-border, enterprise adoption barriers, RWA tokenization).
- Strong structural analysis of why crypto card settlement is architecturally fiat settlement.
- Thorough enumeration of enterprise adoption barriers with specific, concrete objections.
- Missing: quantified USDT vs USDC market share in cross-border corridors, updated BENJI multi-chain deployment, BUIDL growth trajectory, competitive landscape among RWA issuers beyond brief descriptions.
- The privacy thesis is well-argued but lacks concrete examples of production ZK-based settlement systems (e.g., current status of Aztec, Penumbra, or similar).

### Source Quality: 4.0 / 5

**Rationale:**
- Sources cited are credible and primary (Visa press releases, SEC filings, World Bank databases, corporate announcements).
- The brief explicitly lists verified sources and separately lists unverifiable claims -- a strong methodological practice.
- Weakness: Several claims cite "multiple sources" or "industry analysis" without specific attribution.
- The brief would benefit from direct links to SEC filings (Marqeta 10-K), RWA.xyz dashboards, and World Bank data tables rather than general source descriptions.
- No fabricated data detected. All numbers that could be verified traced back to real sources, even where the specific figures were slightly off.

### Overall: 3.8 / 5

**Summary:** This is a well-researched brief with strong analytical frameworks and honest uncertainty flagging. The two numeric errors (Marqeta revenue, BUIDL AUM) are the primary deficiencies. The BUIDL understatement is particularly notable because it actually undermines the brief's own thesis -- the RWA tokenization market has grown far faster than the brief suggests, which strengthens rather than weakens the argument being made. The Marqeta revenue overstatement is more concerning as it inflates the apparent scale of the card program manager market. Both should be corrected before this brief is used for product planning decisions.

---

## Appendix: Corrections Required

| # | Claim in Brief | Verified Figure | Severity |
|---|---------------|-----------------|----------|
| 1 | Marqeta revenue "~$700M+" (2024) | $507M net revenue (FY2024); ~$675M (FY2023 prior presentation) | High |
| 2 | BlackRock BUIDL "exceeded $1B by early 2026" | ~$18B by Feb 2026; $1B milestone reached March 2025 | High |
| 3 | World Bank remittance cost "approximately 6.2% as of Q3 2024" | 6.62% (Q3 2024) | Low |
| 4 | Visa pilot partners "Crypto.com, Worldpay" | Crypto.com (issuer pilot, Ethereum); Worldpay and Nuvei (acquirer pilot, Solana) | Low |
| 5 | BENJI "on Stellar and Polygon" | Now on 8+ chains including Ethereum, Solana, Base, Arbitrum, Avalanche, Aptos | Medium |
| 6 | "80-90% cost advantage" for stablecoin cross-border | "Up to 80%" is better supported; 90% not independently confirmed | Medium |

---

*Audit completed March 31, 2026*
*Auditor: Independent QA -- Research Team 3*
