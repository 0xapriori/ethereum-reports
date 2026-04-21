---
title: "Token-Warrant Economics and the Immutability Drift"
date: 2026-04-20
---

# Token-Warrant Economics and the Immutability Drift

*A synthesis report written by the apriori-writer agent. | April 2026*

## tl;dr

- **Token warrants were the largest of several forces — alongside regulatory chill, founder psychology, and exchange/institutional listing requirements — biasing 2020-2023 protocol architectures toward governance-rich, upgradable, token-compatible designs.**
- **A fund structure that marks liquid token positions daily cannot tolerate portfolio companies that never launch tokens, so by 2022 the implicit term sheet was "ship token-compatible governance or you are not a venture case," which traded fairness-and-process legitimacy for performance legitimacy at the protocol layer.**
- **Immutable designs like Uniswap V2, Liquity V1, WETH9, and the Tornado Cash pools survived durably — but survivorship cuts both ways, and the honest claim is that immutability is the right choice for small, known-correct specifications that the warrant regime structurally underfunded, not that "immutability won the merits."**
- **The 2024-2026 shift — points, airdrops, "real revenue," AI-agent tokens — is early mutation, not regime reset; historical base rates from junk bonds, SPACs, and securitization say every financing wrapper mutates under pressure and none gets killed outright in the first three years.**
- **The financing regime that could fund the next Uniswap V2 without a token does not exist as an off-the-shelf kit; RetroPGF, fee-revenue self-funding, SBT-gated credentials, and foundation-over-Labs structures are real pieces, but none compose into something a founder can sign a term sheet against in six weeks.**

---

## Table of Contents

1. [Why the financing question is the design question](#1-why-the-financing-question-is-the-design-question)
2. [Financing as a legitimacy question](#2-financing-as-a-legitimacy-question)
3. [Financing-wrapper base rates: 1980s to today](#3-financing-wrapper-base-rates-1980s-to-today)
4. [The mechanics of token warrants](#4-the-mechanics-of-token-warrants)
5. [The LP-to-founder incentive chain](#5-the-lp-to-founder-incentive-chain)
6. [Case studies: did the financing structure shape the design?](#6-case-studies-did-the-financing-structure-shape-the-design)
7. [The quantitative record](#7-the-quantitative-record)
8. [Counterfactual financing structures](#8-counterfactual-financing-structures)
9. [Is the 2024-2026 shift real?](#9-is-the-2024-2026-shift-real)
10. [The political-community critique and the steelman](#10-the-political-community-critique-and-the-steelman)
11. [Scenarios and what this means for builders](#11-scenarios-and-what-this-means-for-builders)
12. [Data Sources & Methodology](#data-sources--methodology)
13. [Sources](#sources)

---

## 1. Why the financing question is the design question

The prior report in this series — [Immutable Applications on Ethereum Mainnet, April 2026](/ethereum/immutable-applications-on-ethereum-mainnet-april-2026.md) — argued at the macro level that five structural forces pulled Ethereum builders away from immutability during the last cycle, and that one of those forces was the token-warrant financing regime that dominated the 2020-2023 cohort. That claim was made in a paragraph. This report makes it at the micro level, with mechanism, numbers, and the cases that either support or complicate it.

The argument is not that venture capital is bad, or that token warrants are bad, or that any particular founder was acting in bad faith. The argument is narrower and stranger: that a specific legal and financial instrument — a warrant for tokens attached to an equity investment, combined with a fund structure that marked liquid tokens to market daily — acted as a selection filter on the population of protocols that got built. It did not do this by banning any design. It did this by making some designs financeable and others not, and by making some founder paths compoundable and others not. A protocol founder in 2021 could build a mutable governance-rich AMM with a token and raise a $20M round at a $200M valuation in eight weeks, or could build an immutable governance-free AMM and raise $2-3M from a small, unusual set of investors over six months. Both were possible. Only one was easy.

Over the course of a three-year cycle, that asymmetry compounded into what this report will call *the immutability drift* — a gradual, cumulative sorting of talent, capital, and attention toward protocols whose architecture was compatible with the financing that was available. The drift was not announced, was not the product of any individual decision, and was not generally understood as a financing phenomenon. It was generally understood as a design phenomenon: "users want governance," "governance enables bootstrapping," "token incentives are how you solve cold-start." These narratives were not exactly wrong. But they were downstream of the financing structure that made them necessary. The token-warrant regime did not cause teams to build bad protocols. It caused teams to build the protocols it was set up to finance.

The question this report tries to answer is: how tight was that filter, and how much is changing now?

The honest answer is that the filter was tighter than most people writing about DeFi in 2021 were willing to say, and that the 2024-2026 shift is looser than most people writing about DeFi now are willing to say. Both overstatements are convenient. This report tries to sit with the uncomfortable middle.

One more framing caveat before proceeding. The token-warrant regime was the *largest* of several co-drivers of the immutability drift, not the sole cause. At least three other forces moved in the same direction, independently:

- **Regulatory risk.** Post-Kik, post-Telegram, and especially post-Tornado-Cash, protocol teams chose upgradable architectures in part to preserve optionality under OFAC, SEC, or DOJ action. A governance surface that could pause a contract, rotate an oracle, or blacklist an address was not just a concession to VCs; it was an insurance policy against regulatory exposure. Teams that were never financed by warrants also trended upgradable over 2022-2024.
- **Founder psychology and survivor optimization.** Some teams chose upgradability because their founders came out of Silicon Valley orthodoxy — where "move fast, iterate" is the default — and had never seriously considered a deploy-once-and-walk-away model. Others, operating at a 20-year horizon, chose upgradable designs from conviction rather than pressure. The financing pulled the same way but was not the only hand on the rope.
- **Exchange and institutional demand-side selection.** Coinbase listing processes, Binance tokenomics requirements, market-maker inventory needs, and custodian integration pipelines all privileged governance-rich, upgradable tokens with clear corporate counterparties. A protocol that shipped as immutable-with-no-token was harder to list, harder to hedge, harder to integrate. This selected for the same architectures as the warrant regime, through a different channel.

The claim of this report is that the warrant regime was the largest and most reliably describable of these forces, and that its mechanism is legible in a way the others are not. It is not the claim that the others did not matter.

## 2. Financing as a legitimacy question

Before descending into instruments and fund lifecycles, one conceptual frame is worth establishing. Vitalik Buterin has argued that blockchain systems derive legitimacy from six overlapping sources: *brute force*, *continuity*, *fairness*, *process*, *performance*, and *participation*. A system can be legitimate along some of these dimensions and illegitimate along others; shifts in how a system draws its legitimacy are usually shifts in which source it leans on.

The warrant regime is legible in this frame. It spends *fairness* legitimacy (rapid insider unlocks against retail buyers) and *process* legitimacy (opaque negotiated allocations, closed-door term sheet standardization) to buy *performance* legitimacy (liquid TGE, TVL bootstrap, 18-month markup cycle). Immutable-primitive financing does the opposite: it forgoes a fast performance metric (no big TGE) in exchange for a credible fairness story (no insider unlock cliff) and a process story (publicly legible seed round, minimal governance surface). This is the actual structural argument of the report, rendered in one sentence: the 2020-2023 financing regime was a trade of fairness-and-process legitimacy for performance legitimacy, and immutable designs were the ones that refused the trade.

Two adjacent ideas from the same body of writing are worth naming.

**Credible neutrality.** A mechanism is credibly neutral if it cannot be bent to favor any particular participant after the fact. Uniswap V2 is credibly neutral in this sense — the pool math cannot be changed to penalize one trader or reward another. An upgradable lending market, even under a well-intentioned DAO, is not credibly neutral in the same sense, because the upgrade path exists and can be exercised under future pressure.

**Sanctuary tech.** Code that cannot be coerced by its deployer, its investors, or any government acting on any of them. Immutable contracts with no admin key are sanctuary tech by construction; upgradable contracts are not, because the upgrade key is a coercion surface. This framing is what made the Fifth Circuit's November 2024 ruling on the Tornado Cash immutable pools coherent — the court essentially found that sanctuary tech exists in a legal sense when it exists in an architectural sense.

The engine-vs-arbiter framing captures the deeper shift. The warrant regime makes investors *arbiters* — their marks, their unlock needs, their reputations for launching tokens are the feedback the system responds to. A retroactive-funding regime like RetroPGF or a fee-revenue regime would make communities *engines of discovery* instead: the protocols that demonstrate value get rewarded ex post, rather than the protocols that match investor expectations ex ante. This is not a subtle shift. It changes who decides what gets built.

## 3. Financing-wrapper base rates: 1980s to today

The token-warrant regime is not a crypto anomaly. It is the current iteration of a pattern that every credit-expansion cycle since the 1980s has produced.

Consider the base rate:

- **1980s junk bonds (Michael Milken / Drexel Burnham Lambert, roughly 1983-1989).** High-yield debt became the wrapper that enabled leveraged buyouts and corporate raiding at scale. It collapsed (Drexel bankrupt 1990, Milken imprisoned), but it did not disappear; it mutated into Business Development Companies (BDCs) and the modern leveraged-loan market, which is now several trillion dollars.
- **1998-2000 dotcom IPOs.** The IPO window functioned as a financing wrapper: a venture round priced at $50M could be marked to market at $2B eighteen months later if the company could file an S-1 into a retail demand window. When the window closed in 2001-2002, the wrapper mutated into SPACs (first wave 2003-2007; second wave 2020-2022), direct listings (Spotify 2018, Coinbase 2021), and more recently, private-market secondary tender offers.
- **2003-2007 securitization (CDOs, CDS, RMBS).** The mortgage-backed security and its synthetic cousins were the financing wrapper that let late-stage capital mark origination assets to market faster than the underlying credit performance could support. It collapsed in 2008. It mutated into Collateralized Loan Obligations (CLOs), the private-credit market, and the direct-lending BDC stack — all now many multiples larger than the 2007 CDO market.
- **2020-2022 SPAC wave.** Roughly $250B of SPAC capital raised 2020-2021. The wrapper collapsed on deal performance (2022-2023 median post-merger returns deeply negative). It is mutating toward reverse mergers, Regulation A+ deals, and — in a specific kind of convergence — token-like structures.
- **2020-2024 token warrants.** The current object of this report. In the process of mutating toward points, airdrops, and "real revenue" structures.

Of the five wrapper cycles, *zero* were killed outright in their first three years of post-peak criticism. All five mutated. All five preserved the core economic function (fast markup for late-stage capital; implicit valuation upgrades via a scarce primary mechanism) under altered regulatory or structural packaging.

This is not an argument that token warrants are benign. It is an argument that the correct expectation is *early mutation, not reset*. The 2024-2026 period is analogous to 2004-2006 in the subprime arc (continuation under rebranding) more than to 2001-2003 in the dotcom arc (genuine rerating). Token warrants will mutate; they will not be killed. Anyone writing a financing structure meant to replace them needs to assume the warrants will still be the default in 2028 and work out how to compete against them rather than waiting for them to vanish.

The crypto-specific version of this base rate observation is that the token-warrant regime's successors — points, liquid-token funds, "real revenue" token designs — all preserve the core function of the warrant (accrue a claim on future liquid supply) while reassigning the claimholder (user instead of VC) or relabeling the justification (fee distribution instead of governance). Relabeling a financing wrapper is not the same as replacing it.

One more caveat. Financing reform, on the historical base rate, takes 10-20 years from the triggering event to structural change. Glass-Steagall passed in 1933, four years after the 1929 crash. Sarbanes-Oxley passed in 2002 after Enron. Dodd-Frank passed in 2010 after the 2008 crisis. We are approximately three years into the token-warrant unwind. Major structural reform is unlikely before 2030 at the earliest, and that is the optimistic case.

## 4. The mechanics of token warrants

**A note on terminology.** "Token warrant" is used throughout this report as shorthand for the broader family of token-rights instruments paired with equity financing. Practitioners (Cooley, Latham, Gunderson, Orrick) reserve the strict term for a specific document — the right to purchase future tokens at a formula price, typically attached as a rider or standalone Token Warrant Agreement (TWA) to an equity SAFE or priced round. The full instrument family includes:

- **SAFT** (Simple Agreement for Future Tokens) — the 2017-2018 era instrument, progressively abandoned as a US fundraising vehicle after the Kik and Telegram rulings; still used in some non-US jurisdictions.
- **SAFE** (Simple Agreement for Future Equity) — the Y Combinator equity document; the equity leg of the modern bundle, not itself a token instrument.
- **Token Warrant / Token Warrant Agreement (TWA)** — the dominant 2021-2025 rider granting a right to purchase future tokens at a formula price.
- **Token Purchase Agreement (TPA)** — used at or after TGE for priced token sales; distinct from warrants.
- **Token Side Letter** — a lighter contractual promise of future token allocation, legally weaker than a warrant but commonly used.
- **Token Rights Agreement / Token Allocation Agreement** — Gunderson/Orrick terminology for a pro-rata token-right document, functionally similar to a warrant.
- **2024-2026 adjacent structures**: Token Distribution Agreements (post-TGE allocation mechanics), airdrop side letters, and Points Agreements (loyalty-style, not securities instruments).

The canonical 2022-2025 US-market bundle is **SAFE or priced equity round + token warrant (or token side letter)**. When this report says "token warrants" in the aggregate, it refers to this broader category of token-rights instruments and the incentive structures they produce — not strictly the TWA as a legal document. Where a specific instrument matters for the argument, it is named directly.

### 4.1 The SAFT era and its collapse

The modern token-warrant structure is what the market built after the SAFT — the Simple Agreement for Future Tokens — was progressively abandoned as a US fundraising instrument between 2018 and 2020, culminating in the Kik and Telegram rulings.

The attrition was gradual rather than abrupt. The SEC's 2017 DAO Report signaled the securities analysis would reach to token offerings. The June 2018 Hinman speech introduced — and then left unresolved — the idea that sufficiently decentralized networks could evolve out of security status. The 2018 Paragon and AirFox settlements established precedent before either of the major contested cases closed. By the time Kik and Telegram resolved in 2020, the SAFT was already terminal rather than healthy; the two rulings were the death-blow rather than the cause of death.

The SAFT was codified by Marco Santori, then a partner at Cooley, in a 2017 whitepaper. The theory was elegant: at the time of fundraise, accredited investors bought a contract (the SAFT) under Regulation D, where the contract was the security. When the network launched and tokens were delivered, the tokens themselves would — the theory went — be functional utility and thus not securities. The two-step structure treated the fundraising instrument (the SAFT) as a security and the delivered asset (the token) as a commodity or utility ([Coindesk, 2020](https://www.coindesk.com/markets/2020/04/28/with-kik-and-telegram-cases-the-sec-tries-to-kill-the-saft)).

Two cases destroyed the theory. In *SEC v. Kik Interactive*, the court granted summary judgment for the SEC on September 30, 2020, holding that Kik's 2017 Kin offering — partly conducted via SAFTs to accredited investors, partly via a public sale — was a single integrated securities offering ([Jones Day, 2020](https://www.jonesday.com/en/insights/2020/10/court-rules-that-sales-of-digital-tokens-were-illegal-unregistered-securities-offerings)). In *SEC v. Telegram*, the Southern District of New York granted the SEC's motion for a preliminary injunction on March 24, 2020, finding that the SAFT sale to accredited investors and the subsequent distribution of Grams to those investors were properly analyzed as parts of a single scheme to distribute unregistered securities to the public. Telegram returned $1.2 billion to investors and paid an $18.5M civil penalty in a settlement announced in June 2020 ([SEC, 2020](https://www.sec.gov/newsroom/press-releases/2020-146); [Cooley, 2020](https://www.cooley.com/news/insight/2020/2020-05-07-sec-v-telegram-key-takeaways-implications)).

The doctrinal frames in the two cases were distinct, though often blended in practitioner summaries. Kik applied *Howey* directly to what the court treated as a single integrated offering, refusing to separate the accredited SAFT sale from the subsequent public TDE sale. Telegram's court relied more on a Section 2(a)(3) / Rule 144 integration analysis — treating the accredited SAFT purchasers as statutory underwriters for the Gram distribution that followed — without strictly re-running *Howey* on the public side. The practical result was convergent (no US SAFT path survived for a materially secondary-distributed token), but the doctrine is not monolithic and practitioners should not conflate the two reasonings. After those rulings, any SAFT structure that could be characterized as part of an integrated public distribution looked unsafe. The market responded immediately. SAFT structures persisted for some niche cases (long post-TGE lockups; foreign-only distributions; instruments where the token was unambiguously a commodity from day one), but as the default US venture instrument for protocol fundraising, the SAFT was dead by the end of 2020.

### 4.2 The SAFE-plus-warrant structure

What replaced the SAFT was a two-instrument structure borrowed from traditional Silicon Valley venture practice and bolted onto a new asset class.

The first instrument is a SAFE — a Simple Agreement for Future Equity, originally codified by Y Combinator in 2013 for non-crypto startup financing. A SAFE is not a loan and not equity. It is a contract under which the investor's cash becomes equity on a triggering event (priced round, IPO, liquidity event) with specified conversion terms — typically a valuation cap, a discount, and often a most-favored-nation clause ([CRV, 2026](https://www.crv.com/content/safe-agreements-for-startups); [Wyrick Robbins, n.d.](https://www.wyrick.com/news-insights/safe-financing-valuation-cap-vs-discount-variants)). The SAFE is the equity side of the deal: it gives the investor a claim on shares of the company when the company next prices a round.

The second instrument is the token warrant, or "token side-letter." Mechanically, it is a contract that grants the investor the right to purchase some number of tokens at the time of the company's token launch, at a price typically pegged to the equity valuation the investor paid for the SAFE ([Hashed EM, 2022](https://hashedem.substack.com/p/safe-token-warrant-meta); [DaosPv, 2023](https://blog.daospv.com/token-warrants-safes-and-safts-a-founders-guide-to-web3-fundraising-instruments/)). The typical formulation: the investor is entitled to a *pro rata* share of tokens corresponding to their equity ownership, at a strike price that represents the same effective valuation — so a fund that paid $5M for ~7% of the company's SAFE converts to ~7% of the tokens at the equity-implied unit price when the token launches.

Three features of the token warrant mattered structurally:

**It was contingent, not obligatory.** The warrant does not force the company to launch a token. In early 2020-2021 drafts, the trigger was often phrased as "at the Company's discretion" — no token, no warrant. By 2022, the "Company's discretion" language was widely replaced by more specific triggers (any material token issuance, any governance-token airdrop, a capped time window). The loophole was closed because investors had been burned by portfolio companies that reasoned: "if we never launch the token, the warrant never triggers, and we keep 100% of the token supply."

**It was valuation-parity, not market-parity.** Because the warrant strike was pegged to the equity valuation, any gap between equity FDV and token FDV accrued to the investor. A fund that paid $5M for 7% of a company valued at ~$70M post-money, and whose token then launched at a $700M FDV, saw its effective token holding worth ~$49M — not from token appreciation, but from the valuation spread at launch. This is where most venture returns in the 2021-2022 vintage actually came from. It also explains why the FDV at token launch mattered so much to VC returns: a launch at $500M FDV versus $2B FDV was a 4x difference in IRR for the same portfolio company, at the same moment.

**It applied MFN through to the token side.** Sophisticated investors negotiated for MFN on both the SAFE and the warrant, so a later, lower-cap SAFE round or a later, cheaper warrant round automatically adjusted earlier terms downward ([Capbase, n.d.](https://capbase.com/most-favored-nation-mfn-clause-in-startup-investing-what-it-is-and-how-it-works/)). The effect was to turn the token supply into a ratcheting pool: as each new round came in, the portion of tokens that had to be reserved for existing investors grew, tightening the founder/team allocation.

### 4.3 The implicit obligation to launch a token

A token warrant does not, on its face, require the company to launch a token. Any team with a warrant on its cap table could, in principle, decide not to launch. The architecture of the incentives made that decision functionally impossible for most teams.

Consider a team with a four-year vesting schedule and a one-year cliff — the Silicon Valley default, carried into crypto almost without modification. The founders' equity cliff vests 25% at year one and then monthly through year four. The VC's SAFE converts into priced equity at the next round. If the team has not issued a token by the end of year two, three things happen: (1) the warrant is open-ended and overhangs any exit event; (2) the company's equity valuation is pegged to cash flows and is — for a protocol with an open-source codebase and no equity-level monetization — usually small; (3) the VC's mark in the fund's quarterly report is stuck at the last SAFE conversion price.

The constraint is fund-lifecycle, not principle. A 10-year close-end fund needs to show DPI — distributed to paid-in, the fraction of committed capital actually returned — to raise its next fund. Liquid-token positions count toward DPI once they can be distributed. Equity positions count only on exit. The math is stark: a GP that launched five portfolio tokens in year three and distributed liquid shares back to LPs could raise a larger Fund II from those LPs in year four. A GP that waited for equity exits would be raising Fund II in year seven, against a shrinking pool of LPs who had already recycled their allocations to faster-paying managers.

This is the mechanism that turned "token warrant" into "implicit obligation to launch a token." The warrant was the legal instrument. The fund lifecycle was the motive force. And the standard vesting schedule was the clock.

### 4.4 The 2022-2023 refinements

As the market matured, the warrant structure was refined in ways that tightened the token launch pathway:

- **Token warrant agreements (TWAs)** became standalone documents rather than side letters attached to SAFEs, making them independently enforceable and allowing separate vesting terms (e.g., a 12-month post-TGE lockup for investors that differed from the equity cliff).
- **Conversion triggers** were specified more narrowly to close the "no token, no warrant" loophole: any airdrop, any material distribution, any chain-level token issuance, any protocol governance mechanism that referenced a token.
- **Pro rata provisions** extended into future token rounds, so a fund that participated in the SAFE+TWA round could match its pro rata in any later token sale.
- **Distinct fund types** emerged to hold different parts of the stack: an equity fund for the SAFE, a liquid-token fund for post-TGE holdings, sometimes a third "evergreen" vehicle for tokens still in lockup. Each fund had its own fee and carry structure, and the GPs captured management fees on each layer.

### 4.5 The point

Nothing about this stack, taken instrument by instrument, is unique to crypto. A SAFE is a Y Combinator document. A warrant is a piece of standard corporate finance. MFN is a term out of commercial law. The vesting schedule is pure Silicon Valley. The fund lifecycle is straight out of Preqin.

What is unique to crypto is the combination: a capital stack whose return realization depends on the portfolio company issuing a *programmable, permissionless, instantaneously-liquid instrument* — a token — within a window of years, at a scale and price that justifies the valuation the VC paid for the SAFE.

That combination did not make it harder to build protocols. It made it harder to build a specific kind of protocol: one that does not need, want, or benefit from a token. The protocols that fit the financing got built. The protocols that did not, mostly did not.

## 5. The LP-to-founder incentive chain

To understand why the warrant structure was more than a legal detail, it helps to walk backward from the LP.

### 5.1 The fund

A typical crypto venture fund of the 2021 vintage was a 10-year close-end vehicle with standard 2-and-20 economics — 2% annual management fee on committed capital, 20% carried interest over some preferred return. Electric Capital's March 2022 $1B close-end fund, Paradigm's $1B Fund II in November 2021 followed by a $2.5B Fund III, and a16z Crypto's $4.5B Fund IV in May 2022 all fit variations of this structure. (The two Paradigm funds should not be conflated: Fund II was the second fund, Fund III the third; each was a distinct vehicle with separate LPs and investment periods.) The commitments came from a mix of institutional LPs — endowments (Harvard, Yale, MIT), family offices, fund-of-funds, and sovereign wealth vehicles — plus a minority from high-net-worth individuals and strategic participants.

By 2024, the AUM of a16z crypto's four crypto funds had contracted nearly 40% to roughly $9.5B, and Multicoin's AUM shrank from $8-9B at peak to ~$2.7B by 2025 ([ChainCatcher, 2024](https://www.chaincatcher.com/en/article/2259439); [Fortune, April 2026](https://fortune.com/2026/04/16/top-crypto-vcs-paradigm-pantera-a16z-multicoin-haun-dragonfly/)). This contraction was not a GP decision; it was marks-to-market on the liquid tokens held in the funds as 2021-2022 vintage tokens traded down from their launch FDVs.

### 5.2 The accounting

The distinctive feature of crypto venture accounting is that tokens, once liquid, are marked to market daily. Traditional venture portfolio companies are marked on the next priced round — a stale input that moves annually at best. A crypto VC that launches a portfolio token at a $2B FDV and holds 2% of that supply has $40M of mark-to-market value the day the token trades, regardless of whether the fund intends to sell.

This had two consequences.

First, *the markup cycle* compressed dramatically. A VC that invested at a $50M equity SAFE cap could see a portfolio company launch its token at $500M FDV eighteen months later, and immediately the fund's mark on that position went from $5M to $50M (less any discounting for lockup). IRR calculations are extremely sensitive to timing. A 10x markup in 18 months is a set of numbers that justify a second fund; a 3x markup in five years is a set of numbers that don't. The speed of the markup was the primary reputation-compounding mechanism for the 2021-2022 vintage of crypto GPs.

Second, *LP psychology shifted*. Cambridge Associates and other institutional advisors flagged the volatility of token-denominated crypto funds as early as 2022: "blockchain/crypto VC funds often generate significant liquid token holdings once private investments launch public tokens through ICOs or Token Generation Events, and because tokens are typically much more volatile than public equities, the overall volatility of these funds can be substantially higher than LPs might expect from conventional private investments" ([Cambridge Associates](https://www.cambridgeassociates.com/insight/blockchain-and-crypto-vc-funds/)). But the same volatility that made the downside worse also made the upside faster. An endowment that was willing to stomach the volatility got a faster-compounding return; one that wasn't, exited. The LP base that remained by 2022-2023 was the subset that had learned to price token liquidity favorably — which meant the LPs that stayed were the LPs that *wanted* portfolio tokens to launch.

### 5.3 The GP

A GP at a 2021-vintage crypto fund was managing a portfolio against two audiences: the LPs who needed liquidity events, and the market of founders who were the fund's deal flow. Both audiences selected for the same thing. LPs wanted tokens launched; founders wanted GPs who had launched tokens. A GP that had not sponsored a successful token launch by year three of the fund was in a worse position to win the next deal than one that had — reputation in crypto venture ran through the portfolio companies' tokens, not through any other metric.

The *Polychain-Celestia* episode in 2024-2025 is instructive. Polychain invested ~$20M in Celestia and, through staking rewards alone on its locked TIA position, realized over $80M in liquid sales before the main unlock — turning a locked position into a 4x pure from yield ([Unchained, 2024](https://unchainedcrypto.com/polychain-made-80-million-from-selling-celestia-staking-rewards/); [The Block, 2025](https://www.theblock.co/post/364155/polychain-sells-remaining-62-5-million-tia-stake-to-celestia-foundation-amid-criticism-of-staking-reward-sales)). The legal mechanism (staking rewards are not subject to the lock, only the principal) was unusual. The economic mechanism (convert portfolio paper into portfolio liquidity as fast as the mechanism allows) was not. TIA's price fell roughly 90% from peak through the insider-unlock cycle ([Cryptonews, 2025](https://cryptonews.com/news/celestia-tokenomics-crisis-tia-plunges-90-after-aggressive-unlocks-is-airdrop-hype-to-blame/)). Polychain realized the fund return; retail holders of TIA did not.

The Celestia case is not a story of any individual impropriety. It is a story of the incentive structure working exactly as designed. The fund had a mandate to return capital to LPs; the token was the mechanism; the mechanism had yield-extraction paths that were commercially rational. The LP-to-founder chain selected for the design that allowed this, and got it.

### 5.4 The founder

At the end of the chain is the founder, who faces a coherent set of pressures from the financing stack:

- Raise a round in which a token warrant is table stakes (by 2021, standard term sheets in US crypto venture assumed one).
- Commit to a protocol architecture compatible with launching that token — which means the protocol must have some governance surface the token can sit atop, which means the contracts must be upgradable or at least parameter-tunable, which means admin keys or a DAO-controlled upgrade path.
- Plan a token launch on a timeline consistent with the fund's markup cycle — roughly 18-36 months after seed.
- Design an emission schedule that permits insider selling at a pace that satisfies the fund's DPI needs without destroying the retail market too fast (hence the elaborate 4-year linear vests, the cliffs, the optional extensions).

The founder who tried to do any of this differently — for example, to ship an immutable governance-free primitive — had a much harder path. Not impossible. Harder. The LP-to-founder chain is not determinative in any individual case. Hayden Adams shipped Uniswap V2 with no token. The Liquity team shipped V1 with a small seed round and explicit commitment to governance minimization. But these were outliers precisely because the default path of the financing stack pointed in the other direction.

Reading the chain back in legitimacy terms: the LP-to-founder selection pressure runs against every source of legitimacy except performance, and performance legitimacy regresses sharply when the market does. Fairness (rapid insider unlocks against retail), process (opaque term sheets, closed-door allocations), continuity (teams under pressure to ship a token within a fund cycle rather than iterate on a primitive), and participation (the community enters as farmers and exits as bagholders) are all traded for TGE-compressed markup velocity. When performance collapses in a drawdown, as it did in 2022-2025 for the 2021 vintage, the system has no other legitimacy source to fall back on. That is why the drawdown feels like a legitimacy crisis and not merely a price correction.

## 6. Case studies: did the financing structure shape the design?

The strongest test of the thesis is case-by-case. A blanket claim that financing determines design is a just-so story. A claim that specific financing structures correlate with specific design choices is a testable proposition. This section walks through ten protocols and asks, in each case, whether the financing they had is consistent with the design they produced — and what a different financing structure would have made possible.

### 6.1 Uniswap V1, V2, V3, V4

**Uniswap V1** (November 2018) was built by Hayden Adams on an Ethereum Foundation grant of ~$65K. There was no VC round, no token warrant, no fund. The contracts were immutable.

**Uniswap V2** (May 18, 2020) was built after Uniswap Labs raised a Paradigm-led seed in April 2019 (~$1M), and before its Series A. No token existed when V2 launched. The V2 contracts were deployed once, had no admin key that could touch pool math, and have run with identical bytecode for nearly six years. The only governance surface — the `feeTo` parameter — was trivially narrow and remained inactive until V3-specific activation in 2024.

The missing causal link in the popular V2-as-pristine-primitive narrative is the **August 2020 a16z-led $11M Series A for Uniswap Labs**. The Series A closed between V2's May deployment and UNI's September launch — and is the most plausible proximate catalyst for the UNI retrofit, alongside (and probably more causal than) the Sushi vampire attack.

**UNI launched September 16-17, 2020**, approximately three weeks after the SushiSwap vampire attack that began August 26, 2020. The UNI launch was framed as a governance token with 60% allocated to the community ([Uniswap Labs, 2020](https://blog.uniswap.org/uni)). It was not, notably, framed as satisfaction of a warrant to Paradigm or a16z — but the token's emergence four months after the Series A closed is consistent with warrant pressure finding expression, regardless of how the announcement characterized it.

**Uniswap V3** (May 2021) was deployed with UNI already in existence and with a business-source license (BSL) that granted exclusive commercial rights to Uniswap Labs for two years. The core pool contracts retained the V2 design discipline — not upgradable, no parameter-changing governance over the math. Protocol fees, enabled via DAO vote on specific pools starting in 2024, are a narrower governance surface than a mutable pool.

**Uniswap V4** (January 2025) introduced hooks, which reintroduce extensibility at the per-pool level, and ships with BSL. The core pool manager retains the design philosophy of prior versions.

**UNIfication** (December 2025) — the Uniswap DAO voted overwhelmingly in favor of activating fee redirection to a UNI buyback-and-burn mechanism, structured as a "token jar" users can redeem against. Reported vote margins exceeded 99%; the exact tally should be verified against the live governance record ([Blockworks, 2025](https://blockworks.co/news/uniswap-fee-switch); [Coindesk, 2025](https://www.coindesk.com/markets/2025/12/22/uniswap-token-burn-moves-closer-to-reality-as-99-of-voters-in-favor-of-fee-switch-proposal)).

What does this tell us?

The "V2 shipped before the warrant era" framing should be accepted with a caveat. Paradigm in 2019 was already a sophisticated fund, and Hayden Adams had clear and publicly stated commitments to a governance-minimized architecture. The reason V2 shipped immutably is not only that the warrant regime had not yet congealed into an industry-wide default; it is also that a specific founder was hard to push around and a specific investor accepted the constraint. Later Uniswap events — the UNI retrofit in September 2020, the BSL on V3, the UNIfication fee switch in 2025 — suggest that *pressure on the warrant logic still found expression*, on a longer timeline. The design that shipped at V2 was a window, not a structural freedom; the window closed as the cap table deepened.

The counterfactual is worth stating explicitly, with appropriate epistemic humility: if Uniswap Labs had raised its seed in 2022 under standard token-warrant terms, it is unlikely (though not certain) that V2 would have deployed as a governance-free immutable primitive. More plausibly, V2 would have shipped with UNI as a Day 1 governance token, a fee switch controlled by UNI holders from inception, and some residual admin control over the pool factory. The design that actually shipped was enabled by — not determined by — the absence of the financing structure that later became standard.

### 6.2 Liquity V1 and V2

Liquity V1 (April 2021) is the purest case of a governance-free stablecoin deployed on Ethereum. The Liquity team raised a $2.4M seed led by Polychain in September 2020, with participation from a_capital, Lemniscap, 1kx, the DFINITY Ecosystem Fund, Robot Ventures, and Alex Pack ([Liquity, 2020](https://www.liquity.org/blog/liquity-protocol-raises-2-4m-in-seed-funding-led-by-polychain-capital); [The Block, 2020](https://www.theblock.co/linked/78795/liquity-ethereum-lending-seed-funds)). A later $6M March 2021 round was reportedly led by Pantera Capital (the specific round structure should be verified against a primary source). The LQTY token launched at protocol deployment, but the token governs fee-sharing and staking rewards, not protocol parameters — the mint, redemption, and liquidation mechanics are fixed in bytecode.

Two things about Liquity's financing enabled the design:

First, *the round size was small*. $2.4M at seed is a fraction of what a comparable team could have raised under standard token-warrant terms in 2020-2021. A smaller round means less dilution, smaller per-investor positions, and less absolute-dollar pressure for a large token FDV to justify the valuation. The team took less capital; the investors got a smaller absolute stake; the protocol did not need to launch a $500M-FDV governance token to clear anyone's expectations.

Second, *the investors were thesis-aligned*. Polychain, 1kx, and the other seed participants were willing to back a protocol whose explicit design goal was to minimize governance. This was not universal. By 2022, the thesis-aligned investor population had shrunk — most capital that flowed into DeFi wanted governance tokens.

Liquity V2 introduced a narrow layer of governance — specifically for directing protocol liquidity incentives among front-end operators and integrators — not over core mechanics. The V1 team was explicit in its reasoning: "minimizing governance" was not a slogan, it was a long-term risk-management claim about the shape of capture surfaces inside a stablecoin.

The Liquity case supports the thesis in a specific way: it shows that governance minimization was possible in the 2020-2021 cohort when the financing was small, thesis-aligned, and deliberately not structured to require a token FDV at exit. Where it wasn't possible was in the part of the market where the financing was large, non-thesis-aligned, and structured around token FDV.

A reverse-causal reading of the Liquity case is also defensible and should be stated. It is at least as plausible that the team's design thesis attracted a thesis-aligned seed round as that the small round enabled the design; the two are deeply entangled and probably bidirectional. What is *not* defensible is the claim that Liquity could have shipped as V1 against a $20M warrant-heavy round. The thesis and the round co-produced each other; the warrant-heavy counterfactual would not have produced either.

### 6.3 MakerDAO

MakerDAO's funding history has two distinct rounds that should not be collapsed into a single narrative. In **December 2017**, a16z and Polychain co-led a $12M round in MakerDAO — the instrument structure was a token purchase with investor rights, not a pure direct MKR-as-commodity purchase. In **September 2018**, a16z's crypto practice made a follow-on $15M purchase of approximately 6% of MKR at a valuation reported at ~$250M ([MakerDAO blog, 2018](https://blog.makerdao.com/2018/09/a16z-buys-6-of-all-mkr-tokens/); [Gemini Cryptopedia](https://www.gemini.com/cryptopedia/makerdao-defi-mkr-dai-coins); [Cryptorank](https://cryptorank.io/ico/maker)). MKR itself had been introduced in August 2015 without an ICO.

The original version of this report argued that Maker's case was outside the warrant regime because the instrument was the token itself rather than a warrant on future tokens. That framing is too clean and should be revised. A direct purchase of governance tokens *is* functionally a call option on future network value held against cash paid now — it has the same economic function as a warrant, settled at time of purchase rather than at TGE. It differs in the settlement timing (immediate rather than deferred) and the securities analysis (commodity treatment rather than warrant-plus-SAFE), but the economic claim on the network's future performance is substantively the same.

What this means is that Maker's DAO-governed upgradable architecture is better read as the *prototype* that the later warrant regime inherited rather than a counterexample outside the regime. The Maker template — governance token with claims on protocol fees, upgrade paths controlled by token holders, investor allocations at privileged valuations — is recognizably the template most 2020-2022 warrant-financed protocols followed. Maker sits at the origin of the regime, not outside it.

The more honest summary: Maker's case is partly inside the thesis population (the architecture is what warrant-era protocols converged on) and partly distinct (the instrument and timing were different). Previous framing as wholly outside the regime reads as motivated definitional gerrymander and is withdrawn here.

### 6.4 Aave (formerly ETHLend)

Aave originally launched as ETHLend with the LEND token via an ICO in 2017. The ICO raised ~$16M. In September 2020, the protocol migrated LEND to AAVE at a 100:1 ratio, reducing the circulating supply from 1.3B LEND to 13M AAVE for the community, with an additional 3M AAVE minted to the protocol reserve ([Bingx](https://bingx.com/en/learn/article/what-is-aave-crypto-lending-and-how-does-it-work); [MEXC](https://www.mexc.com/price/AAVE/tokenomics)). Standard allocation terms in the post-migration AAVE token included a meaningful investor tranche, a team tranche, and an ecosystem reserve.

Aave's governance is extensive: risk parameters, asset listings, interest rate models, and safety module parameters are all DAO-controlled. The architecture is upgradable. The protocol has suffered from the complexity — a series of frozen markets, governance attacks, and parameter mis-calibrations — while also benefiting from it: Aave has adapted to new collateral types, L2 deployments, and risk regimes in ways an immutable protocol could not.

Aave is a mixed case. Its early structure pre-dated the token-warrant era (ICO-funded, token in place before the warrant regime consolidated). But its subsequent fundraising — including later rounds — fit the standard pattern, and its architecture has grown into exactly the governance-rich, upgradable shape the warrant regime favors. Aave's complexity is partly a function of the age of its governance surface.

### 6.5 Compound

COMP launched in April 2020 via a liquidity-mining distribution. Robert Leshner's team had raised earlier rounds from a16z and others; the token was introduced to "decentralize" governance and bootstrap usage via liquidity mining. The distribution allocated roughly half of COMP to users via lending-and-borrowing activity, and the other half to team, founders, and investors ([Gemini Cryptopedia](https://www.gemini.com/cryptopedia/compound-crypto-defi-decentralized-finance)).

The liquidity mining launch of COMP was the defining moment of DeFi Summer 2020. It worked spectacularly at attracting TVL — but it also established the template of "launch a governance token, reward users to use the protocol in its denomination, watch TVL grow to numbers that justify the valuation." This template is the template a token-warrant-financed protocol needs to execute against. COMP's launch was not primarily the warrant mechanism at work — it was a bootstrapping mechanism — but it defined the playbook that later warrant-financed protocols followed.

### 6.6 Tornado Cash

Tornado Cash shipped its pool contracts in 2019-2020 without a token. The project was initially funded via Gitcoin grants and small VC rounds. TORN, the governance token, was announced in December 2020 and launched to enable trustless governance and incentive mining ([Cointelegraph](https://cointelegraph.com/news/torn-soars-200-as-tornado-cash-s-governance-token-becomes-tradable); [Coinbrief](https://cryptobriefing.com/tornado-cash-token-release-airdrop/)). The allocation included 5% to early users, 10% for anonymity mining, 55% to the DAO treasury on a **3-month cliff plus 5-year linear** vesting schedule, and 30% to founding developers and early supporters on a **1-year cliff plus 3-year linear** schedule. Cliff structure is material: the 3-month DAO-treasury cliff meant governance was effectively distributed well before any insider unlock began, while the 1-year founder cliff preserved team-alignment against a launch-and-dump pattern.

The anonymity-mining allocation deserves specific acknowledgment rather than treatment as peripheral governance. By subsidizing deposits (the longer you kept funds in a pool, the more you earned), the mining program genuinely changed pool dynamics — increasing anonymity set depth, shifting the optimal strategies for users, and creating a direct-to-user subsidy that was unusual in the token-warrant era. The design of the mining program deserves more credit than "peripheral governance": it was a thoughtful use of token-economic engineering to improve the underlying privacy product.

The important architectural aspect of Tornado Cash remains that the *protocol* was shipped as immutable — the pool contracts are unchangeable — and the *token* governed peripheral parameters (anonymity mining rate, grants, non-core matters) rather than pool math. This is the architectural split ENS had pioneered (immutable core, governed periphery) and that Morpho Blue would later formalize.

The Tornado Cash case supports the thesis in a specific way: even when a project ships with no token, the pressure to introduce one — from VCs, from users farming rewards, from the general expectation that a successful crypto project should have a token — is strong enough that the token comes anyway, and it comes *scoped to the periphery* to avoid compromising the immutable core. This is a defensible design. But it also shows the gravitational pull of the token-warrant logic: even governance-minimized protocols feel it.

The subsequent OFAC sanction of the Tornado Cash pool contracts in August 2022, the Roman Storm indictment and partial conviction, and the Fifth Circuit's November 2024 ruling that the immutable contracts "lacked the hallmarks of ownership, control, and exclusivity" ([Mayer Brown, 2024](https://www.mayerbrown.com/en/insights/publications/2024/12/federal-appeals-court-tosses-ofac-sanctions-on-tornado-cash-and-limits-federal-governments-ability-to-police-crypto-transactions)) are addressed in the prior report. What matters here is that the *immutability of the core contracts* was the legal and moral hinge that saved the protocol — and that immutability was only possible because it was baked in before the token layer.

### 6.7 Curve and the CRV wars

Curve launched CRV in August 2020 with a vote-escrow lockup mechanism (veCRV) that traded governance rights for time-locked stakes. Lock durations range from one week to four years, with voting power scaling linearly by lock length — maximum-duration locks receive the maximum voting weight. The four-year cap is a design parameter, not a fixed lock length. CRV emissions are the core incentive: pool liquidity providers earn emissions, and veCRV holders vote to direct those emissions to specific pools. By 2022, Convex Finance had accumulated ~40-50% of the total veCRV supply through its vlCVX wrapper, and Convex — rather than CRV holders directly — became the de facto arbiter of CRV emission direction ([Coindesk, 2021](https://www.coindesk.com/business/2021/11/11/curve-wars-heat-up-emergency-dao-invoked-after-clear-governance-attack); [The Defiant](https://thedefiant.io/curve-convex-governance-crv-control/); [Pontem](https://pontem.network/posts/curve-and-the-convex-wars-2)).

The "Curve Wars" were a governance concentration that the Curve architecture *required*: the entire liquidity-provision value proposition was denominated in CRV emissions. A stablecoin-centric AMM that was not emissions-driven would not have needed the veCRV mechanism, would not have needed Convex, and would not have produced the rent-seeking supra-protocols that sat on top. The emissions mechanism is the thing that generated the governance complexity.

This is a clean instance of a distinction Vitalik has drawn between *coordination* and *collusion*. Coordination is the legitimate process by which participants in a system come to agreement about shared parameters through transparent voting and advocacy; collusion is the formation of concentrated voting blocs that extract rent from other participants by conditioning their votes on side payments. veCRV / Convex / vlCVX is a textbook collusion surface. The token architecture made bribes for emission votes the equilibrium strategy — Votium and similar marketplaces formalized this — and the returns to collusion exceeded the returns to honest coordination. The governance design did not prevent this because the design itself was the surface the collusion formed on.

Was the emissions mechanism necessary? For a *bootstrap* it was clearly useful. Curve's early liquidity depth was partly because lenders got paid in CRV to provide it. But the structural choice — to make emissions permanent, to make them voted-on, to build a veCRV lock with maximum four years — was an architectural choice downstream of the token being present at all. A stablecoin-centric AMM without a token (the Uniswap V2 structural equivalent) would not have had a Convex war, would not have had the MEV-related governance exploits, and would not have required the elaborate rent-allocation layers. Curve's governance complexity is the shape its design takes when the token is the center of the design. The token is the center of the design because the financing expected it there.

### 6.8 Morpho: Optimizer to Blue

Morpho's evolution is the clearest case-study in the dataset because it happened in two phases visible to the public.

**Phase 1 (2022-2023):** Morpho Optimizer launched as an overlay on top of Aave and Compound, peer-to-peer matching lenders and borrowers to capture the spread between supply and borrow rates. The design inherited Aave's and Compound's risk models and governance. Morpho's 2022 ~$18M round led by Andreessen Horowitz and Variant (with earlier rounds from Polychain, Nascent, and others) was structured as a token sale rather than a pure Series A with warrants; characterizing it precisely matters because the token-side claim was settled at purchase, not deferred to a warrant trigger. The economic function approximates a warrant (investor claim on future token value), but the instrument mechanics are different.

**Phase 2 (February 2024 mainnet launch, governance activated late 2023 / early January 2024):** Morpho Blue shipped as a standalone primitive. ~650 lines of Solidity. Not upgradable. Five parameters fixed at market creation. Permissionless market deployment. Risk management externalized to curators and MetaMorpho vaults. The MORPHO token exists; token transferability was activated November 21, 2024, under MIP-75 (this MIP number should be verified against the live governance archive), via a wrapped-token mechanism that enables onchain vote tracking — and its governance scope is limited. Critically, Morpho's governance forum explicitly states: "Morpho Governance cannot halt the operation of a market or manage funds on users' behalf, nor does it impose specific oracle implementations" ([morpho.org, 2024](https://morpho.org/blog/morpho-blue-and-how-it-enables-our-vision-for-defi-lending/); [The Defiant, 2024](https://thedefiant.io/news/defi/morpho-restructures-to-align-token-value-with-company-equity)).

Morpho also restructured its corporate form in 2024-2025: Morpho Labs SAS, the French for-profit development company, became a wholly-owned subsidiary of the Morpho Association, a French nonprofit, via a 100% share transfer. The Association cannot distribute profits externally ([DL News](https://www.dlnews.com/articles/defi/morpho-nonprofit-to-absorb-development-team/)).

The corporate restructuring should be read as a *credible-commitment legitimacy move*, not only as operational neatness. By eliminating the for-profit equity layer above the development team, Morpho removed a structural pathway by which future investors or acquirers could redirect protocol value away from token-holders. It is also a plural-ownership experiment of a kind Glen Weyl's Plurality writing would recognize: the protocol's operational team is held by a nonprofit association whose mandate aligns with the protocol's token-holders rather than with a separate pool of Labs-equity holders. This is a more meaningful move than most post-hoc "foundation" structures because the equity layer is eliminated rather than merely supplemented.

Morpho's path is the most informative in the dataset because it shows: (1) the financing structure produced an overlay protocol (the Optimizer), because that's what financed architectures could safely ship on top of the existing governance-heavy primitives; (2) it took until 2024 — after the team had scale, reputation, and Series-A money to operate from — to ship the immutable primitive (Blue) that the architecture had implied all along; (3) the token side was resolved via a wrapped-token mechanism that kept governance narrow and aligned the fund structure with the protocol structure. Morpho is the case of a team *that had the financing to escape the financing*. Most teams did not, because most teams did not reach Morpho's scale and credibility before their runway ran out.

### 6.9 Euler

Euler Finance shipped a governance-rich, upgradable lending protocol. The `donateToReserves` function was introduced under eIP-14 in July 2022, eight months before the exploit. On March 13, 2023, a flash-loan attack exploited a missing health check in that function to drain ~$197M from the protocol. The attacker was persuaded to return most of the funds over the subsequent weeks ([Cyfrin, 2023](https://www.cyfrin.io/blog/how-did-the-euler-finance-hack-happen-hack-analysis); [BlockSec](https://blocksec.com/blog/euler-finance-incident-the-largest-hack-of-2023); [Chainalysis](https://www.chainalysis.com/blog/euler-finance-flash-loan-attack/)). The eight-month gap between introduction and exploit is material: the function sat in production, through audits and monitoring, before the vulnerability was discovered and exercised.

The counterfactual: could Euler have shipped immutably in 2022? Architecturally, yes — a lending protocol can be specified with fixed parameters (as Morpho Blue later demonstrated). Financially, the answer is speculation: Euler had raised a standard Series A with standard token-warrant terms, and an immutable-no-upgrade design was not the architecture those terms typically financed in 2022, but there is no observed counterfactual in which Euler attempted an immutable design and was refused funding. The claim that warrant terms *precluded* an immutable Euler is reasoned inference, not documented fact, and should be labeled as such.

The *donateToReserves* function is the specific emblem. A protocol that cannot be upgraded cannot introduce a new function that turns into an attack surface. Euler was hacked by code that did not exist at deployment. This is the specific failure mode token-warrant-financed architectures are vulnerable to, and the specific failure mode immutable designs do not exhibit (they have other failure modes — latent bugs — but not this one).

### 6.10 Augur V1 as a counterexample

Augur shipped V1 in July 2018 as a fully immutable, fully decentralized prediction market, funded by one of the larger ICOs to that point. It had 37 daily average users as of August 2018 ([PM Wiki](https://pm.wiki/projects/augur); [Wikipedia](https://en.wikipedia.org/wiki/Augur_(software))). The UX was a failure. Augur V2, launched in 2020, introduced DAI settlement and moved away from strict immutability to improve the product.

The Augur case is sometimes cited as proof that immutability fails in practice. A more rigorous reading is different: Augur V1 was *correctly designed for immutability but incorrectly designed as a product*. It shipped too much complexity (user-interface state, slow oracles, week-long settlement) and relied on later iteration to patch UX — which contradicts the immutability commitment. The lesson is that immutable protocols must ship simple, known-correct designs. Uniswap V2 did. Augur V1 did not.

This is not a case against immutability; it is a case against *ambitious* immutability. The immutable-primitive playbook works when the specification is small and correct. Augur's specification was large and contested.

## 7. The quantitative record

### 7.1 Crypto VC funding by year

The aggregate numbers capture the cycle shape:

| Year | Total crypto VC funding | Primary data source |
|---|---|---|
| 2021 | ~$33B (peak) | [Blockworks / Galaxy compilations, 2022](https://blockworks.co/news/crypto-vc-2021-record-year) |
| 2022 | ~$26-30B (modest decline from peak) | Galaxy / PitchBook compilations |
| 2023 | ~$10.7B | Galaxy / PitchBook compilations |
| 2024 | ~$13.7B | [The Block](https://www.theblock.co/post/332539/the-funding-crypto-vc-recap-2024) |
| 2025 | ~$17B+ (some estimates $34B+ including extensions) | [CryptoRank](https://cryptorank.io/insights/reports/crypto-fundraising-report-Q1-25) |
| Q1 2026 | $6.81B across 222 rounds | [Crypto Fundraising](https://crypto-fundraising.info/blog/q1-2026-crypto-fundraising-report/) |

The peak was 2021, not 2022; a prior draft of this report had the two years inverted, which changes the cycle shape meaningfully. The 2022 number was a modest decline from the 2021 peak, followed by a steep drawdown in 2023. Peak-to-trough 2021 → 2023 was ~3x. Deal counts compressed — 1,551 deals in 2024 versus 898 in 2025 YTD per some compilations — while the median check grew. Seed median rose from ~$4M in 2024 to ~$5-8M by 2025-26 ([insights4vc](https://insights4vc.substack.com/p/q1-2025-vc-report-inside-us-and-crypto); [Carta](https://carta.com/data/state-of-pre-seed-q2-2025/)).

The composition of the remaining deal flow shifted too. By 2025, AI-integrated projects and RWA tokenization were the dominant categories, capturing roughly 40% of 2025 capital according to compilations ([insights4vc](https://insights4vc.substack.com/p/2024-crypto-venture-capital-trends); [Cointelegraph](https://cointelegraph.com/research/crypto-vc-funding-doubled-in-2025-as-rwa-tokenization-took-the-lead)). Pure DeFi-primitive rounds shrank as a share of the total.

The concentrated set of funds responsible for most of the 2021-2022 cohort's warrant-structured rounds — a16z crypto, Paradigm, Pantera, Multicoin, Haun Ventures, Dragonfly, Electric Capital, Framework Ventures, Standard Crypto, Hack VC, 6th Man Ventures — also overlapped on the majority of the high-FDV launches that subsequently drew down. This is not a fund-specific critique; it is a description of how thin the concentrated-conviction set was.

### 7.2 The 2021-2022 vintage token record

The core empirical question is: of the protocols funded in the 2021-2022 vintage that launched tokens, what fraction trade above their TGE price as of April 2026?

A complete answer requires a dataset no public source maintains cleanly. A reasonable directional estimate — stitched from compilations of 2021-2022 vintage liquid-token launches tracked by Tokenomist, Messari, and CoinGecko and cross-referenced against fund AUM contractions — is that **fewer than 20% of 2021-2022 vintage tokens trade above their TGE price in Q1 2026**, and the median drawdown from peak is greater than 80%. This figure is directional, not exhaustive; a clean survivorship-adjusted dataset does not exist in public form, and this report does not construct one. The figure should be read as a useful order of magnitude, not a verified statistic.

The survivorship denominator for immutable protocols is also absent from the public record. The immutable-primitive case studies discussed in this report (Uniswap V2, Liquity V1, WETH9, Tornado Cash pools, Morpho Blue) are winners by construction — we do not see the immutable primitives that shipped with latent bugs and died silently because they had no governance layer to surface a post-hoc patch. Survivorship bias cuts both ways. The correct reading is that *for the small, known-correct specifications that shipped immutably, the record is durable*, not that *immutability is uniformly correct*.

Hack-rate comparisons between immutable and upgradable protocols would be useful here but are not available at the population level that the directional claim requires. At the case level, the Euler and Tornado Cash comparison is suggestive but not probative.

The directionally-usable evidence on the warrant-financed cohort is that the 2021-2022 vintage is the worst-performing in crypto VC history. Multicoin's AUM fell ~90% from 2021 to 2022 and further through 2024-2025 ([ChainCatcher](https://www.chaincatcher.com/en/article/2259439)). Top-tier fund AUMs (a16z crypto, Paradigm, Pantera, Haun, Dragonfly) collectively contracted by 30-50% into 2026 as portfolio tokens traded down from launch FDVs ([Fortune, April 2026](https://fortune.com/2026/04/16/top-crypto-vcs-paradigm-pantera-a16z-multicoin-haun-dragonfly/)).

The specific cases illustrate the shape:

- **Sui**: raised $336M total — Series A of $36M in December 2021 at $1.8B valuation, followed by a Series B of $300M in September 2022 at $2B valuation. Investors received 714M SUI at ~$0.05 per token. Monthly unlocks of ~40M SUI to Series A and B investors began May 2024. Peak FDV ~$41B in 2024; trading at ~$9.4B FDV in early 2026 ([tokenomist.ai](https://tokenomist.ai/sui); [AINvest](https://www.ainvest.com/news/token-supply-dynamics-september-2025-evaluating-market-impact-sui-4-5-billion-unlock-ftn-aptos-arbitrum-2508/)).
- **Aptos**: FDV ~$2.4B early 2026, down from 2023 peak; investor vesting cliff triggered material drawdowns ([tokenomist.ai](https://tokenomist.ai/aptos)).
- **Celestia**: TIA peaked at $20.85 on February 10, 2024; dropped to ~$1.65 through 2024-2025, a ~90% decline ([Cryptonews](https://cryptonews.com/news/celestia-tokenomics-crisis-tia-plunges-90-after-aggressive-unlocks-is-airdrop-hype-to-blame/)).
- **EigenLayer/EigenCloud**: TGE October 1, 2024 at $6B launch valuation; FDV ~$233M by spring 2026, a ~97% drawdown ([tokenomist.ai](https://tokenomist.ai/eigenlayer)).
- **Berachain**: raised $42M Series A (April 2023) and $100M Series B (March 2024), totaling at least $142M in publicly disclosed venture capital. FDV ~$300M in early 2026, materially below aggregate VC exposure ([tokenomist.ai](https://tokenomist.ai/berachain-bera)).

These are not cherry-picked examples; they are five of the most prominent VC-backed token launches in the recent cycle. Each one shows the same pattern: high launch FDV (justifying the VC's mark), insider unlock cliffs starting 6-12 months post-launch, sustained selling pressure from vesting, and a 70-97% drawdown over 12-24 months.

Compare to the protocols that either did not launch a token or launched it late on independent terms:

- **Uniswap V2**: immutable since May 2020; handled ~$4T in cumulative volume through 2025; fee switch activated December 2025 via UNIfication, with buyback-and-burn mechanism directing protocol revenue to UNI.
- **Liquity V1**: immutable since April 2021; runs LUSD at ~110% ETH collateral ratio with no protocol-level governance.
- **Morpho Blue**: launched January 2024; TVL peaked ~$10B in late 2025, running at ~$5.8-6.9B by April 2026; MORPHO transferable November 2024.
- **WETH9**: deployed 2017, never touched; ~275M operations on mainnet since the Merge alone.

The pattern is that the protocols with the longest operational records and the least governance surface are also the protocols with the most durable usage, not the most dramatic market capitalizations. This does not prove that governance-minimization caused durability. It does suggest that the opposite claim — that elaborate tokenized governance was necessary for success — is not supported by the data.

### 7.3 The fee-revenue/trading-volume distinction

A necessary methodological aside: the sentence "Uniswap handled $4T in volume" is not the same as "Uniswap produced $4T in fees." It is easy to conflate them in loose reporting. Uniswap's *protocol fee revenue* (the portion of swap fees that would accrue to the protocol after UNIfication) is a much smaller number — on the order of hundreds of millions annually — than the trading volume. Similarly, Hyperliquid's $747M annual fees and $664M revenue figures ([DefiLlama](https://defillama.com/protocol/hyperliquid)) are real fee flows, not trading volume. When comparing protocol economic size across projects, use fee revenue; when comparing usage intensity, use volume. They move together but are different magnitudes.

One implication for the core thesis: fee revenue — not FDV, not TVL — is the metric that correlates with durable legitimacy. FDV is a market-set number that moves with sentiment and insider unlock cliffs; TVL is a stock measure subject to rehypothecation and mercenary mining; fee revenue is a flow measure that reflects actual user willingness to pay for access. A protocol that generates $500M annual fee revenue has demonstrated something a $5B FDV alone does not. Readers comparing protocols across the warrant-financed and immutable cohorts should prefer fee-revenue comparisons where the data exists.

### 7.4 What the numbers imply

The numbers support a specific claim, not a blanket one. The 2021-2022 token-warrant vintage generated dramatic paper gains for VC funds at token launch and then transferred substantial value from retail token buyers to insider unlock flows. LPs in those funds received DPI in token form, which they could sell if they could time the lockups. Retail buyers of those tokens, in aggregate, lost money on the 2022-2025 holding period.

The same period produced a handful of protocols with durable usage — some without tokens at launch, some with governance-minimized tokens, and some governance-rich like Aave or Lido — whose economic footprint is measurable in fees, TVL, and operational continuity rather than FDV.

The original draft of this report claimed, in one sentence, that "the financing structure extracted value from the tokens; the protocol-level value accrued to the designs that did not require tokens." That binary is too clean. Aave, Lido, and Maker — all governance-rich, upgradable, warrant-era-aligned in architectural style — run durable operations with meaningful fee revenue; the presence of a token and a governance surface did not prevent durability. Conversely, Augur V1 was immutable and failed. The correlation between token presence and durability is not 1.0 in either direction.

The more careful claim: the financing structure *maximized paper value at TGE* while *imposing unlock-cliff pressure* that systematically transferred token value from late token-holders to early token-holders. That transfer is visible and measurable. The orthogonal question of whether the protocol produces durable fee revenue is real but distinct — some warrant-financed protocols do (Aave, Lido, Hyperliquid), and some don't, and the warrant structure did not cleanly determine one outcome or the other.

The gap between performance legitimacy (TGE paper value) and the other legitimacy sources (fairness, process, continuity, participation) is large enough that the question of whether the financing structure was *net additive* or *net distortive* for Ethereum's application layer is a live one. But it is a more subtle question than the extraction-vs-accrual binary suggests.

## 8. Counterfactual financing structures

What would have funded immutable primitives in 2020-2024 if token warrants had not dominated?

A caveat before proceeding. The clean binary that public-goods-style VC (foundation grants, RetroPGF, sovereign capital) is legitimacy-preserving while token-economic VC is legitimacy-extracting is too clean. Public-goods capital can also produce capture dynamics: the Foundry maintainership dispute (Paradigm's ownership of the dominant Ethereum developer tooling pipeline), the Rust Foundation governance disputes, and the recurring ZK-research-to-proprietary-prover pipeline (academic zkSNARK work funded by public grants, productized under closed licenses by companies whose early equity was priced off that research) all show that non-token capital produces its own concentration pressures. The distinction between "clean" public-goods funding and "dirty" warrant-driven funding is more useful as a gradient than as a binary. Every capital source imposes constraints on what gets built and by whom; the question is which constraints are the least bad for the kind of protocols you want to see.

### 8.1 Ethereum Foundation and similar grants

The EF's DEVgrants program (April 2015), Ethereum Foundation Grants (March 2018), and the Ecosystem Support Program (November 2019) funded core infrastructure and selected application work ([ethereum.org/community/grants](https://ethereum.org/community/grants/)). Grant sizes were typically small — $25K to a few hundred thousand — which is sufficient for early-stage primitive work (Uniswap V1 was written against a grant of this size) but not for sustained team-building.

The EF has been explicit in 2024-2025 about funding constraints: it paused the Open Grants program in 2025 to redirect funding ([The Block, 2025](https://www.theblock.co/post/368804/ethereum-foundation-pauses-grants-programs-as-it-looks-to-cut-burn-rate)) and swapped 5,000 ETH into stablecoins for operational runway ([The Block](https://www.theblock.co/post/396728/ethereum-foundation-swaps-5000-eth-stablecoins-operational-grant-funding)). The EF is a finite treasury with real constraints; it cannot fund the application-layer backlog on its own.

### 8.2 Retroactive public goods funding, quadratic funding, and credential infrastructure

Optimism's RetroPGF has distributed over $100M cumulatively across six rounds, with $1.3B+ earmarked for future rounds ([retropgf.com](https://www.retropgf.com/); [ETH Daily](https://ethdaily.io/429)). This is a large pool — at the scale of a mid-sized venture fund — even if denominated in OP and Optimism-ecosystem-scoped.

The earlier draft of this report dismissed RetroPGF too quickly. A more considered reading: RetroPGF creates an *ex ante expectation of reward* that functions as de facto runway for mission-aligned builders, even though it is not an ex ante grant. A builder who can plausibly model "if I ship this primitive successfully on Optimism, I should expect to receive $X in a future RetroPGF round" can bridge the funding gap in ways pure ex post funding cannot. The expectation itself is the runway.

This reframes the question of why RetroPGF has not yet composed into a founder-facing product. The technical pieces exist (six rounds of demonstrated distribution, transparent voting, clear categories). The missing piece is the *commitment device*: a way for a founder to plausibly borrow against expected RetroPGF payments while still in early-build phase. This is a coordination problem, not an instrument problem — it would require either (a) a third-party entity that advances against expected RetroPGF claims, or (b) RetroPGF itself adopting a forward-commitment track (e.g., milestone-conditional advances). Neither has been built.

**Quadratic funding and its dependency on immutability.** Gitcoin Grants and the broader quadratic-funding (QF) ecosystem distribute matching capital based on the number of distinct donors to a project, weighting small contributions more heavily than large ones. QF depends on *sybil-resistant matching* — a matching function that cannot be gamed by a single actor spawning many identities — which in practice depends on the matching smart contract itself being immutable and credibly neutral. If the matching logic could be changed after donations were collected, the mechanism would collapse into opaque grants-by-committee. The QF ecosystem's dependence on immutability at the matching layer is itself a structural argument for the broader thesis of this report: the financing regimes that could fund immutable primitives depend on immutable primitives as infrastructure.

**Soul-bound tokens / SBTs / non-transferable credentials.** SBTs are non-transferable on-chain credentials that could provide a legibility layer for thesis-aligned capital — a way for a founder committed to governance-minimized design to signal that commitment credibly across fundraising rounds, or for a thesis-aligned fund to signal its preference for immutable-first protocols in a way that matters for deal flow. SBTs remain in experimental deployment (Gitcoin Passport, various attestation protocols) but have not yet composed into a production credential layer for protocol financing. If they did, they would make the thesis-aligned investor population easier to find and harder to fake, which would strengthen the composite financing stack this report describes in §8.7.

**Plurality and the Morpho Association model.** The broader intellectual frame here is Glen Weyl's Plurality — a body of thinking that centers on plural-ownership experiments and cross-boundary coordination structures. The Morpho Association (§6.8) is a live Plurality-adjacent experiment: a nonprofit association holding a for-profit Labs subsidiary, with token-holders' governance rights aligned against a non-distributing nonprofit rather than against a separately-equity-holding Labs. This is not a clean Plurality structure, but it is closer to one than any pre-2024 DeFi corporate form. The success or failure of the Morpho Association over 2026-2028 will be an important empirical data point on whether plural-ownership structures can sustainably hold protocols at scale.

RetroPGF, QF-with-immutable-matching, SBT-gated credentials, and Plurality-adjacent corporate structures are, together, pieces of a credible alternative to the warrant regime. None of them is a complete substitute individually. A founder in 2026 cannot sign a term sheet for a bundled RetroPGF+QF+SBT+nonprofit financing package; that package does not exist as a signable instrument. The pieces are materials; the product has not been assembled.

### 8.3 Protocol-revenue royalties and fee switches

A project that reaches fee-generating scale can, in principle, fund itself from its own revenue. Uniswap's UNIfication (December 2025), activating the fee switch and directing protocol revenue to UNI via a burn mechanism, is the highest-profile example. But Uniswap was already running at massive scale when it activated — the fee switch funded the *ongoing* protocol, not the original research. Hyperliquid generates ~$664M annual revenue and directs a large fraction to HYPE buybacks ([DefiLlama](https://defillama.com/protocol/hyperliquid); [Coindesk](https://www.coindesk.com/tech/2025/11/19/the-protocol-hyperliquid-introduces-proposal-to-cut-fees)); Pendle, Aerodrome, and others distribute fees to locked tokens.

None of these funds a new primitive. Fee royalties work as *retention* mechanisms for existing protocols, not as *initial development* mechanisms for new ones.

### 8.4 Foundation / nonprofit corporate structures

The Morpho Association model — nonprofit holding company owning a for-profit Labs subsidiary — is a deliberate attempt to align token-holder interests with operational interests without the Labs-vs-Foundation value leak that plagued earlier DeFi governance. It works if the protocol has already generated a token to align value around. It does not answer the question of how to fund the research that precedes the token.

A similar pattern: Uniswap Labs operates as an equity-financed company whose revenue comes from interface fees, analytics, and related services, while the protocol itself is DAO-governed. This works for Uniswap in 2026, because Uniswap is a successful protocol with strong non-token monetization paths. It does not generalize easily to new primitives.

### 8.5 Sovereign, academic, and cooperative capital

Sovereign wealth funds and state-backed digital-asset initiatives (Japanese government web3 funds, Taipei/Taiwan grants, various EU programs, UAE crypto initiatives) have provided non-dilutive capital to some teams. Academic grants funded most of the foundational zkSNARK research that now powers production systems. Cooperative structures exist (Mondragon-style) but are vanishingly rare in crypto.

At the 2026 scale, these sources fund *specific work*, not *general operations*. A team can win a grant for a specific research milestone, but cannot run a 12-person protocol development shop on sovereign capital.

### 8.6 Direct user purchase and patronage

Patronage models — Nouns DAO as the outlier case, where NFT proceeds fund ecosystem development — have produced real but limited results. Nouns has funded creative and infrastructural work at the scale of hundreds of thousands to low millions per initiative. It does not fund serious protocol engineering at the Morpho scale.

### 8.7 The composite

What would need to be true for any of these to fund the next Uniswap V2?

A realistic answer looks like a *composite*: a small foundation grant for initial prototyping; a thesis-aligned seed of $1-3M from a specific category of investors who accept zero or narrow-scope token economics (the Liquity model); a grants pipeline from an ecosystem fund (Optimism, Arbitrum, Base, etc.); eventual fee-revenue self-funding once the protocol reaches scale.

This composite exists in principle. It does not exist as an off-the-shelf fundraising kit that a founder can execute against in the same way a SAFE + token warrant can. A founder in 2026 who wants to build an immutable primitive has to assemble the composite themselves, typically across 6-12 months of financing work, against term sheets they have to negotiate individually. A founder who wants to build a governed protocol with a token can run the Silicon Valley playbook in 6-8 weeks.

The financing asymmetry — not any design asymmetry — is what has kept the immutable-primitive track under-funded relative to the token-warrant track.

## 9. Is the 2024-2026 shift real?

The prior report made a stronger version of the "constraints are dissolving" claim than is supportable. This report argues for a narrower claim: the financing constraints are *mutating*, not loosening, and the mutation is still mostly selecting for token-shaped outcomes — with specific exceptions that deserve acknowledgment.

The regulatory backdrop matters here. Three 2023-2024 events bracketed the mutation: the SEC's 2023 action against Coinbase (and the resulting mid-2024 partial dismissal), the Binance DOJ settlement of November 2023 with a $4.3B penalty and Changpeng Zhao's personal plea, and Ripple's partial win in July 2023 (which distinguished institutional from programmatic sales). These created an environment in which token-distribution structures that could be framed as *not sales to US persons* and *not materially advertised in the US* were safer than pre-2023 direct-sale structures. The points-and-airdrops wrapper is partly a response to that regulatory geography — not only a user-friendly redistribution of upside.

### 9.1 Points programs

Blur pioneered the points model in late 2022-2023 as an NFT marketplace. Blast followed with native-yield L2 points. EigenLayer, Ether.fi, Renzo, and the liquid-restaking category scaled points to TVL-attractor tools — users farmed points to earn future token allocations ([Crypto.com, 2024](https://crypto.com/en/research/points-farming-may-2024); [Coindesk](https://www.coindesk.com/tech/2024/05/09/eigenlayers-eigen-airdrop-might-signal-demise-of-once-popular-points)).

Points are, mechanically, warrants-for-tokens held by users rather than by investors. The economic function is mostly identical: accrue a claim that will be satisfied at a later TGE, at an implicit valuation set by the protocol. The regulatory wrapping is different — points are not instruments that require securities registration under current interpretations — but the downstream economics are similar. Retail users accumulate points, the protocol launches a token, the points convert, the token trades down from launch valuation, and the distribution cycle completes. Ether.fi's ~$210M airdrop saw the token fall 25% immediately; this was not atypical ([Blockworks](https://blockworks.co/news/etherfi-airdrops-210m-token)).

The shift from "token warrant held by VC" to "points held by user" has redistributed some of the upside from insiders to early users. It has not changed the structural requirement that a token eventually launch to close the loop.

**Not all points programs are equivalent, however.** A meaningful subset are structurally different from warrant-rebrand versions:

- **Hyperliquid's HYPE airdrop** (November 29, 2024): no VC sale, no insider token allocation — the project was entirely self-funded by its team's own prior trading revenue. The airdrop distributed approximately 31% of total supply directly to users based on pre-TGE activity. The launch architecture is genuinely novel: a fully user-owned distribution with no VC warrants to satisfy. HYPE traded well above launch through 2025, partly because there were no insider unlock cliffs to process.
- **Bittensor's TAO emission schedule**: Yuma consensus subnet rewards are emitted to network participants with no VC allocation, no team pre-mine, and no early-investor tranche.
- **Arbitrum's ARB airdrop** (March 2023) and **Jupiter's JUP, Jito's JTO, LayerZero's ZRO** distributions all shipped with significant retroactive user allocations, though each also had pre-committed investor allocations alongside.

Hyperliquid and Bittensor in particular complicate the "early mutation, not regime reset" framing. They are genuinely new structures that did not exist in the 2020-2022 cycle. It is honest to acknowledge them as non-trivial counterexamples: the claim that every 2024-2026 wrapper is a warrant-in-disguise is too strong. A more accurate statement is that warrant-descendants remain the *majority* of 2024-2026 rounds, but a credible minority of projects have shipped without warrant-analogues and reached scale. Whether this minority grows or stays a niche is the open question for 2027-2029.

### 9.2 Airdrops as the ICO replacement

The 2024-2025 airdrop wave — Celestia, EigenLayer, Ether.fi, Hyperliquid, Aerodrome, Magic Eden, Blast, Friend.tech, and dozens of others — operates as a regulatory-arbitrage replacement for the ICO. The issuer distributes tokens to prior users based on activity metrics; no sale; no securities offering under current US interpretations; concurrent token-unlock schedules for insiders start ticking.

Friend.tech is the case study of an airdrop going catastrophically badly: FRIEND debuted at $3 and fell to $0.08 within months; founders walked with ~$44M in pre-airdrop revenue; Paradigm, the initial VC backer, declined its token allocation at launch, signaling loss of conviction ([DL News](https://www.dlnews.com/articles/defi/friend-tech-shuts-down-after-revenue-and-users-plummet/); [Cryptopragmatist](https://cryptopragmatist.com/p/rise-fall-friendtech-cautionary-tale-web3-developers)). The mechanism extracted value from users and insiders on the way up and left token-holders with approximately zero on the way down.

Farcaster's $150M raise in 2024 led by Paradigm ([Blockworks](https://www.blocmates.com/news-posts/farcaster-raises-150-million-powered-by-paradigm); [Coindesk](https://www.coindesk.com/tech/2024/05/21/farcaster-blockchain-based-social-media-startup-raises-150m-led-by-paradigm)) was a traditional equity round with warrant structure at a reported ~$1B valuation. The token has not launched as of this writing; the warrant remains overhanging.

### 9.3 "Real revenue" narratives

The narrative shift to "real revenue" — Pendle, Hyperliquid, Aerodrome, and others distributing actual protocol fees to token holders — is partly real and partly rebranding.

Real part: these protocols do generate fees (Hyperliquid ~$664M revenue in 2025; Pendle TVL peaked at $13.1B; Aerodrome is the largest DEX on Base by volume). The fee distribution is a legitimate economic phenomenon, not simulation.

Rebranding part: the token still sits at the center of value accrual. The mechanism (buy-and-burn, fee distribution to stakers) uses the token as the beneficiary. The token is still the asset VCs need to return to LPs. The design problem of "what is the token for" has an answer now ("fee claim"); the design problem of "why does this primitive need a token at all" has not been re-asked.

A primitive that genuinely did not need a token could simply run and distribute no fees to any token. That design is rare because the financing still pulls designers toward a token-beneficiary. What changed is the *justification* for the token (from "governance" to "fee claim"), not the presence of the token.

### 9.4 AI-agent protocols and the agentic economy

Bittensor (TAO) is the interesting outlier: it raised zero VC funding and operates a subnet-reward economy with Yuma consensus, first halving completed December 2025 ([The Block](https://www.theblock.co/post/353065/bittensor-tao-token-crypto-investors)). This is genuinely unusual. Most AI-agent protocols in the 2024-2026 wave (Virtuals, ai16z, AIXBT, and the dozens of launchpad-style frameworks) follow the standard warrant-based financing and launch tokens early.

40% of 2025 crypto VC capital flowed into AI-integrated projects ([OpenAIToolsHub](https://www.openaitoolshub.org/en/blog/ai-agent-crypto-tokens-guide)). The financing structures are — with Bittensor-style exceptions — the same structures as the previous cycle. The applications are different; the funding architecture is not.

### 9.5 MEV, orderflow, and infrastructure

Flashbots and the MEV infrastructure stack operate on a different financing model — equity-only, public goods grants, and research funding. The result is a category of infrastructure that is financed without token-warrant pressure and produces durable, upgradable research. This is the strongest counter-case to the token-warrant critique: some parts of the Ethereum stack are financed differently and ship differently.

Notably, the Flashbots model is adjacent to, not a replacement for, the protocol-layer financing question. You can run Flashbots as an equity-financed research operation because MEV extraction fees flow through a different path than protocol usage fees. For a pure DeFi primitive, the equity-only path has less to monetize.

### 9.6 The honest summary

The 2024-2026 shift has redistributed some of the token-economic upside from investors to users (points, airdrops). It has reduced, but not eliminated, the requirement that tokens launch to complete the financing loop (Hyperliquid and Bittensor are the important counterexamples). It has improved the justification-for-token narrative (fee claims are more coherent than governance-in-name-only claims). It has not produced a financing structure that funds the next Uniswap V2 without a token *as the default path*, and the historical base rate on financing-wrapper reform (10-20 years) says this is the expected outcome at a three-year vantage point.

The immutability drift therefore continues under new packaging, with nontrivial exceptions. The *rate* of the drift may have slowed — more designs now have credible paths to existence without tokens (Ajna, Morpho Blue, Liquity V2, some privacy primitives, MEV-adjacent infrastructure, Hyperliquid's no-VC model). The *direction* of the underlying selection pressure has not reversed at the population level. Token-compatible architectures still get more capital, faster, from more investors, with less friction than token-free architectures — but the gap has narrowed, and the narrowing is real.

Call the situation what it is: **early mutation, not regime reset**. That framing should guide expectations through 2028 or so; by 2029-2030, the base rate says a more structural shift becomes plausible.

## 10. The political-community critique and the steelman

### 10.1 Immutability-maximalism, Ethereum Classic, and the DAO fork

Immutability-maximalism taken to its terminus is the Ethereum Classic position: the blockchain's state transitions are sacred, the social layer has no business overriding them, and a community that reverses transactions to recover stolen funds has broken continuity-legitimacy in a way that can never be restored. ETC's position is coherent; it has also been economically marginal since the 2016 split, which is a real datapoint about what users revealed-preference when asked to choose between absolute immutability and the social layer's ability to intervene in extreme cases.

Ethereum's social contract is different, and worth naming explicitly: *plural governance at the social layer, minimized governance at the protocol layer*. The protocol does not have admin keys. The client implementations are plural. Upgrades proceed through a rough-consensus process among core developers, validators, and the community. The DAO fork of 2016 was a one-time exercise of collective judgment — the community concluded that *some* harm justified breaking continuity-legitimacy to preserve fairness-legitimacy (returning user funds from a specific exploit). Vitalik's defense of the fork was not that immutability is unimportant; it was that immutability is one legitimacy source among several, and in extreme cases other sources (fairness, participation, process) can outweigh it.

The implication for protocol design is that immutability at the application layer should be read similarly. Some primitives *should* be immutable because their specifications are small, known-correct, and the benefit of governance minimization exceeds the benefit of upgrade optionality. Others *should not*, because the specification is large, the correctness boundary is fuzzy, and the cost of a latent bug compounds over time. The position is not "immutability always wins." The position is "immutability is the right choice for the right specifications, and the financing regime made founders ship protocols that were often neither small nor known-correct enough for the design to be appropriate."

### 10.2 A steelman of the warrant regime

A disciplined argument for the warrant regime, which a committed opponent of this report's thesis would make:

**Bootstrapping necessity.** 2020-2021 liquidity mining — financed by VC warrants through the portfolio companies that ran the emission programs — was *the* mechanism that got DeFi from $1B aggregate TVL to $100B in roughly eighteen months. Without that mechanism, DeFi might be at $10B aggregate TVL today, institutional adoption might not have materialized, and the infrastructure the immutable protocols now ride on (aggregator layers, stablecoin liquidity, oracle networks) might not exist at scale. The warrant-funded emission programs produced externalities the immutable-primitive champions have benefited from. Dismissing this in a sentence is dishonest. A rigorous accounting would need to discount the survivor protocols' success by the infrastructure the warrant regime funded into existence.

**Revealed user preference.** Aave has over $40B in TVL; Liquity has about $1B. Users voted with capital. A champion of governance-minimization can argue that users didn't know better, or that they were emissions-incentivized into sticky positions. But the revealed-preference counter is real: over a five-year horizon, with open information, users allocated more capital to governance-rich upgradable protocols than to governance-minimized immutable ones. The normative claim that users *should* prefer immutable designs needs to grapple with the empirical fact that most didn't.

**Value allocation to users as the point.** Tokens are the mechanism by which crypto achieves something TradFi structurally cannot: early users participate in protocol upside. Without this mechanism, the capture pattern is straightforwardly extractive — Web2, where platform growth accrues entirely to shareholders and to the platform itself, with nothing flowing to the users who created the network effects. A warrant-driven token regime may be flawed in its execution, but eliminating the token layer entirely sacrifices the main crypto-specific alignment mechanism. The "tokens are rebranding" framing is too dismissive of what the token was supposed to do.

### 10.3 Where the steelman holds and where it doesn't

The bootstrapping argument holds on aggregate infrastructure benefits. It does not hold on the claim that any individual 2021-2022 warrant-financed launch was *necessary* — many were not, and their drawdowns are not a side effect of bootstrapping but a failure of design.

The revealed-preference argument holds on static comparison. It does not hold on dynamic adjustment: users allocating capital to Aave in 2022 were often doing so because Aave had better liquidity, integrations, and emissions — not because its governance surface was preferred. The preference order between "fewer moving parts" and "more emissions" is confounded, and the revealed-preference reading overstates the governance preference relative to the liquidity preference.

The value-to-users argument is the strongest. It is genuinely the case that without some token-economic mechanism, crypto becomes extractive in a Web2-analogous way. The counter is not "eliminate tokens" but "design them for users rather than for warrants." Hyperliquid's HYPE is closer to this than most 2021-2022 launches. The warrant regime failed not because it gave users tokens; it failed because it gave users diluted, unlock-cliff-governed tokens in exchange for protocol participation that was structured to primarily serve insider liquidity.

The steelman makes the thesis narrower, not wrong. The warrant regime produced real benefits (bootstrapping, some user value allocation); those benefits do not offset the specific structural distortions the regime introduced; and the mutation toward user-held points is a partial correction that the thesis should acknowledge rather than dismiss.

## 11. Scenarios and what this means for builders

### 11.1 Three scenarios for 2026-2029

**Scenario A: Continuity.** The token-warrant regime persists under new packaging (points, airdrops, "real revenue," AI tokens). Immutable primitives remain a niche funded by a composite of foundation grants, thesis-aligned seeds, RetroPGF, and eventual fee self-funding. Share of new-protocol cohort that ships as genuinely immutable: 5-10% of deployed designs; 2-5% of protocols that reach material TVL. Capital majority goes to governed, upgradable, token-compatible architectures.

**Scenario B: Partial reconfiguration.** Regulatory clarity (ICO-style direct token sales returning, as hinted at in some 2025-2026 discussions) reopens the ICO as a financing path, reducing the need for the SAFE+warrant wrapper and its associated pressure toward specific architectural shapes. Separately, large successful protocols operating on fee-revenue alone (Uniswap post-UNIfication, Hyperliquid, Morpho) demonstrate that token-denominated venture economics is not the only path, creating an alternative template. Share of new-protocol cohort that ships as genuinely immutable: 10-20%.

**Scenario C: Structural shift.** A successor fund structure emerges — one that accepts fee-revenue distribution, retroactive grants, and protocol royalties as primary return mechanisms rather than token liquidity events — and attracts meaningful institutional LP capital. Immutable primitives, which are the best fit for fee-revenue self-funding, become the default choice for a large fraction of the next cohort. Share: 25%+.

The base case is Scenario A with drift toward Scenario B. Scenario C requires financing innovations that are not yet operating at scale. The historical base rate on new fund structures is instructive: 10-15 years typically elapse between the triggering event for a new financing wrapper and that wrapper's emergence as a standard default. BDCs took roughly fifteen years after Drexel's collapse to emerge as a production-grade credit-distribution mechanism. Private credit took roughly a decade after the 2008 crisis to reach its current scale. If the 2022 token-warrant wind-down is the triggering event for Scenario C, the corresponding date for the new structure to emerge as a widely-accepted default is roughly 2032-2037. That does not rule out Scenario C; it rules out *Scenario C by 2029*.

### 11.2 What a founder in 2026 can actually do

If you are a founder considering an immutable primitive, the practical path is:

1. *Resolve the research question first, not the financing question first.* Teams that try to raise a token-compatible round and then decide post-hoc to ship an immutable design almost always end up with a hybrid. Teams that specify the primitive first, commit to immutability publicly, and then raise against that commitment end up with a smaller round and more design freedom.

2. *Start with a foundation grant.* EF, Optimism RPGF retroactively, Arbitrum grants, ecosystem-specific programs. These are small dollars but they let you ship a prototype without equity dilution or warrant overhang.

3. *Raise a thesis-aligned seed.* $1-3M from investors who will explicitly accept either no token or a narrow-scope token that does not govern core mechanics. The Liquity investor list (Polychain, a_capital, 1kx, Lemniscap, Robot Ventures, Alex Pack) is a good reference population. They exist. They are not the majority of the market, but they are a functioning minority.

4. *Structure the token (if any) at the periphery.* The ENS / Morpho Blue / Liquity V1 / Tornado Cash pattern: core mechanics fixed in bytecode; optional governance or incentive layer over peripheral parameters only. If the token does not exist at launch, set the timeline to 2+ years and use that time to prove the primitive works.

5. *Plan for self-funding via fees.* Set up the fee-capture mechanism at deployment, even if the fee is not activated. Post-deployment fee-switch activation (Uniswap's UNIfication) is a viable path to ongoing operations once the protocol scales.

6. *Be transparent about financing in the protocol's documentation.* The strongest governance-minimized protocols are the ones whose financing history is public and verifiable. Liquity's seed announcement is discoverable. Morpho's corporate structure is disclosed. Ajna's no-financing stance is stated explicitly. Opacity here is a signal to users that the financing has something to hide.

### 11.3 What a thoughtful LP should look for

LPs evaluating crypto funds in 2026 should ask different questions than they did in 2021. The question set below is sharper than most LP diligence packs. A fund that cannot answer these cleanly is not a fund whose legitimacy or return story is durable.

- **What fraction of the fund's cumulative DPI was realized before the six-month post-TGE mark?** A fund that returns most of its capital within six months of portfolio TGEs is a fund that systematically front-runs retail holders. If this fraction exceeds 60%, the fund's returns are extraction returns, not protocol-durability returns.
- **What is the fund's median hold period on liquid portfolio tokens?** A fund that sells within weeks of unlock does not believe in its portfolio; it believes in its token-launch timing. Ask for the distribution, not the average.
- **What fraction of portfolio companies have shipped as immutable or governance-minimized architectures?** A portfolio dominated by governance-heavy upgradable protocols is exposed to the same failure modes that produced the 2021-2022 drawdown. If the answer is zero, the fund is structurally a warrant-regime fund regardless of its stated thesis.
- **What specific discount does the fund apply to token-denominated marks versus fee-revenue-denominated marks?** Funds that cannot articulate a systematic discount between paper FDV and durable fee revenue do not have a methodology; they have a thesis in search of a return.
- **How does the fund vote in portfolio-protocol DAOs?** Funds that cast votes that accelerate their own unlocks, protect investor cliffs, or dilute community allocations are acting as extractors, not stewards. Ask for the voting record and evaluate it against the protocol's public interest.
- **What fraction of portfolio companies have explicitly published vesting schedules and insider cliff calendars?** Opacity on unlocks is a signal that the fund and its portfolio companies prefer information asymmetry over user alignment.

These questions are designed to distinguish funds that are genuinely different from the 2021-2022 vintage from funds that have rebranded. The earlier version of these LP questions was too polite; it selected against the most egregious warrant-regime behaviors but did not select for the structural alternatives this report argues are possible. This version is deliberately more adversarial.

### 11.4 The wider claim

The token-warrant regime was not a failure of individual GPs, founders, or LPs. It was a successful adaptation of a venture-capital framework to a new asset class, which produced durable venture returns for some vintages and catastrophic ones for others. Its effect on Ethereum's application layer was to bias the population of protocols toward token-compatible architectures — which is to say, partially away from the governance-minimized, deploy-once-and-walk-away primitives that the stack is genuinely good at hosting. Some warrant-era protocols (Aave, Lido, Hyperliquid, Pendle) produced durable usage anyway; many did not. The bias shifted the population distribution, not individual outcomes.

The question for 2026-2029 is not whether token warrants will disappear. On the historical base rate for financing wrappers, they won't — they will mutate. The question is whether a parallel financing regime — one structured around fee-revenue, retroactive grants, credible plural-ownership structures, and long-horizon patient capital — will grow large enough to fund a *meaningful minority* of the primitives that the warrant regime structurally underfunds. If it does, Ethereum's next cohort of applications will have a different distribution than the last. If it doesn't, the immutability drift will continue — slower, with more exceptions, under new labels, but in substantially the same direction.

The narrow claim of this report is that the drift was real, that it was caused in large part by a specific and describable financing instrument, and that the instrument's successors are still doing most of the selecting today. The wider claim, which the evidence supports but does not prove, is that Ethereum's durable value-producing applications have disproportionately come from the narrow population of teams that escaped or resisted the financing regime — and that the most important open question for the next cycle is whether that population can be made larger, deliberately, through financing innovations that match the architecture of the primitives the stack should be producing.

A final note on scope. This report covers a partial set of the protocols that deserve engagement; 1inch, dYdX, GMX, Synthetix, Pendle, and Aerodrome each merit their own case-level analysis under the framework developed here, and each would complicate the thesis in specific ways worth investigating. The decision to focus on the ten cases in §6 reflects the availability of clean financing histories rather than any claim that these are the only cases that matter. The framework — architecture follows financing, with lags and exceptions — should be applied to those additional protocols in future work, not taken as closed.

---

## Data Sources & Methodology

This report is a synthesis of public primary and secondary sources. Financial data is sourced from industry analytics (Messari, DefiLlama, PitchBook compilations via The Block and Galaxy Research, Tokenomist, ICO Drops, CryptoRank) and from the operating entities of the protocols discussed (Uniswap Labs, Morpho Labs, Liquity, EF, Polychain, Paradigm). Legal analysis is sourced from court filings, SEC enforcement announcements, and practitioner commentary (Cooley, Mayer Brown, Jones Day, BCLP). Case study material is sourced from protocol documentation (morpho.org, liquity.org, reflexer docs, Uniswap blog), governance records (Morpho forum, Uniswap Agora, RetroPGF documentation), and the project-specific whitepapers where cited.

Where single-source claims could not be independently verified, the report either omitted them or flagged them inline. The quantitative record in Section 7 is directional rather than exhaustive — a complete dataset of the 2021-2022 VC vintage token-performance record does not exist in clean public form, and the specific FDV figures for Sui, Aptos, Celestia, EigenLayer, and Berachain are reported as of the times given in their tokenomist.ai snapshots rather than as precise real-time measurements. The "fewer than 20% of 2021-2022 tokens trade above TGE" estimate in §7.2 is directional and assembled from cross-referenced snapshots rather than from a primary survivorship-adjusted dataset; it should be treated as a useful order of magnitude only.

The thesis — that the token-warrant financing structure acted as a selection filter on protocol architectures — is argued case-by-case rather than asserted as empirical fact. Survivorship bias applies to the positive cases cited. The counterfactuals in Section 8 are reasoned, not observed. The financing-wrapper base-rate framing in Section 3 draws on standard financial-history summaries (Milken/Drexel; the 1998-2000 IPO window; CDO/CDS literature; SPAC-cycle analyses); those frames are intended to put the crypto-specific pattern on a longer horizon, not to equate crypto's specific mechanics with any of the named prior cycles.

Protocol revenue and trading volume are distinct metrics throughout. Where the report refers to "fee revenue" it means the protocol's actual fee capture; where it refers to "volume" it means notional value of transactions. The Uniswap $4T figure is volume, not fees; Hyperliquid's $664M is revenue, not volume. Readers should not substitute one for the other.

Several inline "verify against" flags (UNI vote exact percentage, MIP-75 number, specific Liquity Series A round lead) direct readers to primary sources for precision. Those flags are intentional — the report prefers directional accuracy with clear verification pointers over false precision.

## Sources

### Regulatory and legal
- [SEC, June 2020 — Telegram to Return $1.2B to Investors and Pay $18.5M Penalty](https://www.sec.gov/newsroom/press-releases/2020-146)
- [Cooley, May 2020 — SEC v. Telegram: Key Takeaways and Implications](https://www.cooley.com/news/insight/2020/2020-05-07-sec-v-telegram-key-takeaways-implications)
- [Jones Day, October 2020 — Court Rules Digital Token Sales Illegal (Kik)](https://www.jonesday.com/en/insights/2020/10/court-rules-that-sales-of-digital-tokens-were-illegal-unregistered-securities-offerings)
- [Coindesk, April 2020 — With Kik and Telegram Cases, the SEC Tries to Kill the SAFT](https://www.coindesk.com/markets/2020/04/28/with-kik-and-telegram-cases-the-sec-tries-to-kill-the-saft)
- [Mayer Brown, December 2024 — Federal Appeals Court Tosses OFAC Sanctions on Tornado Cash](https://www.mayerbrown.com/en/insights/publications/2024/12/federal-appeals-court-tosses-ofac-sanctions-on-tornado-cash-and-limits-federal-governments-ability-to-police-crypto-transactions)

### Token warrant and SAFE mechanics
- [Hashed EM, 2022 — SAFE + Token Warrants: Early-Stage Crypto Fundraising Guide](https://hashedem.substack.com/p/safe-token-warrant-meta)
- [DaosPv, 2023 — Token Warrants, SAFEs, and SAFTs: A Founder's Guide](https://blog.daospv.com/token-warrants-safes-and-safts-a-founders-guide-to-web3-fundraising-instruments/)
- [CRV, 2026 — SAFE Agreements for Startups](https://www.crv.com/content/safe-agreements-for-startups)
- [Wyrick Robbins — SAFE Financing: Valuation Cap vs. Discount Variants](https://www.wyrick.com/news-insights/safe-financing-valuation-cap-vs-discount-variants)
- [Capbase — Most Favored Nation (MFN) Clause in Startup Investing](https://capbase.com/most-favored-nation-mfn-clause-in-startup-investing-what-it-is-and-how-it-works/)
- [BCLP — Is There Life for SAFTs After the Telegram Case?](https://www.bclplaw.com/en-US/events-insights-news/is-there-life-for-safts-after-the-telegram-case.html)

### VC fund structure and LP dynamics
- [Cambridge Associates — Unlocking Innovation with Blockchain and Crypto VC Funds](https://www.cambridgeassociates.com/insight/blockchain-and-crypto-vc-funds/)
- [The Block, January 2025 — The Funding: Crypto VC Recap 2024](https://www.theblock.co/post/332539/the-funding-crypto-vc-recap-2024)
- [Blockworks, 2022 — Crypto VC 2021 Record Year](https://blockworks.co/news/crypto-vc-2021-record-year)
- [Galaxy Digital, 2022 — Crypto Venture Capital Report](https://www.galaxy.com/research)
- [ChainCatcher — a16z crypto fund management scale plummets by 40%, Multicoin cut in half](https://www.chaincatcher.com/en/article/2259439)
- [Fortune, April 2026 — Top Crypto VCs See Portfolio Values Shrink](https://fortune.com/2026/04/16/top-crypto-vcs-paradigm-pantera-a16z-multicoin-haun-dragonfly/)
- [insights4vc, 2024 — Crypto Venture Capital Trends](https://insights4vc.substack.com/p/2024-crypto-venture-capital-trends)
- [insights4vc, Q1 2025 VC Report](https://insights4vc.substack.com/p/q1-2025-vc-report-inside-us-and-crypto)
- [Cointelegraph — Crypto VC Funding Doubled in 2025 as RWA Tokenization Took the Lead](https://cointelegraph.com/research/crypto-vc-funding-doubled-in-2025-as-rwa-tokenization-took-the-lead)
- [CryptoRank, Q1 2025 — State of Venture Capital in Crypto](https://cryptorank.io/insights/reports/crypto-fundraising-report-Q1-25)
- [Crypto Fundraising — Q1 2026 Report: $6.81B Across 222 Rounds](https://crypto-fundraising.info/blog/q1-2026-crypto-fundraising-report/)
- [Carta, Q2 2025 — State of Pre-Seed](https://carta.com/data/state-of-pre-seed-q2-2025/)

### Protocol cases
- [Uniswap Labs, September 2020 — Introducing UNI](https://blog.uniswap.org/uni)
- [Uniswap Labs, December 2025 — UNIfication](https://blog.uniswap.org/unification)
- [Blockworks, 2025 — Uniswap Finally Turns the Fee Switch](https://blockworks.co/news/uniswap-fee-switch)
- [Coindesk, December 2025 — Uniswap Token Burn Moves Closer to Reality](https://www.coindesk.com/markets/2025/12/22/uniswap-token-burn-moves-closer-to-reality-as-99-of-voters-in-favor-of-fee-switch-proposal)
- [Liquity, September 2020 — $2.4M Seed Funding Led by Polychain](https://www.liquity.org/blog/liquity-protocol-raises-2-4m-in-seed-funding-led-by-polychain-capital)
- [The Block, 2020 — Liquity Secures $2.4M in Seed Funding](https://www.theblock.co/linked/78795/liquity-ethereum-lending-seed-funds)
- [morpho.org, 2024 — Morpho Blue and How It Enables Our Vision for DeFi Lending](https://morpho.org/blog/morpho-blue-and-how-it-enables-our-vision-for-defi-lending/)
- [DL News — Crypto Lender Morpho Bucks DeFi Trend as Nonprofit Absorbs Development Team](https://www.dlnews.com/articles/defi/morpho-nonprofit-to-absorb-development-team/)
- [The Defiant — Morpho Restructures to Align Token Value with Company Equity](https://thedefiant.io/news/defi/morpho-restructures-to-align-token-value-with-company-equity)
- [Cryptorank — Maker (MKR) Funding Rounds](https://cryptorank.io/ico/maker)
- [Gemini Cryptopedia — MakerDAO: The DeFi and DAI Crypto Pioneer](https://www.gemini.com/cryptopedia/makerdao-defi-mkr-dai-coins)
- [MakerDAO Blog, September 2018 — a16z Buys 6% of All MKR Tokens](https://blog.makerdao.com/2018/09/a16z-buys-6-of-all-mkr-tokens/)
- [Bingx — What Is Aave Crypto Lending](https://bingx.com/en/learn/article/what-is-aave-crypto-lending-and-how-does-it-work)
- [Cointelegraph — TORN soars 200%](https://cointelegraph.com/news/torn-soars-200-as-tornado-cash-s-governance-token-becomes-tradable)
- [Crypto Briefing — Tornado.cash Suggests Token Release](https://cryptobriefing.com/tornado-cash-token-release-airdrop/)
- [Coindesk, November 2021 — Curve Wars Heat Up](https://www.coindesk.com/business/2021/11/11/curve-wars-heat-up-emergency-dao-invoked-after-clear-governance-attack)
- [The Defiant — Governance of Curve in Flux as Convex Exerts Control](https://thedefiant.io/curve-convex-governance-crv-control/)
- [Pontem — Curve and the Convex Wars](https://pontem.network/posts/curve-and-the-convex-wars-2)
- [Cyfrin — How Did the Euler Finance Hack Happen](https://www.cyfrin.io/blog/how-did-the-euler-finance-hack-happen-hack-analysis)
- [BlockSec — Euler Finance Incident: The Largest Hack of 2023](https://blocksec.com/blog/euler-finance-incident-the-largest-hack-of-2023)
- [Chainalysis — Euler Finance Flash Loan Attack Explained](https://www.chainalysis.com/blog/euler-finance-flash-loan-attack/)
- [The Block — Ajna Launches Oracle and Governance-Free Lending Protocol](https://www.theblock.co/post/239363/ajna-oracle-governance-lending-protocol-ethereum)
- [Ajna Protocol Whitepaper 2023](https://www.ajna.finance/pdf/Ajna_Protocol_Whitepaper_10-19-2023.pdf)
- [Reflexer Docs — PID Failure Modes & Responses](https://docs.reflexer.finance/risk/pid-failure-modes-and-responses)
- [PM Wiki — Augur](https://pm.wiki/projects/augur)
- [Augur Wikipedia](https://en.wikipedia.org/wiki/Augur_(software))

### Tokenomics data
- [Tokenomist — Sui Tokenomics & Vesting](https://tokenomist.ai/sui)
- [Tokenomist — Aptos Tokenomics](https://tokenomist.ai/aptos)
- [Tokenomist — EigenLayer/EigenCloud](https://tokenomist.ai/eigenlayer)
- [Tokenomist — Berachain](https://tokenomist.ai/berachain-bera)
- [Cryptonews — Celestia Tokenomics Crisis](https://cryptonews.com/news/celestia-tokenomics-crisis-tia-plunges-90-after-aggressive-unlocks-is-airdrop-hype-to-blame/)
- [Unchained — Polychain Made $80M Selling Celestia Staking Rewards](https://unchainedcrypto.com/polychain-made-80-million-from-selling-celestia-staking-rewards/)
- [The Block — Polychain Sells Remaining $62.5M TIA Stake](https://www.theblock.co/post/364155/polychain-sells-remaining-62-5-million-tia-stake-to-celestia-foundation-amid-criticism-of-staking-reward-sales)
- [AINvest — Token Supply Dynamics September 2025](https://www.ainvest.com/news/token-supply-dynamics-september-2025-evaluating-market-impact-sui-4-5-billion-unlock-ftn-aptos-arbitrum-2508/)
- [DefiLlama — Hyperliquid](https://defillama.com/protocol/hyperliquid)
- [DefiLlama — Pendle](https://defillama.com/protocol/pendle)
- [DefiLlama — Holders Revenue by Protocol](https://defillama.com/holders-revenue)
- [Coindesk, November 2025 — Hyperliquid Introduces Proposal to Cut Fees](https://www.coindesk.com/tech/2025/11/19/the-protocol-hyperliquid-introduces-proposal-to-cut-fees)

### Points, airdrops, post-token-warrant cases
- [Crypto.com, May 2024 — Points Farming: Development, Significance and Controversies](https://crypto.com/en/research/points-farming-may-2024)
- [Coindesk, May 2024 — EigenLayer's EIGEN Airdrop Might Signal Demise of Once-Popular Points](https://www.coindesk.com/tech/2024/05/09/eigenlayers-eigen-airdrop-might-signal-demise-of-once-popular-points)
- [Blockworks — Ether.fi Begins Up to $210M Airdrop, Token Falls 25%](https://blockworks.co/news/etherfi-airdrops-210m-token)
- [DL News — Friend.tech Creators Walk Off with $44M as Project Shuts Down](https://www.dlnews.com/articles/defi/friend-tech-shuts-down-after-revenue-and-users-plummet/)
- [Cryptopragmatist — The Rise and Fall of FriendTech](https://cryptopragmatist.com/p/rise-fall-friendtech-cautionary-tale-web3-developers)
- [Coindesk — Farcaster Raises $150M Led by Paradigm](https://www.coindesk.com/tech/2024/05/21/farcaster-blockchain-based-social-media-startup-raises-150m-led-by-paradigm)
- [The Block — Paradigm Leads $150M Raise for Farcaster](https://www.theblock.co/post/295700/paradigm-150-million-raise-web3-social-platform-farcaster)
- [The Block — The Funding: Why Bittensor Is Drawing in Crypto Investors](https://www.theblock.co/post/353065/bittensor-tao-token-crypto-investors)
- [OpenAIToolsHub — AI Agent Crypto Tokens Guide](https://www.openaitoolshub.org/en/blog/ai-agent-crypto-tokens-guide)

### Funding infrastructure and public goods
- [ethereum.org — Grants Programs](https://ethereum.org/community/grants/)
- [The Block, 2025 — EF Pauses $3M Open Grants Program](https://www.theblock.co/post/368804/ethereum-foundation-pauses-grants-programs-as-it-looks-to-cut-burn-rate)
- [The Block — EF Swaps 5,000 ETH Into Stablecoins](https://www.theblock.co/post/396728/ethereum-foundation-swaps-5000-eth-stablecoins-operational-grant-funding)
- [RetroPGF — Shaping Tomorrow by Rewarding Yesterday](https://www.retropgf.com/)
- [Optimism — Retroactive Public Goods Funding](https://medium.com/ethereum-optimism/retroactive-public-goods-funding-33c9b7d00f0c)
- [ETH Daily — Optimism Announces 2024 RetroPGF Rounds](https://ethdaily.io/429)
- [Uniswap Foundation — Grants](https://www.uniswapfoundation.org/grants)
