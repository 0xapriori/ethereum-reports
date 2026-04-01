# Regulatory Clarity Post-GENIUS Act: What Tokens Are and Aren't, and the Securities Question

**Research Date: March 31, 2026**

---

**tl;dr**

- **The GENIUS Act (signed July 2025) covers only payment stablecoins** -- it does not address governance tokens, utility tokens, DeFi protocol tokens, or the broader "are tokens securities?" question at all.
- **The SEC-CFTC Joint Guidance (March 17, 2026) is the real game-changer** -- a 68-page interpretive document creating a five-category token taxonomy (digital commodities, digital collectibles, digital tools, stablecoins, digital securities), with 16+ tokens explicitly named as digital commodities (not securities).
- **Governance tokens are tentatively classified as digital commodities** under the new framework, but this is interpretive guidance, not legislation -- it can be revised by future administrations and has not been tested in court.
- **The Howey test still governs** -- the SEC reaffirmed it as the standard, but provided new guidance on how tokens can transition out of investment contract status as networks decentralize.
- **Market structure legislation (the actual law that would codify all of this) is still not passed** -- two Senate bills are in committee, with floor votes unlikely before mid-2026 at earliest.
- **The Across Protocol ACX-to-equity conversion (March 2026) raises the uncomfortable question**: if governance tokens function like equity, maybe the regulatory distinction is just branding.
- **The current "clarity" is fragile** -- it rests on executive branch interpretation, not statutory law, and could reverse with a change in administration.

---

## Table of Contents

1. [The GENIUS Act: What It Actually Covers](#1-the-genius-act-what-it-actually-covers)
2. [The SEC-CFTC Joint Guidance: The Real Regulatory Event of 2026](#2-the-sec-cftc-joint-guidance-the-real-regulatory-event-of-2026)
3. [Are Tokens Securities? The Current State of Play](#3-are-tokens-securities-the-current-state-of-play)
4. [SEC Enforcement Posture Under the Atkins Administration](#4-sec-enforcement-posture-under-the-atkins-administration)
5. [Market Structure Legislation: Still Pending](#5-market-structure-legislation-still-pending)
6. [The SAFT/SAFE to Token Pipeline: Problems and Lessons](#6-the-saftsafe-to-token-pipeline-problems-and-lessons)
7. [If Everything Is On-Chain, Is This Just a Branding Problem?](#7-if-everything-is-on-chain-is-this-just-a-branding-problem)
8. [What Regulatory Changes Would Make Tokens "Fine"?](#8-what-regulatory-changes-would-make-tokens-fine)
9. [Key Open Questions and Risk Assessment](#9-key-open-questions-and-risk-assessment)
10. [Sources](#10-sources)

---

## 1. The GENIUS Act: What It Actually Covers

### Scope: Payment Stablecoins Only

The GENIUS Act (Guiding and Establishing National Innovation for U.S. Stablecoins Act) was signed into law by President Trump on July 18, 2025, after passing the Senate 68-30 on June 17, 2025, and the House 308-122 on July 17, 2025. It is the first major federal crypto-specific legislation to become law.

**What it covers:**

| Element | Details |
|---------|---------|
| **Asset type** | "Payment stablecoins" only -- digital assets redeemable at a fixed monetary value, used for payments and settlements |
| **Reserve requirements** | 1:1 backing by U.S. dollars or other low-risk assets |
| **Issuer oversight** | Bank subsidiaries regulated by primary financial regulator; nonbank issuers by OCC; state-licensed issuers under $10B by state regulators |
| **Securities exemption** | Permitted payment stablecoins explicitly carved out as NOT securities under federal securities laws and NOT commodities under the CEA |
| **AML/BSA compliance** | All issuers subject to Bank Secrecy Act requirements |
| **Consumer protection** | Stablecoin holders get priority claims over general creditors in issuer bankruptcy |
| **Enforcement tools** | Issuers must have technical capability to freeze, seize, or burn stablecoins when legally required |
| **Effective date** | January 18, 2027, or 120 days after final regulations are issued, whichever comes first |

**What it does NOT cover:**

- Governance tokens (UNI, AAVE, ACX, etc.)
- Utility tokens
- DeFi protocol tokens of any kind
- The securities classification question for non-stablecoin tokens
- Market structure (SEC vs. CFTC jurisdiction)
- DEX or DeFi protocol regulation
- NFTs or digital collectibles
- Token issuance frameworks (ICOs, airdrops, points systems)

**Key takeaway:** The GENIUS Act is important infrastructure legislation, but it answers exactly one question: how stablecoins are regulated. It intentionally defers the broader and harder question of what other tokens are.

---

## 2. The SEC-CFTC Joint Guidance: The Real Regulatory Event of 2026

### The March 17, 2026 Joint Interpretive Release

On March 17, 2026, the SEC and CFTC jointly released a 68-page interpretive document (Release No. 33-11412) that fundamentally redefines how crypto assets are classified under federal law. This is the most consequential regulatory development for non-stablecoin tokens since the SEC's 2019 "Framework for Investment Contract Analysis of Digital Assets."

This guidance was produced under SEC Chairman Paul Atkins' "Project Crypto" initiative, announced in July 2025 and detailed in a November 2025 speech.

### The Five-Category Token Taxonomy

| Category | Definition | Regulatory Status | Examples |
|----------|-----------|-------------------|----------|
| **Digital Commodities** | Crypto assets intrinsically linked to and deriving value from the programmatic operation of a functional crypto system, alongside supply and demand dynamics. Lack intrinsic economic rights like dividends or profit shares. | NOT securities. Subject to CFTC oversight. | BTC, ETH, SOL, XRP, ADA, LINK, AVAX, DOT, XLM, HBAR, LTC, DOGE, SHIB, XTZ, BCH, APT, ALGO (and governance tokens broadly) |
| **Digital Collectibles** | Crypto assets designed to be collected or used (artwork, music, in-game items, memes). Value based on supply/demand, not managerial efforts. | NOT securities. | NFTs, digital art, gaming items |
| **Digital Tools** | Crypto assets performing practical functions: memberships, tickets, credentials, identity badges. | NOT securities. | Access tokens, membership passes |
| **Stablecoins** | Already covered by GENIUS Act framework. | NOT securities (per GENIUS Act). | USDC, USDT, etc. |
| **Digital Securities** | Tokens that are investment contracts under Howey or otherwise meet the definition of a security. | Securities. Full SEC jurisdiction. | Tokens with explicit profit-sharing, equity-like rights from identifiable issuer |

### The 16 Named Digital Commodities

The agencies explicitly named 16 tokens as digital commodities (some sources report 18 depending on how sub-classifications are counted):

XRP, Ethereum (ETH), Solana (SOL), Cardano (ADA), Chainlink (LINK), Avalanche (AVAX), Polkadot (DOT), Stellar (XLM), Hedera (HBAR), Litecoin (LTC), Dogecoin (DOGE), Shiba Inu (SHIB), Tezos (XTZ), Bitcoin Cash (BCH), Aptos (APT), Algorand (ALGO).

**Critical note on governance tokens:** The guidance appears to classify governance tokens broadly under digital commodities, reasoning that their value derives from the programmatic operation of a crypto system and supply/demand rather than "the expectation of profits from the essential managerial efforts of others." However, this classification is not without nuance -- the guidance acknowledges that more work remains on defining "sufficient decentralization."

### What the Guidance Explicitly Clears

The interpretive release also provides clarity on several activities:

- **Protocol staking**: Not a securities transaction
- **Protocol mining**: Not a securities transaction
- **Airdrops of non-security crypto assets**: Not securities if recipients don't provide consideration (but conditional airdrops requiring activities may still implicate Howey)
- **Wrapping of non-security assets**: Not a securities transaction
- **Bridges**: Cleared as infrastructure, not securities activity

### Critical Limitations

1. **This is interpretive guidance, not law.** It can be revised, withdrawn, or reinterpreted by a future SEC chair without Congressional action.
2. **It has not been tested in court.** No federal court has yet ruled on cases applying this framework.
3. **"Sufficiently decentralized" remains undefined.** The guidance acknowledges this concept's relevance but does not provide a bright-line test.
4. **It does not cover all tokens.** Thousands of tokens remain unclassified -- the named 16 are examples, not an exhaustive list.
5. **The digital securities category is vaguely defined.** What exactly triggers classification as a digital security (vs. a digital commodity with governance rights) is not fully articulated.

---

## 3. Are Tokens Securities? The Current State of Play

### The Howey Test: Still the Law of the Land

Despite years of industry lobbying for a crypto-specific framework, the SEC's March 2026 guidance reaffirmed that the Howey test (from SEC v. W.J. Howey Co., 1946) remains the governing standard. A transaction is an investment contract (security) if it involves:

1. An investment of money
2. In a common enterprise
3. With an expectation of profits
4. Derived primarily from the efforts of others

### The "Investment Contract" vs. "The Token Itself" Distinction

This has been one of the most important conceptual developments in crypto securities law. The March 2026 guidance reinforces a distinction that courts have been grappling with:

- **The investment contract is the transaction, not the asset.** A token sold in a fundraising round (ICO, SAFT) may be part of an investment contract at the point of sale, even if the token itself is not inherently a security.
- **Tokens can transition.** A token that was initially sold as part of a securities offering can transition to non-security status as the network becomes functional and decentralized and the token's value no longer depends on a central team's efforts.
- **The "orange grove" analogy matters.** Just as oranges themselves are not securities even when sold through an investment contract (the Howey case involved orange grove interests), tokens themselves may not be securities even when the manner of their initial distribution was a securities transaction.

### Has Any Token Been Formally Classified as a Non-Security?

Yes -- as of March 2026, this is the first time federal regulators have explicitly named specific tokens as non-securities (digital commodities). The 16 named tokens in the joint guidance represent the first formal, positive classification.

Previously:
- Bitcoin was generally understood to not be a security (former SEC Chair Clayton, former Director Hinman), but this was informal guidance, not a formal classification.
- ETH's status was ambiguous until the March 2026 guidance.
- XRP's status was litigated for four years (SEC v. Ripple) before the case was settled.

### "Sufficiently Decentralized" -- Still Relevant but Still Undefined

The concept originates from former SEC Director William Hinman's 2018 speech suggesting that a token could start as a security but become a non-security once the network is "sufficiently decentralized."

The March 2026 guidance:
- Acknowledges the concept's relevance
- Does not define a bright-line test for what constitutes sufficient decentralization
- Suggests it is one factor among many (not the sole determinant)
- Leaves further development for future rulemaking

This is a significant gap. Without a clear test, protocol teams cannot know with certainty when they have crossed the threshold.

---

## 4. SEC Enforcement Posture Under the Atkins Administration

### The Great Unwinding: 2025-2026

The shift from the Gensler-era SEC to the Atkins-era SEC has been dramatic:

| Action | Date | Details |
|--------|------|---------|
| **Coinbase dismissal** | Feb 27, 2025 | Joint stipulation to dismiss. The SEC cited the "pending work of the Crypto Task Force." |
| **Ripple settlement** | May 2025 | $125M penalty and narrow injunction limited to institutional sales. Both sides dropped appeals in August 2025. |
| **Binance case** | 2025 | Dismissed or settled (specifics vary by sub-action) |
| **Gemini investigation** | Feb 2025 | Closed without enforcement action |
| **Robinhood crypto investigation** | 2025 | Closed without charges |
| **Kraken case** | 2025 | Dismissed |
| **Total cases dropped/closed** | Jan 2025 - present | At least 12+ crypto-related cases dismissed or closed |

### The Policy Pivot

- **"Project Crypto" launched July 31, 2025**: Commission-wide initiative under Chairman Atkins to modernize securities regulation for digital assets.
- **SEC Crypto Task Force**: Established early 2025, issued multiple Requests for Information, held public roundtables.
- **SEC 2026 agenda**: Removed all crypto-specific enforcement priorities, signaling institutional pivot away from enforcement-as-regulation.
- **"Innovation exemption" framework proposed**: Companies could test novel business models under principles-based safeguards rather than full compliance, reporting periodically to the SEC.

### Dissenting View

Commissioner Crenshaw issued a dissent on the Ripple settlement, arguing that the SEC was abandoning "meritorious litigated cases" including ones where courts had issued favorable rulings. A January 2026 letter from House Financial Services Democrats flagged concerns about the abandonment of enforcement actions.

### Risk Assessment

The current enforcement posture is highly favorable to crypto. However, it is entirely dependent on the current administration. A change in SEC leadership could reverse course. The key question is whether the interpretive guidance and any forthcoming "Regulation Crypto" rulemaking can be formalized enough to survive a change in administration.

---

## 5. Market Structure Legislation: Still Pending

This is the most important unfinished business. While the GENIUS Act addressed stablecoins and the SEC-CFTC guidance provided interpretive clarity, there is no federal statute that codifies the token taxonomy or establishes clear SEC vs. CFTC jurisdictional boundaries for non-stablecoin tokens.

### Legislative Timeline

| Bill | Chamber | Status (as of March 2026) |
|------|---------|---------------------------|
| **FIT21** (H.R. 4763) | House (passed May 2024, 279-136) | Stalled in Senate. Superseded by newer bills. |
| **CLARITY Act / Digital Asset Market Clarity Act** (H.R. 3633) | House (passed July 17, 2025, 294-134) | Awaiting Senate action |
| **Digital Commodity Intermediaries Act** | Senate Agriculture Committee | Advanced 12-11 on January 29, 2026. First crypto market structure bill to pass a Senate committee. |
| **Senate Banking Committee market structure bill** | Senate Banking Committee | Markup announced for Jan 15, 2026, then postponed. 100+ amendments filed. |

### Key Unresolved Issues

1. **Stablecoin yield**: The banking industry wants to preserve its monopoly on interest-bearing products. The White House set a March 1, 2026 deadline for compromise on stablecoin yield -- deadline passed without resolution.
2. **SEC vs. CFTC jurisdiction**: The fundamental question of which agency regulates which tokens and when a token transitions between jurisdictions.
3. **DeFi protocol treatment**: No bill adequately addresses how decentralized protocols (with no legal entity) should be regulated.
4. **100+ amendments**: The volume of proposed amendments to the Senate Banking bill reflects deep disagreement on details.

### Timeline Assessment

- **Optimistic case**: Both Senate committees complete markups by mid-2026, floor vote in summer 2026, reconciliation with House by year-end.
- **Realistic case**: Legislative gridlock continues. Senate campaigning begins in earnest in August 2026, making floor time scarce. Market structure bill likely pushed to 2027.
- **Pessimistic case**: Bills die in the 119th Congress and must be reintroduced in the 120th.

**Key implication:** The SEC-CFTC joint guidance is doing the work that Congress has failed to do. But guidance is not law, and the entire framework rests on executive branch discretion.

---

## 6. The SAFT/SAFE to Token Pipeline: Problems and Lessons

### How SAFTs Were Structured

A Simple Agreement for Future Tokens (SAFT) is an investment contract between a blockchain project and an investor:

- Investor provides capital upfront
- Project promises to deliver tokens at a future date (typically at network launch)
- The SAFT itself is acknowledged as a security (an investment contract)
- The theory: the SAFT is a security, but the tokens delivered later are utility tokens (not securities) if the network is functional

### The Core Problem: The Theory Never Held Up

The SAFT framework rested on a critical assumption: that a token could be sold as part of a securities offering but emerge as a non-security once the network launched. This assumption was systematically challenged:

1. **Telegram (SEC v. Telegram, 2020)**: The court held that Telegram's SAFT-like structure (called a "purchase agreement") was inseparable from the tokens. The entire arrangement was an unregistered securities offering. The form of the document did not matter -- economic reality did.

2. **Kik (SEC v. Kik, 2020)**: Similar holding. The court looked at the economic substance of the transaction, not the label.

3. **Ripple (SEC v. Ripple, 2023-2025)**: Partially vindicated the distinction -- Judge Torres ruled that programmatic sales of XRP on exchanges were not securities transactions, while institutional sales were. But this was a district court ruling with limited precedential value, and the SEC settled rather than appealing.

### Were SAFT Investors Always Buying Unregistered Securities?

Functionally, yes. The SAFT framework was always a legal fiction that attempted to separate the "investment contract" wrapper from the underlying token. The SEC's consistent position (under both Gensler and the earlier Clayton era) was that this separation was formalistic, not substantive.

The key lesson: **the manner of distribution matters as much as the characteristics of the token.** A token with genuine utility can still be part of a securities offering if it is sold with an expectation of profit based on the efforts of a development team.

### The Modern Workaround: Airdrops, Points, and Governance Tokens

The ICO-era crackdown killed the direct token sale model. What replaced it:

| Model | How It Works | Securities Risk |
|-------|-------------|----------------|
| **Airdrops** | Free distribution to users or community members | Lower risk if no consideration provided. March 2026 guidance says no-consideration airdrops fail Howey. But conditional airdrops (requiring activities) may still implicate securities laws. |
| **Points systems** | Users earn "points" through protocol interaction, later converted to tokens | Gray area. Points create an implicit expectation of future token value. No formal SEC guidance specific to points. |
| **Governance token distribution** | Tokens distributed to liquidity providers, stakers, or community members | Lower risk under March 2026 guidance (governance tokens classified as digital commodities). But initial distributions tied to fundraising still carry risk. |
| **Retroactive airdrops** | Tokens distributed to past users based on historical activity | Generally lower risk -- no prospective investment of money. |

**Critical observation:** These models evolved specifically to avoid the "investment of money" and "efforts of others" prongs of Howey. They are legal engineering solutions to a regulatory problem. The March 2026 guidance gives them more support, but the fundamental question -- whether points/airdrops are just SAFTs with extra steps -- has not been definitively resolved.

---

## 7. If Everything Is On-Chain, Is This Just a Branding Problem?

### The Across Protocol Case Study

Across Protocol's March 2026 proposal to convert from a DAO to a C-corporation, with ACX tokens exchangeable 1:1 for equity, is the most direct test of this question.

**The proposal:**
- Dissolve the DAO structure
- Form a U.S. C-corporation ("AcrossCo")
- ACX holders can exchange tokens for equity (1:1 ratio) or redeem for USDC at $0.04375 (25% premium to 30-day average)
- Smaller holders (250K-5M ACX) can access equity through a no-fee SPV
- Larger holders (5M+) convert directly

**The stated rationale:**
- Enterprise partners need enforceable contracts and a legal counterparty
- Revenue agreements require traditional corporate structure
- The DAO structure became a "bottleneck" for institutional growth

**The uncomfortable implication:**
- If ACX can be converted 1:1 to equity, what was the functional difference between the token and equity?
- ACX already provided governance rights (voting on protocol parameters) and economic exposure (token value tied to protocol performance)
- The conversion is an admission that the token was functioning as quasi-equity all along

### Governance Token vs. Equity: Functional Comparison

| Feature | Governance Token (typical) | Common Equity | Functional Difference |
|---------|---------------------------|---------------|----------------------|
| **Voting rights** | Yes (protocol parameters, treasury) | Yes (board, major decisions) | Narrow (token votes are usually more limited in scope) |
| **Economic exposure** | Yes (token price tracks protocol value) | Yes (share price tracks company value) | Minimal in practice |
| **Revenue share** | Sometimes ("real yield" models) | Yes (dividends) | When revenue share exists, the difference is essentially zero |
| **Liquidation preference** | No formal rights | Yes (statutory) | Significant legal difference |
| **Legal ownership** | No ownership of underlying entity (usually no entity exists) | Yes, fractional ownership of entity | Fundamental structural difference |
| **Regulatory protection** | Minimal | Securities laws, fiduciary duties | Massive gap in investor protection |
| **Transferability** | Permissionless, 24/7, global | Restricted (unless public), T+1 settlement | Token is more liquid |
| **Enforcement** | Smart contract, on-chain governance | Courts, regulatory agencies | Different enforcement mechanisms |

### Can Smart Contracts Replicate All Equity Rights?

Technically, yes. Smart contracts can encode:
- Dividend distributions (revenue share to token holders)
- Voting rights (on-chain governance)
- Pro-rata claims (liquidation waterfalls)
- Transfer restrictions (KYC/AML compliance)
- Vesting schedules

This is exactly what tokenized equity platforms like Securitize and tZERO do. Securitize's Q1 2026 "Stocks on Securitize" launch offers natively tokenized (not synthetic) equity shares on-chain with full regulatory compliance.

### The Branding Problem

The honest answer is: **for many governance tokens, the distinction from equity is primarily a legal/regulatory arbitrage, not a functional one.** The reasons protocols chose tokens over equity were:

1. **Regulatory avoidance**: Issuing equity requires SEC registration or an exemption. Issuing a "governance token" operated in a gray area.
2. **Global distribution**: Equity offerings have strict geographical restrictions. Token airdrops can reach a global audience.
3. **Liquidity**: Tokens trade 24/7 on permissionless markets. Private equity is illiquid.
4. **Composability**: Tokens can be used as collateral, staked, or integrated into other DeFi protocols in ways equity cannot.

The March 2026 guidance provides some regulatory support for treating governance tokens as digital commodities (not securities), but this rests on the assertion that their value derives from "supply and demand dynamics" rather than "the expectation of profits from the essential managerial efforts of others." For many DeFi protocol tokens, this assertion is debatable.

---

## 8. What Regulatory Changes Would Make Tokens "Fine"?

### What Exists Today

| Regulatory Instrument | Status | What It Provides |
|----------------------|--------|-----------------|
| **GENIUS Act** | Law (effective Jan 2027) | Stablecoin framework only |
| **SEC-CFTC Joint Guidance** | Interpretive guidance (March 2026) | Five-category taxonomy, 16 named digital commodities. Not law. Reversible. |
| **Project Crypto / Regulation Crypto** | Proposed rulemaking (forthcoming) | Safe harbors, tailored disclosures, innovation exemptions. Not yet finalized. |
| **Market structure bills** | In committee (Senate) | Would codify SEC/CFTC jurisdiction, define digital commodities in statute. Not yet law. |

### What Would Give Tokens Clear Non-Security Status

1. **Passage of market structure legislation.** The CLARITY Act (House-passed) and Digital Commodity Intermediaries Act (Senate Ag Committee-passed) would codify the digital commodity category in federal statute, removing the risk that a future administration could reclassify tokens through guidance alone.

2. **Finalization of "Regulation Crypto."** Chairman Atkins has signaled a formal rulemaking package that would create:
   - A token safe harbor (evolved from Commissioner Peirce's Rule 195 proposals)
   - Tailored disclosure requirements for token issuers
   - An "innovation exemption" framework for testing novel business models
   - Clear criteria for when a token transitions from security to non-security status

3. **Judicial precedent.** No federal appellate court has ruled on the application of Howey to governance tokens under the new framework. A circuit court decision upholding the digital commodity classification would provide durable legal certainty.

4. **A statutory definition of "sufficiently decentralized."** This remains the single biggest gap. Without a clear, objective test, protocol teams cannot know with certainty whether their token qualifies as a digital commodity or remains a digital security.

### Is the Across Conversion Premature?

Arguments that it IS premature:
- The March 2026 guidance classifies governance tokens as digital commodities (not securities), which is exactly what ACX holders would want
- Market structure legislation could codify this classification into law within 12-18 months
- Converting to a C-corp forfeits the regulatory optionality of the token structure
- The 25% USDC premium buyout price may undervalue the token if regulatory clarity increases token valuations broadly

Arguments that it is NOT premature:
- The regulatory clarity is guidance, not law -- it could be reversed
- Institutional partners need legal counterparties NOW, not when legislation passes
- The DAO structure has real operational limitations for enterprise deals
- The token-to-equity conversion actually validates the token's value by giving it recognized legal standing
- If tokens ARE functionally equity, formalizing this removes regulatory risk rather than creating it

---

## 9. Key Open Questions and Risk Assessment

### High-Confidence Assessments

- The GENIUS Act is law and provides durable stablecoin clarity. **Confidence: Very High.**
- The SEC under Atkins will not pursue enforcement actions against major token projects. **Confidence: High** (administration-dependent).
- The 16 named digital commodities will not be reclassified as securities under the current administration. **Confidence: High.**
- Market structure legislation will eventually pass, but the timeline is uncertain. **Confidence: Medium-High** on eventual passage; **Low** on specific timing.

### Key Risks

| Risk | Likelihood | Impact | Notes |
|------|-----------|--------|-------|
| **Administration change reverses guidance** | Medium (depends on 2028 election) | Very High | The entire framework rests on executive discretion. A hostile SEC chair could withdraw the interpretive guidance. |
| **Court challenge to guidance** | Low-Medium | High | An enforcement action under a future administration could test whether the guidance has legal force. |
| **Market structure bill fails in 119th Congress** | Medium-High | Medium | Would mean continued reliance on guidance rather than statute. |
| **"Sufficiently decentralized" remains undefined** | High | Medium | Creates ongoing uncertainty for newer/smaller protocols. |
| **Revenue-sharing tokens reclassified** | Low (current admin) / Medium (future admin) | High | Tokens with explicit revenue share most closely resemble securities. |
| **Points-to-token pipelines challenged** | Low-Medium | Medium | The guidance clears no-consideration airdrops but is ambiguous on conditional distributions. |

### The Fundamental Tension

The March 2026 guidance classifies governance tokens as digital commodities based on the premise that their value derives from "the programmatic operation of a functional crypto system" and "supply and demand dynamics" rather than managerial efforts. But for many DeFi protocols:

- A small core team does most of the development work
- Token holders vote on parameters but rarely exercise meaningful governance
- Token value is directly correlated with protocol revenue generated by the core team's efforts
- The "decentralization" is often more formal than functional

The guidance works because the current administration wants it to work. Whether it would survive adversarial scrutiny from a future enforcement-minded SEC is an open question.

---

## 10. Sources

### GENIUS Act
- [Fact Sheet: President Trump Signs GENIUS Act into Law -- The White House](https://www.whitehouse.gov/fact-sheets/2025/07/fact-sheet-president-donald-j-trump-signs-genius-act-into-law/)
- [The GENIUS Act of 2025: Stablecoin Legislation Adopted in the US -- Latham & Watkins](https://www.lw.com/en/insights/the-genius-act-of-2025-stablecoin-legislation-adopted-in-the-us)
- [S.1582 - 119th Congress: GENIUS Act -- Congress.gov](https://www.congress.gov/bill/119th-congress/senate-bill/1582)
- [What You Need To Know About the New Stablecoin Legislation -- Arnold & Porter](https://www.arnoldporter.com/en/perspectives/advisories/2025/07/new-stablecoin-legislation-analyzing-the-genius-act)
- [FDIC Approves Proposal to Establish GENIUS Act Application Procedures -- FDIC.gov](https://www.fdic.gov/news/press-releases/2025/fdic-approves-proposal-establish-genius-act-application-procedures-fdic)
- [GENIUS Act Explained -- State Street Global Advisors](https://www.ssga.com/us/en/intermediary/insights/genius-act-explained-what-it-means-for-crypto-and-digital-assets)

### SEC-CFTC Joint Guidance (March 2026)
- [SEC Clarifies the Application of Federal Securities Laws to Crypto Assets -- SEC.gov](https://www.sec.gov/newsroom/press-releases/2026-30-sec-clarifies-application-federal-securities-laws-crypto-assets)
- [SEC and CFTC Issue Landmark Joint Guidance -- Ropes & Gray](https://www.ropesgray.com/en/insights/alerts/2026/03/sec-and-cftc-issue-landmark-joint-guidance-on-classification-of-crypto-assets)
- [SEC and CFTC Issue Landmark Joint Interpretation -- Jenner & Block](https://www.jenner.com/en/news-insights/client-alerts/sec-and-cftc-issue-landmark-joint-interpretation-on-crypto-asset-classification)
- [Crypto Clarity: SEC and CFTC Issue Comprehensive Crypto Asset Guidance -- Morgan Lewis](https://www.morganlewis.com/pubs/2026/03/crypto-clarity-sec-and-cftc-issue-comprehensive-crypto-asset-guidance-part-1)
- [The SEC's New Framework for Crypto Assets Under Howey -- WilmerHale](https://www.wilmerhale.com/en/insights/client-alerts/20260324-the-secs-new-framework-for-crypto-assets-under-howey)
- [SEC-CFTC Crypto Guidance 2026: What Changed -- Aurum Law](https://aurum.law/newsroom/sec-cftc-crypto-guidance-2026-release-33-11412-what-builders-need-to-know)
- [SEC and CFTC Issue Joint Crypto Asset Token Taxonomy -- Prokopiev Law](https://www.prokopievlaw.com/post/sec-and-cftc-issue-joint-crypto-asset-token-taxonomy-interpretation-march-2026)
- [SEC CFTC Crypto Commodity List 2026: All 16 Digital Assets -- CoinPedia](https://coinpedia.org/news/sec-cftc-crypto-commodity-list-2026-all-16-digital-assets-named-and-what-it-means/)
- [SEC and CFTC Classify 16 Major Cryptocurrencies as Digital Commodities -- Finance Police](https://financepolice.com/sec-and-cftc-classify-16-major-cryptocurrencies-as-digital-commodities-in-landmark-2026-guidance/)
- [Interpretive Release No. 33-11412 (PDF) -- SEC.gov](https://www.sec.gov/files/rules/interp/2026/33-11412.pdf)

### SEC Enforcement and Project Crypto
- [SEC Enforcement: 2025 Year in Review -- Harvard Law School Forum](https://corpgov.law.harvard.edu/2026/01/21/sec-enforcement-2025-year-in-review/)
- [SEC Announces Dismissal of Civil Enforcement Action Against Coinbase -- SEC.gov](https://www.sec.gov/newsroom/press-releases/2025-47)
- [Statement on the Agency's Settlement with Ripple Labs -- SEC.gov (Commissioner Crenshaw)](https://www.sec.gov/newsroom/speeches-statements/crenshaw-statement-ripple-050825)
- [From Coinbase to Ripple: The Biggest Crypto Cases Dumped by Trump's SEC -- Yahoo Finance](https://finance.yahoo.com/news/coinbase-ripple-biggest-crypto-cases-202640480.html)
- [The SEC's Approach to Digital Assets: Inside "Project Crypto" -- SEC.gov](https://www.sec.gov/newsroom/speeches-statements/atkins-111225-secs-approach-digital-assets-inside-project-crypto)
- [Regulation Crypto Assets: A Token Safe Harbor -- SEC.gov](https://www.sec.gov/newsroom/speeches-statements/atkins-remarks-regulation-crypto-assets-031726)
- [Breaking Down "Project Crypto" -- Sidley Austin](https://www.sidley.com/en/insights/newsupdates/2025/11/breaking-down-project-crypto-sec-chairman-atkins-outlines-next-phase-of-digital-asset-oversight)
- [SEC is done with crypto: Removes all mention from its agenda for 2026 -- CryptoSlate](https://cryptoslate.com/sec-is-done-with-crypto-removes-all-mention-from-its-agenda-for-2026/)

### Market Structure Legislation
- [Crypto market structure bill: no hearing before 2026 -- CoinDesk](https://www.coindesk.com/policy/2025/12/15/senate-punts-crypto-market-structure-bill-to-next-year)
- [New Market Structure Bill Builds on FIT21 Framework -- Jones Day](https://www.jonesday.com/en/insights/2025/05/new-market-structure-bill-builds-on-fit21-framework)
- [Senate Ag Committee Releases Updated Crypto Market Structure Text -- Davis Wright Tremaine](https://www.dwt.com/blogs/financial-services-law-advisor/2026/01/senate-ag-committee-crypto-market-structure-text)
- [Chairman Scott Announces Digital Asset Market Structure Markup -- Senate Banking Committee](https://www.banking.senate.gov/newsroom/majority/chairman-scott-announces-digital-asset-market-structure-markup)
- [Senate panel passes crypto CFTC regulation bill -- CNBC](https://www.cnbc.com/2026/01/29/senate-ag-committee-advances-crypto-bill-to-establish-cftc-regulatory-authority.html)
- [What Is the CLARITY Act? -- FinTech Weekly](https://www.fintechweekly.com/news/what-is-the-clarity-act-digital-asset-market-structure-explained-2026)

### Across Protocol / Token-to-Equity
- [Paradigm-backed Across Protocol explores letting ACX holders exchange tokens for equity -- The Block](https://www.theblock.co/post/393192/paradigm-backed-across-protocol-acx-token-equity-exchange)
- [Across's ACX rockets 80% on plans to dump its DAO structure -- CoinDesk](https://www.coindesk.com/markets/2026/03/12/across-s-acx-rockets-80-massively-beating-bitcoin-on-plans-to-dump-its-dao-structure/)
- [ACX jumps 85% as Across Protocol weighs token-to-equity shift -- Crypto.news](https://crypto.news/acx-price-across-protocol-token-equity-proposal-2026/)
- [Across Protocol Considering Transition from DAO to Private Company -- Bankless](https://www.bankless.com/read/news/across-protocol-considering-transition-from-dao-to-private-company-token-for-equity-swap)

### SAFT Framework and Token Safe Harbor
- [Token Safe Harbor Proposal 2.0 -- SEC.gov (Commissioner Peirce)](https://www.sec.gov/newsroom/speeches-statements/peirce-statement-token-safe-harbor-proposal-20)
- [The Return of the Token Safe Harbor -- Latham & Watkins / JD Supra](https://www.jdsupra.com/legalnews/the-return-of-the-token-safe-harbor-4114830/)
- [There Must Be Some Way Out of Here -- SEC.gov (Commissioner Peirce)](https://www.sec.gov/newsroom/speeches-statements/peirce-statement-rfi-022125)

### Governance Tokens and Securities Classification
- [Separating Governance Tokens from Securities -- Cardozo Law Review](https://cardozolawreview.com/separating-governance-tokens-from-securities-how-the-utility-token-may-fall-short-of-the-investment-contract/)
- [Which tokens should include governance rights? -- a16z Crypto](https://a16zcrypto.com/posts/article/tokens-governance-rights/)
- [Tokenized U.S. Equities, DeFi Trading, and the SEC's Exemptive Authority (written testimony) -- SEC.gov](https://www.sec.gov/files/ctf-written-james-overdahl-tokenized-us-equities-01-22-2026.pdf)

### Tokenized Equity
- [Securitize -- Leading Tokenization Platform](https://securitize.io/)
- [tZERO readying 2026 IPO -- tZERO.com](https://www.tzero.com/media/tokenized-securities-market-tzero-is-readying-2026-ipo)

---

*Research conducted March 31, 2026. All findings are based on publicly available sources as cited. This document reflects the regulatory landscape as of the date of research and should not be construed as legal advice.*
