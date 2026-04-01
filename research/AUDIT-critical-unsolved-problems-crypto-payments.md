---
title: "Audit Report: Critical Unsolved Problems in Crypto Payments"
date: 2026-03-31
auditor: "QA Supervisor"
report_audited: "research/critical-unsolved-problems-crypto-payments.md"
---

# Audit Report: Critical Unsolved Problems in Crypto Payments

*Audit Date: March 31, 2026*

---

## tl;dr

- **The report is well-structured and broadly accurate, but carries forward a known error (FATF denominator) that was already corrected in other reports in this same repository.** This is the most serious finding: the "85 of 117 FATF jurisdictions" claim uses an outdated denominator. The correct figure per the FATF's own 2025 Targeted Update is 85 of 163 jurisdictions (52%), not 117 (73%). This was identified and corrected in at least three other repository documents (stablecoin-privacy-payments-synthesis.md, audit-regulatory-landscape-private-stablecoins-2026.md, stablecoin-privacy-payments-report-march-2026.md). Repeating the wrong number in a later report is an internal consistency failure.
- **The Aztec status description is misleading.** The report describes Aztec's "Ignition Chain launched on Ethereum mainnet (Nov 2025)" without noting the critical vulnerability disclosed March 17, 2026, or that it was an alpha testnet (not a full mainnet with live transactions). Other repository documents correctly describe this as "alpha testnet" with transactions "expected to go live early 2026." The payments report makes Aztec sound more production-ready than it is.
- **Most quantitative claims check out or are reasonably sourced**, including fee structures, Stripe/Bridge details, FASB treatment, IRS classification, and the compliance tech market projection. The $1.9T Stripe figure, while not independently verifiable from the report's sources alone, is consistent with Stripe's public disclosures and widely cited.
- **Circle Arc description as "TEE-based stablecoin L1" is accurate but incomplete.** Circle's own announcement describes a "path to MPC and ZK proofs," and the report does note this. The characterization is defensible.
- **Overall grade: 3.5 out of 5.** Strong analytical framework with good source quality, undermined by one known-wrong statistic carried forward from earlier work and a misleading characterization of Aztec's status.

---

## Table of Contents

1. [Claim-by-Claim Fact Check](#1-claim-by-claim-fact-check)
2. [Internal Contradictions With Other Repository Reports](#2-internal-contradictions-with-other-repository-reports)
3. [Stale or Potentially Outdated Claims](#3-stale-or-potentially-outdated-claims)
4. [Gaps and Missing Context](#4-gaps-and-missing-context)
5. [Grades](#5-grades)
6. [Required Corrections](#6-required-corrections)

---

## 1. Claim-by-Claim Fact Check

### 1a. On/Off Ramp Fees: 1% Bank Transfer to 4.5% Credit Card

| Sub-Claim | Verdict | Notes |
|-----------|---------|-------|
| MoonPay credit/debit card fee of 4.5% | **ACCURATE** | MoonPay's public pricing disclosure lists 4.5% for card purchases with a minimum of approximately $3.99. Consistent with primary source. |
| MoonPay bank transfer fee of 1% | **ACCURATE** | Listed on MoonPay's fee support page. Minimum fee applies. |
| Transak fees ~1% | **ACCURATE** | Transak documentation shows variable fees starting around 1% depending on method and region. The "~" qualifier is appropriate. |
| Ramp Network 0.49-2.9% | **ACCURATE** | Ramp's FAQ confirms this range. |
| Overall "1% to 4.5%" range characterization | **ACCURATE** | This correctly represents the spectrum from cheapest bank transfer (MoonPay, Transak) to most expensive card purchase (MoonPay). |

**Source quality: 4/5.** Fees are sourced from provider websites (primary sources). The report appropriately notes fees vary by region and method.

---

### 1b. Stripe/Bridge OCC National Trust Bank Charter (Feb 2026)

**VERIFIED.** CoinDesk reported on February 17, 2026 that Bridge received initial approval of a national bank trust charter from the OCC. The report in the stablecoin market landscape document (`defi/stablecoin-market-landscape-march-2026.md`, line 166) confirms: "Received conditional approval for national trust bank charter (February 17, 2026)." The report correctly describes this as "conditional."

**Source quality: 4/5.** Primary source (CoinDesk), cross-referenced within the repository.

---

### 1c. Stripe Processed $1.9T in 2025

**PLAUSIBLE BUT NOT INDEPENDENTLY VERIFIED FROM CITED SOURCES.** The report cites this figure with "up 34% YoY." Stripe is a private company and does not file public financials. The $1.9T figure appears in the CoinDesk and insights4vc sources cited. Stripe's 2023 public statement was $1T+ in processing volume. A 34% YoY increase from a ~$1.4T 2024 base would reach approximately $1.9T, which is internally consistent.

The Citi projection in the repository's dex-projections-2026-2029.md references "$1.9 trillion" in a different context (stablecoin market projections), which is a coincidence but not a conflation.

**Source quality: 3/5.** Widely cited figure, directionally consistent with public statements, but ultimately sourced from press reporting on a private company's undisclosed financials.

---

### 1d. Travel Rule: "85 of 117 FATF Jurisdictions"

**INCORRECT DENOMINATOR -- KNOWN ERROR.** This is the most significant factual error in the report.

- The report states: "85 of 117 FATF jurisdictions have passed or are drafting legislation."
- The FATF's own 2025 Targeted Update on VA/VASPs surveyed **163 jurisdictions**, not 117.
- 85 of 163 = **52%**, not the implied 73%.
- The 85 jurisdictions figure is correct. Only the denominator is wrong.

**This error was already identified and corrected in multiple other documents in this repository:**
- `defi/stablecoin-privacy-payments-report-march-2026.md` (line 205): "FATF Travel Rule adoption is at 52% (85 of 163 jurisdictions surveyed), not the 73% figure commonly cited."
- `privacy/audit-regulatory-landscape-private-stablecoins-2026.md` (line 98): "The FATF's own 2025 Targeted Update reports 85 of 163 surveyed jurisdictions (52%), not 117."
- `defi/stablecoin-privacy-payments-synthesis.md` (line 65): "[Corrected per audit]"
- `research/crypto-payments-first-principles-full-stack-analysis.md` (line 486): Still uses the incorrect "85 of 117" -- this report also needs correction.

The source appears to be the InnReg Travel Rule Guide 2026, which itself may use an older FATF survey denominator (117 was the FATF membership count at a prior date). The FATF's actual survey covers both members and FATF-style regional body members, totaling 163.

**Source quality: 2/5.** Secondary source (InnReg blog) that contradicts the primary source (FATF's own publication). The primary source was already identified in prior audits but the correction was not applied here.

---

### 1e. Compliance Tech Market $2.14B in 2025, Projected $4.87B by 2028

**UNVERIFIABLE FROM CITED SOURCES.** The report does not cite a specific source for this market sizing. No source in the Sources section directly corresponds to this figure. Market sizing numbers like these typically come from reports by Grand View Research, Mordor Intelligence, or similar firms. Without a named source:

- The figures are plausible given the growth of blockchain analytics firms (Chainalysis raised at a $4.2B valuation; TRM Labs and Elliptic have raised significant rounds).
- The 31.5% implied CAGR (2025-2028) is reasonable for a high-growth compliance tech segment.
- But plausibility is not verification.

**Source quality: 2/5.** No primary source cited. The figures cannot be verified or challenged without knowing the original market research report.

---

### 1f. B2B Stablecoin Payments Grew from <$100M/month to >$3B/month

**PARTIALLY VERIFIABLE.** The stablecoin privacy payments report in this repository (line 54) shows B2B payments at $226B in 2025 (annual), which would be approximately $18.8B/month -- significantly higher than the ">$3B/month" claim in this report.

However, these may measure different things: the $226B figure from McKinsey/Artemis measures the subset of on-chain volume identified as real B2B payments, while the "$3B/month" figure may refer to stablecoin-specific B2B settlement volume through identifiable payment processors. Without the original source for the $3B figure, it is impossible to determine whether these are measuring the same thing.

The directional claim (massive growth from a small base) is consistent across all repository documents.

**Source quality: 2/5.** No specific source cited for the $100M or $3B figures. The growth narrative is directionally supported but the specific numbers are unattributed.

---

### 1g. FASB Excluded Stablecoins from ASU 2023-08

**VERIFIED.** ASU 2023-08, "Accounting for and Disclosure of Crypto Assets," effective December 2024, applies to crypto assets meeting specific criteria. Stablecoins are excluded because they are designed to maintain a stable value relative to a reference asset, which disqualifies them under the standard's definition of "crypto assets." The report cites Deloitte's FASB FAQ and Gordon Law, both of which confirm this exclusion.

The report's additional claim that FASB launched a study in late October 2025 on cash-equivalent treatment is supported by the Bloomberg Tax source cited.

**Source quality: 5/5.** Primary and high-quality secondary sources (FASB standard, Deloitte, Bloomberg Tax).

---

### 1h. IRS Treats Stablecoins as Property

**VERIFIED.** IRS Notice 2014-21 establishes that virtual currency is treated as property for federal tax purposes. Subsequent guidance (Rev. Rul. 2019-24, the Infrastructure Investment and Jobs Act reporting requirements, and IRS FAQ updates) has reinforced this position. Stablecoins are not exempted from this treatment.

The report correctly notes that even minor depeg events create reportable gains/losses, which is an accurate and underappreciated consequence of property treatment.

**Source quality: 5/5.** Primary regulatory source (IRS Notice).

---

### 1i. Aztec Mainnet Nov 2025

**MISLEADING.** The report states: "Ignition Chain launched on Ethereum mainnet (Nov 2025). 185+ operators across 5 continents, 3,400+ sequencers. Token sale completed Dec 2025. Transactions expected to go live early 2026."

The final sentence ("transactions expected to go live early 2026") is a critical qualifier that partially mitigates the misleading nature of the claim, but the framing is still problematic:

1. **Other repository documents describe this as an "alpha testnet"** (stablecoin-privacy-payments-report-march-2026.md, line 138), not a production mainnet.
2. **A critical vulnerability was disclosed on March 17, 2026** that could have enabled theft via the proving system. The fix is targeted for v5 in July 2026.
3. The report was dated March 31, 2026 -- two weeks after the vulnerability disclosure -- but makes no mention of it.

The CoinDesk and The Block sources cited do describe the November 2025 event as a "mainnet launch" and "Ignition Chain lights up on Ethereum," which is what the original sources reported. However, the report should have noted the March 2026 vulnerability, which was known at the time of writing.

**Source quality: 3/5.** Sources are real and accurately cited, but the report fails to include material subsequent information known at publication date.

---

### 1j. Circle Arc as "TEE-based stablecoin L1"

**ACCURATE WITH NUANCE.** The report describes Circle Arc as: "Purpose-built L1 for stablecoins using TEEs (Trusted Execution Environments) with path to MPC and ZK proofs." This matches Circle's own blog post introducing Arc. The "with path to MPC and ZK proofs" qualifier is important and present.

The report correctly describes the selective privacy model (amounts shielded, addresses visible) and the "view keys" approach for auditors/regulators.

The other repository document (stablecoin-privacy-payments-report-march-2026.md, line 129) places Arc in Zaki Manian's "Tier 1" TEE-based institutional privacy framework, which is consistent.

**Source quality: 4/5.** Primary source (Circle blog, Arc website).

---

### 1k. TRISA vs OpenVASP vs Notabene Messaging Protocols

**VERIFIED -- ALL THREE EXIST.**

- **TRISA** (Travel Rule Information Sharing Architecture): Open-source protocol. Confirmed via TRISA's website and documentation.
- **OpenVASP**: Open protocol for VASP-to-VASP communication. Confirmed via OpenVASP documentation.
- **Notabene**: Commercial Travel Rule messaging platform. Confirmed via Notabene's website and their 2025 State of Crypto Travel Rule Compliance Report.

The fragmentation claim (they "do not fully interoperate") is supported by the Notabene report and Chainalysis integration documentation. Chainalysis's blog explicitly discusses interoperability challenges.

**Source quality: 4/5.** Verified against primary sources (protocol documentation, compliance reports).

---

## 2. Internal Contradictions With Other Repository Reports

### 2a. FATF Denominator (CRITICAL)

| This Report | Other Reports | Correct Figure |
|-------------|---------------|----------------|
| 85 of 117 jurisdictions (73%) | 85 of 163 jurisdictions (52%) | **85 of 163 (52%)** per FATF 2025 Targeted Update |

The crypto-payments-first-principles-full-stack-analysis.md (line 486) also carries this error. Two reports in the research/ directory use the wrong denominator; at least three other repository documents have corrected it.

### 2b. Aztec Status

| This Report | Other Reports | Assessment |
|-------------|---------------|------------|
| "Ignition Chain launched on Ethereum mainnet (Nov 2025)" with transactions expected early 2026 | "Alpha testnet; TGE Feb 2026" (stablecoin-privacy-payments-report-march-2026.md); critical vulnerability March 17, 2026; v5 fix July 2026 | This report overstates Aztec's readiness and omits the vulnerability |

### 2c. B2B Payment Volume

| This Report | Other Reports | Assessment |
|-------------|---------------|------------|
| "<$100M/month to >$3B/month" | $226B annual = ~$18.8B/month (stablecoin-privacy-payments-report-march-2026.md) | Likely measuring different things, but the discrepancy (6x difference) deserves a note |

### 2d. No Contradictions Found On

- Fee structures (consistent or not mentioned elsewhere)
- FASB/IRS treatment (consistent across all reports)
- Stripe/Bridge charter details (consistent)
- Privacy protocol descriptions (consistent)
- Interoperability analysis (consistent)

---

## 3. Stale or Potentially Outdated Claims

| Claim | Risk | Notes |
|-------|------|-------|
| Deloitte "75% of US retailers plan to accept crypto by 2026" | **MEDIUM** | Sourced from a 2024 Deloitte survey. The report correctly attributes the date, but the projection year (2026) is now current. Actual adoption data should be available to test this claim. |
| MoonPay "30M+ customers" | **LOW** | Company-reported, likely from late 2025/early 2026. |
| Transak "$16M raise from Tether (Aug 2025)" | **LOW** | Dated but not stale for a March 2026 report. |
| BitPay supports "BUSD" | **HIGH** | Binance USD (BUSD) was ordered to stop minting by NYDFS in February 2023 and has been winding down. By March 2026, BUSD support is likely irrelevant or deprecated. This should be checked and likely removed. |
| Coinbase Commerce "merging into Coinbase Business by Mar 2026" | **MEDIUM** | This is the current month. The report should state whether this has occurred. |

---

## 4. Gaps and Missing Context

1. **No mention of MiCA Transfer of Funds Regulation**: The stablecoin privacy report (line 205) notes that MiCA's Transfer of Funds Regulation, effective December 30, 2024, requires full originator and beneficiary identification for all crypto transfers with no minimum threshold in the EU. This is a major development for the Travel Rule section that is absent from this report.

2. **No mention of Aztec's March 2026 vulnerability**: Known at publication date. Material omission for the privacy section.

3. **Compliance tech market figure is unsourced**: A $2.14B-to-$4.87B projection without a named research firm is an analytical red flag.

4. **B2B stablecoin payment figures are unsourced**: The <$100M to >$3B growth claim has no attribution.

5. **No discussion of Tether/USDT regulatory risk**: Tron carries 50%+ of USDT volume (correctly noted), but the implications for compliance and merchant acceptance of USDT specifically are underdeveloped.

6. **The Stripe $1.9T figure**: While widely reported, it would benefit from a caveat that Stripe is private and this is not audited financial data.

---

## 5. Grades

### Accuracy: 3.5 / 5

**Rationale**: The majority of claims are accurate or reasonably sourced. However, the FATF denominator error (already corrected elsewhere in the repository) is a significant failure of internal consistency. The Aztec characterization is misleading. The BUSD reference is stale. Two quantitative claims (compliance tech market size, B2B payment growth) lack source attribution.

### Completeness: 4 / 5

**Rationale**: The report covers seven distinct problem areas with genuine depth. The synthesis section correctly identifies interdependencies. The merchant acceptance and accounting/tax sections are particularly strong. The main completeness gap is the omission of MiCA's Transfer of Funds Regulation and the Aztec vulnerability.

### Source Quality: 3.5 / 5

**Rationale**: The source list is extensive (60+ sources) and includes primary sources (FASB, IRS, FATF, company documentation), reputable financial press (CoinDesk, The Block, Bloomberg Tax), and professional services analysis (Deloitte, Latham & Watkins). However, the FATF statistic is sourced from a secondary blog (InnReg) that contradicts the primary source. Two market-sizing claims have no source at all. The Stripe volume figure is press-reported for a private company.

### Overall: 3.5 / 5

**Rationale**: This is a strong analytical report that correctly identifies the interconnected nature of crypto payment challenges. The framework is sound, the analysis is substantive, and most claims are verifiable. It loses points for: (1) carrying forward a known-wrong FATF statistic that was corrected in other repository reports, (2) omitting the Aztec vulnerability known at publication date, (3) two unsourced quantitative claims, and (4) a stale BUSD reference. None of these are catastrophic errors, but in aggregate they indicate insufficient cross-referencing against the repository's own prior audit findings.

---

## 6. Required Corrections

### Priority 1 (Must Fix)

1. **FATF denominator**: Change "85 of 117 FATF jurisdictions" to "85 of 163 FATF-surveyed jurisdictions (52%)" in both the tl;dr bullet and Section 2. Source: FATF 2025 Targeted Update on VA/VASPs.

2. **Aztec status**: Add a note to the Aztec description in Section 7 mentioning the critical vulnerability disclosed March 17, 2026, and that v5 fix is targeted for July 2026. Clarify that Ignition Chain launched as an alpha testnet (operators and sequencers running, but transactions not yet live at publication date).

### Priority 2 (Should Fix)

3. **BUSD reference**: Remove or flag "BUSD" from the BitPay row in the merchant infrastructure table. BUSD has been winding down since February 2023.

4. **Compliance tech market source**: Add a citation for the $2.14B / $4.87B projection. If the source cannot be identified, add a qualifier: "according to industry estimates (source not independently verified)."

5. **B2B stablecoin payment growth source**: Add a citation for the <$100M/month to >$3B/month claim, or qualify it as an industry estimate.

6. **Add MiCA Transfer of Funds Regulation**: In Section 2, note that MiCA's regulation (effective December 30, 2024) eliminated minimum thresholds for Travel Rule data in the EU, making it the strictest major-market implementation.

### Priority 3 (Consider)

7. **Stripe $1.9T caveat**: Add a brief note that this is press-reported and not from audited financials, as Stripe is a private company.

8. **Coinbase Commerce status**: Verify whether the merge into Coinbase Business has occurred and update accordingly.

9. **Cross-reference the B2B volume figure** with the $226B annual figure from the stablecoin privacy payments report. If these measure different things, explain the distinction. If not, reconcile.

---

*This audit was conducted by cross-referencing the report against its cited sources, other documents in the ethereum-reports repository, and known primary sources (FATF, FASB, IRS). Claims that could not be independently verified from available sources are flagged as such. No data was fabricated in this audit.*
