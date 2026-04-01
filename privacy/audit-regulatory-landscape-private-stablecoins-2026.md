# Audit Report: Regulatory Landscape & Compliance for Private Stablecoins

**Auditor:** Independent QA Review
**Date:** 2026-03-31
**Subject:** Regulatory landscape research brief
**File under review:** `/Users/apriori/ethereum-reports/privacy/regulatory-landscape-private-stablecoins-2026.md`

---

## tl;dr

- **The brief is substantially accurate on its major factual claims**, with one significant dating error (Tornado Cash Fifth Circuit ruling) and several areas where presentation compresses nuance in ways that could mislead.
- **Source quality is strong.** The brief cites primary sources (Congress.gov, OCC, FDIC, EDPB, ESMA), reputable law firms, and credible media outlets. Payy-related claims appropriately caveat self-reported data.
- **The brief's weakest point is its treatment of regulatory risk.** It leans toward the optimistic interpretation of the "privacy is infrastructure" argument without adequately emphasizing how untested that framing remains with actual regulators.

---

## Table of Contents

1. [Fact-Check Results](#1-fact-check-results)
2. [Identified Gaps and Unsupported Claims](#2-identified-gaps-and-unsupported-claims)
3. [Overstated Clarity / Understated Risk](#3-overstated-clarity--understated-risk)
4. [Grading](#4-grading)

---

## 1. Fact-Check Results

### GENIUS Act signed July 18, 2025 by President Trump

**VERIFIED.** The White House fact sheet, SEC statement by Chairman Atkins (dated 07/18/25), Congress.gov records, and multiple law firm analyses all confirm the GENIUS Act was signed into law on July 18, 2025. The Senate passed it 68-30 on June 17, 2025; the House passed it 308-122 on July 17, 2025. The brief's date and description are accurate.

### OCC Bulletin 2026-3 issued February 25, 2026

**VERIFIED.** The OCC's own website confirms Bulletin 2026-3 was issued February 25, 2026, as a notice of proposed rulemaking to implement the GENIUS Act. The proposal is a 376-page document establishing a new 12 CFR Part 15. Comments due May 1, 2026. The brief's claim is accurate.

### FDIC proposed application procedures December 2025

**VERIFIED.** The FDIC Board approved the notice of proposed rulemaking on December 16, 2025, with the Federal Register publication on December 19, 2025. Comment period closed February 17, 2026. The brief's claim is accurate, though it does not specify the exact December date.

### Tornado Cash Fifth Circuit Reversal -- "March 2025"

**INACCURATE -- DATE IS WRONG.** This is the most significant factual error in the brief. The Fifth Circuit ruling occurred on **November 26, 2024**, not March 2025. The brief conflates two distinct events:

1. **November 26, 2024:** The Fifth Circuit issued its opinion in *Van Loon v. Dep't of the Treasury* (No. 23-50669), ruling that OFAC exceeded its authority by sanctioning Tornado Cash's immutable smart contracts.
2. **March 21, 2025:** OFAC officially delisted Tornado Cash from the SDN list, lifting sanctions.

The brief repeatedly refers to "the Tornado Cash sanctions reversal (March 2025)" as if the court ruling and the delisting were a single event. The court ruling was November 2024. The administrative delisting was March 2025. These are legally and chronologically distinct. The tl;dr bullet stating "The Tornado Cash sanctions reversal (March 2025)" is misleading because it implies the judicial ruling happened in March 2025. The body text on line 131 cites "The Tornado Cash Fifth Circuit ruled..." alongside "March 2025" sourcing, further blurring the timeline.

**Required correction:** The brief must separate the Fifth Circuit ruling (November 26, 2024) from the Treasury delisting (March 21, 2025) and use precise dates for each.

### Roman Storm conviction August 2025

**VERIFIED WITH MINOR NUANCE.** The mixed verdict was delivered on August 6, 2025. The brief's characterization is accurate:
- Guilty on conspiracy to operate an unlicensed money transmitting business
- Deadlocked on conspiracy to commit money laundering and conspiracy to violate sanctions
- Maximum sentence of 5 years on the guilty count

The brief correctly notes the appeal is expected. Additional context from IRS and CoinDesk coverage confirms Storm's attorneys filed a motion for acquittal in October 2025. The brief's framing of this as establishing the "tool vs. service" distinction is a fair interpretive summary, though calling the conviction one for "unlicensed money transmitting" (as in the tl;dr) slightly simplifies -- the actual charge was *conspiracy* to operate an unlicensed money transmitting business.

### MiCA fully in force with TFR enforcement from December 30, 2024

**VERIFIED.** ESMA, InnReg, and multiple sources confirm:
- MiCA stablecoin provisions (ARTs/EMTs) became applicable June 30, 2024
- Full CASP authorization and TFR requirements applied December 30, 2024
- Transitional periods vary by member state, with final deadlines up to July 1, 2026

The brief's characterization is accurate. The note about TFR lacking de minimis thresholds for many transaction types (unlike TradFi) is a fair observation supported by the Notabene and DLA Piper analyses.

### Hong Kong stablecoin framework took effect August 2025

**VERIFIED.** The Stablecoins Ordinance took effect on **August 1, 2025**, under HKMA oversight. The brief's details about licensing requirements, HK$25M paid-up capital, HK$3M liquid capital, and 12-month operating expense requirements are all confirmed by Sidley Austin and Davis Polk analyses. The HKMA received 77 expressions of interest by August 31, 2025.

The brief states "First stablecoin licenses expected around March 24, 2026." The HKMA indicated the first batch would be granted "in early 2026." The specific date of March 24 could not be independently verified to that level of precision, but the general timeframe is consistent.

### EDPB blockchain guidelines April 2025

**VERIFIED.** The EDPB adopted Guidelines 02/2025 on processing of personal data through blockchain technologies during its April 2025 plenary. The guidelines were published for public consultation with a deadline of June 9, 2025. The brief correctly notes these guidelines confirm GDPR applies fully to on-chain data. However, the brief should note these were adopted for **public consultation**, not as final guidelines. As of the research date, the final adopted version may or may not have been published.

### GENIUS Act prohibits issuers from paying yield/interest

**VERIFIED.** Multiple sources confirm the GENIUS Act prohibits issuers from paying yield or interest on stablecoins directly. The brief's statement is accurate. However, the brief does not mention the significant loophole: third-party platforms and affiliates can still offer yield on stablecoins, which has become a contentious issue. The OCC's proposed rulemaking (Bulletin 2026-3) proposes expanding the prohibition to affiliates and third parties. This gap is worth noting for completeness.

### Privacy provision: targeted advertising prohibition

**VERIFIED.** The enacted text of S.1582 confirms that nonpublic personal information obtained from stablecoin transaction data may not be used to target, personalize, or rank advertising, or be sold to third parties, without consumer consent. The brief's characterization is accurate.

### GLBA requiring financial institutions to protect customer data

**VERIFIED.** The GLBA/Reg P requirements described in the brief are accurate. The argument that transparent blockchains could violate the GLBA Safeguards Rule by exposing customer financial data is a legitimate legal interpretation, though it has not been tested in court or enforcement action.

### The "tool vs. service" legal distinction

**VERIFIED AS INTERPRETIVE FRAMEWORK.** The brief correctly characterizes the emerging legal distinction. The Fifth Circuit ruling (immutable code cannot be sanctioned as property) combined with the Storm conviction (operating a financial service without registration is prosecutable) does create the "tool vs. service" dichotomy the brief describes. Multiple law firm analyses (Mayer Brown, Kelman PLLC) use similar framing. This is a fair synthesis, not an overstatement.

### FATF Travel Rule statistics

**VERIFIED WITH DISCREPANCY.** The brief claims "73% of countries (85 of 117 jurisdictions surveyed)" have passed or are passing Travel Rule legislation as of January 2026. The FATF's own 2025 Targeted Update reports 85 of 163 surveyed jurisdictions (52%), not 117. The brief appears to use an older survey denominator. The 85 jurisdictions figure is correct; the denominator and percentage are wrong. **This needs correction.**

### SAB 121 rescission

**VERIFIED.** The brief mentions SAB 121 rescission in passing (line 291). SEC published SAB 122 rescinding SAB 121 on January 23, 2025. The brief's characterization as removing barriers to bank custody of crypto is accurate.

---

## 2. Identified Gaps and Unsupported Claims

### Gap 1: No discussion of FinCEN's role in GENIUS Act implementation

The brief focuses on OCC and FDIC rulemaking but omits FinCEN's role. The OCC's proposed rulemaking explicitly excludes BSA/AML regulations, which will be addressed in a separate rulemaking coordinated with Treasury/FinCEN. For a brief focused on privacy-preserving stablecoins, the FinCEN rulemaking is arguably the most consequential upcoming regulatory event, as it will define the specific AML/KYC requirements that privacy-preserving architectures must satisfy. This is a significant gap.

### Gap 2: No mention of state-level regulatory actions

The brief notes state money transmitter licenses in passing but does not discuss state-level regulatory developments around stablecoins, including New York's BitLicense framework and its implications for privacy-preserving systems, or whether GENIUS Act federal licensing preempts state requirements (it does in certain circumstances but not all).

### Gap 3: The yield prohibition loophole is omitted

As noted above, the GENIUS Act's yield prohibition applies to issuers but not third-party platforms. The OCC's proposed rulemaking would expand this prohibition. This is directly relevant to stablecoin market dynamics and competitive positioning.

### Gap 4: No discussion of Alexey Pertsev (Tornado Cash co-founder) conviction in the Netherlands

The brief discusses Roman Storm but omits the May 2024 conviction of Alexey Pertsev in the Netherlands for money laundering, which resulted in a 64-month prison sentence. This is relevant because it shows the international dimension of developer liability and the divergent legal outcomes across jurisdictions for the same protocol.

### Gap 5: The EDPB guidelines were in consultation, not finalized

The brief states "The EDPB's April 2025 Guidelines 02/2025 explicitly confirmed that blockchain technology receives no exemption from GDPR requirements" (line 315-316). While the guidelines were adopted for public consultation in April 2025, they may not have been finalized as of the brief's date. The brief should clarify the status.

### Gap 6: No discussion of the Federal Reserve's role

The brief mentions OCC and FDIC but omits the Federal Reserve, which also has supervisory authority over certain bank types under the GENIUS Act. The Fed's approach to privacy-preserving stablecoin activities could differ from OCC/FDIC.

### Gap 7: Payy traction numbers are unverified

The brief acknowledges this in Section 6 ("Key Uncertainties and Gaps"), which is good practice. However, the Payy comparison table in Section 2 presents "$130M annualized volume, 100K+ users, 120 countries" without the caveat. The caveat appears only much later. The table should include a footnote or qualifier.

---

## 3. Overstated Clarity / Understated Risk

### Issue 1: The "privacy is infrastructure" argument is presented more favorably than the evidence warrants

The brief builds a strong case that privacy may be legally required (GLBA, GDPR, BSA logic). However, it understates the reality that:
- No U.S. regulator has endorsed ZKP-based compliance as satisfying BSA requirements
- No jurisdiction has formally accepted ZKP proofs as Travel Rule compliant
- The FATF has not issued guidance on ZKP compliance
- The "transparent chains violate GLBA" argument has never been tested in enforcement

The brief does acknowledge some of these gaps in Section 6, but the body text (especially Section 5) reads as advocacy rather than neutral analysis. Lines 330-338 present five "framings that work with regulators" as if these are proven strategies, when they are largely theoretical positioning arguments.

### Issue 2: The Tornado Cash precedent is framed more definitively than warranted

The brief states the Fifth Circuit ruling established that "Anonymity-enabling technologies cannot be sanctioned solely because a minority of users misuse them" (line 133). This overstates the ruling's scope. The Fifth Circuit ruled narrowly that *immutable smart contracts* are not "property" under IEEPA. It did not broadly rule on anonymity-enabling technologies. Future sanctions against privacy protocols could use different legal theories or new legislation, as the brief itself notes but de-emphasizes.

### Issue 3: The "regulatory clarity" framing overstates the U.S. situation

Line 118 lists the U.S. as having "clear and operational" regulatory clarity with "(GENIUS Act enacted but implementing)." The parenthetical qualifier is important but gets lost. As of March 2026, the GENIUS Act is enacted but the implementing regulations are still in proposed rulemaking. No stablecoin issuer has yet been licensed under the new framework. Calling this "clear and operational" risks overstating the current state.

### Issue 4: Political risk is underweighted

The brief mentions the Trump administration's pro-crypto posture (Section 4) but does not adequately address the risk that:
- Administrations change; regulatory posture could reverse
- Congressional support could shift, especially if a stablecoin-related scandal occurs
- The "narrow window" framing implies urgency but not the fragility of the political alignment

### Issue 5: The ZKP compliance performance claims need stronger caveats

The brief flags the academic performance numbers (97% data reduction, 96.7% fraud accuracy, 28% cost reduction) as unverified (line 213), which is responsible. However, these numbers still appear in the main body of the brief and could be cited by downstream readers without the caveat. Consider moving them to a footnote or appendix, or adding inline "[unverified]" markers.

---

## 4. Grading

### Accuracy: 3.5 / 5

**Rationale:** The majority of factual claims are verified and accurate. However, the Tornado Cash dating error (conflating the November 2024 Fifth Circuit ruling with the March 2025 OFAC delisting) is a significant mistake for a legal analysis brief. The FATF Travel Rule denominator error (117 vs. 163 jurisdictions) is a smaller but still notable inaccuracy. These are not minor issues in a brief that may be used for compliance or investment decision-making.

Deductions:
- -0.5 for Tornado Cash date conflation
- -0.5 for FATF denominator error
- -0.5 for overstating the scope of the Fifth Circuit ruling

### Completeness: 4 / 5

**Rationale:** The brief covers an impressively wide range of regulatory developments across multiple jurisdictions. The global comparison, the Tornado Cash / Roman Storm analysis, the ZKP compliance architecture discussion, and the "privacy is infrastructure" legal argument are all substantive and well-structured. The brief also demonstrates good research discipline by flagging its own uncertainties in Section 6.

Deductions:
- -0.5 for omitting FinCEN's upcoming BSA/AML rulemaking (critical for the privacy question)
- -0.5 for omitting the Pertsev conviction and the Federal Reserve's role

### Source Quality: 4.5 / 5

**Rationale:** This is the brief's strongest dimension. Sources include primary government documents (Congress.gov, OCC bulletins, FDIC press releases, EDPB guidelines, ESMA), top-tier law firm analyses (Latham & Watkins, Gibson Dunn, Mayer Brown, Sullivan & Cromwell, Davis Polk), credible media (The Block, CoinDesk), and academic papers (SSRN, IACR). The brief appropriately distinguishes between verified facts and self-reported company claims (Payy).

Deduction:
- -0.5 for not noting the EDPB guidelines' consultation status (vs. finalized) at point of citation

---

## Summary of Required Corrections

| Priority | Item | Current Text | Correction Needed |
|----------|------|-------------|-------------------|
| **HIGH** | Tornado Cash timeline | "March 2025" used for court ruling | Separate: Fifth Circuit ruling was November 26, 2024; OFAC delisting was March 21, 2025 |
| **HIGH** | FATF Travel Rule stats | "73% of countries (85 of 117 jurisdictions)" | Denominator is 163, not 117; percentage is ~52%, not 73% |
| **MEDIUM** | Fifth Circuit scope | "Anonymity-enabling technologies cannot be sanctioned" | Narrow the characterization: ruling was specific to immutable smart contracts under IEEPA |
| **MEDIUM** | EDPB guidelines status | Presented as definitive | Note these were adopted for public consultation, not necessarily finalized |
| **MEDIUM** | FinCEN rulemaking | Not mentioned | Add discussion of upcoming BSA/AML implementing regulations |
| **LOW** | Yield prohibition | No mention of loophole | Note the third-party/affiliate loophole and OCC proposed expansion |
| **LOW** | Payy traction table | Numbers presented without caveat | Add qualifier that figures are self-reported |
| **LOW** | Pertsev conviction | Not mentioned | Add for international context on developer liability |

---

## Overall Assessment

This is a well-researched and well-structured brief that covers a complex regulatory landscape with appropriate breadth. The sourcing is strong, the analytical framework (particularly the "tool vs. service" distinction and the three-pillar privacy argument) is sound, and the brief demonstrates intellectual honesty by flagging its own uncertainties.

The primary weaknesses are: (1) the Tornado Cash dating error, which undermines credibility on the legal analysis that is central to the brief's thesis; (2) the tendency to present the "privacy is infrastructure" argument more favorably than the current regulatory reality warrants; and (3) the omission of FinCEN's role in GENIUS Act implementation, which is the single most consequential upcoming regulatory event for privacy-preserving stablecoins.

After corrections are applied, this brief would merit a higher accuracy score. The analytical quality and source rigor are above average for this type of research product.

---

*Audit conducted 2026-03-31. All fact-checks performed via web search against primary sources and reputable secondary sources.*
