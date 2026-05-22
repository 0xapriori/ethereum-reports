---
title: "Stablecoins: From Pegged IOUs to Ungovernable Money"
date: 2026-05-22
---

# Stablecoins: From Pegged IOUs to Ungovernable Money

*The design space, the graveyard, the fork-choice problem, and the flatcoin frontier — and why the most successful stablecoins gave up nearly everything crypto was for.*

---

## tl;dr

- **The dream was bearer dollars without a banker. We mostly built bankers without the bearer dollars.** The two coins that won — USDT ($189.5B) and USDC ($76.8B) — are **82.7% of a $321.9B market** (DefiLlama, 2026-05-22) and are permissioned IOUs with corporate freeze switches, bank-dependent reserves, and KYC-gated redemption. Fiat-backed stables are a *distribution* success, not an *innovation*. The innovation lives in the immutable, crypto-collateralized line: SAI → LUSD → RAI → the flatcoin frontier.
- **The design space has three families and one trilemma.** Crypto-overcollateralized (BitUSD, SAI/DAI, LUSD), fiat-backed (USDT, USDC), and undercollateralized/algorithmic (Basis, ESD, UST). You can pick at most two of {decentralized, capital-efficient, robustly pegged}. Every blowup is a system that pretended otherwise.
- **MakerDAO is the cautionary arc, not a hero story.** ETH-only SAI (2017) → multi-collateral DAI (2019) → the Peg Stability Module (2020) → ~60% USDC-collateralized by 2022. The most successful "decentralized" stable progressively became wrapped USDC, and after the SVB depeg its governance *voted ~79% to keep USDC anyway.* Decentralization decays under the gravity of capital efficiency.
- **The failure modes are a clean taxonomy, not random bad luck.** Algorithmic reflexivity/death spiral (UST, IRON/TITAN, the seigniorage wave), collateral risk (fUSD, Frax↔SVB), bank-run/coordination failure (IRON), governance/oracle exploit (Beanstalk, IRON's TWAP), and yield-subsidy unsustainability (Anchor's 20%, OHM's rebase APY). Durable pegs need exogenous uncorrelated collateral + a redemption mechanism that closes the arbitrage under stress + organic demand. Every casualty was missing at least one.
- **Haseeb called the fork-choice problem in 2019; the Merge proved it in 2022.** The canonical essay is Qureshi & Lee, *"Ethereum is now unforkable, thanks to DeFi"* (Oct 31, 2019) — written about a *hypothetical* ProgPoW fork, not ETHPoW. The claim: in a contentious fork, Circle/Tether decide which chain their stable is redeemable on, DeFi composability forces everyone to follow, and that becomes the Schelling point. The Sept 2022 ETHPoW fork was the live experiment: Circle and Tether pre-committed to PoS-only, and ETHW collapsed to single digits. Two private companies' redemption policies ratified canonical Ethereum.
- **The incumbents' failings are now empirically proven on both ends.** USDC inherited a bank run (SVB, March 2023, traded to $0.8774 with $3.3B trapped). Tether has never published a full audit and was fined by the CFTC for being backed only 27.6% of the time in a 2016–2018 sample. And on censorship: the courts found Tornado Cash's *code* couldn't be sanctioned (Fifth Circuit 2024 → delisting March 2025), yet the centralized stables flowing through it *were* frozen instantly — on a sanction that didn't survive review. The protocol was more censorship-resistant than the "dollars."
- **Regulation is entrenching the centralized duopoly, not correcting it.** The GENIUS Act (signed July 18, 2025) rewards the USDC/Tether model — 100% cash/Treasury reserves, licensed issuers, monthly disclosure — and *bans paying yield to holders*, which structurally disadvantages decentralized designs. MiCA did the same in the EU and pushed USDT off regulated venues.
- **The flatcoin frontier is where the original dream survives — but the scaling question is unsolved.** "Flatcoin" has a strict sense (pegged to purchasing power / a CPI basket — Balaji 2021 → Coinbase 2023 → Vitalik) and a loose sense (decentralized, non-fiat-pegged stable-value money — RAI). The genuinely immutable, censorship-resistant designs today are **LUSD (Liquity v1)** and **RAI**; everything else is decentralized-but-governed (BOLD, crvUSD, GHO) or not decentralized at all (frxUSD, Ethena USDe). The frontier exemplar is **ZAI on Zcash** — an oracle-free, ZEC-collateralized, shielded-native CDP — but it is a *simulation/research proposal, not deployed.* The open problem all of them share: you cannot have decentralized + capital-efficient + scalable at once.

---

## Contents

1. [The Design Space: What Were We Actually Trying to Build?](#1-the-design-space)
2. [The Graveyard: Failure Modes as a Taxonomy](#2-the-graveyard)
3. [The Fork-Choice Problem: Who Decides Canonical Ethereum?](#3-the-fork-choice-problem)
4. [The Failings of the Incumbents (USDC / Tether)](#4-the-failings-of-the-incumbents)
5. [The Flatcoin Frontier: Ungovernable Money That Actually Works](#5-the-flatcoin-frontier)
6. [Sources](#6-sources)
7. [Data Notes & Caveats](#7-data-notes--caveats)

---

## 1. The Design Space
### What were we actually trying to build?

A stablecoin is an attempt to put a *stable unit of account* on a permissionless ledger. The disagreement — the entire history of the field — is over **what you trust to keep it stable.** That trust choice sorts every design into one of three families.

**Crypto-overcollateralized (the immutable line).** Lock volatile crypto worth more than the stable you mint; defend the peg with liquidations and redemption arbitrage.
- **BitUSD on BitShares** — July 21, 2014, the first stablecoin (Larimer/Hoskinson). Behaved more like a volatile margin instrument and lost its peg in 2018.
- **MakerDAO Single-Collateral Dai (SAI)** — whitepaper Dec 2017; ETH-only collateral. This is the genuine innovation in the lineage: a dollar you could mint against ETH with no issuer, no bank, no redemption desk.
- **Multi-Collateral Dai (MCD)** — Nov 18, 2019; the original token renamed SAI.

**Fiat-backed (the custodial line).** Hold dollars (or T-bills) in a bank; issue a token that's a claim on them.
- **Tether (USDT)** — launched as "Realcoin" July 2014 (renamed Tether Nov 2014). **USDC (Circle/CENTRE)** — announced May 2018, launched Sept 26, 2018.
- These are the winners by size, and the point worth making bluntly: they are not a crypto innovation. They are a wrapper around the existing banking system. The interesting properties of crypto — bearer settlement, censorship resistance, permissionlessness — are exactly the ones they give up.

**Undercollateralized / algorithmic (the seigniorage line).** Back the coin with nothing exogenous — use a reflexive sister token and supply expansion/contraction to chase the peg.
- Robert Sams' "Seigniorage Shares" paper (2014) is the intellectual root; **Basis/Basecoin** the famous early attempt (raised $133M, shut down Dec 13, 2018 over securities concerns); **Empty Set Dollar, Frax (fractional)** the DeFi-era descendants; **UST** the catastrophic apex.

### The trilemma

You can have at most two of: **decentralized**, **capital-efficient**, **robustly pegged**.
- Fiat-backed: capital-efficient + robustly pegged, *not* decentralized.
- Overcollateralized CDPs: decentralized + robustly pegged, *not* capital-efficient (you must lock $150+ to mint $100).
- Algorithmic: decentralized + capital-efficient, *not* robustly pegged (and the graveyard is the proof).

Haseeb Qureshi's foundational taxonomy essay frames the peg itself as a **Schelling point** — "if enough people believe the system will survive, that belief can lead to a virtuous cycle." That insight is the hinge of the whole analysis: pegs are confidence games, and the design question is whether the mechanism *adds* confidence under stress or *consumes* it.

### The MakerDAO arc — the cautionary tale, not the victory lap

This is the most important thread in the history, because it shows decentralization *decaying* under economic gravity:

- **Sept 2020:** even before the PSM scaled, DAI was already ~61% backed by centralized assets (~52% from centralized stablecoins), because stablecoin collateral needs only ~$101 to mint 100 DAI vs ~$150 in ETH.
- **Late 2020:** the **Peg Stability Module (MIP29)** lets anyone swap USDC↔DAI 1:1. Great for the peg; mechanically pulls USDC onto Maker's balance sheet.
- **2021–22:** USDC-backing climbs past 50% via the PSM, peaking around **~60% USDC-collateralized by Aug 2022** (Messari). Critics start calling DAI "wrapped USDC."
- **March 2023 (the test):** USDC depegs in the SVB crisis with ~$3B sitting in Maker's PSM. Governance **votes ~79% to keep USDC as the primary reserve anyway.** Rune later concedes it's "almost inevitable" DAI abandons the strict USD peg given this dependence.

The lesson: capital efficiency is a one-way ratchet toward centralization unless you deliberately weld the door shut — which is exactly what LUSD does (§5).

### Vitalik's two tests (the evaluation lens)

From *"Two thought experiments to evaluate automated stablecoins"* (May 25, 2022, written days after Terra):
- **Test 1 — graceful wind-down:** *"can the stablecoin, even in theory, safely 'wind down' to zero users?"*
- **Test 2 — negative rates:** *"what happens if you try to peg the stablecoin to an index that goes up 20% per year?"* — i.e., can the system express a negative interest rate honestly rather than papering it over with token printing?

He praised RAI as the "ideal type" — ETH-only, floating target — and, in *"What in the Ethereum application ecosystem excites me"* (Dec 5, 2022), planted the flatcoin seed: a governance-minimized coin could track *"a global average CPI index"* and advertise *"abstract best-effort price stability,"* with lower regulatory risk because it isn't pretending to be a digital dollar.

---

## 2. The Graveyard
### Failure modes as a taxonomy, not a list of disasters

Don't read these chronologically — sort them by *failure category.* There are five, laid out at the end.

### Terra UST / LUNA — the canonical death spiral (May 2022)

- **Mechanism:** $1 algorithmic peg with no exogenous collateral; maintained by mint/burn arbitrage with the volatile sister token LUNA (burn $1 of LUNA → mint 1 UST, and vice versa). "Backing" = the market cap of LUNA itself.
- **Demand driver:** Anchor Protocol's ~19.5% APY on UST, structurally subsidized (deposits >> borrowing); LFG topped up the reserve with $450M in Feb 2022.
- **Failure:** when UST depegged and stayed there, redemptions printed astronomical new LUNA — supply went from ~343M (May 9) to ~6.53 trillion in about a week — hyperinflating and destroying the very backstop meant to absorb the run. LFG's BTC reserve was drained from 80,394 BTC to 313 BTC defending it.
- **Dates:** depeg May 9; chain halted May 13, 2022. LUNA fell from a $119.51 ATH to ~zero.
- **Scale:** ~$40–50B notional value destroyed (value wiped, not user clawback).
- **Lesson:** a peg "backed" by its own reflexive token has no floor; the stabilizer *accelerates* collapse. A 20% yield that manufactures demand is a liability, not demand.

### OlympusDAO (OHM) — *not a stablecoin* (2021–2022)

- **Framing correction:** OHM was never pegged. It was a free-floating "reserve currency" with a *floor* of 1 DAI of backing, routinely trading at a large premium above backing.
- **Mechanism:** bonding (sell assets/LP to the protocol for discounted vesting OHM → builds Protocol-Owned Liquidity) + staking (rebase rewards at thousands-of-percent APY, funded by dilution). The "(3,3)" meme: if everyone stakes, everyone wins; the reflexive loop pumps on the way up and reverses violently on the way down.
- **History:** launched March 23, 2021; **ATH $1,415 (Apr 25, 2021)** per CoinGecko; fell to a low of **~$7.54 (Nov 26, 2022)**; ~$19 today. (Heavy rebasing acts like repeated stock-splits, so raw price comparisons are distorted — but the CoinGecko raw figures here are the cleanest reference.)
- **Lesson:** the cleanest example that this isn't a *peg* problem at all — it's a reflexive-Ponzi problem that afflicts any emissions-funded "money." The 1 DAI floor protected backing, not market price; everyone who bought the premium lost.

### Fantom fUSD — overcollateralized, yet depegged *and stayed depegged*

- **The name:** it's **fUSD** — Fantom's overcollateralized, DAI-style CDP minted against staked FTM, in the Andre Cronje–adjacent ecosystem. *Not* an algorithmic mint/burn coin.
- **Failure (the instructive part):** the redemption/liquidation path back to $1 didn't function effectively — there was no reliable way to redeem fUSD for $1 of value — so when FTM collapsed (post-Terra contagion, 2022) the peg broke and *had no force pulling it back.* It fell to roughly **$0.68–0.69 during the May 2022 depeg** and traded below peg for an extended period after. Cronje proposed an fUSD "optimization" on May 20, 2022 specifically to address the entrenched depeg.
- **Lesson:** overcollateralization is *necessary but not sufficient.* If the arbitrage-closing mechanism is broken or undercapitalized, a coin can sit below peg indefinitely. And backing a stable with a single, highly-correlated ecosystem token means collateral and demand collapse together.

### Iron Finance — IRON / TITAN: "DeFi's first large-scale bank run" (June 16, 2021)

- **Mechanism:** partially-collateralized stable on Polygon, ~75% USDC + 25% TITAN (the reflexive share token), governed by dynamic target/effective collateral ratios. Redeeming IRON mints fresh TITAN.
- **Failure:** as TITAN fell, holders redeemed and dumped TITAN; the 10-minute TWAP oracle lagged the crashing spot price, so redeemers got TITAN valued at the stale high price → more minting → lower price → panic. Rational holders were *incentivized to run* even though "the system worked as designed."
- **Scale:** TITAN → ~$0 (supply hyperinflated into the tens of thousands of billions); ~$2B value wiped (estimate). Mark Cuban was an investor, confirmed losses, and called for stablecoin regulation.
- **Lesson:** partial collateral + reflexive share token = the same spiral as pure-algo, just delayed by the USDC cushion. Oracle design is part of the attack surface. This was the dress rehearsal for Terra 11 months later — the Fed wrote it up alongside UST.

### The seigniorage wave — Basis Cash, ESD, DSD (late 2020 – 2021)

- **Mechanism:** rebase/bond designs. Above $1, mint new coin to shareholders (seigniorage); below $1, sell discount "bonds/coupons" (burning supply) redeemable later *if* the peg recovers.
- **Why they all died:** the contraction mechanism is a pure confidence bet — coupon buyers only show up if they believe the peg will recover; once it stays broken, they vanish, supply can't contract, peg stays dead. ESD issued ~$550M at peak → ~$0.23; DSD ~$300M → ~$0.24; Basis Cash never durably held peg (its co-founder was later revealed to be Do Kwon — a tidy bit of foreshadowing).
- **Lesson:** uncollateralized seigniorage needs *perpetual net new demand;* the stabilizer evaporates exactly when needed. **NuBits (2014–2018)** is the proof this pattern predates DeFi — same two-token design, broke in 2016, terminally failed March 2018.

### Honorable mentions (one-liners that expand the taxonomy)

- **Beanstalk (April 2022):** ~$1B Aave flash loan → bought >67% of governance in one block → passed a malicious proposal draining ~$182M (attacker netted ~$80M). *Governance is part of the attack surface; unaudited, no flash-loan/timelock protection.*
- **Wonderland / MIM / "Sifu" (Jan 2022):** the "decentralized" treasury manager was revealed to be a QuadrigaCX co-founder. *Key-man and founder-fraud risk in supposedly trustless systems; correlated-token contagion.*
- **Frax near-miss (March 2023):** heavily USDC-collateralized, depegged to ~$0.877 when USDC did; subsequently moved to full collateralization. *Even "careful" fractional-algo inherits its reserve's weakest asset.*

### Synthesis — the five failure categories

1. **Algorithmic reflexivity / death spiral** — UST, IRON/TITAN, ESD/DSD/Basis, NuBits. The dominant killer.
2. **Collateral risk** — fUSD (correlated, broken redemption), Frax↔SVB (off-system shock).
3. **Bank-run / coordination failure** — IRON, the textbook case.
4. **Governance / oracle exploit** — Beanstalk (flash-loan governance), IRON (lagging TWAP), Wonderland (treasury control).
5. **Yield-subsidy unsustainability** — Anchor's 20%, OHM's rebase APY.

**The through-line:** durable pegs need (a) exogenous, uncorrelated collateral; (b) a redemption mechanism that closes the arbitrage *under stress*; (c) organic demand not propped by unsustainable yield. The algorithmic casualties were missing all three.

---

## 3. The Fork-Choice Problem
### Who decides canonical Ethereum?

### The actual source — and the correction

The canonical essay is **Haseeb Qureshi & Leland Lee, *"Ethereum is now unforkable, thanks to DeFi"* (Oct 31, 2019).** Two things to get right:
1. It's co-authored with Leland Lee.
2. It was written **~3 years before the Merge,** framed around the *then-active, hypothetical* **ProgPoW** mining dispute — not ETHPoW. The argument is structural, and the Merge later became its real-world confirmation. That is the sharper framing: a 2019 prediction about a hypothetical fork, validated by a 2022 fork nobody was thinking about when he wrote it.

### The argument, precisely

Thesis: *"Ethereum will never again have a meaningful minority fork, in large part because of DeFi's inherent fragility."* The mechanism is composability + centralized chokepoints, with USDC as the keystone:
- CENTRE announces USDC will only be redeemable on one fork.
- *"All DeFi operators are forced to now follow USDC's lead. They cannot defy CENTRE… Composability both rules and constrains everything."*
- *"DeFi operators would have no choice but to side with CENTRE and throw all of their weight behind the USDC-blessed fork, regardless of where community opinion came down."*
- Supporting cascade: oracles stop posting on the minority fork (*"There are no more prices"*), and Maker would *"simply trigger global settlement"* on it.

The issuer's redemption choice becomes the Schelling point. Coordination incentives do the rest.

### Vitalik conceded the same point (Aug 2022)

At BUIDL Asia (Seoul, ~Aug 4–5, 2022), weeks before the Merge: you'll have *"100 billion of USDT on one chain and 100 billion of USDT on the other chain… and so, they [Tether] need to stop respecting one of them."* He called it a 5–10-year concern and suggested diversifying stablecoins (USDC *and* DAI) as mitigation. (Reported quotes from a conference talk, not a written primary.)

### The live experiment — the Merge / ETHPoW (Sept 2022)

- **Circle** (~Aug 8–9, 2022): *"intends to fully and solely support"* the PoS chain; USDC *"can only exist as a single valid version."*
- **Tether** (~Aug 8–9, 2022): committed to support PoS Ethereum "in line with the official schedule"; Ardoino said stablecoins *"should act responsibly and avoid disruption."*
- **Outcome:** ETHPoW (ETHW) went live ~Sept 15–16, 2022. With USDC/USDT unredeemable on it and oracle-dependent DeFi worthless there, ETHW collapsed from ~$50 to a low of **~$4.2 by Sept 19, 2022** (after a replay-attack scare), and later **~$3.13 during the Nov 2022 FTX collapse**. The fork survived as a marginal chain and captured essentially none of the economic activity.

### Why this is the deepest indictment

The sharpest framing: the canonicity of "real" Ethereum — the supposedly credibly-neutral, unstoppable settlement layer — was effectively ratified by the redemption policies of two private companies. This isn't a peg risk or a counterparty risk; it's a **sovereignty** risk at the base layer. It is the strongest argument for why decentralized stables aren't a nice-to-have but a precondition for Ethereum's neutrality. (Related: Lyn Alden, *"Proof-of-Stake and Stablecoins: A Blockchain Centralization Dilemma."*)

---

## 4. The Failings of the Incumbents
### USDC and Tether — the price of winning

### USDC and the SVB depeg (March 2023) — banking risk, imported wholesale

- Circle confirmed **$3.3B** of USDC reserves stuck at Silicon Valley Bank (~8% of ~$40B reserves), disclosed March 11, 2023.
- USDC traded as low as **$0.8774** (CoinGecko all-time low, March 11, 2023); sub-$0.87 wicks on individual venues were thin-order-book artifacts, not the aggregate low.
- Recovery came only after the Treasury/Fed/FDIC backstop on Sunday March 12 (6:15pm ET) guaranteed all depositors; full repeg once Circle resumed redemptions Monday March 13.
- **Takeaway:** the "1:1 dollar" was only as good as the solvency of the banks holding the cash. Absent a discretionary government bailout, the $3.3B haircut would have been real. A fiat-backed stable imports the entire counterparty risk of the banking system.

### Tether — reserves, transparency, and the audit that never comes

- **NYAG settlement ($18.5M, Feb 23, 2021):** found Tether's "backed 1:1" claim false; for periods it *"held no reserves to back tethers in circulation at the rate of one dollar for every tether."*
- **CFTC settlement ($41M, Oct 15, 2021):** Tether held sufficient fiat reserves only **27.6% of the days** in a 26-month sample (2016–2018).
- **Now:** reserves shifted from commercial-paper-heavy to Treasuries. FY2025 attestation (BDO, released Jan 30, 2026): net profit **>$10B**, total direct+indirect U.S. Treasury exposure **>$141B**, excess reserves $6.3B. USDT in circulation **$189.5B** (DefiLlama, May 2026; Tether's own Q4'25 figure was >$186B).
- **The persistent failing:** Tether has **never published a full financial audit** — only point-in-time *attestations.* A ~$189B liability issuer running on attestations is the transparency gap to name plainly.

### Censorship / freeze power — the core CROPS failure

- Both USDT and USDC contracts contain centralized **blacklist/freeze functions**; the issuer can immobilize any balance on-chain.
- **Tether:** has frozen **>$4.4B cumulatively** (>$2.1B tied to U.S. authorities); e.g., $344M across two Tron wallets in April 2026. (Notably *declined* to proactively freeze Tornado Cash in 2022, calling unilateral freezing "reckless.")
- **Circle / Tornado Cash (the canonical case):** OFAC sanctioned Tornado Cash Aug 8, 2022, listing **44 addresses** (38 ETH + 6 USDC). Within hours Circle blacklisted **~75 addresses**, freezing **~$75,000 of USDC**. (Three distinct numbers — 44 sanctioned, ~75 blacklisted, ~$75K frozen — that frequently get conflated.)
- **The proof of the point:** the Fifth Circuit ruled (Nov 2024) that immutable smart contracts aren't sanctionable "property"; Treasury delisted Tornado Cash March 21, 2025. The code couldn't be censored — but the centralized stables flowing through it were, instantly, on a sanction later found unlawful. Decentralized protocol > "decentralized" dollar.

### Counterparty / regulatory capture — structural, not incidental

- Issued by regulated entities (Circle is NYSE: CRCL) that comply with sanctions/freeze requests as routine.
- Reserves sit inside the banking + Treasury system — the exact channel that produced the SVB depeg.
- **Redemption is permissioned:** direct 1:1 redemption only for KYC'd institutions. Retail are price-takers on secondary markets — which is *why* USDC could trade to $0.87 instead of being arbitraged to par instantly.

### Scale + regulatory status (early–mid 2026)

- Total stablecoin market **$321.9B**; **USDT $189.5B (58.9%)**, **USDC $76.8B (23.9%)**, combined **82.7%** — a near-duopoly (DefiLlama API, 2026-05-22). For scale, the next tier is far behind: USDS (Sky) $8.8B, USD1 (World Liberty) $4.8B, DAI $4.6B, Ethena USDe $4.4B.
- **GENIUS Act** (signed July 18, 2025): 100% reserves in cash/short-term Treasuries, monthly disclosures, licensed-issuer regime — and **bans paying yield to holders.** Effective the earlier of Jan 18, 2027 or 120 days after final rules; rulemaking in progress as of early 2026.
- **MiCA (EU):** stablecoin rules enforceable from mid-2024; Tether did not seek authorization, so regulated EU venues delisted/restricted USDT through early 2025, shifting EU volume toward USDC.

### The irony

The most "successful" stablecoins are the most centralized — and **regulation is entrenching that, not fixing it.** GENIUS rewards the fiat/T-bill model and bans yield (protecting bank deposit franchises and Treasury demand), while effectively excluding algorithmic and disadvantaging decentralized designs that can't post fiat reserves through a licensed depository. The state is blessing a duopoly that gives up every CROPS property — **C**ensorship-**R**esistant, **O**pen-source, **P**rivate, **S**ecure — that motivated crypto in the first place. CROPS is the rubric used to score the flatcoins in §5.

---

## 5. The Flatcoin Frontier
### Ungovernable money that actually works

### Definition first — two senses of "flatcoin"

- **Strict sense (purchasing power):** pegged to a CPI basket / real goods, *not* to $1. Coined by **Balaji Srinivasan (2021)** ("one coin buys a hamburger today and in five years"), made a build priority by **Coinbase (Feb 2023)**, named a top opportunity by **Vitalik (late 2022)**. The clearest live example is **Nuon (Laguna)**, which markets itself as a decentralized flatcoin pegged to a goods basket via Truflation.
- **Loose sense (decentralized, non-fiat-pegged stable-value):** **RAI** lives here — it floats around a redemption target set by a controller; it is *not* CPI-pegged. The two should not be conflated.

LUSD, RAI, HAI, and ZAI are often grouped together — but the unifying property isn't the peg target, it's the **trust model**: decentralized, crypto-collateralized, minimal-governance, censorship-resistant. That is the real category — call it "ungovernable money" for the loose-sense umbrella.

### The two critiques the category must answer

1. **Volatility of the underlying collateral.** ETH is volatile, so you need *some* shock absorber: overcollateralization + liquidations (LUSD), or a floating redemption price that absorbs volatility into the peg target itself (RAI/HAI), or a soft-liquidation curve (crvUSD's LLAMMA).
2. **Capital efficiency / scalability.** This is the unsolved one. Overcollateralized CDP supply is **capped by demand for leverage** — you only get stablecoins when someone wants to borrow against crypto. That's why DAI reached for USDC: to scale past the leverage ceiling, it had to centralize collateral. The honest statement: no decentralized stable has yet scaled to USDC-size without centralizing collateral. The trilemma is alive.

### The protocols (sizes from DefiLlama/CoinGecko, 2026-05-22)

**LUSD — Liquity v1 (the immutability gold standard).** ETH-only, 0% interest, 110% minimum collateral ratio, Stability Pool + redemption arbitrage giving a ~$1.00–1.10 band. **No admin keys, no governance, no upgradeability** — it cannot be changed. The strongest CROPS score in the field and the cleanest counterexample to "decentralized stables inevitably centralize." Cost: capital inefficiency and a soft ceiling on size. ($28.6M mcap; $242M Liquity v1 TVL. Note its all-time low was $0.897 in Jan 2022, so the "$1.00–1.10 band" holds in normal conditions but isn't a hard floor.)

**RAI — Reflexer (the non-pegged ideal type).** ETH-only, **non-pegged**: an on-chain **PID controller** sets a floating *redemption price* via a *redemption rate* (price above target → negative rate nudges it down; below → positive). "Ungovernable money," governance minimized. **Reflexer Labs has wound down**; contracts are immutable and live but static and tiny ($1.7M mcap; $2.3M protocol TVL; redemption price now ~$3.06 — it never targeted $1). The point it proves: you *can* build stable-value money with no peg promise and no governance — Vitalik's "ideal type."

**BOLD — Liquity v2.** Adds **user-set interest rates** (redemptions hit the lowest-rate loans first, not the lowest-CR ones) and **multi-collateral** (WETH, wstETH, rETH in isolated branches); Earn yield now comes from real borrower interest, not token emissions. More flexible, *governed* (so a notch below v1 on immutability). ($33M BOLD circulating; $82M Liquity v2 TVL.)

**HAI & Open Dollar (OD) — the RAI descendants.** HAI ("Let's Get HAI") is a **multi-collateral RAI fork on Optimism** (Ameen Soleimani; mainnet Feb 2024) with a **KITE governance token** — so more governed than RAI. Alive but tiny (~$0.6M circulating). **Open Dollar** is a *distinct* project (not a HAI rebrand): GEB-framework, governance-minimized, LST-backed, ~$1.00-floating on Arbitrum, by Joseph Schiarizzi, whose twist is **Non-Fungible Vaults** (the CDP itself is a tradable NFT) — but it is effectively dormant now (~$6K TVL), so it reads as a design idea, not a live option.

**ZAI on Zcash — the frontier exemplar.** An **oracle-free, ZEC-collateralized CDP "flatcoin"**: no Chainlink — an AMM TWAP is the *only* price input — and it's **shielded-transaction native** (depends on Zcash Shielded Assets / ZSAs). Posted to the Zcash forum **Feb 2026** by pseudonymous dev **lamb356** (repo `lamb356/zai-sim`); claims **$0 bad debt** in backtested replays of Black Thursday, FTX, and Luna.

> Crucial caveat: ZAI is a **simulation / research proposal — not deployed.** No testnet, immutability not yet addressed. It is the most *on-thesis* design in the field — it pushes on the two hardest things at once (oracle dependence and privacy, the **P** in CROPS that almost nobody else even attempts) — but it belongs in any survey as a frontier *direction*, not a usable product.

**Others worth a line each:**
- **crvUSD (Curve):** **LLAMMA soft-liquidation** (collateral is gradually swapped around the liquidation zone rather than dumped) — genuinely novel mechanism; governed. ($239M.)
- **GHO (Aave):** overcollateralized, multi-collateral, *governed* by Aave. ($583M — the largest of the governed-decentralized set.)
- **frxUSD (Frax):** moved to RWA/custodial backing — **not censorship-resistant**; the "decentralized brand, centralized reality" example. (FRAX ~$198M; frxUSD is the RWA/custodial successor.)
- **AMPL / SPOT (Ampleforth):** AMPL targets a **CPI-adjusted dollar** via rebasing; SPOT is the non-rebasing low-volatility derivative — the most credible *commodity-money / strict-flatcoin* attempt, but both are now tiny/illiquid (AMPL ~$6M mcap; SPOT thinly traded).
- **Nuon (Laguna):** the clearest explicit strict-sense flatcoin (Truflation goods basket) — but effectively dormant (~$9K TVL); a concept, not a going concern.
- **Ethena USDe:** delta-neutral, CEX-hedged synthetic dollar ($4.44B circulating, down from a ~$14B peak — the unwind continued through early 2026). **The foil:** decentralized-*flavored* but CeFi-dependent — the contrast against LUSD that makes the "what counts as decentralized" point.

### Comparison table

| Name | Collateral | Peg type | Governance / immutability | Size (DefiLlama/CoinGecko, 2026-05-22) | CROPS-ish read |
|---|---|---|---|---|---|
| **LUSD** (Liquity v1) | ETH only | Soft $1 (1.00–1.10 band) | **Immutable, no governance** | $28.6M mcap / $242M TVL | Gold standard |
| **RAI** (Reflexer) | ETH only | **Non-pegged**, floating redemption price | Governance-minimized, wound down | $1.7M mcap / $2.3M TVL | Ideal type, dormant |
| **BOLD** (Liquity v2) | ETH + LSTs | Soft $1, user-set rates | Governed | $33M circ / $82M TVL | Strong, but governed |
| **HAI** | ETH + LSTs + RAI | RAI-style floating | KITE governance | ~$0.6M circ | Alive but tiny |
| **Open Dollar (OD)** | LSTs | ~$1 floating | Governance-minimized | ~$6K TVL | Novel (NFV vaults), dormant |
| **crvUSD** | ETH/LSTs/BTC | Soft $1 (LLAMMA) | Curve governance | $239M | Novel liquidations |
| **GHO** | Multi | $1 | Aave governance | $583M | Governed, largest |
| **ZAI (Zcash)** | ZEC | CDP "flatcoin", oracle-free | **Not yet built** | N/A — simulation | Most on-thesis, vaporware-stage |
| **frxUSD** | RWA/custodial | $1 | Governed | ~$198M (FRAX) | Not censorship-resistant |
| **Ethena USDe** | CEX delta-neutral | $1 | Governed | $4.44B | The foil (CeFi-dependent) |

### The honest close

Score them on CROPS and only **LUSD and RAI** clear the censorship-resistant + immutable bar cleanly today. BOLD/crvUSD/GHO are decentralized-but-governed (a real category, not a dismissal). ZAI is the frontier — the only design even *trying* to add **Privacy** to the stack — but it's an idea, not a product. And the scaling question hangs over all of them: the immutable ones can't scale to USDC-size without centralizing, and the day they try, they become MakerDAO. That tension — not any single protocol — is the real subject.

Worth sitting with: outside GHO ($583M) and crvUSD ($239M), the immutable/censorship-resistant designs worth championing — LUSD ($28.6M), RAI ($1.7M) — are *rounding errors* against USDT's $189B. That scale gap *is* the argument.

---

## 6. Sources

**Design space / history**
- Vitalik, *Two thought experiments to evaluate automated stablecoins* (2022-05-25) — https://vitalik.eth.limo/general/2022/05/25/stable.html
- Vitalik, *What in the Ethereum application ecosystem excites me* (2022-12-05) — https://vitalik.eth.limo/general/2022/12/05/excited.html
- Haseeb Qureshi, *Stablecoins: designing a price-stable cryptocurrency* — https://haseebq.com/stablecoins-designing-a-price-stable-cryptocurrency/
- MakerDAO MIP29 (PSM) — https://mips.makerdao.com/mips/details/MIP29
- Deribit Insights, *DAI is now 60% backed by centralized assets* (2020-09-26) — https://insights.deribit.com/market-research/dai-is-now-60-backed-by-centralized-assets-what-does-that-mean/
- Messari, *State of Maker Q2 2022* — https://messari.io/report/state-of-maker-q2-2022
- CoinDesk, *MakerDAO votes to retain USDC after depeg* (2023-03-23) — https://www.coindesk.com/business/2023/03/23/stablecoin-issuer-makerdao-votes-to-retain-usdc-as-primary-reserve-even-after-depeg
- Maker Dec-2017 whitepaper — https://makerdao.com/whitepaper/Dai-Whitepaper-Dec17-en.pdf
- BitUSD background — https://medium.com/the-ledger-by-spark/what-is-bitusd-everything-you-need-to-know-about-the-first-stablecoin-ever-created-72337c53fdfa
- CoinDesk, *Basis shutdown* (2018-12-13) — https://www.coindesk.com/markets/2018/12/13/basis-stablecoin-confirms-shutdown-blaming-regulatory-constraints

**Failure modes**
- LFG reserves drained — https://www.coindesk.com/business/2022/05/16/luna-foundation-guard-left-with-313-bitcoin-after-ust-crash
- Harvard, *Anatomy of a Run: Terra-Luna* — https://corpgov.law.harvard.edu/2023/05/22/anatomy-of-a-run-the-terra-luna-crash/
- Anchor yield — https://www.coindesk.com/markets/2022/01/28/anchor-protocol-reserves-slide-as-money-markets-founder-talks-down-concerns
- Olympus bonding primer — https://olympusdao.medium.com/a-primer-on-oly-bonds-9763f125c124 ; CoinGecko OHM — https://www.coingecko.com/en/coins/olympus
- fUSD relaunch / depeg — https://cointelegraph.com/news/fusd-stablecoin-launch-and-rumors-of-cronje-s-return-send-fantom-ftm-price-higher ; https://decrypt.co/120530/ftm-soars-week-fantom-preps-stablecoin-relaunch
- Iron Finance post-mortem — https://ironfinance.medium.com/iron-finance-post-mortem-17-june-2021-6a4e9ccf23f5 ; Fed note — https://www.federalreserve.gov/econres/notes/feds-notes/runs-on-algorithmic-stablecoins-evidence-from-iron-titan-and-steel-20220602.html
- Seigniorage algos (ESD/DSD figures) — https://aws.okx.com/learn/algorithmic-stablecoins-performance-suggests-they-have-yet-to-justify-their-models ; https://dailydefi.org/articles/what-are-seigniorage-stablecoins/
- Beanstalk — https://www.coindesk.com/tech/2022/04/17/attacker-drains-182m-from-beanstalk-stablecoin-protocol ; https://medium.com/immunefi/hack-analysis-beanstalk-governance-attack-april-2022-f42788fc821e
- Wonderland/Sifu — https://www.coindesk.com/tech/2022/01/27/how-did-a-former-quadriga-exec-end-up-running-a-defi-protocol
- Frax/SVB — https://www.coingecko.com/research/publications/stablecoins-supply-svb-impact
- NuBits — https://medium.com/reserve-currency/the-end-of-a-stablecoin-the-case-of-nubits-dd1f0fb427a9

**Fork-choice problem**
- **Qureshi & Lee, *Ethereum is now unforkable, thanks to DeFi* (2019-10-31)** — https://haseebq.com/ethereum-is-now-unforkable-thanks-to-defi/
- Circle, *USDC and Ethereum's Upcoming Merge* (Aug 2022) — https://www.circle.com/blog/usdc-and-ethereums-upcoming-merge
- Fortune, *Circle and Tether support PoS Ethereum* (2022-08-09) — https://fortune.com/crypto/2022/08/09/stablecoins-circle-tether-support-ethereum-merge-proof-of-stake/
- Vitalik @ BUIDL Asia (reported) — https://cointelegraph.com/news/vitalik-centralized-usdc-could-decide-the-future-of-contentious-eth-hard-forks
- Lyn Alden, *Proof-of-Stake and Stablecoins* — https://www.lynalden.com/proof-of-stake/

**Incumbent failings**
- USDC SVB — https://www.coindesk.com/business/2023/03/11/circle-confirms-33b-of-usdcs-cash-reserves-stuck-at-failed-silicon-valley-bank ; Fed FEDS note — https://www.federalreserve.gov/econres/notes/feds-notes/in-the-shadow-of-bank-run-lessons-from-the-silicon-valley-bank-failure-and-its-impact-on-stablecoins-20251217.html
- NYAG settlement — https://ag.ny.gov/press-release/2021/attorney-general-james-ends-virtual-currency-trading-platform-bitfinexs-illegal
- CFTC settlement — https://www.cftc.gov/PressRoom/PressReleases/8450-21
- Tether Q4'25 attestation — https://tether.io/news/tether-delivers-10b-profits-in-2025-6-3b-in-excess-reserves-and-record-141-billion-exposure-in-u-s-treasury-holdings/
- Tornado Cash sanction — https://home.treasury.gov/news/press-releases/jy0916 ; delisting — https://home.treasury.gov/news/press-releases/sb0057
- Circle freeze (Dune-cited) — https://www.theblock.co/post/162172/circle-freezes-usdc-funds-in-tornado-cashs-us-treasury-sanctioned-wallets
- GENIUS Act — https://www.whitehouse.gov/fact-sheets/2025/07/fact-sheet-president-donald-j-trump-signs-genius-act-into-law/ ; https://www.cov.com/news-and-insights/insights/2025/07/the-genius-act-becomes-law-key-provisions-from-the-federal-stablecoin-regulatory-framework
- DefiLlama stablecoins (live data) — https://defillama.com/stablecoins

**Flatcoins / decentralized stables**
- Liquity docs (LUSD/BOLD), Reflexer/GEB docs (RAI), HAI / Open Dollar docs, Ampleforth (AMPL/SPOT), Nuon (Laguna), Ethena — primary protocol docs; current sizes from DefiLlama/CoinGecko.
- ZAI on Zcash — Zcash forum thread (Feb 2026) + repo `lamb356/zai-sim`.

---

## 7. Data Notes & Caveats

- **Live data** was pulled from the DefiLlama and CoinGecko APIs on **2026-05-22** and cross-checked between them. Market figures move daily.
- **fUSD's exact depeg trough** is approximate: the best-documented figure is ~$0.68–0.69 (May 2022); fUSD is now delisted from CoinGecko, so a clean long-run chart is hard to source. The structural point (it depegged and stayed depegged) is solid regardless.
- **Peak % of DAI backed by USDC**: the best-supported dated figure is ~60% USDC-collateralized (Aug 2022, Messari); a Sept-2020 Deribit snapshot put centralized-stablecoin backing at ~52%. The precise all-time peak would want a Makerburn/daistats snapshot.
- **Terra (~$40–50B) and Iron Finance (~$2B)** figures are *notional value wiped*, not user clawback.
- **OHM** raw prices are distorted by heavy rebasing (repeated stock-split-like dilution); the CoinGecko ATH ($1,415, Apr 2021) and low ($7.54, Nov 2022) are the cleanest reference points.
- **Vitalik's BUIDL Asia remarks** are reported from a conference talk, not an authored text.
- **ZAI (Zcash)** is a simulation/research proposal, not a deployed protocol.
