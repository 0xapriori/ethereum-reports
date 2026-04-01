---
title: "Regulatory Landscape & Compliance for Private Stablecoins"
date: 2026-03-31
team: Research Team 4
---

# Regulatory Landscape & Compliance for Private Stablecoins

*Research Brief | March 31, 2026*

## tl;dr

- **The GENIUS Act, signed into law July 18, 2025, is the first comprehensive U.S. federal stablecoin framework.** It subjects issuers to the Bank Secrecy Act and requires 1:1 reserve backing, but includes consumer data privacy restrictions that prohibit using transaction data for targeted advertising. Full regulatory implementation is due by July 2026, with an effective date no later than January 2027.

- **The Tornado Cash sanctions reversal (March 2025) and Roman Storm's mixed verdict (August 2025) fundamentally reset the legal landscape for privacy protocols.** The Fifth Circuit ruled immutable smart contracts cannot be sanctioned as "property," but Storm was convicted for operating an unlicensed money transmitting business -- establishing that building privacy tools is not illegal, but operating them without registration may be.

- **Zero-knowledge proofs have emerged as the consensus technical solution for the privacy-compliance tension**, with academic research, IMF publications, and live deployments (Payy, Midnight, Taurus) all converging on "compliance-by-design" architectures that use ZKPs for selective disclosure -- proving regulatory eligibility without revealing identity.

- **GDPR, GLBA, and BSA collectively create a legal argument that financial privacy is not optional but required.** The EU's EDPB guidelines on blockchain (April 2025) confirm GDPR applies fully to on-chain data. The irony: transparent blockchains may violate existing privacy law, while privacy-preserving systems may be more compliant, not less.

- **The jurisdictional race is live.** Switzerland, Singapore, UAE, and Hong Kong all have functioning stablecoin frameworks. The UK is still finalizing rules (expected 2026). The GENIUS Act puts the U.S. in the game but implementation is incomplete. The window for privacy-preserving stablecoin infrastructure is open but narrowing as regulatory frameworks solidify.

---

## Table of Contents

1. [Stablecoin Regulation Status (Global)](#1-stablecoin-regulation-status-global)
2. [Privacy vs. Compliance Tension](#2-privacy-vs-compliance-tension)
3. [KYC/AML in Private Payment Systems](#3-kycaml-in-private-payment-systems)
4. [Institutional & Enterprise Compliance Requirements](#4-institutional--enterprise-compliance-requirements)
5. [The "Privacy is Infrastructure" Argument](#5-the-privacy-is-infrastructure-argument)
6. [Sources](#sources)

---

## 1. Stablecoin Regulation Status (Global)

### United States: The GENIUS Act

**Status:** Signed into law July 18, 2025 by President Trump.

**Key provisions:**
- Creates a federal definition of "payment stablecoins" and restricts issuance to regulated entities: banks, credit unions, and specially licensed non-bank issuers under OCC oversight
- Requires 1:1 reserve backing with U.S. dollars, deposits at insured depository institutions, or U.S. Treasury securities (93-day max maturity)
- Subjects all stablecoin issuers to the Bank Secrecy Act, requiring AML/KYC programs, sanctions list verification, and customer identification
- Requires monthly public disclosure of reserve composition
- Prohibits issuers from paying yield/interest on stablecoins
- **Privacy provision:** Prohibits issuers from using transaction data for targeted advertising or sharing with non-affiliates without consent
- **Consumer data protections** are explicit but limited compared to the compliance mandates

**Implementation timeline:**
- OCC notice of proposed rulemaking issued February 25, 2026 ([OCC Bulletin 2026-3](https://www.occ.treas.gov/news-issuances/bulletins/2026/bulletin-2026-3.html))
- FDIC proposed application procedures for supervised institutions (December 2025) ([FDIC](https://www.fdic.gov/news/press-releases/2025/fdic-approves-proposal-establish-genius-act-application-procedures-fdic))
- Final regulations required by July 18, 2026
- Effective date: earlier of 18 months post-enactment (January 18, 2027) or 120 days after final regulations

**The STABLE Act** was the House companion bill. It was effectively superseded by the GENIUS Act's passage. Both had similar goals but differed on details around state vs. federal jurisdiction and non-bank issuer requirements.

**Source:** [Congress.gov S.394](https://www.congress.gov/bill/119th-congress/senate-bill/394/text), [Latham & Watkins](https://www.lw.com/en/insights/the-genius-act-of-2025-stablecoin-legislation-adopted-in-the-us), [White House Fact Sheet](https://www.whitehouse.gov/fact-sheets/2025/07/fact-sheet-president-donald-j-trump-signs-genius-act-into-law/), [Gibson Dunn](https://www.gibsondunn.com/the-genius-act-a-new-era-of-stablecoin-regulation/)

### European Union: MiCA

**Status:** Fully in force. All transitional periods expiring by mid-2026.

- Stablecoin-specific rules (Asset-Referenced Tokens / ARTs and E-Money Tokens / EMTs) became applicable June 30, 2024
- Full CASP authorization requirements applied December 30, 2024
- Transfer of Funds Regulation (TFR) enforcement began December 30, 2024 -- **this is the EU's Travel Rule implementation**, requiring CASPs to exchange personal data of senders and recipients for all crypto transfers
- Transitional periods vary by member state: Netherlands (July 1, 2025), others up to July 1, 2026
- No further grace periods after national transitional deadlines

**Privacy implications:** MiCA's TFR implementation directly conflicts with privacy-preserving transactions. Critics argue it transforms crypto into a system with more surveillance than traditional banking, since TradFi has de minimis thresholds that MiCA's TFR lacks for many transaction types.

**Source:** [ESMA MiCA page](https://www.esma.europa.eu/esmas-activities/digital-finance-and-innovation/markets-crypto-assets-regulation-mica), [Sumsub MiCA Guide](https://sumsub.com/blog/crypto-regulations-in-the-european-union-markets-in-crypto-assets-mica/), [InnReg MiCA Guide 2026](https://www.innreg.com/blog/mica-regulation-guide)

### Hong Kong

- Stablecoin framework took effect August 2025 under the HKMA
- Any entity issuing, marketing, or distributing fiat-backed stablecoins to the public requires an HKMA license
- First stablecoin licenses expected around March 24, 2026
- Minimum financial requirements: HK$25M paid-up share capital, HK$3M liquid capital, 12 months operating expenses in excess liquid capital
- Sandbox program allows testing under supervision before full authorization

**Source:** [Sidley Austin](https://www.sidley.com/en/insights/newsupdates/2025/08/hong-kong-implements-new-regulatory-framework-for-stablecoins), [Sumsub HK Guide](https://sumsub.com/blog/hong-kong-stablecoin-regulation/)

### Singapore

- MAS Stablecoin Regulatory Framework in force since August 2023
- Covers single-currency stablecoins (SCS) pegged to SGD or G10 currencies
- Full reserve backing required
- One of the earliest jurisdictions with a clear framework, providing a template for others

**Source:** [BVNK Global Stablecoin Regulations 2026](https://bvnk.com/blog/global-stablecoin-regulations-2026)

### United Arab Emirates

- Central Bank of UAE (CBUAE) Payment Token Services Regulation (PTSR) effective August 2024
- Transitional compliance period ended June 2025
- VARA (Dubai) and ADGM (Abu Dhabi) provide additional regulatory frameworks
- Zero personal income tax on crypto
- Rapidly becoming an institutional hub for digital asset operations

**Source:** [BVNK](https://bvnk.com/blog/global-stablecoin-regulations-2026), [Fireblocks 2025-2026 Policy](https://www.fireblocks.com/blog/policy-changes-2025-outlook-2026)

### United Kingdom

- **Still in development.** No final rules yet.
- FCA Consultation Papers CP25/14 (issuance/custody) and CP25/15 (prudential regime) published May 2025
- Consultation closed end of July 2025
- Final rules expected sometime in 2026
- Framework will cover backing, redemption, issuer governance, and payment system supervision
- The UK is behind all major competitors in having a finalized stablecoin framework

**Source:** [Sumsub Crypto Regulation 2026](https://sumsub.com/blog/global-crypto-regulations/), [AML Network](https://amlnetwork.org/crypto-fintech/top-2026-crypto-regulations-mica-us-stablecoins-uk-fca-transform-markets/)

### What "Regulatory Clarity" Actually Looks Like in 2026

Regulatory clarity is no longer aspirational -- it exists, but unevenly:
- **Clear and operational:** Singapore, UAE, EU (MiCA), Hong Kong, U.S. (GENIUS Act enacted but implementing)
- **Drafted but not final:** UK
- The dominant pattern: regulated issuance (who can issue), reserve requirements (what backs it), and AML/KYC compliance (who can use it)
- The dominant gap: **none of these frameworks explicitly address transaction-level privacy or provide a framework for privacy-preserving compliance**

---

## 2. Privacy vs. Compliance Tension

### How Regulators View Privacy-Preserving Transactions

The regulatory stance is nuanced and jurisdiction-dependent, but the overall trajectory as of early 2026:

- **The Tornado Cash sanctions reversal (March 2025)** was a watershed. The Fifth Circuit ruled OFAC exceeded its authority by sanctioning immutable smart contracts under IEEPA, determining that autonomous code is not "property" that can be sanctioned. Treasury subsequently lifted the sanctions. ([CryptoSlate](https://cryptoslate.com/us-court-overturns-treasury-sanctions-on-tornado-cash/), [AInvest](https://www.ainvest.com/news/treasury-lifts-tornado-cash-sanctions-court-ruling-2503-1))

- **Key legal precedent established:** Anonymity-enabling technologies cannot be sanctioned solely because a minority of users misuse them. Regulators must demonstrate a direct link between a specific violation and the tool used. The court explicitly stated that "there is no person in control and therefore no party with which to contract." ([Bitcoin Policy Institute](https://www.btcpolicy.org/articles/tornado-cash-where-code-privacy-and-sanctions-collide), [Aurum Law](https://aurum.law/newsroom/Anonymity-Is-No-Longer-a-Crime-Analysis-of-the-Tornado-Cash-Case-Ruling))

- **The shift:** Regulators are now expected to target behavior rather than code. Future regulation of privacy protocols will likely require new legislation rather than reinterpretation of existing frameworks.

### Roman Storm Trial: The Developer Liability Question

- **August 6, 2025:** Jury delivered a mixed verdict
  - **Guilty:** Conspiracy to operate an unlicensed money transmitting business (up to 5 years)
  - **Deadlocked:** Conspiracy to commit money laundering AND conspiracy to violate sanctions
- The conviction did not require proving ties to illicit funds -- only that Tornado Cash operated as a money transmitter without registration
- **Appeal expected** on the question of whether someone can "operate" a system they designed to be beyond their operational control
- Defense argued building a neutral tool with lawful and unlawful uses (like a VPN or encrypted messaging) is not criminal

**Net precedent:** Building privacy technology is not sanctionable, but operating a financial service without registration is prosecutable -- even if you claim no operational control. The distinction between "tool" and "service" is now the critical legal frontier.

**Source:** [Mayer Brown](https://www.mayerbrown.com/en/insights/publications/2025/08/the-tornado-cash-trials-mixed-verdict-implications-for-developer-liability), [Merkle Science](https://www.merklescience.com/blog/what-tornado-cashs-sanctions-reversal-means-for-crypto-compliance), [Kelman PLLC](https://kelman.law/roman-storms-tornado-cash-verdict-what-it-means-for-crypto/)

### The FATF Travel Rule

- As of January 2026, **73% of countries (85 of 117 jurisdictions surveyed)** have passed or are passing Travel Rule legislation for virtual assets, up from 65 jurisdictions in 2024
- The Travel Rule requires VASPs to collect and share sender/recipient details for every crypto transfer
- FATF's 2025 update to Recommendation 16 further tightened requirements
- **Direct conflict with privacy:** The Travel Rule fundamentally assumes transaction transparency. Privacy-preserving transactions that hide sender/receiver identities appear prima facie non-compliant
- **The interoperability problem:** Different global standards and conflicting data privacy laws (especially GDPR) create operational challenges even for transparent systems

**Source:** [InnReg Travel Rule Guide 2026](https://www.innreg.com/blog/crypto-travel-rule-guide), [Sumsub Travel Rule Guide](https://sumsub.com/blog/what-is-the-fatf-travel-rule/), [21 Analytics](https://www.21analytics.co/travel-rule-regulations/united-states-travel-rule-regulation/)

### Can You Be Compliant AND Private?

**Yes, but only with specific technical architectures. The solutions:**

1. **Zero-Knowledge KYC (zkKYC):** Users prove they meet identity requirements (not sanctioned, correct jurisdiction, accredited investor status) without revealing identity. A ZKP proof is included with each transaction proving eligibility within the KYC perimeter.

2. **Selective Disclosure:** Users can prove specific attributes (over 18, not on OFAC list, resident of eligible jurisdiction) without revealing the underlying data. This satisfies the regulatory intent of the Travel Rule while preserving privacy.

3. **Compliance-by-Design Architectures:** As described in IMF and academic research, stablecoin systems can be designed where:
   - Regulators can verify aggregate compliance without seeing individual transactions
   - Law enforcement can obtain targeted disclosure via judicial process
   - Normal transactions remain private

4. **Tiered Privacy Models:** Different privacy levels based on transaction size/risk -- small transactions fully private, larger transactions require additional disclosure (analogous to CTR thresholds in TradFi)

**Source:** [IMF Stablecoin Balancing Act](https://www.imf.org/en/publications/fandd/issues/2025/09/the-stablecoin-balancing-act-darrell-duffie), [HAL Privatbank ZKP Stablecoin Paper](https://www.hal-privatbank.com/fileadmin/HAL/Downloads/Publikation/202212_How_to_design_a_compliant__privacy-preserving_fiat_stablecoin_via_zero-knowledge_proofs.pdf), [PARScoin Paper](https://eprint.iacr.org/2023/1908.pdf)

### How Payy Positions Differently from Tornado Cash

Based on publicly available information from Payy's seed announcement (March 2026, $6M led by FirstMark Capital):

| Dimension | Tornado Cash | Payy |
|---|---|---|
| **Structure** | Immutable smart contracts, no operator | Registered company, CEO (Sid Gandhi), corporate entity |
| **Regulatory posture** | No registration, no KYC, no compliance | Actively pursuing institutional compliance |
| **Technology** | Mixer model (break transaction graph) | ZK-proof L2 rollup (shield sender, receiver, amounts) |
| **Target user** | Any user seeking anonymity | Enterprises, financial institutions, fintech platforms |
| **Business model** | No revenue model | Visa card integration, enterprise payment rails |
| **Custody** | Non-custodial mixer | Self-custodial wallet with compliance layer |
| **Traction** | ~$7B+ in deposits before sanctions | ~$130M annualized volume, 100K+ users, 120 countries |

**Payy's core positioning:** Privacy is a feature for legitimate institutional use (protecting competitive intelligence, payroll confidentiality, trade settlement privacy), not a tool for anonymity. The challenge Payy itself acknowledges: "convincing financial institutions that privacy and compliance can coexist on the same payment network, a proposition that regulators have yet to formally endorse."

**Source:** [The Block](https://www.theblock.co/amp/post/395106/stablecoin-startup-payy-funding-private-transactions), [Protocol Labs](https://www.protocol.ai/blog/payy-and-privacy-for-stablecoins/), [AInvest](https://www.ainvest.com/news/payy-6m-seed-privacy-institutional-stablecoin-flows-2603/)

---

## 3. KYC/AML in Private Payment Systems

### How Privacy-Preserving Systems Handle KYC/AML

The emerging consensus architecture:

1. **On-ramp KYC:** Users complete traditional KYC once with an identity provider. Their verified status is converted into a cryptographic credential.
2. **ZKP attestation per transaction:** Each transaction includes a zero-knowledge proof that the sender (a) completed KYC, (b) is not on sanctions lists, (c) meets jurisdictional requirements -- without revealing who they are.
3. **Regulatory access via judicial process:** A "trapdoor" or "compliance key" mechanism allows authorized parties (with court orders) to decrypt specific transaction details.
4. **Ongoing screening:** The sanctions list check happens against updated lists, with proofs regenerated periodically.

**Performance claims from academic research (flag: these are from papers, not production systems):**
- ZKP-based KYC verification reportedly reduces exposed user data by 97%
- AI-enhanced ZKP fraud detection claims 96.7% accuracy vs. conventional rule-based AML
- ZKP-based compliance reportedly reduces compliance costs by 28%

**I cannot independently verify these specific performance numbers.** They come from academic papers ([SSRN](https://papers.ssrn.com/sol3/Delivery.cfm/5170329.pdf?abstractid=5170329&mirid=1)) and should be treated as theoretical/lab results, not production benchmarks.

### Which Jurisdictions Are Most Friendly to Privacy-Preserving FinTech?

**Tier 1 -- Most hospitable:**
- **Switzerland:** "Gold standard" for financial privacy culture. Principled regulation through SRO membership and FINMA licenses. Crypto Valley (Zug) ecosystem. Capital gains tax-free for individuals. Strong data protection laws.
- **Singapore:** Early, clear framework. MAS pragmatic approach. Privacy not hostile but compliance-first.
- **UAE (Dubai/Abu Dhabi):** VARA and ADGM frameworks. Zero income tax. Rapidly building institutional infrastructure. Less historical privacy culture but regulatory flexibility.

**Tier 2 -- Workable with effort:**
- **Hong Kong:** New framework live but untested on privacy questions. HKMA traditionally conservative.
- **Liechtenstein:** Blockchain Act (TVTG) provides token economy framework. Privacy culture similar to Switzerland.

**Tier 3 -- Challenging for privacy:**
- **EU (MiCA jurisdictions):** TFR requirements create direct tension with privacy. However, GDPR paradoxically supports the argument that on-chain transparency violates data protection.
- **UK:** Framework unfinished. Historically pragmatic but cautious.
- **US:** GENIUS Act does not address privacy. BSA requirements are extensive. However, the Tornado Cash ruling provides constitutional-level protection for privacy tools.

**Source:** [EscapeArtist 2026 Guide](https://www.escapeartist.com/blog/crypto-friendly-jurisdictions-asset-protection-2026/), [Crypto Legal Top 5](https://www.cryptolegal.uk/top-5-crypto-friendly-jurisdictions-in-2026/), [GLI Switzerland](https://www.globallegalinsights.com/practice-areas/blockchain-cryptocurrency-laws-and-regulations/switzerland/)

### Compliance Certifications That Matter

For a privacy-preserving stablecoin infrastructure provider targeting institutions:

- **SOC 2 Type II:** The baseline for any B2B fintech. Demonstrates controls over security, availability, processing integrity, confidentiality, and privacy. Institutions will not engage without it.
- **ISO 27001:** Information security management system certification. Increasingly expected globally, required in some jurisdictions.
- **PCI DSS:** If handling card payment data (relevant if offering Visa card integration like Payy).
- **State money transmitter licenses (U.S.):** Required in most states for money transmission. Under GENIUS Act, federal licensing may eventually preempt this.
- **Smart contract audits:** Not a "certification" per se, but institutions expect multiple audits from recognized firms (Trail of Bits, OpenZeppelin, Consensys Diligence, etc.).
- **Penetration testing:** Annual third-party pen tests are a minimum for institutional partnerships.

---

## 4. Institutional & Enterprise Compliance Requirements

### What Banks Need from Privacy/Compliance Perspective

For a bank to use private stablecoin rails, the following requirements must be met:

1. **Regulatory approval:** Under GENIUS Act, banks need explicit approval from their federal regulator (OCC, FDIC, or Fed) to engage with stablecoin activities
2. **BSA/AML compliance:** Full transaction monitoring, suspicious activity reporting (SARs), and currency transaction reporting (CTRs) capabilities. A privacy system must demonstrate it can support these requirements.
3. **Examiner access:** Bank examiners must be able to audit transaction histories. Complete privacy that prevents examiner review is a non-starter for any bank-integrated system.
4. **Vendor risk management:** OCC Bulletin 2013-29 and successor guidance require banks to conduct thorough due diligence on all third-party technology providers.
5. **Capital treatment clarity:** How do privacy-preserving stablecoin positions get treated for capital adequacy purposes? This is still undefined.
6. **Interoperability with existing compliance infrastructure:** Must integrate with existing KYC/AML systems, sanctions screening tools, and reporting frameworks.

### Tokenized Deposits vs. Stablecoins: Key Regulatory Differences

| Dimension | Tokenized Deposits | Stablecoins (under GENIUS Act) |
|---|---|---|
| **Legal nature** | Deposit claim on a bank (existing liability) | New payment instrument |
| **FDIC insurance** | Yes (up to $250K per depositor) | No |
| **Regulatory regime** | Existing banking regulation | New GENIUS Act framework |
| **Issuer** | Only insured depository institutions | Banks + licensed non-bank issuers |
| **Reserve requirements** | Bank capital adequacy (Basel framework) | 1:1 backing with specified assets |
| **Yield/interest** | Can pay interest | Prohibited under GENIUS Act |
| **Privacy** | Subject to GLBA, BSA | Subject to BSA, GENIUS Act data provisions |

**Key insight:** Tokenized deposits remain a bank product and inherit all existing banking privacy protections (GLBA) and compliance requirements. Stablecoins under GENIUS Act are a new category with their own rules. For privacy-preserving payments, tokenized deposits may actually offer a more established legal basis for confidentiality, since bank secrecy is well-established in TradFi.

### What Regulatory Shifts Are Creating Urgency Now?

1. **GENIUS Act implementation deadline (July 2026):** Institutions are scrambling to understand and prepare for the new framework. First-mover advantage for infrastructure providers that can demonstrate compliance.

2. **MiCA full enforcement (mid-2026):** EU CASPs without authorization must stop operating. Those remaining need compliant infrastructure.

3. **Hong Kong first licenses (March 2026):** A new market is opening with institutions seeking compliant stablecoin infrastructure.

4. **The FDIC/OCC posture shift:** The Trump administration's FDIC has explicitly stated it is "rightsizing regulation to promote American opportunity." The FDIC and OCC are actively building frameworks for bank participation in stablecoin activities, a 180-degree reversal from the 2022-2023 posture.

5. **Institutional demand for on-chain settlement:** Major financial institutions have publicly stated they will not move meaningful payment flows on-chain without privacy. This is not hypothetical -- Payy cites this as their founding insight.

### Political Landscape Impact

The Trump administration's pro-crypto posture has created a narrow but real window:
- GENIUS Act passage was directly enabled by executive branch support
- "Rightsizing regulation" rhetoric from FDIC and OCC
- SAB 121 rescission removed barriers to bank custody of crypto
- However, BSA/AML requirements are not being weakened -- the political support is for regulated innovation, not deregulation of compliance

**Source:** [FDIC Speeches 2026](https://www.fdic.gov/news/speeches/2026/update-prudential-regulators-rightsizing-regulation-promote-american-opportunity), [Cleary Gottlieb 2026 Update](https://www.clearygottlieb.com/news-and-insights/publication-listing/2026-digital-assets-regulatory-update-a-landmark-2025-but-more-developments-on-the-horizon), [Richmond Fed](https://www.richmondfed.org/banking/banker_resources/news_flash/2025/20251118_genius_act)

---

## 5. The "Privacy is Infrastructure" Argument

### Legal Basis for Privacy as a Requirement

The argument that financial transaction privacy is not a feature but a legal requirement has three pillars:

**Pillar 1: U.S. Federal Law Already Requires Financial Privacy**

- **Gramm-Leach-Bliley Act (GLBA/Reg P):** Requires financial institutions to (a) explain information-sharing practices, (b) safeguard sensitive data, (c) allow customers to opt out of third-party data sharing, (d) ensure third-party providers maintain adequate data protection. A public blockchain with transparent transactions would likely violate GLBA's Safeguards Rule if it exposes customer financial data to anyone who can read the chain.
- **Bank Secrecy Act (BSA):** While primarily focused on reporting requirements, BSA does not require that all transaction data be publicly visible. It requires reporting to FinCEN, not disclosure to the world. Public blockchain transparency exceeds BSA's requirements in ways that may conflict with other privacy obligations.

**Source:** [FTC GLBA](https://www.ftc.gov/business-guidance/privacy-security/gramm-leach-bliley-act), [FDIC GLBA Manual](https://www.fdic.gov/consumer-compliance-examination-manual/viii-1-gramm-leach-bliley-act-privacy-consumer-financial), [ABA Reg P](https://www.aba.com/banking-topics/compliance/acts/gramm-leach-bliley-act)

**Pillar 2: EU Data Protection Law (GDPR)**

- GDPR's data minimization principle requires collecting only necessary personal data
- GDPR's storage limitation mandates deletion when data is no longer necessary
- GDPR's right to erasure ("right to be forgotten") conflicts with blockchain immutability
- **The EDPB's April 2025 Guidelines 02/2025** explicitly confirmed that blockchain technology receives no exemption from GDPR requirements
- **The paradox:** A transparent blockchain where transaction data is visible to all participants may be inherently non-compliant with GDPR. A privacy-preserving blockchain using ZKPs may be the *more compliant* architecture.

**Source:** [EDPB Guidelines 02/2025](https://www.edpb.europa.eu/system/files/2025-04/edpb_guidelines_202502_blockchain_en.pdf), [CNIL Blockchain Guidance](https://www.cnil.fr/en/blockchain-and-gdpr-solutions-responsible-use-blockchain-context-personal-data), [Oxford Academic](https://academic.oup.com/cybersecurity/article/11/1/tyaf002/8024082)

**Pillar 3: Constitutional and Common Law Privacy Expectations**

- The Fifth Circuit's Tornado Cash ruling implicitly recognized that privacy in financial transactions has legal protection -- the government cannot simply ban tools that enable it
- Fourth Amendment protections against unreasonable search apply to financial records (though the third-party doctrine complicates this)
- State-level financial privacy laws (e.g., California Financial Information Privacy Act) provide additional protections

### How to Position Privacy as Pro-Compliance

The framing that works with regulators:

1. **"Privacy enables compliance, not evasion."** Without privacy, institutions cannot use on-chain systems because they would expose proprietary trading strategies, client information, and competitive intelligence. Privacy is the prerequisite for institutional compliance, not the obstacle to it.

2. **"We provide more to regulators than transparent chains do."** A well-designed privacy system with compliance proofs gives regulators mathematically verifiable compliance rather than raw transaction data that requires human interpretation. ZKP compliance proofs are more reliable than manual transaction monitoring.

3. **"Transparent chains violate existing law."** Under GLBA and GDPR, exposing customer financial data on a public ledger is arguably a violation. Privacy-preserving systems resolve this conflict.

4. **"We follow the TradFi model."** Banks have always operated with transaction privacy (your bank balance is not public). On-chain privacy simply restores the privacy expectations that have existed in finance for centuries. The anomaly is transparent blockchains, not private ones.

5. **"Selective disclosure gives regulators more control, not less."** Unlike TradFi where regulators must subpoena records and trust that they are complete, ZKP-based compliance proofs provide cryptographic guarantees that the underlying data meets regulatory requirements.

---

## Key Uncertainties and Gaps

The following items I could not verify or where the situation remains genuinely unresolved:

1. **GENIUS Act and privacy:** The Act does not explicitly address or authorize privacy-preserving transaction mechanisms. Whether ZKP-based compliance will be accepted under the GENIUS Act's BSA requirements has not been tested.

2. **Roman Storm sentencing:** As of my research date, sentencing has not occurred. The deadlocked counts may be retried. Appeal is expected. The final legal precedent is still forming.

3. **No jurisdiction has formally endorsed ZKP-based compliance** as satisfying Travel Rule or BSA requirements. The technical architecture exists and is being deployed, but regulatory sign-off is still informal/theoretical.

4. **Payy's compliance status:** Payy describes itself as seeking institutional compliance, but I found no evidence of specific regulatory approvals, licenses, or compliance certifications obtained. The claim of 100K+ users and $130M annualized volume comes from the company itself and I could not independently verify it.

5. **The FATF's stance on ZKP compliance:** FATF has not issued specific guidance on whether zero-knowledge proofs can satisfy Travel Rule requirements. This is perhaps the single most important outstanding regulatory question for the entire privacy-preserving stablecoin sector.

6. **Performance claims for ZKP compliance systems** (97% data reduction, 96.7% fraud detection accuracy, 28% cost reduction) come from academic papers and have not been validated in production environments at scale.

---

## Sources

### U.S. Regulation
- [Congress.gov - GENIUS Act S.394](https://www.congress.gov/bill/119th-congress/senate-bill/394/text)
- [Congress.gov - GENIUS Act S.1582](https://www.congress.gov/bill/119th-congress/senate-bill/1582/text)
- [OCC Bulletin 2026-3](https://www.occ.treas.gov/news-issuances/bulletins/2026/bulletin-2026-3.html)
- [Sullivan & Cromwell - GENIUS Act Implementation](https://www.sullcrom.com/insights/memo/2026/March/OCC-Proposes-Regulations-Implement-GENIUS-Act)
- [White House Fact Sheet](https://www.whitehouse.gov/fact-sheets/2025/07/fact-sheet-president-donald-j-trump-signs-genius-act-into-law/)
- [Latham & Watkins - GENIUS Act Overview](https://www.lw.com/en/insights/the-genius-act-of-2025-stablecoin-legislation-adopted-in-the-us)
- [Gibson Dunn - GENIUS Act Analysis](https://www.gibsondunn.com/the-genius-act-a-new-era-of-stablecoin-regulation/)
- [Arnold & Porter - Stablecoin Legislation](https://www.arnoldporter.com/en/perspectives/advisories/2025/07/new-stablecoin-legislation-analyzing-the-genius-act)
- [Richmond Fed - GENIUS Act Overview](https://www.richmondfed.org/banking/banker_resources/news_flash/2025/20251118_genius_act)
- [St. Louis Fed - Regulated Payment Stablecoins](https://www.stlouisfed.org/on-the-economy/2025/dec/regulated-payment-stablecoins-become-reality-us)
- [FDIC - GENIUS Act Procedures](https://www.fdic.gov/news/press-releases/2025/fdic-approves-proposal-establish-genius-act-application-procedures-fdic)
- [Cleary Gottlieb - 2026 Digital Assets Update](https://www.clearygottlieb.com/news-and-insights/publication-listing/2026-digital-assets-regulatory-update-a-landmark-2025-but-more-developments-on-the-horizon)

### EU / MiCA
- [ESMA - MiCA](https://www.esma.europa.eu/esmas-activities/digital-finance-and-innovation/markets-crypto-assets-regulation-mica)
- [InnReg - MiCA Guide 2026](https://www.innreg.com/blog/mica-regulation-guide)
- [Sumsub - EU Crypto Regulations](https://sumsub.com/blog/crypto-regulations-in-the-european-union-markets-in-crypto-assets-mica/)

### Global Jurisdictions
- [BVNK - Global Stablecoin Regulations 2026](https://bvnk.com/blog/global-stablecoin-regulations-2026)
- [Sidley Austin - Hong Kong Framework](https://www.sidley.com/en/insights/newsupdates/2025/08/hong-kong-implements-new-regulatory-framework-for-stablecoins)
- [Sumsub - Hong Kong Stablecoin Regulation](https://sumsub.com/blog/hong-kong-stablecoin-regulation/)
- [Fireblocks - 2025-2026 Policy Changes](https://www.fireblocks.com/blog/policy-changes-2025-outlook-2026)
- [Sumsub - Crypto Regulation 2026](https://sumsub.com/blog/global-crypto-regulations/)

### Tornado Cash / Privacy Precedent
- [CryptoSlate - Treasury Lifts Tornado Cash Sanctions](https://cryptoslate.com/us-court-overturns-treasury-sanctions-on-tornado-cash/)
- [Merkle Science - Sanctions Reversal Analysis](https://www.merklescience.com/blog/what-tornado-cashs-sanctions-reversal-means-for-crypto-compliance)
- [Bitcoin Policy Institute - Tornado Cash](https://www.btcpolicy.org/articles/tornado-cash-where-code-privacy-and-sanctions-collide)
- [Mayer Brown - Mixed Verdict Implications](https://www.mayerbrown.com/en/insights/publications/2025/08/the-tornado-cash-trials-mixed-verdict-implications-for-developer-liability)
- [Kelman PLLC - Storm Verdict Analysis](https://kelman.law/roman-storms-tornado-cash-verdict-what-it-means-for-crypto/)

### Travel Rule / FATF
- [InnReg - Crypto Travel Rule Guide 2026](https://www.innreg.com/blog/crypto-travel-rule-guide)
- [Sumsub - FATF Travel Rule Guide](https://sumsub.com/blog/what-is-the-fatf-travel-rule/)
- [21 Analytics - US Travel Rule](https://www.21analytics.co/travel-rule-regulations/united-states-travel-rule-regulation/)

### ZKP Compliance
- [IMF - The Stablecoin Balancing Act](https://www.imf.org/en/publications/fandd/issues/2025/09/the-stablecoin-balancing-act-darrell-duffie)
- [HAL Privatbank - Compliant Privacy-Preserving Stablecoin](https://www.hal-privatbank.com/fileadmin/HAL/Downloads/Publikation/202212_How_to_design_a_compliant__privacy-preserving_fiat_stablecoin_via_zero-knowledge_proofs.pdf)
- [PARScoin Paper](https://eprint.iacr.org/2023/1908.pdf)
- [SSRN - ZKP Compliance Models](https://papers.ssrn.com/sol3/Delivery.cfm/5170329.pdf?abstractid=5170329&mirid=1)
- [Chainlink - ZKP Data Privacy](https://chain.link/article/zk-proof-data-privacy)

### Privacy Law
- [EDPB Guidelines 02/2025](https://www.edpb.europa.eu/system/files/2025-04/edpb_guidelines_202502_blockchain_en.pdf)
- [CNIL - Blockchain and GDPR](https://www.cnil.fr/en/blockchain-and-gdpr-solutions-responsible-use-blockchain-context-personal-data)
- [FTC - GLBA](https://www.ftc.gov/business-guidance/privacy-security/gramm-leach-bliley-act)
- [FDIC - GLBA Manual](https://www.fdic.gov/consumer-compliance-examination-manual/viii-1-gramm-leach-bliley-act-privacy-consumer-financial)

### Payy
- [The Block - Payy $6M Seed](https://www.theblock.co/amp/post/395106/stablecoin-startup-payy-funding-private-transactions)
- [Protocol Labs - Payy and Privacy](https://www.protocol.ai/blog/payy-and-privacy-for-stablecoins/)
- [AInvest - Payy Institutional Flows](https://www.ainvest.com/news/payy-6m-seed-privacy-institutional-stablecoin-flows-2603/)

### Jurisdictions
- [EscapeArtist - 2026 Crypto Jurisdictions](https://www.escapeartist.com/blog/crypto-friendly-jurisdictions-asset-protection-2026/)
- [Crypto Legal - Top 5 Jurisdictions 2026](https://www.cryptolegal.uk/top-5-crypto-friendly-jurisdictions-in-2026/)
- [GLI - Switzerland Blockchain Laws 2026](https://www.globallegalinsights.com/practice-areas/blockchain-cryptocurrency-laws-and-regulations/switzerland/)
