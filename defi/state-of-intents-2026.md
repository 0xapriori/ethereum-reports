---
title: "The State of Intents: 2026"
date: 2026-03-18
---

# The State of Intents: 2026

*A synthesis report written by the apriori-writer agent. | March 2026*

## tl;dr

- **Intents are credible commitments to preferences and constraints over possible state transitions** — the shift from telling a blockchain *how* to execute to declaring *what* outcome you want, with competitive solvers handling the rest.

- **The intellectual lineage runs deeper than crypto realizes.** From Prolog (1972) to the Agoric Papers (1988) to Wyvern/OpenSea to modern solver markets — the declarative pattern keeps being independently reinvented. The Agoric Papers described today's intent architecture three decades before CoW Protocol existed.

- **The technology is proven at scale.** 17 projects rated SHIPPED, with CoW Protocol processing $87B in 2025 alone. Estimated total volume across intent systems exceeded $200B in 2025. ERC-7683 has 70+ supporting projects and is becoming the dominant cross-chain intent standard.

- **Intent features beat intent platforms.** The highest-volume systems (CoW, LI.FI, Across, 1inch Fusion) all started as focused products. Purpose-built intent blockchains (Anoma, Essential) have struggled with cold-start problems and shipping delays.

- **Solver centralization is the central unsolved problem.** Structural forces — economies of scale, exclusive orderflow, winner-take-most dynamics — are pushing solver markets toward the same oligopoly patterns crypto was supposed to disrupt. The parallel to Citadel/Virtu in TradFi is uncomfortable and accurate.

- **The AI-agent convergence is real but early.** Prompts and intents share the same structure (declarative specification + delegated search), but the verification gap between probabilistic LLM output and deterministic on-chain settlement remains the hardest open problem. Production impact is 2-3 years out.

- **The bottom line:** Intents delivered on the *declarative* promise but are falling short on the *decentralization* promise. Whether concentrated but transparent solver markets are "good enough" depends on what you think crypto is for.

---

## Table of Contents

1. [Prologue — The Declarative Turn](#i-prologue-----the-declarative-turn)
2. [Intellectual Genealogy](#ii-intellectual-genealogy)
3. [The Crypto Intent Era](#iii-the-crypto-intent-era)
4. [The Shipping Scorecard](#iv-the-shipping-scorecard)
5. [The Interoperability Frontier](#v-the-interoperability-frontier)
6. [The Convergence — Prompts as Intents](#vi-the-convergence-----prompts-as-intents)
7. [The Bull Case and the Bear Case](#vii-the-bull-case-and-the-bear-case)
8. [What We Believe](#viii-what-we-believe)
9. [Sources](#ix-sources)

---

# I. PROLOGUE --- The Declarative Turn

Before the analysis begins, a disclosure: the intent narrative has been good to the projects it labels. When CoW Protocol was solving batch auctions in 2021, it was "just a DEX aggregator." When Paradigm published its intent paper in June 2023, the same mechanism became a paradigm shift worthy of ecosystem-level standardization and hundreds of millions in venture capital. Projects that had been building solver mechanisms or RFQ systems for years discovered they had been building "intent infrastructure" all along. This report takes the technology seriously while remaining honest about the narrative premium attached to the word "intents."

The technology is real, and it has a deeper history than most participants realize.

In 1972, a French question-answering system became the first Prolog program. A user typed a question --- a statement of *what* they wanted to know --- and the machine searched for an answer without any procedural instruction. In 2025, a trader on CoW Protocol signed a message stating they wanted to swap 50,000 USDC for ETH at the best available rate, and roughly thirty competing solvers determined the optimal execution path across dozens of liquidity venues, settling the trade atomically on-chain. Between those two moments lies the same intellectual move: the shift from imperative to declarative, from specifying procedures to specifying outcomes, from *how* to *what*.

This report argues that the modern crypto intent is the convergence point of several distinct intellectual lineages --- logic programming, computational market design, declarative exchange protocols, batch auction mechanisms, and credible commitment theory. They share a common formal structure: **a specification of acceptable outcomes, combined with a delegation of search to an external solving process, under constraints that preserve the specifier's core interests**.

The working definition: **Intents are credible commitments to preferences and constraints over the space of possible state transitions.**

That definition does real work, but it has limits worth acknowledging upfront. "Credible commitments" distinguishes intents from wishes --- they are backed by cryptographic signatures, bonded solvers, and smart contract enforcement. "Preferences and constraints" captures the declarative nature. "Space of possible state transitions" grounds the abstraction in what blockchains actually do. But the definition is cleaner than the reality: unfilled intents are one-sided commitments no solver chose to satisfy; partial fills behave more like utility functions than binary constraints; time-dependent intents decay in ways the static definition does not capture; and MEV-aware intents encode meta-preferences about the execution process that sit awkwardly in a framework focused on final states. The definition is the most useful unifying abstraction available. It is not fully general.

A critical structural distinction the definition glosses over: **how intent satisfaction is verified**. On-chain verification (CoW Protocol, UniswapX) means the settlement contract atomically checks constraints --- the user gets correct execution or reversion, trusting only contract correctness. Optimistic verification (Across, via UMA's oracle) means a claim is posted and can be disputed within a window --- the user trusts disputer liveness and oracle correctness. TEE-based verification (BuilderNet) trusts hardware attestation. These are vastly different trust models, and they matter for whether "credible commitment" means the same thing across different intent systems. It does not.

This report synthesizes three threads: the intellectual genealogy of intents from Prolog to LLMs, a shipping scorecard of which projects delivered and which did not, and an adversarial analysis pitting the strongest bull case against the strongest bear case. Where the threads converge, we have the strongest signals. Where they diverge, we have the most important open questions.

---

# II. INTELLECTUAL GENEALOGY

## a. From Theorem Provers to Constraint Solvers

The genealogy begins with theorem provers, not blockchains. In 1965, John Alan Robinson published "A Machine-Oriented Logic Based on the Resolution Principle" in the *Journal of the ACM*, introducing the resolution inference rule and the unification algorithm ([Robinson, 1965](https://dl.acm.org/doi/10.1145/321250.321253)). Robinson's contribution prepared the ground for a new paradigm: one where the programmer specifies *what must be true*, and the machine finds *how to make it true*.

Robert Kowalski at Edinburgh and Alain Colmerauer at Aix-Marseille built the bridge from theorem proving to programming. By 1972, Colmerauer and Philippe Roussel had implemented the first Prolog system. Kowalski's insight was captured in his landmark equation ([Kowalski, 1979](https://cs.pomona.edu/~jcoa2018/f20/cs190/papers/kowalski.pdf)):

> **Algorithm = Logic + Control**

The *logic* is the declarative specification of what relationships hold. The *control* is the strategy by which the machine searches for solutions. This separation is the deep ancestor of the intent/solver separation in modern crypto. A Prolog query is an intent: "Find me values of X and Y such that these constraints are satisfied." The Prolog runtime is the solver.

An important caveat: Prolog's solver is *deterministic and complete* within its search strategy. Given the same knowledge base and query, you get the same answer. A CoW Protocol solver is *probabilistic and competitive* --- different solvers may produce different execution paths with different outcomes. The non-determinism is a feature (it enables competition) but it means the user gives up the guarantees that made Prolog's declarative model trustworthy. The shift from "guaranteed optimal" to "competitively optimized" is a meaningful relaxation of the declarative promise.

The Constraint Logic Programming (CLP) framework, presented by Jaffar and Lassez at POPL 1987 ([Jaffar and Lassez, 1987](https://dl.acm.org/doi/10.1145/41625.41635)), parameterized logic programming over arbitrary constraint domains. This expansion maps directly onto the evolution of intent expressiveness: a limit order constrains a single pair-price match, while a modern intent ("swap at least X of token A for token B, across any chain, minimizing slippage, before block N") constrains a continuous, multi-dimensional domain.

**Bitcoin's proto-intents deserve mention in this genealogy.** Partially Signed Bitcoin Transactions (PSBTs) allow a party to declare "I am willing to sign this transaction if these conditions are met" --- a credible commitment to a constrained state transition. Hash Time-Locked Contracts (HTLCs) enable cross-chain atomic swaps through conditional commitments.

## b. The Agoric Papers --- Markets as Computation

In 1988, Mark S. Miller and K. Eric Drexler published "Markets and Computation: Agoric Open Systems" ([Miller and Drexler, 1988](https://papers.agoric.com/papers/markets-and-computation-agoric-open-systems/abstract/)). This paper is the single most important intellectual ancestor of intent-based crypto systems, yet it remains under-cited.

The paper's central argument: market mechanisms are a natural model for organizing computation in decentralized systems. A software agent expresses a *willingness to pay* for computational outcomes, and the market mechanism --- acting as solver --- matches preferences against available resources. The agent specifies *what it values*; the market determines *how to allocate*. This is, structurally, an intent system.

Miller's subsequent work on the E programming language and object-capability security deepened the vision in ways that remain directly relevant. Object capabilities --- where holding a reference constitutes authority, and authority can be attenuated but not amplified --- provide the security model for intents. When a user signs an intent specifying "I am willing to spend up to X of token A to receive at least Y of token B," the intent itself is a capability: it grants the solver limited authority to act on the user's behalf, bounded by the constraints in the signed message. Authority is attenuated by construction. This *is* Miller's object-capability model expressed in the language of token transfers. Miller went on to co-found Agoric (the company), which builds on this capability-based smart contract paradigm --- a living continuation of the intellectual lineage, not just historical context.

The Agoric papers demand more attention than the intent discourse gives them. They were published three decades before CoW Protocol, UniswapX, or SUAVE, yet they describe with remarkable precision the architecture those systems would build: competitive markets of solvers, executing user-specified preference functions, with cryptographic guarantees on the bounds of delegation. The crypto industry reinvented Agoric systems without citing them.

Why was this paper ignored for 35 years? The pattern is instructive. Miller and Drexler were writing about computational markets at a time when the infrastructure to implement them did not exist. There were no blockchains, no smart contracts, no cryptographic signatures enabling credible commitments at scale. The theory was right; the engineering substrate was not ready. When Ethereum provided that substrate, builders rediscovered the same design principles through trial and error rather than by reading the literature. Theory anticipates practice by decades, practice reinvents theory independently, and the connection is drawn only in retrospect.

If the Agoric design is structurally inevitable --- if competitive solver markets for user-specified preferences are the natural architecture for decentralized resource allocation --- then the question is not whether this pattern will persist but how to make it work well. The Agoric papers themselves anticipated the concentration problem: market mechanisms can be captured by dominant intermediaries unless the rules are designed to prevent it. Miller's capability-based security model was partly a response to this concern. The intent ecosystem would benefit from revisiting his solutions.

## c. Wyvern --- The Biggest NFT Marketplace Was Running a Prolog Solver in Solidity

The Wyvern Protocol (2017-2018) introduced a model where **orders specify predicates over state transitions** rather than discrete trades ([Wyvern Whitepaper](https://github.com/ProjectWyvern/wyvern-protocol/blob/master/build/whitepaper.pdf)). Buy-side and sell-side orders each provide calldata with a replacement pattern --- a bytemask indicating which bytes can be changed. When orders match, their calldatas are unified: masked and checked for agreement. This is analogous to a simplified form of pattern matching, reminiscent of Prolog's unification algorithm, implemented in Solidity. (Unlike Prolog's unification, which operates over recursive term structures with backtracking, Wyvern's bytemask operates over flat byte arrays --- but the declarative principle is the same.)

Wyvern became the backbone of OpenSea. Every NFT trade on OpenSea through early 2022 ran through Wyvern's `atomicMatch_` function, processing billions of dollars. The biggest NFT marketplace in the world was, without knowing it, running a declarative order matching system descended from the same intellectual lineage as Prolog. Users on OpenSea were expressing intents --- "I want to buy a Bored Ape with these traits for up to this price" --- without knowing or caring about the mechanism.

What Wyvern lacked was a competitive solver market. Orders were matched by OpenSea's centralized order book. The next step in the genealogy would add that missing piece.

## d. Credible Commitments

The concept of credible commitments provides the trust-theoretic foundation. Chris Dixon's "Computers That Can Make Commitments" (2020) articulated the framing: blockchains enable promises enforced by code rather than institutions ([Dixon, 2020](https://a16z.com/what-is-blockchain-computers-that-can-make-commitments/)). For intent systems, credible commitments operate on three levels: the user's intent (cryptographic signature), the solver's execution (bonding and slashing), and the protocol's settlement (smart contract enforcement).

This three-layer commitment structure distinguishes crypto intents from mere preference expression. A Google search query is a kind of intent, but Google's commitment to serve the best results is not credible in the blockchain sense: it can be overridden by advertising incentives, policy changes, or algorithmic manipulation. A CoW Protocol intent is backed by credible commitments at every layer. The gap between "preference expression" (Google) and "credible commitment to preferences" (CoW Protocol) is exactly the gap between web2 and web3 intent systems.

Xinyuan Sun of Flashbots extended this framework to MEV at the Columbia CryptoEconomics Workshop (2022), connecting MEV mitigation directly to credible commitment infrastructure ([Sun et al., 2023](https://arxiv.org/pdf/2311.07815)). The connection is direct: MEV extraction exploits the *lack* of credible commitments in traditional mempool designs. Users submit transactions with no credible commitment from validators about execution ordering. Intent-based systems, backed by credible commitment infrastructure, provide that missing layer.

---

# III. THE CRYPTO INTENT ERA

## a. Proto-Intents and the Rise of CoW Protocol

The earliest intent-based trading forms were request-for-quote (RFQ) systems. **0x Protocol** introduced its RFQ system in 2019, now live across 17+ chains with 150+ liquidity sources, where RFQ beats AMMs 52-77% of the time on top pairs ([0x RFQ System](https://0x.org/docs/0x-swap-api/advanced-topics/about-the-rfq-system)). **Hashflow** built a DEX entirely around RFQ, processing approximately $18-20B in cumulative volume per DefiLlama data ([DefiLlama: Hashflow](https://defillama.com/dexs/hashflow)). **AirSwap** pioneered peer-to-peer RFQ on Ethereum since 2017, though its liquidity concentration among a small number of institutional market makers foreshadowed the concentration dynamics that would plague solver markets broadly.

The pivot from RFQ to full intent-based trading came through Gnosis. **Gnosis Protocol V1** launched in April 2020 with ring trades. **Gnosis Protocol V2** (2021) introduced the defining innovation: the **competitive solver model**, outsourcing the NP-hard optimization of batch settlement to third-party solvers. Users submit signed messages expressing trade preferences. Solvers compete to maximize aggregate trader surplus.

The batch auction model provides structural protection against MEV --- but protection, not elimination. Because orders trading the same pair within a batch clear at a uniform price, *intra-batch* reordering for same-pair orders is useless. But residual MEV vectors remain: cross-batch MEV (strategically timing which intents land in which batch), solver-side MEV (the winning solver can sandwich its own batch settlement on AMMs), and batch boundary manipulation. CoW Protocol's own research acknowledges these residual vectors. Batch auctions mitigate MEV significantly; they do not eliminate it.

The spin-off into CowDAO (GIP-13, January 2022) and rebranding as CoW Protocol marked the maturation of intent-based trading. CoW Protocol processed **$87 billion** in trading volume in 2025, up from $40.2 billion in 2024 --- more than doubling in twelve months. In July 2025, it hit an all-time-high monthly volume exceeding $9 billion. With 30+ active solver teams and a 34.3% market share in DEX aggregation, it is the intent ecosystem's production leader. 20% of its volume comes from internal Coincidence of Wants matching --- direct peer matching that delivers execution quality impossible in a sequential AMM architecture ([CoW DAO: 2025 in Review](https://cow.fi/learn/cow-dao-2025-in-review)).

CoW Protocol is doing exactly what Miller and Drexler described in 1988: running a competitive market where solvers bid to execute user-specified preference functions, with escrowed commitments ensuring delegation bounds are respected. The Agoric vision, realized in Solidity.

But the economic picture complicates the narrative. Despite $87B in volume, CoW Protocol generated approximately $6M in fee revenue in 2024 against roughly $10.3M in costs --- the protocol was not profitable ([PANews: CoW Protocol revenue](https://www.panewslab.com/en/articledetails/v1qtbfsn.html)). This underscores a distinction the report insists on throughout: **volume and fee revenue are fundamentally different metrics**. The $87B measures value flowing *through* the system, not value captured *by* it. Readers should not mistake volume for economic significance.

Aave Labs integrated CoW's solver network in December 2025 for MEV protection --- an institutional validation that demonstrated intent-based execution embedded directly into a major lending protocol's operations. But within weeks, a **$50 million DeFi swap incident** led to dueling post-mortems between Aave and CoW Swap, with community accusations of "stealth privatization" by Aave Labs ([The Block: Aave-CoW $50M](https://www.theblock.co/post/393621/aave-and-cow-swap-publish-dueling-post-mortems-after-50-million-defi-swap-disaster), [The Block: Aave community probes integration](https://www.theblock.co/post/382369/aave-community-probes-cow-swap-integration-and-aave-labs-stealth-privatization-of-protocol)). Intent-based systems are not exempt from the messy operational realities of DeFi.

## b. The Intent Thesis Crystallizes (2022-2023)

The period from late 2022 through 2023 saw the intent thesis move from implicit practice to explicit theory. **Flashbots announced SUAVE** in late 2022 --- the "Single Unifying Auction for Value Expression" --- positioning it as general-purpose credible commitment infrastructure for intents across all blockchains ([The Future of MEV is SUAVE](https://writings.flashbots.net/the-future-of-mev-is-suave)). **Paradigm published its defining essay** in June 2023, formally introducing "intents" to the broader discourse and defining them as "a signed set of declarative constraints which allow a user to outsource transaction creation to a third party without relinquishing full control to the transacting party." Paradigm warned presciently that "to remain competitive, solvers either vertically integrate by running their own builder or secure exclusive deals with builders to ensure their transactions are prioritized, creating an entry barrier for new solvers" ([Paradigm, 2023](https://www.paradigm.xyz/2023/06/intents)). **UniswapX launched** in July 2023, bringing the intent model to the largest DEX ecosystem via Dutch auction-based filler competition ([UniswapX](https://docs.uniswap.org/contracts/uniswapx/overview)). **Anoma** positioned itself as the most ambitious project --- an "intent machine" and "distributed operating system" making intents a first-class computational primitive. **Essential** raised $16.15M for "the first declarative, intent-centric blockchain." The narrative premium attracted enormous venture capital into purpose-built architectures --- capital that, as the scorecard shows, mostly did not translate into production systems.

## c. Standards: ERC-7683 and the Open Intents Framework

**ERC-7683**, proposed in Q2 2024 by Uniswap Labs and Across Protocol, defines a standard for expressing and settling cross-chain intents. Over 70 projects support it, with adoption by major L2s including Arbitrum, Optimism, Polygon, Base, Scroll, and zkSync ([ERC-7683 Spec](https://www.erc7683.org/spec)). **ERC-7521** addresses smart wallet intents. **ERC-8004** targets the AI agent use case.

The **Open Intents Framework**, co-led by the Ethereum Foundation, Hyperlane, and Bootnode, provides institutional backing with 30+ collaborating teams including Coinbase Payments. When the Ethereum Foundation, Uniswap, Across, Coinbase, and 30+ L2s align around a common intent standard, the pattern has been institutionalized.

## d. SUAVE, PBS, and the MEV-Intent Nexus

The relationship between MEV, Proposer-Builder Separation (PBS), and intents:

- **MEV** exists because users over-specify execution paths without credible commitments about ordering.
- **PBS** exists to prevent validators from needing to run sophisticated MEV extraction themselves, which would centralize the validator set. The competitive builder market is a *consequence* of this separation, not its primary purpose. PBS does not aim to eliminate information asymmetry; it aims to separate the entity with the power to propose blocks from the entity with the sophistication to construct them.
- **Intents** allow users to under-specify execution paths while maintaining credible commitments about acceptable outcomes.

SUAVE never shipped as designed. Flashbots pivoted to **BuilderNet** (launched November 2024, jointly operated by Flashbots, Beaverbuild, and Nethermind), addressing the practical problem of centralized block building. In December 2024, Flashbots migrated all builders, orderflow, and refunds to BuilderNet. BuilderNet v1.2 followed in February 2025, and Flashnet research (an anonymous broadcast protocol for censorship resistance) was published in February 2026 ([BuilderNet](https://buildernet.org/blog/introducing-buildernet)). The 90% of Ethereum blocks being built by just two parties was an urgent problem that needed a working solution, not a multi-year research project. BuilderNet is now core Ethereum infrastructure. The TEE component of SUAVE's original vision is being incrementally incorporated. The pivot mirrors a broader pattern: grand theoretical architectures give way to pragmatic infrastructure solving immediate problems.

---

# IV. THE SHIPPING SCORECARD

**A worked example before the scorecard.** Alice wants to move 10,000 USDC from Arbitrum to Base and deposit into Aave. In the imperative model, she must: (1) choose a bridge and approve a spending allowance on Arbitrum, (2) initiate the bridge transaction and pay gas on Arbitrum, (3) wait for bridge finality (minutes to hours depending on the bridge), (4) claim funds on Base and pay gas there, (5) navigate to Aave on Base, approve another spending allowance, (6) deposit into Aave. Six steps, two chains' gas tokens required, multiple points of failure, and 10-30 minutes minimum.

In the intent model, Alice signs a single message: "I have 10,000 USDC on Arbitrum; I want it deposited in Aave on Base." A solver on the Across network sees this intent, evaluates profitability, and fronts the USDC on Base immediately from its own inventory. The solver deposits into Aave on Alice's behalf, completing Alice's desired outcome in seconds. The solver then settles against Alice's Arbitrum USDC through the settlement layer, recouping its capital plus a small fee. Alice's experience: one signature, one outcome, seconds instead of minutes. The solver absorbed the bridging complexity, finality risk, multi-chain gas management, and capital requirements. This is the intent abstraction at work.

Crucially, Alice may not even know she used an intent system. From her perspective, she clicked "Deposit on Aave (Base)" in her wallet and it happened. The intent infrastructure is invisible --- which raises an important question about the "paradigm shift" framing. If users cannot distinguish intent-based execution from traditional execution at the UX level, the paradigm shift is real at the infrastructure level but invisible at the user level. This is how successful infrastructure works. Users do not know they are using TCP/IP when they browse the web. But it does mean the intent narrative's significance is primarily an engineering and market-structure story, not a consumer-facing one.

This example illustrates the promise. The scorecard that follows measures the reality.

The following scorecard rates every major project based on publicly verifiable evidence:

| Project | Rating | Key Metric | Notes |
|---------|--------|------------|-------|
| **CoW Protocol** | **SHIPPED** | $87B volume (2025 annual) | 30+ solver teams, ~$6M revenue vs ~$10.3M costs (2024). Intent king by volume, not yet profitable. |
| **LI.FI** | **SHIPPED** | $60B+ volume (cumulative) | Approaching 1,000 B2B partners. $29M Series A extension (Dec 2025), total funding >$50M. Evolved to solver marketplace via Catalyst. |
| **deBridge (DLN)** | **SHIPPED** | ~$32.8B volume (cumulative, per DefiLlama bridge volume tracker) | $100K/day fees, 385K+ users, zero security incidents. 0-TVL intent model proven at scale. Underappreciated. |
| **Across Protocol** | **SHIPPED** | ~$28.6B volume (cumulative) | Co-authored ERC-7683, powers Optimism Superchain bridge. In March 2026, announced conversion from DAO to corporate structure (ACX surged over 80%). |
| **1inch Fusion** | **SHIPPED** | $25B+ volume (cumulative) | ~20 resolvers. Q3 2025 daily avg $67.6M. Note: 1inch suffered a security incident in March 2025 (unrelated to Fusion mechanism). |
| **Enso** | **SHIPPED** | $17B volume (cumulative) | 145+ enterprise integrations. Under-hyped relative to output. |
| **NEAR Intents** | **SHIPPED** | $10B confirmed (Jan 2026), est. ~$14B by March 2026 | 35+ chains including non-EVM. ~$2.5B/month. |
| **UniswapX** | **SHIPPED** | Integrated into Uniswap frontend | Dutch auction fillers. Volume not publicly isolated from Uniswap v4. |
| **Hashflow** | **SHIPPED** | ~$18-20B volume (cumulative) | RFQ model, expanding to new chains. |
| **0x (RFQ)** | **SHIPPED** | 17+ chains, 150+ sources | Running since 2019. |
| **ERC-7683** | **SHIPPED** | 70+ supporting projects | Dominant cross-chain intent standard. |
| **Open Intents Framework** | **SHIPPED** | 30+ collaborating teams | EF coordination effort. |
| **BuilderNet** | **SHIPPED** | Powers Ethereum block building | SUAVE's practical descendant. |
| **Propeller Heads** | **SHIPPED** | ~$1.5B/month CoWSwap flow | Open-source Tycho framework. |
| **1inch Fusion+** | **SHIPPED (early)** | ~$1M/day median volume (2025) | Cross-chain intents. Growing but modest vs. dedicated bridges. Q1 2025 saw 319% surge. |
| **Particle Network** | **SHIPPED** | Universal Accounts on Cosmos L1 | UniversalX trading platform, 20+ validators. Chain abstraction via intent-like architecture. |
| **Symbiosis** | **SHIPPED** | $3.7B (2025 annual) | 60+ networks. Not marketed as intents but functionally convergent. |
| **AirSwap** | **SHIPPED (niche)** | Operational since 2017 | Liquidity concentrated among few institutional market makers. Small market share. |
| **Anoma** | **PARTIALLY SHIPPED** | XAN token live Sep 2025 | Core protocol adapters pending audit. ~$58-65M raised across 6 years. ARM is novel but unproven. |
| **OneBalance** | **PARTIALLY SHIPPED** | $20M Series A, open beta | Resource locks are novel. No production volume yet. |
| **Socket** | **PARTIALLY SHIPPED** | 300+ chains | Architecture live but usage data opaque. |
| **Khalani Network** | **PARTIALLY SHIPPED** | 40+ chains, API open | Novel solver collaboration thesis. $2.5M seed. Unknown usage scale. |
| **SUAVE** | **PIVOTED** | Never shipped as designed | Vision lives on in BuilderNet. |
| **Essential** | **PRE-PRODUCTION** | Pre-alpha devnet only | $16.15M raised, missed all public timelines, blog went quiet. |

### Three Patterns

**Pattern 1: Intent features beat intent platforms.** The highest-volume systems --- CoW Protocol, LI.FI, Across, 1inch Fusion --- all began as focused products solving specific problems. They shipped narrow, iterated in production, and expanded gradually. Purpose-built intent blockchains (Anoma, Essential) struggled with combinatorial complexity and cold-start problems. The market rewards shipping over storytelling.

**Pattern 2: The solver market is the real innovation.** Across all shipped systems, competitive solver/filler/resolver markets are the common element. CoW has 30+ solver teams. 1inch has ~20 resolvers. deBridge has competitive solvers. Propeller Heads' open-source Tycho handles ~$1.5B/month. The solver market is not a theoretical construct; it is a functioning economy with real participants, real competition, and real capital at risk. This is the Agoric vision realized: markets as computation. It is also, as we discuss below, the part of the stack most vulnerable to concentration.

**Pattern 3: Standards matter more than protocols.** ERC-7683 with 70+ projects may be the most consequential output of the entire movement. A standard dozens of projects build against has more durable impact than any single protocol. The Open Intents Framework's coordination of 30+ teams around common interfaces is creating network effects that individual protocols cannot generate alone. The standardization of the intent lifecycle --- origination, fulfillment, settlement, rebalancing --- is making intents interoperable infrastructure rather than proprietary features.

---

# V. THE INTEROPERABILITY FRONTIER

Cross-chain execution is where intents earn their keep most clearly. The "just an order book" critique falls apart here: no limit order system can express "move my USDC from Arbitrum to Base, swap to ETH, and deposit into Aave" as a single atomic operation.

**Across Protocol** (~$28.6B cumulative volume per DefiLlama) uses a three-layer architecture: Intent Layer (users specify outcomes), Relayer Network (decentralized relayers compete to fulfill), and Settlement Layer (UMA's optimistic oracle for verification). Its co-authorship of ERC-7683 and adoption by Optimism's Superchain bridge interface position it at the center of Ethereum's cross-chain infrastructure. In March 2026, Across announced conversion from a DAO to a traditional corporate structure, with ACX surging over 80% --- a signal that the market values operational clarity over decentralization theater ([CoinDesk: Across restructuring](https://www.coindesk.com/markets/2026/03/12/across-s-acx-rockets-80-massively-beating-bitcoin-on-plans-to-dump-its-dao-structure)).

**LI.FI** ($60B+ lifetime volume, likely $70B+ by March 2026) layers intent/solver mechanics on top of proven aggregation. LI.FI 2.0's Catalyst protocol turns aggregation into a true marketplace where solvers compete against each other and against other liquidity sources. With approaching 1,000 B2B partners (including MetaMask, Phantom, Robinhood, Hyperliquid) and a $29M Series A extension in December 2025 (total funding >$50M), LI.FI demonstrates that the "intent-ification" of existing infrastructure may be the smartest go-to-market in the space: start with distribution, add solver competition later.

**deBridge** (~$32.8B cumulative volume per DefiLlama bridge volume tracker, $100K/day fees, zero security incidents) pioneered the 0-TVL intent model: no locked liquidity, solvers front capital on demand. deBridge is more important than its mindshare suggests --- its corrected volume puts it third in the hierarchy, above 1inch Fusion and NEAR Intents.

**OneBalance** (open beta, $20M Series A) introduced "Credible Accounts" using resource locks as a commitment primitive, enabling cross-chain transactions at destination chain speed. The innovation is real, but resource locks relocate trust from settlement finality to allocator integrity --- trust-restructuring, not trust-minimization.

### Chain Abstraction vs. Intent Abstraction

A subtle but important distinction: **chain abstraction** (the user should not know which chain they are on) vs. **intent abstraction** (the user should not specify how execution happens). These are related but not identical.

Chain abstraction is a UX goal. Intent abstraction is an architectural mechanism. Chain abstraction *can* be achieved without intents (a universal account with a centralized routing backend), and intents *can* exist without chain abstraction (single-chain CoW Protocol trades). But they are natural complements: intents provide the mechanism through which chain abstraction can be achieved competitively and with minimized trust.

Where they interact in practice matters, and the interaction space can be mapped:

- **Chain abstraction without intents** (centralized routing): A universal account with a single backend making all routing decisions. Solves UX but centralizes execution. This is the web2 approach applied to web3 --- simple for users, trust-heavy at the infrastructure layer.
- **Intent abstraction without chain abstraction** (single-chain solvers): CoW Protocol on Ethereum mainnet. Excellent execution quality, but users still manage multi-chain complexity manually.
- **Both** (competitive cross-chain intents): The ideal. The user neither knows nor cares which chain they are on, and the execution path is determined competitively by solvers. NEAR Intents, Particle Network, and Across are closest to this model.
- **Neither** (manual multi-chain transactions): The status quo for most users today, and the source of the UX problems driving the entire intent movement.

NEAR and Particle Network have been the most explicit about this connection, positioning intents as the *mechanism* for chain abstraction. OneBalance frames resource locks as the commitment primitive that makes cross-chain intent execution feel like single-chain operations. Whether chain abstraction *requires* intents or can be achieved through simpler centralized means is a question the market will resolve --- but the decentralization advantages of the intent approach are the strongest argument for it.

### Cross-Ecosystem Convergence

When competing philosophies arrive at the same architectural conclusion through different paths, the conclusion likely reflects structural truth. The ecosystem details are supporting evidence.

**Ethereum** has made an architectural choice: its cross-L2 interoperability strategy is not built on a universal bridge or shared sequencer (though those efforts continue) but on a common intent standard (ERC-7683) with competitive solver fulfillment. The bet is that market competition among solvers produces better outcomes than centralized routing.

**NEAR** has grown from $5B (November 2025) to $10B confirmed (January 2026), estimated ~$14B by March 2026, across 35+ chains including Bitcoin, Solana, Sui, Aptos, Tron, and TON. The breadth of non-EVM chain support is unusual. NEAR's chain signatures technology (MPC-based cross-chain signing) reached full generalization in 2025, and Confidential Intents (privacy-preserving execution via private shards) represents a novel feature no other intent system offers natively.

**Cosmos/IBC** integrated intents through IBC Eureka, combining IBC v2 with Skip:Go's intent-based bridge aggregation --- intent logic woven into the interoperability layer itself, not a standalone protocol.

**Solana** achieves functional convergence without the "intents" label, and this is a stronger signal than convergence under coordinated marketing. Jupiter processes 80%+ of retail liquidity movement, routing through Jito's Block Assembly Marketplace (BAM, launched July 2025) which overhauled block construction to be modular with custom transaction ordering plugins. The architecture is functionally identical to intent systems: users submit desired outcomes through Jupiter, sophisticated actors compete to route execution optimally, and MEV infrastructure mediates the process.

**Particle Network** brings chain abstraction via Universal Accounts on a Cosmos L1, with its UniversalX trading platform (described as the first chain-agnostic trading platform) seeing volume growth. Its inclusion of 20+ genesis validators and Q1 2026 permissionless ecosystem launch make it a material player in the chain abstraction space.

**Sui** has not adopted an explicit intent architecture natively but is being reached from the outside in by NEAR Intents and cross-ecosystem aggregators. Its object-centric model --- which forces explicit dependency declarations for all state access --- makes transaction outcomes statically analyzable in ways account-model chains are not, a property structurally amenable to intent-based execution even without native frameworks.

These ecosystems have different design philosophies. Ethereum optimizes for decentralization. Solana for throughput. Cosmos for sovereignty. NEAR for accessibility. Yet all converge on the same pattern: users express desired outcomes, specialized actors compete to execute them. Intent infrastructure may follow bridges: initially ecosystem-specific, then consolidating into cross-ecosystem layers. If so, the "intent wars" will be fought not between ecosystem-specific implementations but between cross-ecosystem aggregators competing on chain breadth, execution speed, and solver quality. The declarative turn is not Ethereum-specific. It is a property of maturing computational systems, exactly as the Prolog-to-LLM genealogy predicts.

---

# VI. THE CONVERGENCE --- Prompts as Intents

## a. The Structural Parallel and Its Limits

A prompt to an LLM is, structurally, *similar to* an intent: it specifies constraints on the space of possible text completions and delegates search to an external solving process. The mathematical structure maps onto logic programming: a specification language (logical clauses / natural language), a solution space (Herbrand universe / token sequences), a search procedure (SLD resolution / autoregressive sampling), and a satisfaction criterion (logical entailment / human judgment).

But the parallel has a critical gap. The satisfaction criterion for a Prolog query is formal logical entailment --- verifiable, deterministic, composable. The satisfaction criterion for an LLM prompt is human judgment of quality --- none of those things. This gap means the LLM-as-solver analogy is *suggestive* rather than *structural*: the architectural pattern is shared, but the trust properties differ.

Where the analogy does real work: it illuminates what we call **the intent expressiveness frontier** --- the fundamental tradeoff between how much a user can express and how reliably the system can verify satisfaction. Simple swap intents ("at least Y tokens out") are trivially verifiable on-chain. Complex multi-step intents ("rebalance my portfolio optimally across 5 chains minimizing slippage") may not be verifiable on-chain at all --- how does a smart contract verify slippage was "minimized"? Natural language intents make verification essentially impossible without trusting the interpreter. The more expressive the intent language, the harder it is to verify that an execution satisfies the intent. This tension between the expressiveness the intent paradigm celebrates and the verifiability that makes credible commitments credible is not a bug to be fixed --- it is a structural boundary the field must navigate.

## b. AI Agents as Universal Solvers

Circle's **TXT2TXN** framework makes the convergence explicit: an LLM parses freeform English, classifies it as a blockchain action, and converts it into a signed intent. The natural language input is the intent; the LLM is the solver; the on-chain settlement provides credible commitment infrastructure ([Circle: TXT2TXN](https://www.circle.com/blog/txt2txn-using-ai-llms-for-internet-based-applications)).

Three forces make this convergence imminent. First, account abstraction infrastructure: over **200 million smart accounts** deployed across Ethereum and L2s (a 5x increase from mid-2024, per a BlockEden blog post --- though this figure likely includes many inactive or single-use accounts, and no second source has independently corroborated the 200M number; the actual count of active smart accounts interacting with intent protocols is far smaller and more meaningful) ([BlockEden: 200M+ Smart Wallets](https://blockeden.xyz/blog/2026/01/20/account-abstraction-smart-wallets-erc-4337-eip-7702-mainstream/)). EIP-7702 allows EOAs to delegate execution logic to smart contract implementations without requiring migration to new addresses. Second, multi-chain complexity makes manual transaction construction untenable. Third, ERC-8004 explicitly targets the machine economy use case.

| Era | Intent Language | Solver | Commitment Mechanism | Key Limitation |
|-----|----------------|--------|---------------------|----------------|
| Bitcoin PSBTs/HTLCs | Conditional transaction scripts | Protocol rules | Cryptographic + timelock | Limited expressiveness |
| Prolog (1972) | Horn clauses | SLD resolution | Logical soundness | Deterministic only |
| CLP (1987) | Constraint predicates | Domain solvers | Formal semantics | Single-system |
| Agoric (1988) | Valuation functions | Market mechanisms | Escrowed payment | Theoretical only |
| Wyvern (2018) | State transition predicates | Order matching | Atomic settlement | No solver competition |
| CoW Protocol (2020+) | Signed trade preferences | Competitive solvers | Bonded + smart contract | Solver concentration |
| LLM Prompts (2023+) | Natural language | Autoregressive inference | Emerging | Unverifiable satisfaction |
| AI Agents (2025+) | NL goals decomposed to intents | LLM + on-chain solvers | Open question | Semantic gap, no accountability |

The convergence risks are real:

**Semantic gap risk.** When a user says "rebalance my portfolio conservatively," the AI agent must interpret "conservatively" --- a subjective term with no formal specification. The intent model assumes constraints can be precisely specified, but natural language is inherently ambiguous. A solver acting on a misinterpreted intent may execute a technically valid but semantically wrong transaction.

**Agent-solver collusion.** If AI agents become both the interpreters of user intents and the solvers executing them, the separation between specification and execution collapses. This is the fiduciary problem Zamfir identified, compounded by AI opacity.

**Complexity explosion.** The delegation chain --- user speaks to AI agent, agent decomposes into intents, intents broadcast to solvers, solvers compete in auctions, winner executes on-chain --- introduces failure points and information asymmetries at each layer. Debugging becomes difficult when the user's original statement passed through LLM interpretation, constraint decomposition, and a solver auction before touching the blockchain.

These are the engineering challenges of the next phase. The genealogy from Prolog to LLMs is not complete until the commitment mechanism for AI-mediated intents is as robust as the commitment mechanism for cryptographically signed intents.

The claims made in Sections II through VI --- that the declarative pattern is structurally recurring, that production systems have validated it, and that the AI convergence extends it --- now need stress-testing. The strongest arguments for and against the intent thesis follow.

---

# VII. THE BULL CASE AND THE BEAR CASE

## a. Why Intents Might Be Inevitable

Every sufficiently mature computing system migrates from imperative to declarative interfaces. SQL largely supplanted procedural database navigation. The blockchain is following the same trajectory, driven by three forces:

**Account abstraction has laid the plumbing.** Over 200 million smart accounts deployed (per BlockEden, January 2026 --- caveated in Section VI.b), EIP-7702 in production, paymasters sponsoring gas. The infrastructure is deployed.

**Multi-chain complexity demands it.** Dozens of L2s and app-chains make manual routing untenable.

**The agentic economy requires it.** AI agents do not want to construct calldata; they want to declare goals.

The production numbers are substantial: $87B through CoW Protocol alone in 2025. Cross-ecosystem convergence --- Ethereum, NEAR, Cosmos, Solana, Particle Network independently arriving at the same pattern --- is the strongest empirical signal.

**The capital efficiency argument strengthens the case.** AMMs require liquidity providers to lock capital across the price curve; the vast majority sits idle. Solvers deploy capital on-demand. CoW Protocol's batch auction system enables Coincidence of Wants matching, where users wanting opposite sides of a trade are matched directly without touching an AMM. In 2025, 20% of CoW's volume came from internal CoW matches --- approximately $17.4B in trades with pure price improvement, no LP fees, no slippage, no MEV leakage. This is a measured, production-scale advantage the imperative model cannot replicate.

**Institutional adoption signals are strong.** Aave integrated CoW's solver network for MEV protection. Optimism adopted ERC-7683 and Across for Superchain bridging. Coinbase joined the Open Intents Framework. MetaMask, Phantom, and Robinhood integrated LI.FI. The Ethereum Foundation co-led the Open Intents Framework with 30+ teams. When the Foundation, the largest DeFi lending protocol, the largest L2 ecosystem, the largest US exchange, and the largest wallet all converge on the same pattern, the signal is strong. The TradFi parallel also holds: modern institutional equity trading follows a strikingly similar pattern --- investors express intents ("buy 100,000 shares at VWAP over 2 hours") and algorithmic engines determine optimal routing across venues.

## b. Solver Centralization: The Central Problem

The most damaging critique of intents is empirical. The arXiv execution welfare study (Yuminaga, Chen, Sui, 2025) documents extreme concentration ([arXiv: 2503.00738](https://arxiv.org/abs/2503.00738)):

- **UniswapX**: Two solvers (SCP, Wintermute) dominate 90%+ of volume. Active fillers collapsed from over 2,000 addresses (Summer 2023) to just 12 (January 2024).
- **1inch Fusion**: Top three solvers control 80% of volume. A single resolver (Rizzolver) held 38.6% share in Q2 2025.
- **CoW Protocol**: Barter held 28.2% share in September 2025, acquired rival Copium Capital's codebase to push beyond 50%. CoW DAO approved fair combinatorial batch auctions in May 2025 (launched July 2025) specifically to counteract this --- but winner selection still runs on a centralized AWS server.

The game theory makes concentration structural, not incidental. **Adverse selection** means that as markets concentrate, remaining solvers win disproportionately on orders where they have a private information advantage --- meaning worse execution for users. **Solver-builder vertical integration** is the logical endpoint: if the winning solver is also the block builder, the "competitive auction" reduces to a single entity making internal allocation decisions. This is not a risk; it is the equilibrium the structural forces push toward. Paradigm warned about this in 2023; the data shows it happening.

The ESMA July 2025 MEV report found that "current MEV counter-measures deployed so far fail to address these problems" ([ESMA MEV Report](https://www.esma.europa.eu/sites/default/files/2025-07/ESMA50-481369926-29744_Maximal_Extractable_Value_Implications_for_crypto_markets.pdf)). Note: ESMA was assessing MEV under its MiCA regulatory framework for EU crypto-asset markets, not issuing a blanket global verdict. But the finding is directionally important.

**Countervailing forces exist.** CoW's fair combinatorial auctions allow multiple solvers to contribute partial solutions. Propeller Heads' open-source Tycho lowers solver entry barriers (~$1.5B/month already runs through it). Khalani Network enables solver-solver collaboration. Regulatory pressure from ESMA may force structural changes, as MiFID II did in TradFi execution quality reporting. Whether these are sufficient is unknown.

The uncomfortable truth: intent systems do not eliminate intermediation --- they restructure it. Instead of MEV extraction by anonymous searchers in a public mempool, you get value extraction by a permissioned oligopoly of solvers in a private auction. Whether this is better depends entirely on whether auction design and competitive dynamics return surplus to users. The concentration data suggests competitive dynamics are weakening, but the ecosystem is actively working on countermeasures, and the outcome is not yet determined.

A question the report cannot answer but practitioners need to: **what does it cost to run a competitive solver?** The concentration analysis floats above the economic reality without this data. If running a solver is marginally profitable, concentration is inevitable and no auction design will fix it --- the business simply cannot support many participants. If running a solver is highly profitable, there is a puzzle about why entry is not higher. The answer determines whether the concentration problem is fixable or inherent. On-chain solver profitability data exists on Dune dashboards; the intent ecosystem would benefit from making this data a standard transparency metric alongside volume.

**A related transparency gap: intent failure rates.** No major intent protocol publicly reports what percentage of submitted intents expire unfilled, how often solver execution produces worse outcomes than a simple AMM swap, or what the distribution of execution quality looks like across different market conditions. The solver economics question has received attention; the user-outcomes question has not. Until intent systems publish fill rates, execution quality distributions, and failure-case analyses with the same rigor they publish volume numbers, the bull case for user welfare rests on theoretical auction properties rather than measured outcomes.

## c. The "Just an Order Book" Critique

David Ma of Alliance DAO: intents "are actually just limit orders" ([Alliance DAO](https://alliance.xyz/essays/intents-are-just)). For simple swaps --- still the dominant on-chain activity --- the argument has merit.

What is new, and what the limit order framing cannot capture:

1. **Conditional composition**: An intent can say "buy X at price P, but only if Y is also available below price Q, and execute both atomically." This multi-asset, cross-venue conditionality is impossible with standard order book semantics. A limit order specifies one trade. An intent specifies a *relationship* between trades.
2. **Solver competition for complex execution**: For multi-step, cross-chain operations, the solver auction creates a competitive execution market that did not previously exist.
3. **Separation of expression from execution**: The user need not know which DEX, chain, or route is optimal.

For simple swaps, intents are a branding exercise. For complex, multi-step, cross-chain operations with conditional dependencies, they represent a new abstraction. The question is what percentage of activity falls into each category. Simple swaps dominate today; cross-chain and multi-step operations are the fastest-growing segment --- precisely the segment where intents add value.

Zamfir offers a deeper critique worth engaging: making intents explicit creates fiduciary relationships the current system does not honor. True intents, he argues, "are kept hidden for your own safety because if you did other people would use it in disputes against you to wreck you" ([Zamfir, 2023](https://medium.com/smart-transactions/smart-enough-to-not-fall-for-your-intents-transactions-5620bb091bea)). When you sign an intent revealing that you urgently need to swap a large position, you disclose information a rational counterparty can exploit. The intent model requires users to expose preferences to solvers before execution --- and while competitive auctions are supposed to protect users through competition, the concentration data suggests the competition is thinning. Flashbots' Flashnet research (February 2026) --- an anonymous broadcast protocol --- represents one approach to this problem, but privacy-preserving intent propagation remains an unsolved infrastructure challenge.

## d. The Funding-to-Shipping Ratio

The intent ecosystem has attracted enormous venture capital. The divide is stark:

**Purpose-built intent blockchains** ($75M+ in funding): Anoma (~$58-65M across six years) launched XAN token and governance but its core protocol adapters remain pending audit. Essential ($16.15M) has a pre-alpha devnet and missed all timelines.

**Intent features on existing protocols** ($200B+ in production volume): CoW Protocol ($87B in 2025), deBridge (~$32.8B cumulative), Across (~$28.6B), 1inch Fusion ($25B+), NEAR Intents (~$14B estimated). All built as features or extensions of existing infrastructure.

The pattern is unambiguous: **the intent movement succeeded as a feature, not as a platform.** Existing protocols sidestepped the cold-start problem (no users without solvers, no solvers without users) because they already had users and liquidity. This does not mean purpose-built architectures will never deliver --- Anoma's ARM is novel technology with native privacy --- but six years and $60M+ for a system that has not enabled its core functionality in production is a cautionary tale.

## e. Trust Assumptions and Censorship

Paradigm identified the core trust problem: "there needs to be at least one other party who is aware of the intent, incentivized to execute the intent and able to do so." The trust stack has layers, each with failure modes:

- **Propagation**: The Ethereum mempool does not natively support intent propagation, and as Paradigm noted, "concerns around DoS attacks may mean that generic support for fully general intents in the Ethereum mempool is out of the question even in the long term." No decentralized, permissionless intentpool exists. Every protocol runs proprietary intent submission infrastructure. The path from user to solver currently runs through centralized infrastructure: Uniswap's filler registry, CoW Protocol's order book API, project-specific RPC endpoints.
- **Solver selection**: Participation is gated everywhere --- Uniswap controls filler access, CoW requires bonds, 1inch requires whitelisting. A "competitive market" where the operator controls participation is not permissionless in any meaningful sense.
- **Execution**: If no solver finds an intent profitable, it expires unfilled. For liquid pairs on Ethereum mainnet this is rare. For long-tail tokens, exotic pairs, or periods of extreme volatility, execution risk is real. Pre-confirmation guarantees remain an active research area.
- **Settlement**: The strongest layer. Smart contracts atomically enforce that if a solver claims to have filled an intent, the on-chain state must reflect the claimed outcome. This works well and represents the most trusted component.

Intent systems route preferences through intermediaries who can *choose* which intents to execute. This is a censorship vector the public mempool does not have in the same way. A validator can censor individual transactions, but the mempool itself is permissionless. Intent pools are permissioned by design. The censorship resistance implications are not hypothetical --- they are an architectural tradeoff the Ethereum community cares deeply about.

Trustworthy settlement with centralized plumbing --- an inversion of the decentralization narrative. The intent stack is strongest at the bottom (settlement) and weakest at the top (propagation, solver selection). Decentralizing the top of the stack without enabling DoS attacks is an unsolved infrastructure problem that will define the next phase of intent system design.

## f. Intents and Smart Contracts Coexist

Smart contracts do things intents cannot: autonomous execution (liquidations without solver intervention), composable building blocks (permissionless contract-to-contract calls), verifiable determinism, and persistent state. Intents do things smart contracts handle poorly: cross-chain coordination, complex multi-step optimization, and hiding execution complexity from users.

The boundary between them is dynamic, governed by solver sophistication, gas economics, and composability requirements. Operations will migrate to intent-based execution as solvers grow more capable and gas costs make manual routing uneconomical. But autonomous protocol logic, composable DeFi primitives, and deterministic state machines will remain in smart contracts. The maximalist claim that "intents replace smart contracts" is as wrong as saying "SQL replaces application code." They coexist permanently, with the boundary shifting gradually toward intents for user-facing operations while smart contracts remain the substrate for protocol logic.

The migration pattern is visible: swap execution has largely moved to intent-based systems, cross-chain transfers are following, and portfolio management is beginning to experiment (Aave-CoW). But liquidations, governance, and composable DeFi building blocks remain firmly in smart contract territory. Operations where users have clear outcome preferences but unclear execution preferences migrate first; operations requiring deterministic, permissionless execution stay.

---

# VIII. WHAT WE BELIEVE

Five questions define the next phase. Rather than leaving them open, we state our calibrated assessment.

**1. Solver markets will consolidate into stable oligopolies within 2-3 years.** The structural forces --- economies of scale, exclusive orderflow relationships, technology moats, and winner-take-most auction dynamics --- are too strong for current countermeasures. Fair combinatorial auctions and open-source solver tooling slow the concentration but do not reverse it. The most likely outcome is 3-5 dominant solver entities per platform, competing on margins thin enough to maintain the appearance of competition while extracting rents a truly competitive market would not permit. The payment-for-order-flow (PFOF) parallel is apt: the EU's MiFID II review and the SEC's proposed Rule 615 both aimed to address analogous concentration in TradFi, and those regulatory precedents are directly relevant to solver market design. **Confidence: high.**

**2. Intent-as-feature will dominate intent-as-platform for the next 3-5 years.** Purpose-built intent blockchains have failed to ship at scale. Intent features on existing protocols have processed hundreds of billions. The cold-start problem is real and unsolved for new platforms. The market has spoken, and late entrants should build features on existing systems rather than new substrates.

**3. The AI-agent convergence is real but 2-3 years from production impact.** The structural parallel between prompts and intents is genuine, not merely superficial --- both involve declarative specification, delegated search, and constraint satisfaction. But the verification infrastructure is not ready. Until there are credible commitment mechanisms for AI-mediated intents --- cryptographic attestation of model behavior, TEE-based agent execution, on-chain verification of agent reasoning traces --- the convergence remains a research frontier, not production infrastructure. The gap between LLM output quality (probabilistic, unverifiable) and smart contract settlement (deterministic, verifiable) is the hardest problem in the entire convergence thesis.

**4. Cross-ecosystem intent standards will follow the token standard path (fragmentation), not the HTTP path (convergence).** ERC-7683's 70+ projects are impressive within Ethereum. But NEAR's chain signatures, Cosmos/IBC Eureka's intent routing, and Jito's BAM plugins are ecosystem-specific approaches that serve different design philosophies. No universal intent standard will emerge in this cycle. Whether this matters depends on whether cross-ecosystem aggregators (LI.FI, NEAR Intents) can abstract over the fragmentation.

**5. Value will accrue to solvers, not intent protocols.** If intent protocols commoditize (driven by open standards like ERC-7683), the entities with proprietary execution algorithms and market-making infrastructure capture disproportionate value. The analogy is to the browser and the web: HTTP became universal and free, while the entities that controlled the interface layer (Google) captured the value. The solver centralization data makes this uncomfortably plausible: the intent standard becomes like HTTP (universal, commoditized) while solvers become like cloud providers (concentrated, profitable, hard to compete with).

### The Bottom Line

The declarative turn from Prolog to modern intent systems traces a coherent intellectual arc. The production numbers --- $87B through CoW alone in 2025, with estimated 2025-specific volume across intent systems exceeding $200B (aggregated from annual figures where available and annualized run-rates where not) --- prove the technology works at scale. The cross-ecosystem convergence proves the pattern is structural, not narrative.

But the market structure outcomes are trending toward the concentration patterns crypto was supposed to disrupt. Solver oligopolies, permissioned intent pools, centralized auction infrastructure --- these are not bugs of early markets. They are the equilibrium the economic forces push toward.

The technology is real. The decentralization is not --- yet. Whether the ecosystem can close that gap is the only question that matters. We are skeptical but not foreclosed. The structural forces favoring concentration are the same forces that created Citadel and Virtu in traditional finance, and no auction mechanism has yet demonstrated the ability to sustainably counteract them at scale. The intent paradigm's best hope is not to solve the concentration problem but to make the concentrated intermediaries *more accountable* --- through on-chain verification, transparent auction mechanisms, and regulatory oversight --- than the intermediaries they replace.

That may be enough. A world where solver oligopolies compete under transparent, verifiable rules is better than a world where anonymous MEV extractors operate without constraints. But it is not the decentralized, permissionless vision that animated the intent thesis. Intents delivered on the *declarative* promise and fell short of the *decentralization* promise. Whether that tradeoff is acceptable depends on what you thought crypto was for.

The genealogy from Prolog to CoW Protocol to LLM-mediated intents traces a single arc: the progressive expansion of declarative interfaces, from Horn clauses to natural language, from single built-in solvers to competitive global markets, from logical soundness guarantees to an open question about AI accountability. Each era widened the gap between what users could express and what the commitment infrastructure could guarantee. The intent paradigm's next chapter will be defined by whether that gap closes or widens further. The engineering talent, institutional backing, and production infrastructure are in place. The mechanism design problem --- building markets that stay competitive when structural forces push toward oligopoly --- remains unsolved. It is the most important open problem in DeFi.

---

# IX. SOURCES

## Academic and Research Papers

- Robinson, J.A. "A Machine-Oriented Logic Based on the Resolution Principle." *Journal of the ACM*, 1965. ([ACM Digital Library](https://dl.acm.org/doi/10.1145/321250.321253))
- Kowalski, R. "Algorithm = Logic + Control." 1979. ([Pomona College](https://cs.pomona.edu/~jcoa2018/f20/cs190/papers/kowalski.pdf))
- Jaffar, J. and Lassez, J.L. "Constraint Logic Programming." POPL 1987. ([ACM](https://dl.acm.org/doi/10.1145/41625.41635))
- Miller, M.S. and Drexler, K.E. "Markets and Computation: Agoric Open Systems." 1988. ([Agoric Papers](https://papers.agoric.com/papers/markets-and-computation-agoric-open-systems/abstract/))
- Sun, X. et al. "Cooperative AI via Decentralized Commitment Devices." arXiv, 2023. ([arXiv](https://arxiv.org/pdf/2311.07815))
- Yuminaga, Chen, Sui. "Execution Welfare Across Solver-based DEXes." arXiv, 2025. ([arXiv](https://arxiv.org/abs/2503.00738))
- ESMA. "Maximal Extractable Value: Implications for Crypto Markets." July 2025. ([ESMA](https://www.esma.europa.eu/sites/default/files/2025-07/ESMA50-481369926-29744_Maximal_Extractable_Value_Implications_for_crypto_markets.pdf))

## Protocols and Documentation

- [Paradigm: "Intent-Based Architecture and Their Risks." June 2023.](https://www.paradigm.xyz/2023/06/intents)
- [Wyvern Protocol Whitepaper (GitHub)](https://github.com/ProjectWyvern/wyvern-protocol/blob/master/build/whitepaper.pdf)
- [CoW Protocol Documentation](https://docs.cow.fi/)
- [CoW DAO: 2025 in Review](https://cow.fi/learn/cow-dao-2025-in-review)
- [PANews: CoW Protocol Revenue Analysis](https://www.panewslab.com/en/articledetails/v1qtbfsn.html)
- [UniswapX Overview](https://docs.uniswap.org/contracts/uniswapx/overview)
- [ERC-7683 Specification](https://www.erc7683.org/spec)
- [Across Protocol](https://across.to/)
- [Across Protocol on DefiLlama](https://defillama.com/protocol/across)
- [CoinDesk: Across DAO Restructuring](https://www.coindesk.com/markets/2026/03/12/across-s-acx-rockets-80-massively-beating-bitcoin-on-plans-to-dump-its-dao-structure)
- [LI.FI State of Interop for 2026](https://li.fi/knowledge-hub/the-state-of-interop-for-2026/)
- [LI.FI October 2025 Update](https://li.fi/knowledge-hub/li-fi-update-october-2025/)
- [CoinDesk: LI.FI $29M Series A Extension](https://www.coindesk.com/business/2025/12/11/cross-chain-liquidity-protocol-li-fi-raises-usd29m-in-series-a-extension)
- [deBridge on DefiLlama](https://defillama.com/bridge/debridge)
- [NEAR Intents $10B Milestone (Yahoo Finance)](https://finance.yahoo.com/news/near-intents-achieves-10b-swap-183636390.html)
- [1inch Fusion Deep Dive](https://blog.1inch.com/a-deep-dive-into-1inch-fusion/)
- [Messari: State of 1inch Q2 2025](https://messari.io/report/state-of-1inch-q2-2025)
- [0x RFQ System Overview](https://0x.org/docs/0x-swap-api/advanced-topics/about-the-rfq-system)
- [DefiLlama: Hashflow](https://defillama.com/dexs/hashflow)
- [Enso Network](https://www.enso.build/)
- [OneBalance Resource Locks](https://www.onebalance.io/resource-locks)
- [PropellerHeads / Tycho](https://www.propellerheads.xyz/)
- [Particle Network 2025 Review](https://blog.particle.network/2025-review/)
- [Circle: TXT2TXN](https://www.circle.com/blog/txt2txn-using-ai-llms-for-internet-based-applications)
- [BlockEden: 200M+ Smart Wallets](https://blockeden.xyz/blog/2026/01/20/account-abstraction-smart-wallets-erc-4337-eip-7702-mainstream/)

## Flashbots and MEV

- [The Future of MEV is SUAVE](https://writings.flashbots.net/the-future-of-mev-is-suave)
- [Introducing BuilderNet](https://buildernet.org/blog/introducing-buildernet)

## Credible Commitments and Theory

- [Dixon, C. "Computers That Can Make Commitments." a16z, 2020.](https://a16z.com/what-is-blockchain-computers-that-can-make-commitments/)
- [Sun, X. et al. "MEV and Credible Commitment Devices." Columbia CryptoEconomics Workshop, 2022.](https://arxiv.org/pdf/2311.07815)
- [Zamfir, V. "Smart Enough to Not Fall for Your Intents."](https://medium.com/smart-transactions/smart-enough-to-not-fall-for-your-intents-transactions-5620bb091bea)
- [Alliance DAO: "Intents Are Just..."](https://alliance.xyz/essays/intents-are-just)

## Cross-Ecosystem

- [IBC Eureka Launch (The Defiant)](https://thedefiant.io/news/press-releases/interchain-labs-launches-ibc-eureka-connecting-cosmos-ethereum-and-bitcoin-ecosystems-with-over-260-billion-in-market-cap)
- [Jito BAM](https://blog.millionero.com/blog/jito-jto-review-solanas-staking-and-mev-powerhouse/)
- [The Block: Aave-CoW $50M Swap Incident](https://www.theblock.co/post/393621/aave-and-cow-swap-publish-dueling-post-mortems-after-50-million-defi-swap-disaster)
- [Halborn: 1inch Hack March 2025](https://www.halborn.com/blog/post/explained-the-1inch-hack-march-2025)

## Ethereum Foundation and Standards

- [Hyperlane: The Open Intents Framework](https://www.hyperlane.xyz/post/the-open-intents-framework-unifying-ethereams-fragmented-ecosystem)
- [The Block: EF Intents Framework](https://www.theblock.co/post/342194/ethereum-foundation-collaborates-on-new-framework-to-standardize-buzzy-intents-transactions)
- [Turnkey: EIP-7702](https://www.turnkey.com/blog/account-abstraction-erc-4337-eip-7702)
- [Alchemy: Account Abstraction](https://www.alchemy.com/overviews/what-is-account-abstraction)

---

*This report reconciled data across multiple independent research sources. Where sources disagreed, discrepancies were resolved through public analytics platforms (DefiLlama, Dune, Messari, CoinGecko, Crunchbase). No data was fabricated. Where specific metrics could not be independently verified, this is noted in the text. Volume figures represent trading/transfer volume, not fee revenue --- these are fundamentally different metrics. The "$200B+" ecosystem aggregate is estimated from 2025-specific annual volumes where available (CoW $87B, deBridge ~$18B annualized, 1inch Fusion ~$24B annualized) and pro-rated run-rates where annual figures are not isolated; it should be treated as an order-of-magnitude estimate, not a precise sum.*