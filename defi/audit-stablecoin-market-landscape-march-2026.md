# AUDIT REPORT: Stablecoin Market Landscape & Ecosystem Players

**Auditor:** Independent QA Supervisor
**Date:** March 31, 2026
**Subject file:** `/Users/apriori/ethereum-reports/defi/stablecoin-market-landscape-march-2026.md`

---

## tl;dr

- **Overall: strong research brief with a few numerical discrepancies and one notable attribution issue.** The report demonstrates rigorous sourcing, proper metric distinctions, and honest flagging of data gaps. It is above average for a DeFi market brief.
- **Two factual corrections required:** the raw transaction volume figure ($33T vs. $35T depending on source), and the USDT market cap is stale (was $187B in Q4 2025, closer to $184B by late March 2026). USDC market cap is also slightly understated ($75.7B vs. ~$77-78B by late March 2026).
- **One structural concern:** the euro stablecoin consortium is described as four banks (ING, UniCredit, KBC, DekaBank) when it is actually nine banks from eight countries.
- **The report correctly avoids conflating fee revenue with trading volume, and explicitly distinguishes raw volume from actual payments** -- this is commendable and consistent with CLAUDE.md instructions.

---

## Table of Contents

1. [Claim-by-Claim Verification](#1-claim-by-claim-verification)
2. [Identified Gaps and Unsupported Claims](#2-identified-gaps-and-unsupported-claims)
3. [Conflation Error Check](#3-conflation-error-check)
4. [Grading](#4-grading)

---

## 1. Claim-by-Claim Verification

### 1A. Stablecoin market cap ~$318B (early 2026)

**VERDICT: APPROXIMATELY CORRECT**

- The report states "~$318B in early 2026" and provides a table showing $317.94B on January 6, 2026 from MEXC News.
- Web verification: DefiLlama and Macquarie data show the market at approximately $310-315B in early 2026, crossing $313B as a fresh all-time high per DefiLlama. One source (news.bitcoin.com) reported "$310B record" at the start of 2026.
- By March 2026, the market had reached ~$320B+, which is consistent with the report's extrapolation.
- The $317.94B figure for January 6 is slightly high relative to the $310B consensus for "start of 2026" but within plausible range given daily fluctuations.

**Minor issue:** The report's table shows $300B for December 2025 "per Arkham Research" and $317.94B for January 6, 2026. Other sources report $306B at end of November 2025 (49% YoY growth). The $300B-$318B range is defensible.

Sources: [DefiLlama](https://defillama.com/stablecoins), [Macquarie via CoinDesk](https://www.coindesk.com/business/2026/03/10/stablecoins-are-starting-to-reshape-payments-and-banking-macquarie-says), [Bitcoin News](https://news.bitcoin.com/stablecoin-market-opens-2026-at-a-new-310b-record/)

---

### 1B. $33 trillion raw on-chain transaction volume in 2025 (72% YoY growth)

**VERDICT: CONFIRMED with caveat**

- Bloomberg and Yahoo Finance both confirm $33T in raw on-chain transaction volume for 2025, with 72% YoY growth, based on Artemis Analytics data.
- USDC accounted for $18.3T, USDT for $13.3T -- both figures confirmed.
- Q4 2025 alone was $11T -- confirmed.

**Caveat:** The McKinsey report (published later, in partnership with Artemis) references "$35 trillion" rather than "$33 trillion" as the raw volume figure. CoinDesk's coverage of the McKinsey report specifically states "stablecoins moved $35 trillion last year." This discrepancy ($33T vs. $35T) likely reflects either updated data or methodological differences between the Bloomberg/Artemis January report and the later McKinsey/Artemis analysis. The research brief uses $33T consistently (the Bloomberg figure) but attributes the $390B adjusted figure to the McKinsey report that uses $35T as its baseline. This creates a minor internal inconsistency: if using the McKinsey framework, the raw volume baseline should be $35T, making the "~1%" calculation slightly different.

Sources: [Bloomberg](https://www.bloomberg.com/news/articles/2026-01-08/stablecoin-transactions-rose-to-record-33-trillion-led-by-usdc), [Yahoo Finance](https://finance.yahoo.com/news/stablecoin-transactions-soared-72-2025-054951388.html), [CoinDesk on McKinsey](https://www.coindesk.com/business/2026/01/23/stablecoins-moved-usd35-trillion-last-year-but-only-1-of-it-was-for-real-world-payments)

---

### 1C. McKinsey/Artemis: only ~$390B (~1%) represents real-world payments

**VERDICT: CONFIRMED**

- McKinsey's own website confirms: "the true volume of stablecoin payments identified in the analysis, about $390 billion in 2025, has more than doubled from 2024 levels."
- The ~1% figure is confirmed across multiple outlets (CoinDesk, Crypto Valley Journal, CoinGeek, American Banker).
- B2B payments at $226B (58-60% of the $390B) confirmed.
- Asia-Pacific at $245B (60% of total payment volume) confirmed.

**Note on attribution:** The report correctly identifies this as "McKinsey/Artemis" research. The methodology uses Artemis Analytics data with McKinsey's analytical framework for filtering out non-payment transactions.

Sources: [McKinsey](https://www.mckinsey.com/industries/financial-services/our-insights/stablecoins-in-payments-what-the-raw-transaction-numbers-miss), [CoinDesk](https://www.coindesk.com/business/2026/01/23/stablecoins-moved-usd35-trillion-last-year-but-only-1-of-it-was-for-real-world-payments)

---

### 1D. B2B payments $226B, up 733% YoY

**VERDICT: CONFIRMED**

- The Defiant confirms: "B2B payments reached $226 billion, accounting for 58 percent of total stablecoin payment volume, with year-over-year growth of 733 percent."
- The report states B2B is "60% of real payment volume" -- the verified figure is 58%. This is a minor discrepancy (the report rounds up).

Sources: [The Defiant](https://thedefiant.io/news/infrastructure/b2b-stablecoin-payments-grew-over-730-percent-yoy-in-2025), [McKinsey](https://www.mckinsey.com/industries/financial-services/our-insights/stablecoins-in-payments-what-the-raw-transaction-numbers-miss)

---

### 1E. Stripe acquired Bridge for $1.1B

**VERDICT: CONFIRMED**

- CNBC, TechCrunch, Stripe's own newsroom, and a16z all confirm $1.1B acquisition closed February 4, 2025.
- Confirmed as Stripe's largest acquisition ever.
- Bridge was previously valued at $200M (Series A in 2024).

Sources: [CNBC](https://www.cnbc.com/2025/02/04/stripe-closes-1point1-billion-bridge-deal-prepares-for-stablecoin-push-.html), [Stripe Newsroom](https://stripe.com/newsroom/news/stripe-completes-bridge-acquisition)

---

### 1F. Mastercard acquired BVNK for $1.8B

**VERDICT: CONFIRMED with nuance**

- CNBC, Fortune, CoinDesk, Bloomberg, and Mastercard's own investor relations page all confirm "up to $1.8 billion."
- The "up to" qualifier is important: the deal includes $300 million in performance-contingent payments. The base price is approximately $1.5B with $300M in earnouts.
- The report correctly states "agreed to acquire BVNK for up to $1.8B" -- the "up to" language is present and accurate.
- Previous valuation of $750M+ confirmed.
- The deal had not yet closed as of March 2026 (pending regulatory approvals).

Sources: [CNBC](https://www.cnbc.com/2026/03/17/mastercard-acquiring-stablecoin-startup-bvnk-in-crypto-bet.html), [Fortune](https://fortune.com/2026/03/17/mastercard-bvnk-acquisition-stablecoins-1-8-billion/), [Mastercard IR](https://investor.mastercard.com/investor-news/investor-news-details/2026/Mastercard-to-Acquire-BVNK-to-Connect-On-Chain-Payments-and-Fiat-Rails/default.aspx)

---

### 1G. GENIUS Act enacted July 2025

**VERDICT: CONFIRMED**

- White House fact sheet confirms President Trump signed the GENIUS Act on July 18, 2025.
- Senate passed 68-30 on June 17, 2025; House passed 308-122 on July 17, 2025.
- Congress.gov, Latham & Watkins, Arnold & Porter, Gibson Dunn all confirm.
- The report's description of key requirements (1:1 reserve backing, prohibition on interest for stablecoin issuers, federal supervision via OCC) is consistent with verified sources.

Sources: [White House Fact Sheet](https://www.whitehouse.gov/fact-sheets/2025/07/fact-sheet-president-donald-j-trump-signs-genius-act-into-law/), [Latham & Watkins](https://www.lw.com/en/insights/the-genius-act-of-2025-stablecoin-legislation-adopted-in-the-us), [Congress.gov](https://www.congress.gov/bill/119th-congress/senate-bill/1582)

---

### 1H. Tether launching USAT for US compliance (January 2026)

**VERDICT: CONFIRMED**

- Fortune, Tether.io press release, Yahoo Finance, PYMNTS, CryptoNews all confirm USAT launched January 27, 2026.
- Issued by Anchorage Digital Bank, N.A. (OCC-regulated, federally chartered).
- Bo Hines appointed as CEO of Tether USA -- confirmed (previously White House Crypto Council Executive Director).
- Reserve custodian: Cantor Fitzgerald -- confirmed.
- GENIUS Act-compliant design -- confirmed.

Sources: [Fortune](https://fortune.com/2026/01/27/crypto-giant-tether-pushes-into-the-u-s-with-usat-stablecoin-to-challenge-circle/), [Tether.io](https://tether.io/news/tether-announces-the-launch-of-usat-the-federally-regulated-dollar-backed-stablecoin-made-in-america/)

---

### 1I. Circle IPO at $32B market cap

**VERDICT: REQUIRES CLARIFICATION**

- Circle IPO'd on NYSE June 5, 2025 (not June 4 as stated in the report -- off by one day, likely a time zone issue with pricing vs. trading).
- IPO priced at $31/share, raising ~$1.1B, with an initial IPO valuation of approximately $6.9B.
- Shares surged to $69-$100 on the first day of trading.
- As of March 2026, Circle's market cap is approximately $31.1B per Yahoo Finance/stock data.

**Issue:** The report states Circle "IPO'd at $32B market cap." This is misleading. Circle IPO'd at a ~$6.9B valuation. The $31-32B figure reflects the market cap as of March 2026 after significant share price appreciation post-IPO. The report's own text on line 131 says "Market cap ~$32B as of March 2026" which is more accurate, but the tl;dr (line 14) compresses this into "Circle IPO'd at $32B market cap" which conflates the current market cap with the IPO valuation.

Sources: [CoinDesk](https://www.coindesk.com/markets/2025/06/05/circle-shares-open-at-69-on-nyse-debut-signaling-strong-appetite-for-stablecoin-issuers), [CNBC](https://www.cnbc.com/2025/06/05/stablecoin-issuer-circle-soars-in-nyse-debut-after-pricing-ipo-above-expected-range.html), [Circle IR](https://investor.circle.com/news/news-details/2025/Circle-Announces-Pricing-of-Upsized-Initial-Public-Offering/default.aspx)

---

### 1J. JPMorgan JPMD live on Base

**VERDICT: CONFIRMED**

- CoinDesk, Decrypt, Ledger Insights, and JPMorgan's own PR confirm JPMD launched on Base (Coinbase L2) in November 2025.
- Expansion to Canton Network announced January 2026 (phased rollout throughout 2026) -- confirmed.
- Described as a deposit token (not a stablecoin) -- confirmed. The report correctly flags this distinction on line 214.

Sources: [CoinDesk](https://www.coindesk.com/tech/2026/01/07/jpmorgan-to-issue-its-jpm-stablecoin-directly-on-privacy-focused-canton-network), [PRNewswire/Digital Asset](https://www.prnewswire.com/news-releases/digital-asset-and-kinexys-by-jp-morgan-announce-intention-to-bring-usd-jpm-coin-jpmd-natively-to-the-canton-network-302654967.html)

---

### 1K. ING/UniCredit/KBC/DekaBank joint euro stablecoin

**VERDICT: PARTIALLY CORRECT -- INCOMPLETE**

- The joint euro stablecoin venture is confirmed, but it involves NINE banks from eight countries, not four. The full list is: Banca Sella, CaixaBank, Danske Bank, DekaBank, ING, KBC, Raiffeisen Bank International, SEB, and UniCredit.
- The company is named Qivalis, domiciled in Amsterdam (Netherlands) -- confirmed.
- MiCA-compliant, targeting second half of 2026 launch -- confirmed.
- The report only names four of the nine banks, which significantly understates the scope of the consortium.

Sources: [Bloomberg](https://www.bloomberg.com/news/articles/2025-09-25/unicredit-ing-among-nine-lenders-developing-euro-stablecoin), [Ledger Insights](https://www.ledgerinsights.com/nine-european-banks-to-launch-joint-euro-stablecoin-in-2026/), [ING](https://www.ingwb.com/en/insights/news/nine-major-european-banks-join-forces-to-issue-stablecoin)

---

### 1L. USDT $187B market cap, USDC $75.7B

**VERDICT: STALE DATA**

- **USDT:** Hit $187B in Q4 2025, but by late March 2026 had declined to approximately $184B following significant burns ($6.5B burned in early 2026). The $187B figure was accurate for "early 2026" but not for "as of March 2026."
- **USDC:** The report states $75.7B. By March 11, 2026, USDC had hit $78.25B (new ATH). As of late March 2026, USDC was approximately $77.3B. The $75.7B figure is ~2-3% understated for March 2026.

**Recommendation:** The market cap table should specify the exact date of the snapshot. The stablecoin supply figures are moving targets and the report should note whether these are point-in-time or approximate.

Sources: [CoinMarketCap - USDT](https://coinmarketcap.com/currencies/tether/), [The Coin Republic](https://www.thecoinrepublic.com/2026/03/11/stablecoin-news-usdc-marketcap-tops-78-billion-after-circles-600-million-mint-this-week/), [Cryptonomist](https://en.cryptonomist.ch/2026/02/05/tether-usdt-market-cap-187b/)

---

### 1M. Treasury Secretary Bessent projecting $3T supply by 2030

**VERDICT: CONFIRMED**

- Yahoo Finance, Decrypt, Politico Pro all confirm Bessent projected stablecoins could reach $3T by 2030.
- He later raised this from an earlier $2T projection (a 50% increase in his estimate).
- Treasury statement confirmed on treasury.gov.
- The report correctly attributes this as a projection, not a certainty.

**Additional context not in report:** Bessent also noted this would generate $114B in annual government savings through Treasury bill demand. JPMorgan's own estimate is more conservative at up to $700B, while Citigroup's bull case is $4T.

Sources: [Yahoo Finance](https://finance.yahoo.com/news/us-treasury-secretary-lifts-stablecoin-122120307.html), [Treasury.gov](https://home.treasury.gov/news/press-releases/sb0197)

---

### 1N. Bloomberg Intelligence projecting $56T in payment flows by 2030

**VERDICT: CONFIRMED**

- CoinTelegraph, Yahoo Finance, CoinMarketCap, FinanceFeeds all confirm Bloomberg Intelligence's projection of $56.6T in stablecoin payment flows by 2030.
- Based on approximately 80% compound annual growth from 2025's $2.9T in total flows.

**Note:** The report says "$56T in payment flows" while the Bloomberg Intelligence figure is specifically $56.6T. Minor rounding.

Sources: [CoinTelegraph](https://cointelegraph.com/news/stablecoin-payment-flows-hit-56-trillion-2030), [Yahoo Finance](https://finance.yahoo.com/news/stablecoins-hit-56t-valuation-2030-090407498.html)

---

## 2. Identified Gaps and Unsupported Claims

### Gaps the Report Acknowledges (Commendable Transparency)

The report explicitly flags the following data gaps in Section 8 ("Data Limitations & Flags"):
- PYUSD market cap not confirmed
- Conduit and Brale volume data not found
- Wise stablecoin routing volume not disclosed
- Solana volume share range is wide (37-74%)
- "8 out of 10 neobanks" claim treated as directional
- McKinsey $390B methodology caveats

This is good practice. The report does not fabricate data to fill these gaps.

### Gaps the Report Does NOT Acknowledge

1. **$33T vs. $35T discrepancy:** The Bloomberg/Artemis figure ($33T) and the McKinsey/Artemis figure ($35T) are not reconciled. The report uses both sources without noting the discrepancy. If the McKinsey figure is $35T, then $390B is approximately 1.1% of raw volume -- still "~1%" but worth noting.

2. **Circle IPO date:** Report states June 4, 2025. Multiple sources confirm trading began June 5, 2025 (pricing was announced the evening of June 4). This is a minor but verifiable error.

3. **Circle IPO valuation vs. current market cap:** The tl;dr compresses "Circle IPO'd at $32B market cap" which conflates the IPO valuation (~$6.9B) with the March 2026 market cap (~$31-32B). The body text is more precise but the tl;dr is misleading.

4. **Euro stablecoin consortium size:** Only four of nine banks are named. Missing: Banca Sella, CaixaBank, Danske Bank, Raiffeisen Bank International, SEB. The venture name (Qivalis) is also not mentioned.

5. **B2B share of payments:** Report says "60%" but verified figure is 58%. Minor rounding discrepancy.

6. **USDT market cap trajectory:** Report does not note the $6.5B in USDT burns in early 2026 that reduced USDT supply from $187B to ~$184B. This is relevant context for a March 2026 report.

7. **Mastercard/BVNK deal status:** The report does not clearly state that the deal had not yet closed as of March 2026 (pending regulatory approvals). It implies the acquisition is complete.

---

## 3. Conflation Error Check

### Fee Revenue vs. Trading Volume

**NO CONFLATION FOUND.** The report does not conflate fee revenue with trading volume anywhere. This is a clean pass.

### Raw Volume vs. Actual Payments

**PROPERLY DISTINGUISHED.** This is the report's strongest analytical feature. The report:
- Explicitly separates raw on-chain volume ($33T) from actual payment volume ($390B)
- Includes a bolded "Critical distinction" callout on lines 67-68
- References the CLAUDE.md instruction to maintain this distinction
- Uses the $390B figure (not $33T) as the baseline for payments analysis throughout
- Correctly identifies B2B at $226B as a share of the $390B adjusted figure, not of the $33T raw figure

**This is exemplary handling of a common analytical trap in stablecoin research.**

### Market Cap vs. Transaction Volume

**PROPERLY DISTINGUISHED.** The report notes that supply dominance (Ethereum) differs from volume dominance (Solana/Tron) and explicitly calls this out as a "Key insight" on line 109. The Section 2 treatment of chain distribution separates supply tables from volume analysis.

### Deposit Tokens vs. Stablecoins

**PROPERLY DISTINGUISHED.** Line 214 explicitly flags that JPM Coin (JPMD) is a deposit token, not a stablecoin, and notes the GENIUS Act implications (deposit tokens can bear interest; stablecoins cannot). This is a nuance many reports miss.

---

## 4. Grading

### Accuracy: 4 / 5

**Justification:** The vast majority of claims are verified and correctly sourced. Deductions for:
- The Circle IPO tl;dr misstatement ($32B market cap attributed to the IPO event rather than the current market cap)
- Stale USDT market cap ($187B vs. current ~$184B)
- Euro consortium undercounting (4 of 9 banks named)
- $33T vs. $35T discrepancy unaddressed
- Minor date error on Circle IPO (June 4 vs. June 5)

None of these are materially misleading in the context of the full report, but they require correction.

### Completeness: 4 / 5

**Justification:** The report covers market size, chain distribution, ecosystem players (issuers, infrastructure, banks, fintechs), use cases by vertical, distribution channels, regulation, and potential partners. Data gaps are explicitly flagged. Deductions for:
- Euro consortium incomplete (missing 5 banks and the Qivalis name)
- No mention of Citigroup's $4T bull-case estimate (useful context alongside Bessent's $3T)
- USDT supply reduction in early 2026 not covered
- Mastercard/BVNK deal closure status not clarified
- The report could benefit from a brief competitive landscape summary (who competes with whom and why)

### Source Quality: 4.5 / 5

**Justification:** Sources include:
- Primary institutional sources: McKinsey, Bloomberg, Congress.gov, OCC, FDIC, White House, JPMorgan, Mastercard IR, Tether.io, Circle IR
- Reputable financial media: CNBC, CoinDesk, Yahoo Finance, Fortune
- Specialized analytics: DefiLlama, Artemis, Arkham, CoinMarketCap, Visa On-Chain Analytics
- Legal analyses: Latham & Watkins, Gibson Dunn, Arnold & Porter

The report is well-sourced and avoids reliance on anonymous or low-quality sources. The Source Index (Section 8) is comprehensive. Minor deduction for:
- Some secondary sources used (StablecoinInsider, Cryptonomist) without independent verification
- The "8 out of 10 neobanks" claim from StablecoinInsider should have been more aggressively flagged or excluded

---

## Summary of Required Corrections

| # | Issue | Severity | Location |
|---|-------|----------|----------|
| 1 | Circle IPO tl;dr says "$32B market cap" -- should say "now valued at ~$32B" or similar to avoid implying it IPO'd at that valuation (IPO valuation was ~$6.9B) | Medium | tl;dr, line 14 |
| 2 | Euro stablecoin consortium lists 4 banks; actual consortium is 9 banks (add Banca Sella, CaixaBank, Danske Bank, Raiffeisen Bank International, SEB); add venture name "Qivalis" | Medium | Line 208, line 369 |
| 3 | USDT market cap $187B is Q4 2025 peak; by March 2026 it is ~$184B due to $6.5B in burns. Add date qualifier or update. | Low | Line 48, line 119 |
| 4 | USDC market cap $75.7B understated for March 2026 (actual ~$77-78B). Add date qualifier or update. | Low | Line 49, line 130 |
| 5 | Circle IPO date: June 4 should be June 5, 2025 (pricing announced evening of June 4, trading began June 5) | Low | Line 131 |
| 6 | $33T vs. $35T raw volume discrepancy between Bloomberg and McKinsey reports should be acknowledged | Low | Lines 60-67 |
| 7 | B2B as "60% of real payment volume" -- verified figure is 58% | Low | Line 11 |
| 8 | Mastercard/BVNK deal status should note it has not yet closed (pending regulatory approval) | Low | Lines 175-180 |

---

## Overall Assessment

This is a high-quality research brief that demonstrates analytical rigor, transparent sourcing, and -- critically -- proper handling of the raw volume vs. actual payments distinction that trips up most stablecoin analysis. The report's explicit data limitations section is a best practice. The eight corrections identified above are relatively minor and do not undermine the report's core conclusions. The research team should be commended for not fabricating data to fill gaps (Conduit, Brale, Wise volumes) and instead flagging them honestly.

**Composite Grade: 4.2 / 5**
