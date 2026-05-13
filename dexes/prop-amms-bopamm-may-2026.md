# Prop AMMs, Block Oracles, and Bebop's bopAMM Bet

> Bringing Solana-style propAMM design to Ethereum via builder coordination — and what gets imported with it.

**Author:** apriori
**Date:** May 2026

## tl;dr

- **bopAMM is a new, distinct Bebop product.** "Block Oracle Priced AMM" is in private beta on Ethereum as of May 2026, sitting alongside (not replacing) Bebop's PMM RFQ and JAM solver auction. It is the first publicly-announced production-bound attempt to bring Solana-style propAMM design to Ethereum.
- **The mechanism is coordination, not curve.** There is no on-chain pool and no inferable price curve. Multiple market makers contribute firm executable quotes off-chain; a benchmark is aggregated and pushed to participating block builders, who commit to incorporating the update into candidate blocks *before* any trade against bopAMM liquidity is sequenced.
- **Builder participation is structural, not advisory.** Titan and BuilderNet are named launch builders. 1010 Trading is the launch MM. Tokenwood and Quasilabs are launch solvers. This is application-specific ordering coordination, materially closer to enforcement than to MEV-Share-style soft routing.
- **EIP-8212 is the bridge.** Bebop's @mo_nokh has submitted block-scoped transient storage as the gas-economics fix that makes frequent in-block price updates viable on EVM. The Priority Update Registry (Flashbots + Uniswap-led ERC) is the standardization play bopAMM intends to converge toward.
- **The benchmark is a second product.** The announcement reframes the bopAMM oracle update as reusable data infrastructure — usable for collateral marks, derivatives settlement, best-execution analysis. That positions Bebop adjacent to Chainlink and Pyth for marking use cases, underpinned by execution commitments rather than off-chain consensus over reported prices.
- **Strict-definition prop AMMs (singleton-MM, active on-chain curve) still have not shipped at scale on EVM.** bopAMM is the closest production-bound attempt, but it gets there via coordination, not via a new on-chain pricing primitive. The category is converging on coordination, not architecture.
- **Volume is not revenue.** DefiLlama as of 2026-05-12 shows Bebop at $1.55B 30-day / $68.25B cumulative, Native at $2.089B 30-day / $26.173B cumulative, Hashflow at $443M 30-day / $21.20B cumulative. Bebop leads cumulative; Native leads current throughput. Fee/revenue data is not publicly disclosed; volume should not be read as a revenue proxy.

## Table of Contents

- [1. What "prop AMM" actually means](#1-what-prop-amm-actually-means)
- [2. Bebop's three products and where bopAMM fits](#2-bebops-three-products-and-where-bopamm-fits)
- [3. The Block Oracle Priced AMM mechanism](#3-the-block-oracle-priced-amm-mechanism)
- [4. The block builder program in practice](#4-the-block-builder-program-in-practice)
- [5. EIP-8212 and the Priority Update Registry](#5-eip-8212-and-the-priority-update-registry)
- [6. The block oracle as data infrastructure](#6-the-block-oracle-as-data-infrastructure)
- [7. What's open](#7-whats-open)
- [Sources](#sources)

## 1. What "prop AMM" actually means

A proprietary AMM is an on-chain liquidity venue where a *single, identified, active market maker* controls the pricing curve and updates it frequently — typically multiple times per second — using off-chain signals such as CEX prices, vol surface, and inventory state. The market maker is also typically the only LP. It is the MM's book, not a passive pool. The two architectures it is *not* the same as:

- **LP-AMM** (Uniswap v2/v3, Curve, Balancer): a passive, formulaic curve over depositor liquidity, updated only by trades against the pool.
- **Pure off-chain MM / RFQ**: no on-chain liquidity venue at all. A market maker signs off-chain quotes that the taker atomically claims via a settlement contract. Inventory lives in the MM's wallet or a vault. There is no published curve.

The defining technical characteristic of a prop AMM is **active price updates pushed to on-chain state, with priority over taker fills**, so quotes reflect current market conditions and the MM bears manageable adverse-selection risk. That priority is the load-bearing piece. Without it, the MM's update can be reordered behind a sniper and the protection collapses.

On Solana the strict-definition prop AMMs have captured majority share. Lifinity (oracle-anchored AMM, launched January 2022) was the first, with explicit single-LP design where the protocol itself is the LP. As of late 2025, prop AMMs control 50–60%+ of Solana DEX volume — Galaxy Research reported >50% as of October 30 2025, and Helius reported a sustained 60%+ average by December 2025. The drivers are exactly what you would expect: cheap state writes, transaction-priority guarantees from sequential execution, and oracle latency that makes top-of-block updates economically viable.

The honest answer for Ethereum is harder. **Strictly-defined prop AMMs — singleton-MM on-chain curve, frequent state-write updates — have still not shipped at meaningful scale on EVM as of May 2026.** Flashbots published an "Optimus" proof of concept on December 19 2025 sketching the design via builder-enforced top-of-block ordering; it remains a PoC. What ships at scale on Ethereum are RFQ and solver-auction systems that look prop-AMM-adjacent without being prop AMMs in the strict sense.

A useful taxonomy of the active / professional MM venue category on EVM, after the bopAMM announcement:

| Bucket | Examples | Where prices live | Who signs | Settlement |
|---|---|---|---|---|
| **Pool-based prop AMM (strict)** | Lifinity (Solana), Flashbots Optimus PoC | On-chain curve params, MM-updated | MM writes state | Standard swap call |
| **RFQ (off-chain quote, on-chain claim)** | Hashflow, 0x RFQ, Bebop PMM RFQ, Native | Off-chain, per-request | MM signs EIP-712 quote | Settlement contract verifies + transfers |
| **Solver auction** | CoW Protocol, UniswapX, 1inch Fusion, Bebop JAM | Off-chain, per-intent | User signs intent; winning solver executes | Solver-driven, sometimes batched |
| **Builder-coordinated oracle-priced (new)** | **Bebop bopAMM (May 2026)** | Off-chain quote formation aggregated to a benchmark; builder enforces incorporation in-block | MMs commit firm executable quotes; builder commits to ordering | Permissionless on-chain execution against the bopAMM benchmark |
| **Formula MM / hybrid** | Clipper FMM (post-April-2022 upgrade) | On-chain formula + oracle | Mixed | Selected per route |

Worth flagging: Clipper's "$20M LP cap" framing is historical. The cap was removed in April 2022 when Clipper migrated to a Formula Market Maker (FMM) model — an on-chain pricing function that combines an invariant with external oracle inputs. Treat Clipper as a hybrid formula MM in 2026, not a capped-LP venue.

The five-bucket cut answers a question worth carrying: what's actually the EVM analogue to a prop AMM, given that the literal architecture hasn't shipped at scale? Before May 2026, the closest production-bound thing was the RFQ + solver-auction stack at Bebop, Hashflow, Native, and CoW. After May 2026 there is a fifth bucket — coordination-via-builders with multi-MM benchmark quotes — and bopAMM is its first occupant. The category is converging on coordination, not architecture. EVM is not getting a Lifinity clone; it is getting a coordination layer that produces *prop-AMM-like behavior* via builder commitments rather than via a new on-chain primitive. Whether that's enough is the question that motivates the rest of this report.

## 2. Bebop's three products and where bopAMM fits

As of May 2026 Bebop ships three coexisting products. They are not three versions of the same thing; they are three points on a coordination spectrum.

**PMM RFQ** (legacy signed-quote stream, settlement at `0xbbbbbBB520d69a9775E85b458C58c648259FAD5F`). A taker requests a quote for a specific pair, size, and side. Bebop's orchestrator polls connected market makers; each returns an off-chain quote with a validity window. The orchestrator selects the best quote and returns an EIP-712 typed message to the taker. The taker signs. With Permit2, ERC-20 approval is bundled into the same signature. On-chain, `BebopSettlement` verifies *both* signatures (taker + maker), checks the price meets the agreed minimums, and atomically transfers tokens. Reverts if any condition fails. Crucially, the maker also signs. This is not a pool the user swaps against — it's a coordinated, signature-gated atomic swap between two parties, mediated by a contract that just enforces the agreed terms. Architecturally identical in shape to Hashflow and 0x RFQ.

**JAM** (Just-in-time Aggregation Model, solver auction, launched December 2023, settlement at `0xbeb0b0623f66bE8cE162EbDfA2ec543A522F4ea6`). The user signs an intent containing trade parameters — sell token(s), buy token(s), min received, expiry. JAM supports multi-token in/out (USDT+USDC → WETH+WBTC style baskets). The JAM orchestrator polls a panel of solvers, who are "independently run services" responsible for quoting *and* executing — they can use any liquidity source (DEXes, their own MM inventory, other RFQ panels). Bebop runs its own solver alongside third-party solvers and PMMs that participate as solvers. The orchestrator picks the solution that maximizes tokens received minus execution cost, and returns it for user signature. Settlement uses `JamBalanceManager` at `0xC5a350853E4e36b73EB0C24aaA4b8816C9A3579a` to hold takers' approvals, so users approve the balance manager, not the settlement contract directly.

**bopAMM** (Block Oracle Priced AMM, private beta, announced May 2026). The subject of this report. Section 3 develops it in full.

The Bebop Router API is the public-facing meta-layer: it queries both PMM RFQ and JAM, picks the better of the two after factoring gas, and returns one quote to the user. Whether bopAMM eventually plugs into the same Router — or whether bopAMM is the strategic destination and PMM/JAM are transitional — is one of the open architectural questions the announcement does not yet answer.

**Is there a pool?** Across all three Bebop products, the answer is no. Bebop's own documentation is direct about this: "Bebop uses atomic swaps where participant's balances are directly transferred between each other in a single transaction. This approach avoids storing assets in smart contracts, reducing attack vectors compared to traditional AMM pools." The bopAMM announcement extends the same logic — "bopAMM pricing is driven by coordinated quote formation rather than directly inferable pool curves." No pool curve to read; the benchmark is aggregated from multiple MMs' firm quotes and incorporated by builder coordination.

This is the architectural point worth surfacing, not as criticism but as taxonomy. **In strict architectural terms, none of Bebop's products are AMMs.** All three are coordination + settlement layers. The product name "bopAMM" is a market-structure label — "*priced like* an AMM that consumes a block oracle update" — not a literal x*y=k claim. The user-visible behavior (active pricing from professional MMs, narrow spreads, MEV-resistant by construction, no signing window for bopAMM) matches what a prop AMM delivers on Solana. The mechanism underneath is different. That distinction matters because the trust assumptions you import are different.

For context on the comparison set: JAM is sometimes mistaken for a CoW-style batch auction. It isn't. JAM is per-order solver competition (inclusive RFQ shape), closer in spirit to 1inch Fusion or UniswapX. CoW runs a fair-combinatorial *batch* with a uniform clearing price across same-direction orders, with COW-token rewards distributed via a VCG-style second-price mechanism. Different fairness commitment, different solver economics, different mental model. Both internalize MEV at the solver layer rather than the builder layer. CoW's design is a stronger fairness commitment to users (no order in the same batch and same direction gets a different price than another), while JAM's per-order design is faster and more flexible — solvers don't have to wait for a batch window. The relevant point for this report: neither CoW nor JAM is structurally builder-aware. bopAMM is.

## 3. The Block Oracle Priced AMM mechanism

The mechanism, per the announcement, has three roles:

**Market makers** continuously provide firm executable quotes and commit capital to fill trades. Multiple MMs compete; the combined depth and tighter pricing forms a *shared benchmark with executable size behind it*. The shared part is the move. In a Solana propAMM the single MM bears the full cost of frequent updates. In bopAMM the cost is spread across the participating MM panel, and the benchmark is the aggregation rather than any single MM's book.

**bopAMM (the coordinator)** aggregates these quotes, publishes the benchmark, and distributes the auditable data on equal terms to participating block builders. The coordinator is also responsible for spoofing / false-quoting detection — the neutral role includes integrity policing.

**Block builders** receive the benchmark update and incorporate it into candidate blocks *before* any trade against bopAMM liquidity is sequenced. Builders continuously rebuild as new information arrives. The commitment is to ordering: update first, trades after.

Takers trade permissionlessly against bopAMM liquidity — direct, via aggregator, via wallet, via dapp. From the taker's perspective it behaves like a venue you can route into without a signing window. From the MM's perspective it preserves CEX hedgability because quote formation still happens off-chain.

Two motivating problems are stated explicitly in the announcement:

1. How can continuous price discovery be reflected within the discrete nature of 12-second Ethereum blocks?
2. How can frequent benchmark price updates remain economically viable so coordination cost doesn't outweigh execution improvements?

Both are answered with coordination. The 12-second block problem is solved by treating the block as the *unit of price commitment* — every block carries a fresh executable benchmark, and the builder's ordering rule guarantees that trades against bopAMM liquidity are sequenced after the incorporated update. The cost-of-frequent-updates problem is solved by spreading the cost across the MM panel and by reducing the per-update gas footprint via block-scoped transient storage (EIP-8212, section 5).

The design lineage is explicit in the announcement. Solana pioneered propAMM designs because low latency and rapid block times made it possible to rethink onchain market structure. Lifinity et al. → expansion to Base → bopAMM as "the new execution primitive for Ethereum that brings this next chapter of market structure to the deepest capital base in crypto." apriori's standing instruction is not to go deep on Solana internals, so the framing here is design lineage only: what bopAMM is porting is the *update-priority pattern*, not the sequencing primitives that make it cheap on Solana. The latter is what builder coordination is trying to substitute for.

The architectural thesis to take seriously is this: bopAMM is firmly on the coordination side of apriori's coordination-vs-architecture axis. The announcement says so explicitly — "coordination layer," "pricing is driven by coordinated quote formation rather than directly inferable pool curves." But the question worth holding is whether builder-enforced ordering promotes coordination from "soft commitment" to something with more structural force — and whether that effectively gives you the *behavior* of a prop AMM without the *architecture* of one.

This is a sharper version of the foil than CoW, 1inch Fusion, or UniswapX. Those systems coordinate over solvers and intents; builders are downstream consumers of orderflow rather than structural participants. bopAMM coordinates over builders directly. The ordering commitment is the product. That doesn't change the architectural category — it is still coordination over pre-existing primitives, not a new state machine — but it tightens the binding in a way that matters for the trust analysis. Section 4 develops what the binding actually looks like in practice.

For completeness: the Anoma resource-model alternative — declarative state-transition specs that say *what* the resulting state should look like and let solvers compete on *how* — is the orthogonal architectural path. bopAMM is not that. bopAMM is imperative coordination at a finer-grained, builder-aware layer. Both can be live at once.

## 4. The block builder program in practice

The block-builder program some observers have been asking about for two years now exists and has a name. It is bopAMM. The launch participants are named publicly.

**Block builders:** Titan, BuilderNet.
**Market maker:** 1010 Trading.
**Solvers:** Tokenwood, Quasilabs.

What's confirmed: builders commit to incorporating bopAMM's benchmark oracle update into candidate blocks *before* trades against bopAMM liquidity are sequenced. Builders continuously rebuild as updates arrive. The mechanism is *application-specific ordering coordination*, distinct from generic private-RPC routing à la Flashbots Protect or MEV Blocker. The bopAMM aggregator distributes auditable data on equal terms to participating builders — no builder gets a private channel that another participating builder doesn't.

What is *not* yet public: the exact builder-side commitment specifics. Is non-compliance contractually penalized, economically penalized via loss of future flow, or honor-system? Is there a settlement-side check that would revert a trade if the incorporated update is stale or wrong? Or is the commitment purely off-chain, with reputation and continued participation as the enforcement mechanism? The announcement doesn't say. The credible interpretation — and this is editorial — is that the v1 enforcement is reputational and economic rather than contractual, with on-chain verification of incorporation being a hard problem on the same level as the rest of the proposer-builder-separation trust stack. If you want a clean read on which way this generalizes, this is the question to track.

The honest interpretation of the launch builder choice is that this is a tradeoff between two things that both matter. To make the ordering commitment meaningful at launch, you need builders with non-trivial share — flow that bopAMM is not exposed to is flow where the ordering rule isn't being enforced. Titan is one of the larger centralized builders; BuilderNet is the decentralized TEE-based builder spun out of Flashbots' Beaverbuild lineage in 2025. Anchoring on Titan + BuilderNet gives bopAMM real coverage on day one. It also creates the obvious tail risk: exclusive orderflow concentrating into the same builder duopoly that BuilderNet was partly designed to dilute. BuilderNet's participation arguably *reduces* concentration relative to a Titan-only launch, because builds happen in TEEs across a permissioned operator set. But the framing matters. This is a real-world tradeoff between the practical need for high-share builders and the long-running structural concern about exclusive flow concentrating building.

The right way to read the launch builder set is as a v1 anchor, not the steady-state. The question that determines whether bopAMM is structurally healthy is what the path to fully-permissioned builder participation looks like — and whether the enforcement model survives the transition from a small, identified, accountable launch set to an open one. Solana's propAMMs don't have this problem because L1 sequencing handles the priority for them. EVM does have this problem, and bopAMM's bet is that builder coordination plus a converging ERC standard (section 5) is enough to substitute.

The value flow: takers get permissionless execution against a firm, fresh, multi-MM benchmark — MEV-resistant by construction because trades sequence *after* the update, removing the snipe-the-stale-quote vector by ordering rather than by hiding the trade. MMs get combined depth and shared cost of frequent updates while preserving CEX hedgability because quote formation still happens off-chain. Builders get preferential flow access plus the priority fees that come with it; the announcement does not disclose whether there is additional Bebop-side payment for the ordering commitment. Bebop gets a coordination-fee position, control of the benchmark dataset (section 6), and a market-share position in the propAMM-on-EVM category.

The comparison set worth holding:

| System | What "builder participation" means | Who does the work |
|---|---|---|
| **Solana propAMM (Lifinity et al.)** | No builder layer — Solana sequencing primitives make priority writes economically viable without external coordination | MM writes to chain state |
| **Flashbots Optimus PoC (Dec 19 2025)** | Builders commit to a top-of-block ordering rule for tx-es targeting a specific global storage contract; in exchange they earn priority fees | Builders enforce ordering for a single MM's writes |
| **Bebop bopAMM (May 2026)** | Multi-MM benchmark quotes aggregated off-chain → builders commit to incorporating the update *before* sequencing trades against bopAMM liquidity | Builders enforce ordering for an aggregated benchmark; bopAMM is the neutral coordinator |
| **MEV-Share** | Builders bid on hint-based orderflow; users get backrun rebates | Builders bid; users opt in via RPC |
| **MEV Blocker** | Builder-side opt-in: builders bid for orderflow, top builders get the flow, users get rebates | Builders bid; users opt in via RPC |
| **UniswapX** | Permissioned exclusivity window for selected fillers, then open; fillers route through whatever builders they choose | Fillers, not builders directly |

bopAMM's distinguishing position is straightforward: it is the first widely-announced production-bound design where builders commit to application-specific ordering coordination for an *aggregated multi-MM* benchmark. The Flashbots Optimus PoC is the closest design relative but it's single-MM and proof-of-concept. Solana's propAMMs don't need a builder layer at all because L1 sequencing handles it. Everything else in the comparison set is generic flow auction rather than application-specific ordering.

## 5. EIP-8212 and the Priority Update Registry

Two infrastructure plays were announced alongside bopAMM. They are not the product, but they are what makes the product story coherent at the EVM level.

**EIP-8212: block-scoped transient storage.** Submitted by Bebop's @mo_nokh. The mechanism: storage accessible across transactions within a block, discarded at block end. The economics: materially reduces the cost of frequent in-block price updates versus persistent storage. Persistent storage is the dominant gas cost for any design that updates on-chain state often. Block-scoped transient storage is the EVM-native primitive that brings the per-update cost close to Solana levels at the contract level, without changing block time or sequencing.

If 8212 lands, it is the change that expands the universe of assets where the cost/benefit math for propAMM-style design works. The current EVM math limits frequent state updates to a small set of high-volume pairs where the spread savings dominate the gas cost. If the per-update gas falls by an order of magnitude or more, that set widens. It is also the EIP that converts "Solana-style propAMM on Ethereum" from a coordination problem to a coordination *plus* primitive problem — bopAMM works today via builder coordination; with 8212 it works with materially better contract-level economics on top.

**The Priority Update Registry.** An industry-standard initiative led by Flashbots and Uniswap that bopAMM is contributing to. The initial bopAMM implementation uses custom mechanisms; Bebop expects to converge toward the ERC. This is a standardization play with familiar shape — ship custom, contribute to the standard, switch over once the standard converges on a design the project can live with. The risk is the usual one: if the ERC converges on a design that meaningfully diverges from Bebop's preferred shape, bopAMM either lives on its custom path or accepts the standard at some friction cost. The opportunity is the bigger one: a standard for application-specific ordering coordination is the piece that takes "builder coordination" from a per-application bilateral arrangement to a generalizable primitive.

The way these two fit together is worth holding. EIP-8212 lowers the contract-level cost of being a prop-AMM-like venue on EVM. The Priority Update Registry lowers the *coordination* cost of being a builder-coordinated venue. Together they describe a future where the bopAMM design pattern is replicable rather than bilateral — where any sufficiently-resourced MM coalition can stand up a builder-coordinated benchmark for any asset class without negotiating ordering rules bilaterally with each builder.

That is a more interesting endgame than bopAMM-the-product, and it is also the endgame in which Bebop's strategic position depends on something other than being the first mover. If the Priority Update Registry succeeds, the moat moves from "we built the coordination layer" to "we run the most credible MM panel and benchmark for assets X, Y, Z." The product question becomes: is the benchmark itself the durable asset?

## 6. The block oracle as data infrastructure

The bopAMM announcement frames the benchmark update as reusable data infrastructure. The language is direct: usable for lending collateral marks, derivatives settlement, and best-execution analysis. Underpinned by real execution commitments and aggregated across multiple makers.

This is the piece of the announcement that quietly says the most. It positions Bebop adjacent to Chainlink and Pyth for collateral-marking and settlement use cases, but the underlying construction is different. Chainlink aggregates off-chain reported prices via a permissioned node operator set. Pyth aggregates first-party reported prices from publishers (exchanges, MMs, market-data providers) with confidence intervals. Both are *reported* price systems — the data primitive is a price quoted by an entity that claims to know.

The bopAMM benchmark is something else. It is a price aggregated from MMs who have *committed firm executable size at that price*. The marking is not "this is what we think the price is"; it is "this is the price at which a panel of MMs has agreed to trade now, with capital committed." For collateral marks on a lending venue or settlement of a derivative, that is a structurally different guarantee than reported pricing. It is closer to an executable mid than to an oracle in the conventional sense.

Whether this materializes as a productized data feed — push, pull, ERC-standardized — is not stated in the announcement. The phrasing is "usable for," which is closer to framing than to commitment. The honest read: this is a real product direction that the team wants to flag now, not a shipped product. The competitive frame is real even if the productization timeline isn't.

What's interesting about this is the symmetry it creates. bopAMM-the-venue depends on multi-MM commitments to firm executable quotes. bopAMM-the-data-feed *is* those commitments, packaged. The same primitive serves both products. The MM panel that powers the execution venue powers the oracle, and the oracle's credibility is downstream of the execution venue's activity. That is a more defensible position than either product alone — the venue's credibility comes from the data, the data's credibility comes from the venue, and the panel of MMs is the joint asset that produces both.

It is also where the architectural question gets most pointed. If the bopAMM benchmark becomes a serious mark/settlement reference for DeFi lending and derivatives, the trust assumptions that bopAMM imports — builder ordering commitments, MM panel composition, the coordinator's anti-spoofing role — become trust assumptions for the consumers of the benchmark too. The same way Chainlink's node set composition matters for every protocol that trusts a Chainlink price, bopAMM's MM panel and builder set would matter for every protocol that trusts a bopAMM mark. That is the reason the announcement spends time on neutrality, equal-terms data distribution, and anti-spoofing detection — those are load-bearing properties for the data product, not nice-to-haves.

## 7. What's open

The announcement leaves a set of structural questions unanswered. They are not gotchas; they are the questions the product has to answer over the next six to twelve months for the design to be evaluated honestly.

**Builder-side enforcement.** The announcement confirms builders commit to incorporating the update before sequencing trades. It does not say what happens when they don't. Reputational pressure plus future flow access is a credible v1 enforcement model, but the absence of an on-chain verification path means the system is leaning on off-chain accountability for a load-bearing property. The honest direction here is either an on-chain receipt that the update was incorporated (with a settlement-level revert if not), or an explicit acknowledgment that this is a trust commitment that scales with the credibility of the participating builder set. Either is defensible. The current state is neither.

**MM commitment under stress.** Firm executable quotes are firm until they aren't. What happens when an MM is asked to fill against a benchmark it just helped form and inventory is depleted, or when a sharp move makes a freshly-incorporated quote stale within the block? The announcement says MMs commit capital. It does not say what backstops the commitment when capital is exhausted. The honest comparison is to TradFi where best-execution obligations carry teeth via regulator enforcement; in DeFi the teeth are reputation and exclusion from future participation. Whether that's enough depends on how the panel is composed and how penalties are imposed.

**Contract surface.** Settlement venue, fee model, value-capture split, integration path with the Bebop Router. None of this is public yet. None of it is unusual for private beta. All of it determines whether bopAMM is a third coexisting Bebop product or the strategic destination that PMM RFQ and JAM eventually fold into.

**Concentration trajectory.** Titan and BuilderNet at launch is a defensible v1 anchor. The question is the trajectory from launch partners to fully-permissioned builder participation. If the answer over the next year is "no change," the structural concern compounds. If the answer is a credible path to open participation governed by the converging ERC, the concern relaxes. This is the single tracking metric that will tell you whether bopAMM generalizes or stays bilateral.

**Coordination vs architecture, restated.** bopAMM is the sharpest version of the coordination foil — sharper than CowSwap, UniswapX, or 1inch Fusion, because builder participation is structural rather than advisory. But it is still coordination. The user-visible behavior approximates a prop AMM; the underlying mechanism is a set of off-chain commitments enforced by reputational and economic pressure. Whether that produces durable advantages over a hypothetical declarative state-transition design that solves the problem at the architecture layer is the question that doesn't get resolved by the announcement and probably won't get resolved by the v1 product.

What the announcement does establish is that the coordination path on EVM is now a real, named, partnered design with public participants and an explicit standards trajectory. That is a meaningful change from where the conversation was a quarter ago, when the same question was framed as "if anyone is going to do this on EVM." Someone is. The question moves from existence to evaluation.

## Sources

**bopAMM (primary, May 2026):**
- Canonical Bebop bopAMM announcement (private source, May 2026)
- EIP-8212 (block-scoped transient storage) — author @mo_nokh
- Priority Update Registry initiative (Flashbots + Uniswap-led ERC, in progress)

**Bebop docs and contracts:**
- [Bebop — Settlement & Smart Contracts](https://docs.bebop.xyz/core-concepts/settlement-smart-contracts.md)
- [Bebop — Supported Chains](https://docs.bebop.xyz/supported-chains.md)
- [Bebop — How Bebop Works](https://docs.bebop.xyz/bebop/how-bebop-works)
- [Bebop docs index (llms.txt)](https://docs.bebop.xyz/llms.txt)
- [GitHub — bebop-dex org](https://github.com/bebop-dex)
- [GitHub — bebop-jam-contracts](https://github.com/bebop-dex/bebop-jam-contracts)
- [GitHub — bebop-dex/jam-whitepaper](https://github.com/bebop-dex/jam-whitepaper)

**Bebop blog / Medium:**
- [Bebop unveils JAM: Intent-Based Liquidity Aggregation System (Dec 2023)](https://medium.com/bebop-dex/bebop-unveils-jam-intent-based-liquidity-aggregation-system-e14a6feedaae)
- [WTF is MEV Protection? — Bebop Medium](https://medium.com/bebop-dex/wtf-is-mev-protection-42628f83e407)
- [WTF makes Bebop so secure? — Bebop Medium](https://medium.com/bebop-dex/wtf-makes-bebop-so-secure-59ebb289de4f)
- [WTF is RFQ on-chain? — Bebop Medium](https://medium.com/bebop-dex/wtf-is-rfq-on-chain-19560e00058b)
- [Bebop is open to the world on Polygon and Ethereum — Katia Banina, Nov 2022](https://medium.com/bebop-dex/bebop-is-open-to-the-world-on-polygon-and-ethereum-4ea4062486e)

**Etherscan (contract verification):**
- [BebopSettlement `0xbbbb…FAD5F`](https://etherscan.io/address/0xbbbbbBB520d69a9775E85b458C58c648259FAD5F)
- [JamSettlement `0xbeb0…4ea6`](https://etherscan.io/address/0xbeb0b0623f66bE8cE162EbDfA2ec543A522F4ea6)
- [JamBalanceManager `0xC5a3…579a`](https://etherscan.io/address/0xC5a350853E4e36b73EB0C24aaA4b8816C9A3579a)
- [Predecessor BebopSettlement `0xbeb09000…`](https://etherscan.io/address/0xbeb09000fa59627dc02bb55448ac1893eaa501a5)

**Prop AMM landscape:**
- [Flashbots Collective — Prop AMMs on EVM: A Proof of Concept (Optimus, Dec 19 2025)](https://collective.flashbots.net/t/prop-amms-on-evm-a-proof-of-concept/5440)
- [Helius — Solana's Proprietary AMM Revolution](https://www.helius.dev/blog/solanas-proprietary-amm-revolution)
- [Galaxy Research — Solana DEX Volumes and Prop AMMs (>50% Oct 30 2025)](https://www.galaxy.com/insights/research/solana-dex-volumes-prop-amms)
- [Shipyard Software — What is a Formula Market Maker?](https://www.shipyardsoftware.org/post/what-is-a-fmm)
- [Phemex — What is Hashflow (HFT) & How Does its RFQ Model Work?](https://phemex.com/academy/what-is-hashflow-hft)

**CoW + intent comparison set:**
- [CoW Protocol — Solvers](https://docs.cow.fi/cow-protocol/concepts/introduction/solvers)
- [CoW Protocol — Fair Combinatorial Batch Auction](https://docs.cow.fi/cow-protocol/concepts/introduction/fair-combinatorial-auction)
- [Modern DEXes, how they're made: CoW Protocol — MixBytes](https://mixbytes.io/blog/modern-dex-es-how-they-re-made-cow-protocol)
- [Intent-based DEX Comparison: UniswapX vs 1inch vs CoW Swap — Gate Learn](https://www.gate.com/learn/articles/intent-based-dex-comparison/5515)
- [How 1inch Fusion powers fast, MEV-protected swaps — 1inch blog](https://blog.1inch.com/a-deep-dive-into-1inch-fusion/)

**Builder ecosystem:**
- [BuilderNet — Introducing BuilderNet](https://buildernet.org/blog/introducing-buildernet)
- [BuilderNet — Beaverbuild → BuilderNet (May 6 2025)](https://buildernet.org/blog/beaverbuild)
- [Flashbots Protect overview](https://docs.flashbots.net/flashbots-protect/overview)
- [Rollup Boost — Introduction](https://rollup-boost.flashbots.net/)
- [Optimus on X — From Solana to the EVM: A New Path for Proprietary AMMs](https://x.com/0xOptimus/status/1981424092818399598)

**Market data (DefiLlama, as of 2026-05-12):**
- [DefiLlama — Bebop](https://defillama.com/protocol/bebop) — $1.55B 30d / $68.25B cumulative
- [DefiLlama — Native](https://defillama.com/protocol/native) — $2.089B 30d / $26.173B cumulative
- [DefiLlama — Hashflow](https://defillama.com/protocol/hashflow) — $443M 30d / $21.20B cumulative
- [Dune — Wintermute Bebop Trading Volume](https://dune.com/danning.sui/wintermute-bebop-trading-volume)
