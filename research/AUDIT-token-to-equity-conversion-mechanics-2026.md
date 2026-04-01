# AUDIT REPORT: Token-to-Equity Conversion Mechanics Research Brief

**Audit Date:** March 31, 2026
**Auditor:** QA Supervisor
**Document Under Review:** `/Users/apriori/ethereum-reports/research/token-to-equity-conversion-mechanics-2026.md`

---

## AUDIT SUMMARY

**Overall Assessment:** The research brief is substantially well-constructed with strong sourcing practices and, critically, honest flagging of data gaps. However, several specific numerical claims contain inaccuracies or overstatements that require correction.

| Category | Grade (1-5) | Notes |
|---|---|---|
| **Accuracy** | 3.5 / 5 | Most claims verified; several numerical errors on IPO data and one tax law date imprecision |
| **Completeness** | 4 / 5 | Thorough coverage; honest about unknowns; minor gap on Kraken revenue figure |
| **Source Quality** | 4.5 / 5 | Primary sources cited (SEC.gov, governance forums, investor relations pages); sources are real and verifiable URLs |

---

## CLAIM-BY-CLAIM VERIFICATION

### 1. Across Protocol "Bridge Across" Proposal Details

| Claim | Verdict | Detail |
|---|---|---|
| Proposal posted March 11, 2026 | **VERIFIED** | Confirmed by CoinDesk, Bankless, The Block, DL News |
| 1:1 token-to-share ratio | **VERIFIED** | Confirmed across multiple sources |
| $0.04375 USDC buyout at 25% premium | **VERIFIED** | Confirmed across multiple sources |
| SPV structure for smaller holders | **VERIFIED** | Confirmed; 250K-5M ACX tier uses SPV |
| 250,000 ACX minimum for SPV | **VERIFIED** | Confirmed |
| Hart Lambur quote about tokens hurting more than helping | **VERIFIED** | Confirmed via CoinDesk and forum post |

**Assessment:** All core proposal mechanics are accurate.

---

### 2. Snapshot Vote Status

| Claim | Verdict | Detail |
|---|---|---|
| Vote scheduled for March 26 or April 2 (sources diverge) | **VERIFIED** | The brief correctly notes source divergence |
| "Could not find confirmed public result" as of March 31 | **VERIFIED** | My own search also found no confirmed result |

**Assessment:** The brief handles this correctly and transparently. This is an example of good research practice -- flagging what is unknown rather than fabricating a result.

---

### 3. Paradigm's $41M Investment in Across

| Claim | Verdict | Detail |
|---|---|---|
| Paradigm led $41M funding round | **VERIFIED** | Confirmed by The Block, Yahoo Finance, CoinMarketCap |
| Completed in two phases in 2024 | **VERIFIED** | Q2 and Q4 2024 per The Block |
| Total raised $51M across two rounds | **VERIFIED** | $10M in Nov 2022 + $41M in 2024 |
| Participants: Bain Capital Crypto, Coinbase Ventures, Multicoin Capital | **VERIFIED** | Confirmed |

**Assessment:** Fully accurate.

---

### 4. Uniswap "UNIfication" Proposal

| Claim | Verdict | Detail |
|---|---|---|
| Passed December 25, 2025 | **VERIFIED** | Confirmed by Yahoo Finance, CoinDesk, AMBCrypto, Bitget |
| 99.9% support | **VERIFIED** | Confirmed; Hayden Adams himself cited this figure |
| 125M+ UNI in favor, 742 against | **VERIFIED** | Exact figure: 125,342,017 UNI in favor |
| Merged Foundation operations into Labs | **VERIFIED** | Confirmed |
| Activated protocol fees for UNI burns | **VERIFIED** | Confirmed |
| 100M UNI one-time burn | **VERIFIED** | Confirmed; entered 2-day timelock after vote |
| 20M UNI annual growth budget ("DUNI" agreement) | **PARTIALLY VERIFIED** | The DUNI agreement is referenced in governance sources but the specific 20M UNI figure was not independently verified in my search results |

**Assessment:** Substantially accurate. The December 25, 2025 date and 99.9% vote figure are confirmed.

---

### 5. dYdX Corporate Structure

| Claim | Verdict | Detail |
|---|---|---|
| dYdX Trading Inc. updated charter to Public Benefit Corporation | **VERIFIED** | Confirmed via dYdX blog and Antonio Juliano's public statement |
| Does not develop/control/participate in protocol operations | **VERIFIED** | Explicit commitment in PBC charter |
| All fees go to validators and stakers | **VERIFIED** | Confirmed for v4/dYdX Chain |
| dYdX Foundation handles governance | **VERIFIED** | Confirmed |

**Assessment:** Fully accurate.

---

### 6. MakerDAO/Sky Restructuring

| Claim | Verdict | Detail |
|---|---|---|
| Rebranded to Sky on August 27, 2024 | **VERIFIED** | Confirmed by CoinDesk, The Block |
| MKR to SKY at 1:24,000 ratio | **VERIFIED** | Confirmed across multiple sources |
| DAI to USDS at 1:1 | **VERIFIED** | Confirmed |
| Only ~14% MKR migrated to SKY as of early 2026 | **PLAUSIBLE** | Blockworks "One Year Into Sky" article discusses low adoption; specific 14% not independently verified but directionally consistent |
| Delayed Upgrade Penalty starting Sept 18, 2025 | **PLAUSIBLE** | Referenced in governance materials but not verified independently |

**Assessment:** Core facts accurate. Minor claims about migration percentage are plausible but not independently confirmed to the exact figure.

---

### 7. 2025 Crypto IPO Data -- ERRORS FOUND

This section contains the most significant accuracy issues in the report.

#### Circle IPO

| Claim in Report | Actual Data | Verdict |
|---|---|---|
| June 4, 2025 | Priced June 4, traded June 5 | **VERIFIED** |
| NYSE | NYSE | **VERIFIED** |
| $31/share IPO price | $31/share | **VERIFIED** |
| First-day close $83.23 (+168%) | Closed at ~$83, +167-168% | **VERIFIED** |
| $1.1B raised | ~$1.1B | **VERIFIED** |
| ~$28.6B valuation (Day 2) | Initial valuation was ~$6.2-6.9B at IPO; the stock surged significantly but $28.6B is a market cap figure from later trading, not a Day 2 figure that is standard to cite | **MISLEADING** -- the IPO valuation was $6.2-6.9B; citing $28.6B as "(Day 2)" conflates post-pop market cap with IPO valuation in a way that inflates the apparent size of the offering |

#### Bullish IPO -- MULTIPLE ERRORS

| Claim in Report | Actual Data | Verdict |
|---|---|---|
| August 2025 | August 13, 2025 | **VERIFIED** |
| $37/share | $37/share | **VERIFIED** |
| "+218% first day" | **INACCURATE** -- Bullish opened at $102, peaked intraday near $118 (218% above IPO price), but **closed at $68**, an 84% gain. The 218% figure reflects an intraday spike, not the first-day close. Multiple authoritative sources (Bloomberg, CNBC, Morningstar) report the first-day performance as +84% to close. The report presents 218% without qualifying it as an intraday high, which is misleading. |
| Amount raised: "--" (not stated) | $1.1B raised (30M shares at $37) | **INCOMPLETE** -- The report leaves this blank when the data was publicly available |
| Valuation: $4.8B | $4.82B at IPO pricing; some sources cite $5.4B | **APPROXIMATELY VERIFIED** |

#### Gemini IPO

| Claim in Report | Actual Data | Verdict |
|---|---|---|
| Sept 12, 2025 | September 11 (priced) / September 12 (traded) | **VERIFIED** |
| NASDAQ | NASDAQ | **VERIFIED** |
| $28/share | $28/share | **VERIFIED** |
| First-day close $32 (+14%) | Opened at $37.01, closed ~$32, +14.3% from IPO price | **VERIFIED** |
| $425M raised | $425M | **VERIFIED** |
| ~$3B valuation | ~$3.3B | **APPROXIMATELY VERIFIED** |

#### Figure IPO

| Claim in Report | Actual Data | Verdict |
|---|---|---|
| Sept 2025 | September 11, 2025 | **VERIFIED** |
| NASDAQ | NASDAQ Global Select Market | **VERIFIED** |
| $25/share | $25/share | **VERIFIED** |
| First-day close $32 (+30%) | Closed at $31.11, +24% per Bloomberg | **INACCURATE** -- The report claims +30% first-day and $32 close; Bloomberg reports $31.11 close and 24% gain. Fortune headline says "soars 30%" but this appears to reference an intraday high, not the close. |
| $787.5M raised | $787.5M | **VERIFIED** |
| ~$6B valuation | ~$5.3B per Alpha Spread, Ventureburn | **INACCURATE** -- Valuation was approximately $5.3B, not $6B |

#### Total IPO Raises

| Claim | Verdict | Detail |
|---|---|---|
| $3.4B raised across crypto IPOs in 2025 | **VERIFIED** | Confirmed by PitchBook, DL News, MoneyCheck, and multiple aggregators |

**Assessment of IPO Section:** The aggregate $3.4B figure is correct, and all four IPOs did happen. However, the Bullish "+218% first day" figure is misleading (that was the intraday peak, not the close), the Figure valuation is overstated by ~$700M, and the Figure first-day close is slightly overstated. The Circle "Day 2" valuation figure is misleading.

---

### 8. Kraken Details

| Claim | Verdict | Detail |
|---|---|---|
| Filed S-1 confidentially Nov 19, 2025 | **VERIFIED** | Confirmed |
| $20B valuation | **VERIFIED** | Based on $800M raise in Nov 2025 |
| $1.5B revenue (implied as current) | **OUTDATED/INACCURATE** -- $1.5B was 2024 revenue. By 2025, Kraken reported $2.2B in revenue (33% growth). The brief does not specify which year this revenue figure applies to, which creates ambiguity. | |
| Raised $800M in Nov 2025 | **VERIFIED** | Confirmed; Jane Street, DRW, Citadel Securities participated |
| IPO paused | **NOT MENTIONED** -- The brief says Kraken is "eyeing 2026 listings" but does not mention that Kraken has reportedly paused its IPO amid market conditions. This is a material omission. |

---

### 9. SEC March 17, 2026 Interpretive Release

| Claim | Verdict | Detail |
|---|---|---|
| March 17, 2026 date | **VERIFIED** | Confirmed via SEC.gov |
| Joint SEC/CFTC release | **VERIFIED** | Confirmed |
| Token taxonomy: digital commodities, collectibles, tools, stablecoins, digital securities | **VERIFIED** | Confirmed |
| 16 named tokens as digital commodities | **VERIFIED** | BTC, ETH, SOL, XRP, DOGE, ADA, AVAX, LINK, DOT, HBAR, LTC, BCH, SHIB, XLM, XTZ, APT |
| Chairman Atkins issued the release | **VERIFIED** | Confirmed |

**Assessment:** Fully accurate.

---

### 10. Atkins Token Safe Harbor Proposal

| Claim | Verdict | Detail |
|---|---|---|
| "Startup exemption" up to 4 years | **VERIFIED** | Referenced in SEC speech and multiple law firm analyses |
| Up to $5M with notices | **VERIFIED** | Confirmed in SEC materials |
| "Fundraising exemption" up to $75M | **VERIFIED** | Confirmed |
| "Investment contract safe harbor" | **VERIFIED** | Confirmed |

**Note:** The brief correctly characterizes these as *proposals* (not enacted rules). This is important -- they are proposed frameworks for future rulemaking, not current law. The brief's language is appropriately careful here.

---

### 11. IRS Section 1031 and Crypto

| Claim | Verdict | Detail |
|---|---|---|
| "Section 1031 like-kind exchange treatment has been unavailable for crypto since January 1, 2018" | **VERIFIED WITH NUANCE** | The Tax Cuts and Jobs Act was passed December 22, 2017, effective January 1, 2018. The report says "since January 1, 2018" which is correct as the effective date. However, the tl;dr says "since 2018" which is accurate. |
| Tax Cuts and Jobs Act of 2017 eliminated like-kind for personal property | **VERIFIED** | Confirmed via IRS.gov and multiple tax sources |
| IRS CCA 202124008 confirmed crypto-to-crypto does not qualify | **VERIFIED** | Confirmed via IRS published guidance (June 2021) |

**Note from the audit request:** The user asked whether the report correctly states 2018 vs 2017. The report says "since January 1, 2018" which is the **correct effective date**. The Tax Cuts and Jobs Act was enacted in 2017 but took effect January 1, 2018 for this provision. The report is accurate on this point.

---

### 12. Reg D 506(c) for SPV Structure

| Claim | Verdict | Detail |
|---|---|---|
| Conversion likely relies on Reg D exemption | **VERIFIED** as reasonable legal analysis | The accredited investor caps (100 US, 500 international) are consistent with Reg D |
| 506(c) "seems most applicable" given public nature of proposal | **REASONABLE** | 506(c) permits general solicitation with verification of accreditation; this is a sound legal inference given the public governance forum post |

**Assessment:** The legal analysis is appropriately hedged ("strongly suggests," "seems most applicable") and the reasoning is sound.

---

## ISSUES REQUIRING CORRECTION

### SEVERITY: HIGH

1. **Bullish "+218% first day" (Section 3.1, IPO table):** This is the intraday peak, not the first-day close. The actual first-day close was approximately +84% (closing at $68 from a $37 IPO price). Bloomberg, CNBC, and Morningstar all report ~84% as the first-day performance. The 218% intraday spike is a real number but presenting it alongside other companies' closing-day figures creates a misleading comparison. **Must be corrected to +84% close (with intraday high of +218% noted separately if desired).**

### SEVERITY: MEDIUM

2. **Figure valuation overstated (Section 3.1, IPO table):** Report claims ~$6B; actual sources indicate ~$5.3B. Difference of $700M.

3. **Figure first-day performance (Section 3.1, IPO table):** Report claims +30% and $32 close; Bloomberg data shows $31.11 close and +24.4% gain. The +30% appears to be an intraday figure from a Fortune headline.

4. **Kraken revenue figure (Section 3.1):** The brief mentions "$1.5B revenue" without specifying this was 2024 revenue. Full-year 2025 revenue was $2.2B. This should be updated or clarified.

5. **Kraken IPO pause not mentioned:** The brief states Kraken is "eyeing 2026 listings" but does not note that Kraken reportedly paused its IPO plans amid market conditions. This is a material omission for a research brief dated March 31, 2026.

6. **Circle valuation of "$28.6B (Day 2)" (Section 3.1, IPO table):** The IPO valuation was $6.2-6.9B. The $28.6B figure may reflect a post-surge market cap, but labeling it as the valuation in an IPO comparison table alongside other companies' IPO-day valuations is misleading.

### SEVERITY: LOW

7. **Bullish amount raised left blank in table:** The $1.1B figure was widely reported and should be included.

8. **Bullish exchange listed as "--" in table:** It traded on the NYSE, which should be stated.

---

## STRUCTURAL AND METHODOLOGICAL ASSESSMENT

### Strengths

- **Transparent about unknowns.** The brief explicitly flags unverified claims, data gaps, and areas of uncertainty. This is rare and commendable. The Snapshot vote handling is a model of honest research practice.
- **Source citations are real and verifiable.** Every major claim links to a specific, real URL. The sources include primary materials (SEC.gov, governance forums, investor relations pages) not just news aggregators.
- **Legal analysis is appropriately hedged.** The brief uses language like "almost certainly," "strongly suggests," and "seems most applicable" rather than stating legal conclusions as certainties.
- **Clear distinction between announced plans and completed actions.** The Across proposal is consistently described as a proposal/temp-check, not a fait accompli.
- **Structural spectrum table (Section 2.4)** is a genuinely useful analytical framework.
- **Section 5 (VC angle)** shows strong analytical thinking about incentive alignment.

### Weaknesses

- **IPO data table contains multiple small-to-medium errors** that undermine credibility of the quantitative claims.
- **No mention of Kraken IPO pause** despite the brief being dated March 31, 2026 and the pause being reported.
- **The tl;dr overstates Bullish performance** by citing 218% (intraday) alongside other companies' actual closing figures, creating a false comparison.

### Conflation / Overstatement Check

- **Announced plans vs completed actions:** The brief handles this well. The Across proposal is described as a proposal. The Snapshot vote is flagged as unconfirmed. No vote results are fabricated.
- **Claims about vote results that have not happened:** None found. The brief is appropriately uncertain about the Snapshot vote.
- **Unverified IPO claims:** All four 2025 IPOs (Circle, Bullish, Gemini, Figure) are verified as having actually occurred. No fabricated IPOs.
- **Legal analysis overstating certainty:** The legal analysis is well-hedged throughout. The Section 368 tax-free reorganization discussion appropriately notes it is an open question.

---

## FINAL GRADES

| Category | Grade | Justification |
|---|---|---|
| **Accuracy** | **3.5 / 5** | Core claims verified but IPO data table has multiple errors (Bullish 218% vs 84% close, Figure valuation $5.3B vs $6B, Kraken revenue outdated). These are not fabrications but they are inaccuracies in a data-driven research brief. |
| **Completeness** | **4 / 5** | Comprehensive coverage of the topic with honest flagging of gaps. Deducted for missing the Kraken IPO pause and not filling in available Bullish data. |
| **Source Quality** | **4.5 / 5** | Excellent sourcing practices. Primary sources used. URLs are real and verifiable. The brief cites SEC.gov, governance forums, and major financial publications. Minor deduction because some IPO figures appear sourced from headlines rather than investor relations filings. |

**Composite Score: 4.0 / 5**

---

## RECOMMENDED CORRECTIONS

1. Fix Bullish first-day figure from 218% to 84% (close), noting 218% as intraday peak
2. Fix Bullish exchange from "--" to "NYSE" and amount raised from "--" to "$1.1B"
3. Fix Figure valuation from ~$6B to ~$5.3B
4. Fix Figure first-day close from $32 (+30%) to $31.11 (+24%)
5. Fix Circle valuation column to show $6.9B (IPO) rather than $28.6B (post-pop market cap)
6. Update Kraken revenue to $2.2B (2025) or clarify $1.5B is 2024 figure
7. Add note about Kraken IPO reportedly being paused as of early 2026

---

*Audit compiled March 31, 2026. All verification performed via web search against primary and secondary sources.*

Sources:
- [CoinDesk: Across's ACX Rockets 80%](https://www.coindesk.com/markets/2026/03/12/across-s-acx-rockets-80-massively-beating-bitcoin-on-plans-to-dump-its-dao-structure)
- [The Block: Paradigm leads $41M round](https://www.theblock.co/post/344470/paradigm-leads-token-round-across-protocol-ethereum)
- [Yahoo Finance: Uniswap Governance Approves UNIfication](https://finance.yahoo.com/news/uniswap-governance-approves-unification-proposal-044254670.html)
- [Bloomberg: Bullish Soars 84% in Debut](https://www.bloomberg.com/news/articles/2025-08-13/crypto-firm-bullish-surges-143-in-debut-after-1-1-billion-ipo)
- [CNBC: Bullish Prices IPO at $37](https://www.cnbc.com/2025/08/13/crypto-exchange-bullish-prices-ipo-at-37-per-share-ahead-of-nyse-debut.html)
- [Bloomberg: Gemini IPO Raises $425M](https://www.bloomberg.com/news/articles/2025-09-11/winklevosses-crypto-exchange-gemini-raises-425-million-in-ipo)
- [Bloomberg: Figure Shares Rise 24% After $787.5M IPO](https://www.bloomberg.com/news/articles/2025-09-11/figure-indicated-to-open-36-higher-after-787-5-million-ipo)
- [Circle: Pricing of Upsized IPO](https://www.businesswire.com/news/home/20250604831596/en/Circle-Announces-Pricing-of-Upsized-Initial-Public-Offering)
- [SEC: Regulation Crypto Assets Token Safe Harbor](https://www.sec.gov/newsroom/speeches-statements/atkins-remarks-regulation-crypto-assets-031726)
- [SEC: Clarifies Application of Securities Laws to Crypto](https://www.sec.gov/newsroom/press-releases/2026-30-sec-clarifies-application-federal-securities-laws-crypto-assets)
- [Disruption Banking: SEC Token Taxonomy 16 Digital Commodities](https://www.disruptionbanking.com/2026/03/19/secs-token-taxonomy-is-official-16-crypto-assets-are-now-digital-commodities/)
- [Finance Magnates: Kraken 2025 Revenue $2.2B](https://www.financemagnates.com/cryptocurrency/krakens-2025-revenue-soared-to-22-billion-as-it-prepares-for-an-ipo/)
- [CoinCentral: Kraken Hits the Brakes on IPO](https://coincentral.com/kraken-hits-the-brakes-on-its-ipo-heres-why/)
- [IRS: Section 1031 Like-Kind Exchanges](https://www.irs.gov/businesses/small-businesses-self-employed/like-kind-exchanges-real-estate-tax-tips)
- [IRS: CCA 202124008 Bitcoin 1031](https://www.irs.gov/pub/irs-wd/202124008.pdf)
- [PitchBook: Crypto Companies Cracked IPO Ceiling](https://pitchbook.com/news/articles/crypto-companies-finally-cracked-the-ipo-ceiling-in-2025-and-2026-could-keep-pace)
- [dYdX: Public Benefit Corporation](https://www.dydx.xyz/blog/public-benefit-corporation)
- [CoinDesk: MakerDAO Is Now Sky](https://www.coindesk.com/business/2024/08/27/makerdao-is-now-sky-as-7b-crypto-lender-rolls-out-new-stablecoin-governance-token)
