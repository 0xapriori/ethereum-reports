---
title: "The State of Zcash: Encrypted Money at an Inflection Point"
date: 2026-03-20
---

# The State of Zcash: Encrypted Money at an Inflection Point

*A comprehensive briefing by apriori | March 2026*

## tl;dr

- **Zcash is the original zero-knowledge cryptocurrency — born from a rejected 2013 proposal to add privacy to Bitcoin itself — and after over a decade of building, it is experiencing a convergence of institutional interest, technological breakthrough, and philosophical vindication that it has never had before.** Cypherpunk Technologies is accumulating ZEC on a MicroStrategy model. Grayscale has filed for the first privacy coin spot ETF. The Ethereum Foundation's March 2026 Mandate explicitly calls for protocol-native privacy — the thesis Zcash has been building since the Zerocoin paper.

- **The January 2026 governance crisis — in which the entire ECC development team resigned — was severe, public, and ultimately resolved in a way that may have strengthened the ecosystem.** The former team raised $25M+ in seed funding, formed ZODL, and continues building. Development is now distributed across four independent organizations instead of one. Zcash passed, in practice, the "walkaway test" that Ethereum is still aspiring to.

- **Project Tachyon, led by cryptographer Sean Bowe, represents the most ambitious privacy scaling architecture proposed in any cryptocurrency.** Proof-carrying data, oblivious synchronization, nullifier redesign, and transaction aggregation together target 100x smaller transactions and thousands of shielded TPS — up from the current maximum of two. If Tachyon ships, it eliminates the performance argument against private money.

- **The privacy landscape has fundamentally shifted.** AI has destroyed practical privacy on transparent chains. Bitcoin's cypherpunk base has migrated into ETFs. Zcash occupies a narrow but defensible niche: cypherpunk-grade store of value with an uncompromised shielded pool and a demonstrated capacity to upgrade.

- **The risks are real: regulatory pressure (73 exchange delistings), adoption uncertainty, and an untested multi-org governance model.** Nothing about Zcash's position is guaranteed, but the bet is clear: it is a high-risk, high-reward play on the premise that the future of digital money requires absolute, cryptographic privacy. The asymmetry between where Zcash sits technically and where market attention has been is striking.

---

## Table of Contents

1. [What Is Zcash?](#i-what-is-zcash)
2. [The Upgrade History: From Sprout to Orchard](#ii-the-upgrade-history-from-sprout-to-orchard)
3. [The Funding Question: How Zcash Stays Alive](#iii-the-funding-question-how-zcash-stays-alive)
4. [The Governance Crisis](#iv-the-governance-crisis)
5. [Who Builds Zcash Now?](#v-who-builds-zcash-now)
6. [Project Tachyon: The Scalability Breakthrough](#vi-project-tachyon-the-scalability-breakthrough)
7. [ZSAs: Zcash Shielded Assets](#vii-zsas-zcash-shielded-assets)
8. [The Post-Quantum Story](#viii-the-post-quantum-story)
9. [Crosslink: The PoS Finality Question](#ix-crosslink-the-pos-finality-question)
10. [The Market Story](#x-the-market-story)
11. [The Privacy Landscape](#xi-the-privacy-landscape)
12. [What to Watch](#xii-what-to-watch)
13. [Sources](#sources)

---

## I. What Is Zcash?

Zcash's story begins not in 2016, but in 2013 — with an attempt to fix Bitcoin.

That year, Johns Hopkins cryptographer Matthew Green and his graduate students Ian Miers and Christina Garman published the Zerocoin paper at the IEEE Symposium on Security and Privacy. The proposal was straightforward: extend Bitcoin's protocol with a cryptographic layer that would allow fully anonymous transactions. Users could convert bitcoins into "zerocoins," breaking the transaction graph, and later redeem them without any link to the original deposit.

The Zerocoin team proposed this as a Bitcoin protocol upgrade. Bitcoin's developers rejected it. The reasons were partly technical — Zerocoin proofs were roughly 25 kilobytes each, which would have bloated the blockchain, and transaction creation was computationally expensive — and partly cultural. The changes were seen as too radical a deviation from Bitcoin's design philosophy.

The researchers didn't stop. In late 2013, the scope expanded into Zerocash, which introduced zk-SNARKs (zero-knowledge succinct non-interactive arguments of knowledge) to compress proof sizes from 25 kilobytes down to 288 bytes while adding full encryption of amounts and both sender and receiver addresses. This was the theoretical breakthrough that made private digital money practical.

Since Bitcoin wouldn't adopt it, they built their own chain. Zcash launched on October 28, 2016, founded by Zooko Wilcox-O'Hearn, as the first cryptocurrency to implement zk-SNARKs for fully shielded transactions at the protocol layer. The core thesis was, and remains, straightforward: money should be private by default, and cryptography can make that possible without trusted intermediaries.

The origin story is inseparable from a ceremony. At launch, six participants generated cryptographic parameters necessary for the zk-SNARK system. Each participant held a fragment of the "toxic waste" — the random values that, if combined, could compromise the system's privacy guarantees. The fragments were destroyed. One participant used an angle grinder. Another burned their hardware. The ceremony was theatrical by necessity: the system's integrity depended on at least one participant being honest, and the destruction needed to be convincing. The trusted setup requirement — the very thing that helped kill the Bitcoin proposal — would haunt Zcash for years until Sean Bowe eliminated it entirely with Halo 2.

Zcash shares Bitcoin's supply schedule — a 21 million coin cap with halvings every four years — and its proof-of-work consensus mechanism. But it diverges in two fundamental ways. First, privacy: while Bitcoin transactions are pseudonymous (every transaction is publicly visible, linked to addresses rather than identities), Zcash offers fully shielded transactions where the sender, receiver, and amount are cryptographically hidden. Second, upgradeability: Zcash has executed multiple network upgrades since launch, while Bitcoin's last consensus change was SegWit in 2017.

The ZK lineage matters beyond Zcash itself. The practical deployment of zk-SNARKs in Zcash proved that zero-knowledge cryptography could work at scale in production systems. The researchers and engineers who built Zcash's proof systems — Sean Bowe, Daira Hopwood, Jack Grigg, and others — produced foundational work that the entire ZK ecosystem now builds on. ZK rollups, zkEVMs, and the broader zero-knowledge infrastructure that Ethereum's scaling roadmap depends upon trace their intellectual heritage directly to Zcash research. This is not merely a marketing narrative; it is an architectural genealogy.

---

## II. The Upgrade History: From Sprout to Orchard

Zcash's upgrade history is, for a cryptocurrency, remarkably active. It also tells a story about the evolution of zero-knowledge cryptography itself.

**Sprout (2016):** The original shielded pool. Built on the trusted setup ceremony described above. Functional but limited — shielded transactions were expensive to generate, requiring significant computation and memory. The trusted setup was a genuine cryptographic vulnerability: if the ceremony's integrity was compromised, an attacker could mint unlimited ZEC without detection. Sprout worked. But it demanded trust in a process.

**Sapling (2018):** A major upgrade that replaced the original ceremony with Powers of Tau — a multi-party computation ceremony involving 87 participants (up from six). More critically, Sapling redesigned the proof system to be mobile-friendly. Generating a shielded transaction went from minutes on specialized hardware to seconds on a smartphone. This was the upgrade that made shielded transactions practically usable for normal people.

**Orchard / NU5:** The upgrade that changed the game. Orchard introduced Halo 2 — a zero-knowledge proof system created by Sean Bowe that eliminated the trusted setup requirement entirely. No ceremony. No toxic waste. No trust assumption. The proof system's security depends only on the underlying mathematical hardness assumptions, not on any participant's behavior at any ceremony. This is the kind of achievement that sounds incremental if you don't understand what it replaced: the elimination of a foundational trust requirement from a trustless system.

The migration data tells its own story. Approximately 30% of ZEC has upgraded through the wallet migrations from Sprout to Sapling to Orchard — a level of coordinated user migration that is, by any measure, unprecedented in cryptocurrency. It demonstrates that Zcash's user base will actually move when the protocol asks them to — a coordination capacity that most blockchains do not have and that Bitcoin has not demonstrated since SegWit.

The upcoming NU7 upgrade will deprecate the Sprout pool entirely, disallowing v4 transactions and burning any remaining Sprout funds at activation. The old shielded pools are being systematically retired — not left to rot as a permanent attack surface.

---

## III. The Funding Question: How Zcash Stays Alive

Every protocol needs to answer the funding question. Zcash's answer has evolved more than most, and the evolution reveals the structural tensions inherent in building freedom technology within existing institutional frameworks.

**Phase 1: The Founders Reward (2016-2020).** At launch, 20% of every block reward was directed to founders, early investors, the Electric Coin Company (ECC), and the Zcash Foundation. This funded the initial development. Among the early investors was Naval Ravikant. The founders reward was set to expire at the first halving — it was always understood as startup capital, not a permanent arrangement.

**Phase 2: The Dev Fund (2020-2024).** As the first halving approached in 2019, the community faced a decision: let all block rewards go to miners (like Bitcoin), or continue funding development. The answer was to continue, but with a restructured allocation split among ECC, the Zcash Foundation, and community grants. Critically, this required moving development into nonprofit structures.

The reason was taxes. As Zaki Manian explained on Deeply Intents: the US tax structure for crypto projects was "brutal." Double taxation on tokens made the for-profit model untenable. The solution was to convert ECC into a nonprofit — Bootstrap — with the Zcash Foundation as a separate entity. ECC (the for-profit company) effectively donated itself to the Bootstrap nonprofit. This was a rational response to a structural constraint, but it created its own structural constraints that would explode into public crisis six years later.

**Phase 3: NU6 and the Lockbox (November 2024).** NU6 introduced the current allocation: 80% of block rewards to miners, 8% to Zcash Community Grants (ZCG), and 12% to a protocol-owned lockbox. The lockbox was a novel mechanism — funds accumulate on-chain, governed by the community rather than any single organization.

**Phase 4: Community & Coinholder Model (NU6.1, November 2025).** The C&C model went further: ZEC holders collectively decide whether and how to distribute lockbox funds. This is coin-weighted voting — a form of governance that invites obvious critiques about plutocracy. But it represents a directional shift from organizational control to community control that is philosophically consistent with the project's decentralization thesis. The model is valid until the third halving, approximately 2028.

The arc is clear: from founder-controlled to org-controlled to community-controlled. Whether community control at this stage means genuine decentralization or governance theater is an open question — but the direction of travel is not.

---

## IV. The Governance Crisis

In January 2026, the entire development team at ECC resigned. This was, depending on your framing, either the most serious crisis in Zcash's history or a demonstration that decentralized governance can survive its most stressful test.

### What Happened

The Zashi wallet — started by Zooko Wilcox at ECC and taken to production quality by CEO Josh Swihart (who took the role in 2023) — became the catalyst. The thesis behind Zashi was that a protocol needs a user-facing product to motivate developers to make the protocol usable. As Zaki Manian put it: "If you have a protocol but no user-facing product, no one on the protocol side is ever gonna be motivated to make the protocol good from a usability point of view."

Zashi worked. It integrated NEAR Intents — a cross-chain swap protocol that allows users to convert assets like BTC directly into shielded ZEC from within the wallet — and generated a reported $5-6M annual run rate in revenue from swap fees. This was the first revenue in the nonprofit's history. This was a genuine achievement. It was also, structurally, a problem.

The success created pressure to commercialize. Wealthy crypto participants became interested in the wallet and in Zcash more broadly as ZEC's price rose in October 2025. But everything — the wallet, the team, the revenue — sat inside a 501(c)(3) nonprofit. Nonprofit law imposes constraints on commercialization. The IRS requires fair-market-value treatment for assets leaving nonprofit control. The Bootstrap board — Zaki Manian, Christina Garman, Alan Fairless, Michelle Lai (referred to as "ZCAM") — maintained that any transaction had to comply with nonprofit law and protect mission-owned assets from private capture.

On January 7, 2026, CEO Josh Swihart announced that all ECC employees had left, citing what he characterized as "constructive discharge" — the board changing employment terms in ways that made continued work impossible. Core developers Jack, Strad, and Jara left publicly and called out the board. ZEC dropped 14-25% on the news.

### The Resolution

Zaki Manian described consuming all of January and February resolving the crisis. The board used AI extensively in the process — building interactive deal-structure websites, dashboards, valuation models, and contract management tools. ("We AGI'd the hell out of dealing with this governance crisis," Zaki said on Deeply Intents.)

The settlement: Bootstrap retained the wallet under a new name, Zotil, with the team continuing to develop it. The former ECC team formed ZODL (Zcash Open Development Lab), raised $25M+ in seed funding by March 2026, and launched cashZ — a separate wallet built from the original Zashi codebase. Bootstrap is pivoting to mining decentralization. Sean Bowe, the cryptographer leading Tachyon, characterized the restructuring positively — freeing builders from overly cautious governance structures.

CoinDesk's assessment: "governance dispute may not be as big as it seems."

### Analysis

The honest read: this was ugly. Public acrimony, a market crash, a complete team departure. But consider the counterfactual. The protocol kept running. The Zcash Foundation continued maintaining the Zebra node. The Tachyon team continued building. A new, well-funded development company was formed within weeks. The ecosystem went from one dominant development organization to four independent ones (Zcash Foundation, Shielded Labs, ZODL, Tachyon team) — arguably a more resilient configuration than what existed before.

Was this a governance failure? In the narrow sense, yes — the nonprofit structure created exactly the kind of friction it was warned about. Was the system's response to failure a demonstration of resilience? Also yes. The EF Mandate published days before this report defines the "walkaway test": the protocol must function even if its core team disappears. Zcash passed that test in production.

A necessary caveat: the walkaway test was passed in favorable conditions. The former ECC team was able to raise $25M within weeks — a fundraising environment made possible by a bull market and surging ZEC price. In a bear market, where no VC is writing checks for Zcash development and ZEC is back at $30, a governance rupture of this magnitude might have been terminal. The resilience was real, but it was also backstopped by favorable market timing. Whether the multi-org structure holds together through a sustained downturn is an untested question.

---

## V. Who Builds Zcash Now?

The post-crisis development landscape is more distributed than at any point in Zcash's history.

**Zcash Foundation:** Maintains the Z3 stack — a modular infrastructure suite comprising Zebra (the Rust-language consensus node that replaces the deprecated zcashd), Zaino (indexing layer), Zallet (wallet toolkit), and Z3G (RPC compatibility layer). Zebra is a ground-up Rust rewrite — memory-safe, async, parallelized. After NU7, Zebra becomes the sole consensus implementation; zcashd is fully retired. The Foundation is the protocol steward, responsible for infrastructure that other teams build on.

**Shielded Labs:** Led by Jason McGee, with Zooko Wilcox as Chief Product Officer. Building Crosslink, the hybrid PoW/PoS finality upgrade. Vitalik Buterin donated to Shielded Labs in February 2026, calling Zcash "one of the most honorable crypto projects." This endorsement matters — not for the money, but for what it signals about how Ethereum's most influential voice views Zcash's technical legitimacy.

**Tachyon Team:** Led by Sean Bowe (creator of Sapling and Halo/Halo 2). Includes C-Node (@colludingnode), a cryptographic engineer working on proof systems and circuits who joined from Celestia around November 2025. Building the fundamental scalability layer described in detail in the next section.

**ZODL (Zcash Open Development Lab):** Josh Swihart's new company, well-funded by a seed round led by Paradigm, a16z crypto, Coinbase Ventures, and Winklevoss Capital. Building the cashZ wallet. This is the former ECC team, now operating as a startup unconstrained by nonprofit law.

**Zcash Community Grants (ZCG):** Receives 8% of block rewards. Funds independent contributors and projects across the ecosystem.

**Bootstrap:** The nonprofit that triggered the governance crisis. Now pivoting to mining decentralization — a less commercially contentious mission. Notably, Foundry Digital is launching a compliance-focused institutional Zcash mining pool in H1 2026, signaling that mining infrastructure is attracting serious institutional attention.

> [GRAPHIC PLACEHOLDER: Insert a 5-node flowchart showing the 80/8/12 block reward split flowing from miners down to ZCG and the Lockbox, and branching out to the 5 development entities (Foundation, Shielded Labs, Tachyon, ZODL, Bootstrap).]

Five organizations with distinct mandates and independent funding. The single point of failure that the ECC governance crisis exposed has been structurally addressed, even if the process of addressing it was chaotic.

---

## VI. Project Tachyon: The Scalability Breakthrough

This is the most technically significant thing happening in Zcash, and arguably one of the most significant developments in privacy technology in any cryptocurrency.

### The Problem

Zcash's current shielded protocol is severely constrained in throughput. Zaki Manian was blunt about this on Deeply Intents: "Zcash is still maximum two transactions per second. It's really not great." (The theoretical protocol maximum may be somewhat higher, but practical observed throughput for shielded transactions is in this range.)

The bottleneck is threefold. Shielded transactions are large and computationally expensive to generate and verify — they require full zero-knowledge proofs. Validators must store all historical nullifiers — unique cryptographic tags published each time a shielded note is spent, which prevent double-spending. Every transaction adds nullifiers, and none can ever be deleted, because the validator needs the full set to confirm a new spend hasn't already occurred. At even modest throughput, this creates runaway state growth measured in gigabytes per day, progressively raising the hardware requirements to run a full node. And wallet synchronization is painfully slow: because everything is encrypted, every wallet must scan every transaction on the network to detect payments addressed to it.

These are not minor limitations. They are existential constraints on shielded money at any meaningful scale. Tachyon addresses all three.

### The Core Innovations

**Proof-Carrying Data (PCD).** The foundational primitive. PCD allows data to carry proofs of its own correctness. When proof-carrying data is combined with other PCD, the mixture "inherits and extends" validation claims — enabling continuous compression of verification without increasing data size. This goes beyond what traditional zk-SNARKs can do.

In practice: wallet state itself becomes proof-carrying data. As blocks arrive, wallets maintain continuous correctness proofs. When a user spends, their transaction extends this PCD, so validators only need to verify against recent intervening blocks — not all historical state. The wallet proves its own solvency incrementally.

**Oblivious Synchronization.** Instead of wallets scanning every transaction on the network, a remote service incrementally constructs a proof that your funds have not been spent. The critical privacy property: the service never learns your actual nullifiers because the protocol forces them to periodically evolve in an unlinkable way.

The practical implication: a mobile wallet user downloads their history in seconds, sends a shielded transaction in real time, without leaking data to anyone — not the wallet provider, not the blockchain, not any observer. This is the feature that makes "private payments on a phone" an actual thing rather than a slide in a conference presentation.

**Nullifier Redesign.** In current Zcash, nullifiers derive from note commitments. Tachyon inverts this relationship — "nullifiers need to be changed so that they are not determined by the note commitment but rather the other way around." This prevents oblivious sync services from correlating nullifiers with specific note locations, closing an information leakage vector at the architectural level.

**Transaction Aggregation.** Block producers can aggregate multiple shielded payments into a single zk-SNARK without requiring user coordination. This reduces marginal transaction size and verification costs dramatically.

**State Pruning.** With wallet PCD attached to transactions, "almost everything in a block can be permanently pruned by validators and ultimately all users of the system." Storage and bandwidth requirements drop by orders of magnitude.

### The Numbers

Tachyon targets transactions approximately 100x smaller than current shielded transactions and throughput in the thousands of TPS. These are not incremental improvements — they represent a categorical change in what shielded money can do.

> [GRAPHIC PLACEHOLDER: Insert a side-by-side bar chart comparing current Zcash maximum throughput (~2 TPS) versus Tachyon's target (1,000+ TPS), alongside a line graph showing the flattening of state-growth curves under PCD-enabled pruning.]

### The Stack

The PCD implementation is called Ragu — a Rust-language framework built by the Tachyon team. It uses a modified Halo recursive SNARK construction, requires no trusted setup, and is compatible with the elliptic curve infrastructure already deployed in Zcash's Orchard pool. The code is open source on GitHub.

### Status

Actively in development. Tachyon has the highest community support among NU7 candidate features. Sean Bowe presented at Zcash Engineering Office Hours #5 on March 17, 2026 — three days before this report. Rollout is planned incrementally: out-of-band payment features and transaction aggregation first, then consensus upgrades for new nullifiers and accumulators, then full proof-carrying transactions. Full mainnet deployment is targeted within approximately one year, contingent on community governance approval.

---

## VII. ZSAs: Zcash Shielded Assets

Zcash Shielded Assets extend Zcash's privacy guarantees beyond ZEC itself. ZSAs allow user-defined tokens within shielded pools — private stablecoins, private tokenized assets, any token that benefits from the same cryptographic guarantees as shielded ZEC.

The specification is mature: ZIP 226 covers transfers and burns, ZIP 227 covers issuance. Both were developed by QEDIT and have been audited by Least Authority. The feature is live on testnet. NU7 inclusion is under active community discussion, with debate around whether ZSAs should take priority over other NU7 candidates or whether competing protocol improvements (including Tachyon components) should come first.

The implications are concrete. A private stablecoin on Zcash would inherit the protocol's full shielded pool anonymity set. This is categorically different from a privacy-wrapped stablecoin on a transparent chain, where the wrapping is an application-layer construct with a smaller anonymity set and potential metadata leakage. As the EF Mandate explicitly states: "No construction layered on top could match the anonymity set the protocol itself could provide."

---

## VIII. The Post-Quantum Story

Quantum computing is a future risk for all public-key cryptography. But for privacy coins, the risk is asymmetrically severe. If the encryption protecting shielded transactions is broken retroactively, every shielded transaction ever made gets exposed. This is not a theoretical concern for a future date — every shielded transaction being made today is encrypted data sitting on a public blockchain, waiting.

Zcash's upgrade history is the relevant evidence here. Three wallet and address upgrades — Sprout to Sapling to Orchard — have been successfully executed, with 30% of ZEC migrating through these transitions. As Zaki Manian noted: "We don't have 10% of the supply in abandoned addresses." Bitcoin, by comparison, has not executed a comparable upgrade since 2017, and a significant portion of its supply sits in addresses using older cryptographic schemes.

The next Zcash address upgrade is designed to "pre-bake a solution to how do we turn off the pre-quantum stuff and upgrade people to post-quantum addresses at some future date." This is not post-quantum cryptography shipping today — it is the architectural preparation for a smooth migration when post-quantum schemes are ready.

Tachyon contributes to this story by decoupling the wallet payment protocol from on-chain operations, removing quantum-vulnerable privacy assumptions from blockchain layers and creating pathways toward post-quantum cryptographic privacy. The through-line: Zcash's demonstrated ability to coordinate upgrades makes it more credibly positioned for the quantum transition than chains that have not upgraded in nearly a decade.

---

## IX. Crosslink: The PoS Finality Question

Crosslink is a hybrid PoW/PoS finality upgrade being built by Shielded Labs. It adds proof-of-stake finality on top of Zcash's existing proof-of-work consensus.

The design preserves Zcash's PoW mining (and its associated security properties and decentralization) while adding economic finality through staked validators. This is a conservative approach — rather than replacing PoW (as Ethereum did), Zcash is layering PoS alongside it. Critically, the staking mechanism is privacy-preserving: stakers operate within the Orchard shielded pool using quantized stakes, maintaining the anonymity set even for consensus participants. The proposed issuance split is 40/40 between miners and stakers (with the remainder to the community allocation).

Development follows a 5-milestone roadmap and is currently at Milestone 4 (staking/tokenomics for PoW+BFT finality), with hardening slated for 2026. Phase 1 targets 12 months; Phase 2 an additional 9 months.

Both Vitalik Buterin and the Winklevoss brothers have donated to Shielded Labs — Vitalik calling Zcash "one of the most honorable crypto projects" in February 2026, and the Winklevoss twins contributing $1.2M. These endorsements signal that Zcash's technical direction is viewed as aligned with the broader goals of the crypto ecosystem, particularly around privacy and protocol design.

Crosslink's progress and its interaction with Tachyon will be worth tracking. Both represent fundamental changes to Zcash's consensus and transaction layers, and their sequencing and compatibility will shape the protocol's architecture for years.

---

## X. The Market Story

The numbers tell a story of extreme dislocation followed by rapid repricing.

ZEC spent much of 2023 and early 2024 below $30. In October 2025, it surged from approximately $72 to $328 — a 350%+ move in a single month. The rally continued through November, with ZEC reaching a cycle peak of approximately $744 on November 7, 2025 — a gain of roughly 1,100-1,200% from its September lows near $60. The price pulled back after the ECC governance crisis in January 2026 and has since been trading in the $200-300 range.

### What Drove It

Several catalysts converged.

**Halving mechanics.** Zcash's second halving occurred on November 23, 2024, cutting the block reward to 1.5625 ZEC. Supply reduction with constant or increasing demand is the oldest thesis in crypto.

**Cypherpunk accumulation.** Cypherpunk Technologies (Nasdaq: CYPH), a Winklevoss-backed treasury company, holds 290,062 ZEC — approximately 1.76% of total supply — and has publicly stated a target of 5%. This is the MicroStrategy model applied to ZEC. Zooko Wilcox was appointed Strategic Advisor to Cypherpunk in December 2025. This is a publicly traded company with a disclosed accumulation strategy — the kind of structural demand that market participants price in.

**Grayscale ETF filing.** On November 26, 2025, Grayscale filed to convert its Zcash Trust into a spot ETF on NYSE Arca (ticker: ZCSH). Grayscale holds approximately 394,400 ZEC. This would be the first regulated spot ETF for a privacy coin. Whether it gets approved is an open question, but the filing itself is a signal of institutional legitimacy that Zcash has never had.

**SEC clearance.** The Zcash Foundation's SEC investigation (case SF-04569, running for over two years) closed on January 15, 2026 with no enforcement action. The regulatory cloud lifted.

**NEAR Intents flows.** Zaki Manian stated on Deeply Intents that "hundreds of millions of dollars of Bitcoin got encrypted" via NEAR Intents flows — Bitcoin holders converting to shielded ZEC. ZODL reported over $600M in ZEC swaps processed through the wallet since October 2025.

**Shielded pool growth.** As of November 2025, approximately 4.8M ZEC (~29-31% of circulating supply) sits in shielded pools — up from roughly 11% at the start of 2025. The breakdown by pool reflects a highly successful migration to the most advanced (trusted-setup-free) shielding technology:

| Shielded Pool | ZEC Held | % of Circulating Supply | Technology Generation |
|---|---|---|---|
| Orchard | 4.2M | 25.4% | Halo 2 (No Trusted Setup) |
| Sapling | 636K | 3.9% | Powers of Tau |
| Sprout | 26K | 0.2% | Legacy (Deprecated) |

Total circulating supply sits at approximately 16.6M of the 21M cap, with annual inflation having dropped from approximately 8% to roughly 4% after the November 2024 halving.

### The Privacy Paradox

Privacy coins were delisted from 73 exchanges globally in 2025. Countries imposing bans or restrictions include Japan, South Korea, India, Australia, UAE, UK, Poland, Belgium, and Ireland. EU AMLR rules are set to take effect in mid-2027.

Despite this — or perhaps because of it — privacy coins dramatically outperformed the broader crypto market in 2025, with the sector delivering approximately 288% returns while Bitcoin ended the year roughly flat. The delistings reduce access but signal exactly the kind of utility that drives demand: if governments are working this hard to restrict privacy tools, those tools must be working.

This is the paradox Zcash navigates. Every delisting reduces liquidity and accessibility. Every delisting also validates the thesis.

---

## XI. The Privacy Landscape

The competitive context has shifted in ways that favor Zcash's positioning, even if the path to mainstream adoption remains uncertain.

### AI Killed Practical Privacy

This is Zaki Manian's thesis, and it is persuasive: "You used to have no privacy but still had practical privacy because it was too complicated to look. All of that is gone now."

Transparent blockchains always leaked information. But the cost of extracting signal from that information was high enough that most users had de facto privacy through obscurity. AI collapses that cost to near zero. Chain analysis powered by AI agents — TRM Labs, Chainalysis, AnChain, and others — can now correlate wallet activity, deanonymize users, and construct behavioral profiles at scale. The US Treasury has explicitly endorsed AI combined with digital identity for crypto oversight. The practical privacy that Bitcoin and Ethereum users relied on — the privacy of "nobody is actually looking at this" — is gone. The privacy coin narrative has shifted accordingly: from "dark web money" to "personal data protection against AI surveillance."

This creates a binary: either your privacy is cryptographic, or you don't have privacy. There is no middle ground anymore.

### Bitcoin's Cypherpunk Migration

The approval of in-kind Bitcoin ETFs created an unexpected dynamic. Long-term self-custody advocates — the cypherpunk base that defended Bitcoin's values — converted their holdings to ETFs for tax efficiency. As Zaki put it: "A lot of fellow travelers who were defenders of those things for Bitcoin are gone."

This weakened Bitcoin's cultural immune system against centralization. And some of those cypherpunks, looking for a proof-of-work, fixed-supply, privacy-preserving asset, found Zcash.

### The Three Tiers

Zaki Manian outlined a framework for where privacy settles by end of 2026:

**Tier 1: Institutional MPC/TEE privacy.** The majority of privacy solutions — Tempo, Arc, RCM on Solana — will use multi-party computation or trusted execution environments. Privacy is invisible to the user, has backdoors, and is not self-sovereign. This is privacy as a feature, not as a right.

**Tier 2: On-chain native privacy.** Aztec, Aleo, and similar platforms offer programmable privacy for on-chain applications. These have genuine privacy properties but trade off against composability, complexity, and the maturity of their privacy models. Penumbra also occupies this space.

**Tier 3: Cypherpunk-grade store of value.** The uncompromised shielded pool. Only Zcash and possibly Ethereum (if the EF Mandate's privacy vision is realized) are credible contenders here. Monero has mandatory privacy — a genuine strength — but its upgrade path is limited by its governance model, and it faces more severe delisting pressure due to the absence of optional transparency. Zcash briefly overtook Monero in market cap in 2025, becoming the largest privacy-focused cryptocurrency by that measure.

The competitive map is worth noting. Aztec launched its Ignition mainnet — programmable privacy on Ethereum. Aleo offers general-purpose private smart contracts. The Ethereum Foundation formed a Privacy Cluster. Each occupies a different part of the design space, and none directly compete with Zcash's "private store of value" positioning. Zcash's differentiator remains unique: the only protocol combining Bitcoin-level scarcity with compliant, programmable privacy and a demonstrated track record of protocol upgrades.

Zcash's bet is that Tier 3 is the only tier that matters for people who actually need privacy rather than convenience. The EF Mandate's language — "protocol-native privacy greatly increases the anonymity set" and "no construction layered on top could match" — validates this position from Ethereum's own institutional voice.

### Zooko's Warning

Zooko Wilcox put it sharply on the Bankless podcast in January 2026: "You can't get privacy from value in flight, you can only get privacy from value at rest." His argument: crypto risks mirroring Linux's trajectory — powering infrastructure everywhere while failing to empower individuals. The cypherpunk vision of building tools for themselves first and then teaching the world failed. The path forward is UX: "It has to be in the same app — if the number of apps you use is greater than one, forget about it." His strategy for Zcash's regulatory resilience is scale: 100 million users.

### The Agent Thesis

Zooko's UX pessimism points directly to the next logical step. If human-facing privacy UX is historically terrible and unlikely to improve fast enough, the market may route around the problem entirely. One of the more provocative framings from the Deeply Intents conversations: "We're never going to teach users how to use privacy protocols — we're going to teach AI agents."

The argument is that the complexity of using shielded transactions, managing keys, and maintaining operational security is never going to be solved by better UX alone. But AI agents that act on behalf of users — managing their privacy, constructing their transactions, maintaining their security posture — could make cryptographic privacy accessible without requiring users to understand it. Combined with intents-based architectures replacing smart contracts for many use cases, this suggests a future where privacy is composed into transaction flows by default, not bolted on by users who know how.

---

## XII. What to Watch

**Tachyon development milestones.** Sean Bowe's Engineering Office Hours presentations and the Ragu GitHub repository are the primary signals. If proof-carrying data works in production, Zcash's scalability story changes fundamentally. But protocol upgrades of this magnitude almost always face delays. What happens to the narrative if Tachyon takes three years instead of one? The current 2 TPS ceiling is a real constraint, and every month it persists is a month where competing privacy solutions can gain ground.

**NU7 activation and ZSA rollout.** Whether Zcash Shielded Assets ship in NU7 determines whether Zcash becomes a platform for private assets or remains limited to private money. The community debate over ZSA vs. Tachyon prioritization in NU7 is itself a governance test.

**Crosslink progress.** The hybrid PoW/PoS upgrade is a complex consensus change. Ethereum's multi-year path to The Merge is the cautionary precedent — consensus layer changes are historically contentious and prone to delay. There is a non-zero risk that miners resist the proposed 40/40 issuance split, creating community friction or even a fork. Its timeline and interaction with Tachyon will ultimately shape the protocol's architecture for years to come.

**ZODL's first products.** The well-funded former ECC team building cashZ. Whether this team delivers outside the nonprofit structure validates the governance crisis's resolution.

**Grayscale ETF decision.** A privacy coin spot ETF approval would be unprecedented. A rejection would be informative in its own way — the regulatory reasoning would reveal how US regulators view privacy technology.

**Privacy regulation trajectory.** EU AMLR rules taking effect mid-2027, ongoing US policy shifts post-Tornado Cash Fifth Circuit ruling, and the broader regulatory stance toward privacy tools. The regulatory vector is the largest exogenous risk to the entire thesis.

**Whether the governance crisis strengthened or weakened the ecosystem.** This is an empirical question that the next 12 months will answer. If four independent organizations deliver more than one did, the crisis was a feature. If coordination breaks down, it was a wound.

---

## Sources

### Foundational Research
- [Zerocoin: Anonymous Distributed E-Cash from Bitcoin (Miers, Garman, Green, Rubin — IEEE S&P 2013)](https://ieeexplore.ieee.org/document/6547123/)
- [Zerocash: Decentralized Anonymous Payments from Bitcoin (Ben-Sasson et al., 2014)](http://zerocash-project.org/media/pdf/zerocash-extended-20140518.pdf)

### Official Documentation & Repositories
- [Project Tachyon Overview](https://tachyon.z.cash/overview/)
- [Tachyon GitHub / Ragu](https://github.com/tachyon-zcash/ragu)
- [Sean Bowe: Tachyon — Scaling Zcash with Oblivious Synchronization](https://seanbowe.com/blog/tachyon-scaling-zcash-oblivious-synchronization/)
- [Sean Bowe: Tachyaction at a Distance](https://seanbowe.com/blog/tachyaction-at-a-distance/)
- [NU7 Upgrade Info](https://z.cash/upgrade/nu7/)
- [ZSAs in NU7 Forum Discussion](https://forum.zcashcommunity.com/t/zsas-in-nu7-h2-2025-extension/51298/1)
- [Zcash Protocol Roadmap 2025/6 (Community Forum)](https://forum.zcashcommunity.com/t/2025-6-zcash-protocol-roadmap/51760)
- [Shielded Labs Crosslink Roadmap](https://shieldedlabs.net/crosslink-roadmap-q1-2025/)
- Ethereum Foundation Mandate, published March 13, 2026 (on-chain IDM)

### News & Market Analysis
- [CoinDesk: Inside Zcash — Encrypted Money at Planetary Scale](https://www.coindesk.com/research/inside-zcash-encrypted-money-at-planetary-scale)
- [Messari: Understanding Zcash (Dec 2025)](https://messari.io/report/understanding-zcash-a-comprehensive-overview)
- [ECC Team Quits After Governance Clash (CoinDesk, Jan 8, 2026)](https://www.coindesk.com/tech/2026/01/08/zcash-developer-team-behind-ecc-quits-after-governance-clash-with-bootstrap-board)
- [CoinDesk: Governance Dispute May Not Be As Big As It Seems](https://www.coindesk.com/business/2026/01/08/zcash-governance-dispute-may-not-be-as-big-as-it-seems)
- [ZODL Raises $25M (CoinDesk, March 9, 2026)](https://www.coindesk.com/business/2026/03/09/josh-swihart-s-zcash-open-development-lab-raises-usd25-million-in-seed-funding)
- [ECC Team Forms New Company (The Block)](https://www.theblock.co/post/384737/zcash-developers-form-new-company)
- [Zcash Draws Institutional Backing (Blockonomi)](https://blockonomi.com/zcash-draws-institutional-backing-amid-privacy-narrative-and-technical-upgrades/)
- [The Privacy Paradox: How Regulation Fueled a 700% Rally (N+1 Insights)](https://www.nplus1insights.com/the-privacy-paradox-how-regulation-fueled-a-700-zcash-rally/)
- [Cointelegraph: 2026 Is the Year of Pragmatic Privacy](https://cointelegraph.com/magazine/2026-pragmatic-privacy-crypto-canton-zcash-ethereum-foundation/)
- [Privacy Coins Regulatory Pressure (CCN)](https://www.ccn.com/education/crypto/countries-banning-privacy-coins-monero-zcash-2026/)
- [Tachyon: The Next Chapter for Zcash (Paragraph)](https://paragraph.com/@lucidmusings/tachyon-the-next-chapter-for-zcash)

### Podcasts & Interviews
- Zaki Manian, Deeply Intents podcast, March 14, 2026 — First public podcast post-governance crisis from a Bootstrap board member
- C-Node (@colludingnode), Deeply Intents podcast, Episodes 1 & 2 — Tachyon team cryptographic engineer
- [Zooko on Bankless: Privacy, AI, and Encrypted Bitcoin (Jan 26, 2026)](https://www.bankless.com/podcast/zcash-founder-on-privacy-ai-and-how-zec-is-encrypted-bitcoin-zooko)
- [ZK Podcast ep. 388: Sean Bowe on Tachyon (Jan 21, 2026)](https://zeroknowledge.fm/podcast/388/)
- [Bitcoin Takeover: Sean Bowe on Scaling Privacy with Tachyon (Oct 18, 2025)](https://podscan.fm/podcasts/bitcoin-takeover-podcast/episodes/s16-e51-sean-bowe-on-bitcoin-zcash-amp-scaling-privacy-with-tachyon)
- [Arjun Khemani: Sean Bowe and Jack Grigg — Encrypted Money at Planetary Scale](https://www.arjunkhemani.com/p/sean-bowe-and-jack-grigg-encrypted)
- [Unchained: Why the Privacy Coins Mania Is Much More Than Price Action](https://unchainedcrypto.com/why-the-privacy-coins-mania-is-much-more-than-price-action/)
- [Zcash Engineering Office Hours #5 — Tachyon with Sean Bowe (March 17, 2026)](https://forum.zcashcommunity.com/t/march-17-16-00-utc-zcash-engineering-office-hours-5-project-tachyon-with-sean-bowe/54923)

### Regulatory & Institutional Filings
- [Grayscale Files for Zcash ETF (CoinDesk, Nov 26, 2025)](https://www.coindesk.com/markets/2025/11/26/grayscale-files-to-launch-1st-zcash-etf-in-the-u-s-amid-1-000-rally/)
- [Cypherpunk Targets 5% ZEC Supply (The Block)](https://www.theblock.co/post/378582/winklevoss-backed-cypherpunk-targets-5-zcash-supply-58-million-treasury-seed)
- [SEC Ends Zcash Investigation (Yahoo Finance, Jan 15, 2026)](https://finance.yahoo.com/news/zcash-foundation-clear-sec-ends-205059906.html)
