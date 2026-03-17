---
title: "Application Design: Solana (SVM) vs. Ethereum (EVM) — A Builder's Perspective"
date: 2026-03-17
---

# Application Design: Solana (SVM) vs. Ethereum (EVM)
## A Builder's Perspective — Comprehensive Comparison Report

**Report Date:** March 17, 2026
**Confidence Level:** High (web-verified data where available, clearly marked training-knowledge sections)

---

## TL;DR — Read This to Understand the Entire Report

**If you're deciding where to build, here's what actually matters:**

**Multisig & Governance:** Both ecosystems have a dominant multisig standard — **Safe** (Ethereum, $50B+ secured) and **Squads** (Solana, $10B+ secured). The key difference: Ethereum governance is more *layered* (Safe + Timelock + Governor pattern = 3-step pipeline), while Solana governance is *flatter* (Squads upgrade authority, often with a timelock bolt-on via Squads v4). Ethereum's proxy-based upgradeability creates complex trust hierarchies; Solana's upgrade authority model is simpler but equally powerful — the upgrade authority key *is* the governance surface.

**Off-Chain Infrastructure:** This is where the ecosystems diverge most sharply. **Solana demands significantly more off-chain infrastructure to run a production app.** You need: a high-quality RPC provider (Helius/Triton, ~$999/mo+ for production), transaction landing services (Jito bundles, priority fee tuning), and likely a Geyser plugin or indexer for real-time data. On Ethereum, you still need RPC (Alchemy/Infura) and indexing (The Graph/Goldsky), but the transaction submission is simpler — no stake-weighted QoS to navigate, though you'll need MEV protection (Flashbots/private mempools). **Both ecosystems require $1-5K/month minimum in infrastructure spend for production apps.**

**Open/Closed Source:** Ethereum has a stronger *social norm* of open-source contracts — Etherscan verification is expected, and unverified contracts are treated with suspicion. Solana is catching up with the Ellipsis Labs/OtterSec verified builds pipeline, but adoption is still nascent. The BSL (Business Source License) trend started on Ethereum (Uniswap v3/v4) and is spreading — it's a "source-available but not forkable" middle ground that both ecosystems now grapple with.

**Dependencies & Composability:** Ethereum has OpenZeppelin as a mature, battle-tested standard library. Solana has Anchor (dominant framework) and the SPL token programs. Key difference: EVM composability works through token approvals, flash loans, and callbacks in a shared-state model. Solana composability works through CPI (cross-program invocation) with explicit account passing — more verbose but eliminates reentrancy. **Solana's account model means you think about data layout upfront; Ethereum's storage model means you think about upgrade safety upfront.**

**Developer Experience:** Solidity + Foundry/Hardhat is easier to learn and has a much larger talent pool. Rust + Anchor has a steeper curve but produces more memory-safe code. Foundry has overtaken Hardhat in usage (51% vs 33% per the 2024 Solidity survey). On Solana, Anchor is dominant for new projects, with native Rust used for performance-critical programs (e.g., Phoenix DEX, Jito).

**Cost to Build & Operate:** Solana is cheaper for users (sub-cent transactions) but more expensive for builders in infrastructure costs. Ethereum is cheaper in infrastructure (more commoditized RPC market) but more expensive for users (gas fees, especially on L1). **L2s (Arbitrum, Base, Optimism) have largely closed this gap for EVM user costs.** Program deployment on Solana costs ~2-5 SOL for rent-exempt accounts; EVM contract deployment costs vary wildly with gas prices.

**Security & Audit Culture:** Different attack surfaces entirely. EVM: reentrancy, storage collisions, proxy upgrade bugs, front-running. SVM: missing signer checks, account confusion, CPI privilege escalation, oracle manipulation. **The EVM audit ecosystem is more mature** (Trail of Bits, OpenZeppelin, Spearbit, Code4rena, Sherlock). Solana's is growing (OtterSec, Neodyme, Sec3/Ackee, Halborn) but has fewer firms and less competitive audit marketplaces.

**Oracles:** Fundamentally different models. EVM uses **Chainlink** (push-based, $100B+ TVS, 83% ETH market share) — your contract calls `latestRoundData()` and gets a price. Solana uses **Pyth** (pull-based, ~$2.5B TVS) — you must fetch a signed price off-chain and include it in your transaction. Push is simpler to integrate; pull is always fresh. Both ecosystems now trend toward hybrid models. **Chainlink SVR** recaptures liquidation MEV (65/35 split with Aave). **Switchboard** is the sole VRF provider on Solana.

**Token Launches:** Radically different culture. Solana is dominated by **pump.fun** (70-77% of all token launches, $1B+ cumulative revenue, 13M+ tokens created). EVM uses Uniswap v2 pools + **Balancer LBPs** (via Fjord Foundry) for fair price discovery. Solana launches are cheaper (~$3 via pump.fun) but graduation rate is ~1.1%. EVM launches cost $100-500 on mainnet, <$5 on L2.

**Network Reliability:** Ethereum L1 has **never gone down** (since 2015). Solana has had **87+ outages** — improving significantly with Firedancer (20.9% of stake) and QUIC protocol, but builders must implement robust retry logic and never assume a transaction landed. L2 sequencer downtime is a real EVM risk (Arbitrum had multi-hour outages).

**Production Debugging:** EVM has **Tenderly** (step-by-step Solidity debugger, 330M+ alerts/yr, simulation with state overrides) — root cause in ~2 minutes. **Solana has no equivalent.** Debugging is manual log parsing, often 10x slower. This is the biggest DX gap between the ecosystems.

**Compliance:** Solana's Token-2022 has **built-in compliance primitives** (freeze, permanent delegate for clawback, confidential transfers, transfer hooks) — used by PYUSD. EVM relies on **per-contract logic** (USDC blacklist, Chainalysis Sanctions Oracle, ERC-3643 for security tokens with $32B+ tokenized).

**The Bottom Line:** If you want the largest developer pool, most mature tooling, and can build on L2s — go EVM. If you need sub-second finality, cheap user transactions on L1, and can invest in off-chain infrastructure — go Solana. Many serious teams are now building on both.

---

## 1. Multisig & Governance Conventions

### Ethereum (EVM)

**The Standard: Safe (formerly Gnosis Safe)**

Safe is the dominant multisig on Ethereum, securing over **$50 billion** in assets. In 2025, Safe processed **$600 billion** in transaction volume (43% of its lifetime volume). The Safe Ecosystem Foundation reported **$10M+ annualized revenue** in 2025, up from $2M in 2024. Safe secures the Ethereum Foundation's 160,000 ETH treasury and Circle's $2.5B USDC reserves.

**The Governance Stack (Layered Pattern):**

Most serious EVM protocols use a 3-layer governance pattern:

1. **Safe Multisig** — Immediate execution for operational decisions (team pays, parameter tweaks)
2. **TimelockController** (OpenZeppelin) — Enforces a delay (24-48h typical) between proposal and execution, giving the community time to react or exit
3. **Governor** (OpenZeppelin/Compound Governor Bravo) — On-chain voting for significant protocol changes, typically token-weighted

Safe processed **$189.6 billion in Q1 2025 alone**, a new quarterly record (65% increase QoQ). The Safe Ecosystem Foundation is targeting break-even and double revenue in 2026, with a long-term $100M ARR goal by 2030.

Real examples:
- **Uniswap:** Governor Bravo + Timelock. Fully on-chain governance, no admin multisig. Contracts are immutable (v2, v3 core) — governance only controls fee switches and treasury
- **Aave:** Governor + Timelock + Guardian multisig (emergency pause capability)
- **Compound:** Governor Bravo (they invented it) + Timelock
- **Lido:** Aragon-based DAO + Easy Track (streamlined for routine operations) + emergency multisig

**Security Note:** The Safe contract itself became an attack surface in 2024 when attackers compromised a multisig governance process by deploying a malicious contract that tricked signers into approving a transaction replacing the legitimate Safe implementation. This highlights that the governance *process* around multisig — not just the contract code — is a security surface.

**Proxy Upgrade Governance:**

For upgradeable protocols, the upgrade path is typically: Governor proposes → Timelock queues → Proxy admin executes after delay. The **ProxyAdmin** contract (OpenZeppelin) is the actual admin of transparent proxies, and ownership of ProxyAdmin is what's governed.

Key patterns:
- **Transparent Proxy:** Admin can upgrade, users can only interact with implementation. Used by most DeFi protocols (Aave, Compound v2)
- **UUPS (Universal Upgradeable Proxy Standard):** Upgrade logic lives in the implementation contract itself. More gas-efficient, favored by newer protocols
- **Beacon Proxy:** Multiple proxies share one implementation pointer. Used by factory patterns (e.g., lending pool deployments)
- **Diamond (EIP-2535):** Multi-facet proxy that routes different function selectors to different implementation contracts. Used when hitting the 24KB contract size limit

### Solana (SVM)

**The Standard: Squads Protocol**

Squads is the dominant multisig on Solana, securing over **$10 billion** in value and **$3B+ in stablecoin transfers**. It's the first formally verified program on Solana, audited by OtterSec, Neodyme, and Bramah Systems.

**Users include:** Jupiter, Raydium, Kamino, Marginfi, Jito, Pyth, Helium, Francium.

**The Governance Model (Flat Pattern):**

Solana's governance is structurally simpler because the **upgrade authority** is a first-class concept in the runtime:

1. Every Solana program has an `upgrade_authority` field — whoever holds this key can deploy new bytecode to that program address
2. Teams transfer their upgrade authority to a **Squads multisig**
3. Squads v4 adds **time locks, spending limits, roles, and sub-accounts** directly to the multisig

Unlike EVM where you need separate Timelock and Governor contracts, Squads bundles this into one protocol. The tradeoff: less composable governance (you can't easily swap out governance modules) but much simpler to set up and reason about.

**Key difference from EVM:** On Solana, "upgrading" means literally replacing the program bytecode. There's no proxy pattern indirection — the program address stays the same, the code behind it changes. This is both simpler and scarier: if the upgrade authority is compromised, the entire program can be replaced with malicious code in a single transaction.

**Immutability option:** Programs can be made immutable by setting the upgrade authority to `None`. Once done, this is irreversible. Examples: SPL Token program (immutable), some parts of the Metaplex protocol.

**Realms (SPL Governance):** Solana's equivalent of on-chain governance (token-weighted voting), used by some DAOs. Less adopted than Squads for protocol governance — most teams prefer multisig control with an intent to progressively decentralize.

### Key Comparison

| Dimension | Ethereum (EVM) | Solana (SVM) |
|---|---|---|
| Dominant multisig | Safe ($50B+ secured) | Squads ($10B+ secured) |
| Governance layers | 3 (Safe + Timelock + Governor) | 1-2 (Squads + optional Realms) |
| Upgrade mechanism | Proxy pattern (delegatecall) | Direct bytecode replacement |
| Immutability | Deploy without proxy (Uniswap) | Set upgrade authority to None |
| Timelock | Separate contract (OpenZeppelin) | Built into Squads v4 |
| Emergency actions | Guardian multisig pattern | Multisig with fast-track |

---

## 2. Off-Chain Infrastructure

### This is the biggest practical difference between the two ecosystems for builders.

### Ethereum (EVM)

**RPC Providers:**
- **Alchemy, Infura, QuickNode** — commoditized market, multiple providers with similar APIs
- Standard JSON-RPC interface means easy provider switching
- Free tiers available (Alchemy: 300M compute units/mo, Infura: 100K requests/day)
- Production costs: **$49-499/mo** for most apps
- **Centralization concern:** Infura and Alchemy handle approximately 90% of Web3 RPC traffic. Infura is addressing this via DIN (Decentralized Infrastructure Network), connecting independent node operators. dRPC offers decentralized routing as a failover mechanism
- **Emerging alternatives:** Chainstack, GetBlock, Ankr (token-governed), dRPC (decentralized routing with backup endpoints at ~$6/M requests)

**Indexing:**
- **The Graph** — decentralized indexing protocol. You write a subgraph (mapping events to a GraphQL schema), deploy to The Graph Network, and query via GraphQL. Mature, battle-tested. Active subgraphs hit **15,087 in Q3 2025** (up 7.6% QoQ), with Base surpassing Ethereum mainnet in subgraph volume for the first time
- **Goldsky** — managed subgraph hosting with sub-second indexing latency. Runs multiple independent node pools with cross-checking. Also offers Goldsky Streams for real-time data pipelines
- **Ponder** — self-hosted indexing framework. Maximum customization but significant operational overhead
- **Envio** — newer alternative, fast sync
- **Custom indexers** — many teams run their own using event logs (EVM events are first-class, cheap, and well-structured)
- **Ormi** — emerging indexer supporting GraphQL, REST, and SQL query interfaces

**Automation/Keepers:**
- **Chainlink Automation** — decentralized keeper network using OCR3 (Off-Chain Reporting 3) consensus. Your contract implements a `checkUpkeep()` function, Chainlink nodes call it, and execute `performUpkeep()` if conditions are met. Used for liquidations, yield harvesting, rebalancing. Enables offloading expensive on-chain computation off-chain with the same cryptographic guarantees. Works across all major EVM chains
- **Gelato Network** — decentralized bot network for smart contract automation. Offers a no-code Web UI for setting up automated tasks, native support for all major EVM chains, gasless transactions through sponsored executions, flexible resolver-based logic, and a task scheduler for time-based execution. Raised $11M for expanding its automation infrastructure
- **OpenZeppelin Defender** — centralized but developer-friendly platform providing: Relayers (managed EOAs for automated tx submission with nonce management, gas estimation, and resubmission during congestion), Sentinels (monitoring + alerts), Admin (governance workflow), and Autotask (serverless functions triggered by on-chain events)
- **Reactive Network** — newer entrant offering cross-chain reactive automation

**MEV Infrastructure:**
- **Flashbots Protect** — private transaction submission to avoid sandwich attacks. Users/apps submit txs to Flashbots RPC instead of public mempool
- **MEV-Boost** — proposer-builder separation (PBS). Validators outsource block building to specialized builders via a trusted relay mediator
- **BuilderNet** (launched Dec 2024, v1.2 Feb 2025) — Flashbots migrated all builders, orderflow, and refunds to BuilderNet, ceasing all centralized block builders on Ethereum. BuilderNet enables multiple parties to collaborate in building blocks, neutralizing exclusive orderflow deals and enhancing censorship resistance
- **Block Builders** (Titan, Beaver, rsync) — compete to produce the most valuable blocks. Builders receive transactions from users and searchers, constructing the most profitable blocks
- **MEV-Share** — Flashbots project enabling users to bypass public mempools by submitting directly to builders, with MEV redistribution to users
- Builders need to think about: front-running protection, transaction ordering, MEV-aware design (e.g., UniswapX uses an RFQ/auction system specifically to capture MEV for users)

**Off-Chain Computation (Intent Systems):**
- **UniswapX** — off-chain Dutch auction for swap execution, with on-chain settlement. Swappers sign off-chain orders; fillers execute and pay gas on behalf of swappers. Competition among fillers produces best prices. Cross-chain bridging integration launched October 2024
- **CoW Protocol** — batch auction with off-chain solvers competing to fill orders. Provides strongest MEV protection via uniform clearing prices. Solvers scan all on-chain liquidity AND provide off-chain liquidity (CEX inventory, private market makers). Set all-time-high of **$9 billion monthly volume**, with **34.3% market share in DEX aggregation**
- **1inch Fusion** — similar intent-based model with resolver network
- **Ethereum Foundation Open Intents Framework (OIF)** — launched February 2025, a modular framework supported by 30+ teams aiming to unify intent-based execution across the entire Ethereum ecosystem
- These represent a major architectural shift: the "smart" part moves off-chain, the "settlement" stays on-chain

**Relayers & Meta-Transactions:**
- **ERC-2771 (Trusted Forwarder)** — standard for meta-transactions. Users sign a message off-chain (free, no gas), a relayer submits the transaction on-chain, paying gas on the user's behalf. The contract uses `ERC2771Context` to recover the original sender from `msg.data` rather than `msg.sender`
- **OpenZeppelin ERC2771Forwarder** — reference implementation of the trusted forwarder pattern
- **OZ Defender Relayers** — managed EOAs with automatic private key storage in a secure vault, nonce lock management, gas estimation, and automatic resubmission during congestion. Each relayer needs to be funded with ETH independently
- Pattern: user signs intent -> relayer submits -> contract verifies signature and executes. Core to gasless onboarding flows

### Solana (SVM)

**RPC Providers (Higher Stakes):**

RPC quality matters *significantly more* on Solana because:
1. Solana's block times are 400ms — stale data is measured in fractions of seconds
2. Transaction landing requires understanding leader schedules, priority fees, and connection quality
3. The Solana RPC API is heavier (getAccountInfo, getProgramAccounts can return large payloads)

Providers:
- **Helius** — Solana-native, offers RPC + enhanced APIs (parsed transactions, DAS, webhooks) + LaserStream (Geyser gRPC replacement). Production plan: **$999/mo+**
- **Triton (via RPC Pool)** — high-performance, used by many top protocols. Offers dedicated nodes and Geyser streams
- **QuickNode** — multi-chain, supports Solana with add-ons
- **Chainstack** — offers dedicated Solana nodes

Free tiers exist but are insufficient for production. **Expect $999-3000/mo minimum for production Solana RPC.**

**Indexing:**

EVM has events (cheap, structured logs designed for indexing). Solana does not have an equivalent — program logs exist but are unstructured strings. This makes indexing fundamentally harder:

- **Helius DAS (Digital Asset Standard)** — API for querying token/NFT data, maintained by Helius. Solana-specific
- **Geyser Plugins** — hook directly into the Solana validator to stream account updates, transactions, slot data. Much faster than polling RPC, but requires running infrastructure or paying for a dedicated node
- **LaserStream** (Helius) — managed Geyser gRPC streaming, drop-in replacement for self-hosted Geyser
- **The Graph** — now supports Solana via Substreams, enabling subgraph-style indexing. Newer support, less battle-tested than Ethereum subgraphs
- **Shyft, Ironforge, HelloMoon** — Solana-specific indexing/data platforms

**Transaction Landing (Unique to Solana):**

This is an infrastructure category that barely exists on Ethereum but is critical on Solana:

- **Jito Bundles** — atomic transaction bundles with tips to validators. Essential for MEV extraction, arbitrage, and reliable transaction execution. Jito's block engine processes bundles and ensures atomic execution
- **Priority Fees** — dynamic fee market where users bid for block inclusion. Must be tuned per-transaction based on current congestion
- **Stake-Weighted QoS** — validators prioritize transactions from staked connections. This means your RPC provider's stake matters for transaction landing rates
- **Jito Tips** — direct payments to validators for bundle inclusion, separate from priority fees

**Cranks (Solana-Specific Concept):**

Unlike EVM where keepers trigger contract functions, Solana programs are stateless — they can't schedule their own execution. "Cranking" means externally calling a program to process pending work:

- **Clockwork** — was the decentralized automation solution for Solana, but **has sunset**. Its code remains open-source on GitHub
- **Current alternatives:** Teams run their own crank bots (common), use Jito bundles for time-sensitive operations, or build custom keeper infrastructure
- No dominant decentralized keeper network exists on Solana (unlike Chainlink Automation on EVM)

**Off-Chain Order Books:**
- **Phoenix** (by Ellipsis Labs) — fully on-chain CLOB (Central Limit Order Book) on Solana. Actually *on-chain*, not off-chain, which is possible because of Solana's throughput
- **OpenBook** — community fork of Serum's order book, on-chain
- Solana's speed makes on-chain order books viable, reducing the need for off-chain matching

### Key Comparison

| Dimension | Ethereum (EVM) | Solana (SVM) |
|---|---|---|
| RPC cost (production) | $49-499/mo | $999-3000/mo |
| Indexing maturity | High (The Graph, events) | Medium (Geyser, DAS, emerging) |
| Event system | First-class (logs/events) | Weak (unstructured program logs) |
| Keeper/automation | Chainlink Automation, Gelato | DIY crank bots (no dominant solution) |
| MEV protection | Flashbots Protect (mature) | Jito bundles (mature but different model) |
| Tx landing complexity | Low (submit and wait) | High (priority fees, QoS, Jito) |
| Monthly infra budget | $500-2,000 | $1,500-5,000+ |

---

## 3. Open Source / Closed Source Conventions

### Ethereum (EVM)

**Etherscan Verification = Social Norm:**

On Ethereum, **source-code verification on Etherscan is a strong social expectation.** An unverified contract on Etherscan shows a "Contract" tab with only bytecode — this is a red flag for users and integrators. Verified contracts show full Solidity source, compiler settings, and constructor arguments.

- Etherscan supports 14+ license types, from "No License" to "Unlicense"
- Verification is free and automated via tools (Hardhat verify plugin, Foundry's `forge verify-contract`)
- Blockscout provides open-source alternative verification

**Licensing Conventions:**
- **MIT** — most permissive, commonly used by infrastructure (OpenZeppelin, Solmate, Solady)
- **GPL** — copyleft, requires derivative works to be open source (Uniswap v2 post-BSL expiry)
- **BUSL (Business Source License)** — the controversial middle ground. Source code is always publicly accessible and free for non-production usage, but commercial use is restricted for a time period before converting to open source:
  - **Uniswap v3** — launched under BSL 1.1 in 2021, expired April 2023, converted to GPL. This was a direct response to SushiSwap's "vampire attack" fork of Uniswap v2. PancakeSwap forked v3 within **one day** of expiration, garnering $140M TVL — validating Uniswap Labs' fears about value-extractive forks
  - **Uniswap v4** — BSL until **2027** (4-year window). Community criticism: "not really open source." The longer window signals the trend toward more restrictive protection periods
  - Trend is spreading: other protocols adopting BSL or similar restrictive licenses to prevent forks while maintaining source availability
  - Security concern with forks: duplication exposes multiple platforms to the same vulnerabilities if the original code has bugs
- **Unlicensed/proprietary** — rare and socially punished. Projects like early Blur contracts drew scrutiny for being unverified

**Vyper vs Solidity Openness:**
- Solidity secures **87% of DeFi TVL** vs Vyper's **8%**. Both languages are open-source
- Vyper's design philosophy emphasizes security and auditability through deliberate restrictions (no inheritance, no modifiers, no function overloading, fixed-size arrays)
- Growing pattern: combining Vyper for secure core contracts with Solidity for flexible periphery
- Vyper developers use **Titanoboa** as a testing framework (analogous to Foundry for Solidity)

**Fork Culture:**

Ethereum has a *strong fork culture* — some of the most successful protocols are forks:
- SushiSwap (fork of Uniswap v2)
- PancakeSwap (fork of Uniswap v2 on BSC)
- Dozens of Aave/Compound forks on various L2s
- This is precisely why BSL was invented — to prevent value-extractive forks

### Solana (SVM)

**Verified Builds (Emerging Standard):**

The verified builds pipeline was created and is maintained by **Ellipsis Labs and OtterSec:**

1. Build your program in a deterministic Docker environment using `solana-verify` CLI
2. The build hash is compared against the on-chain program bytecode
3. A PDA (Program Derived Address) of the verify program stores the verification data (program address, git URL, commit hash, build args)
4. Verification status is queryable via OtterSec's [verified programs API](https://github.com/otter-sec/solana-verified-programs-api)

**Current state:** Adoption is growing but far from universal. Many production Solana programs are still unverified — there's no Etherscan equivalent with a big red/green checkmark that users see before interacting.

**Anchor IDL Publishing:**

Anchor programs can publish their IDL (Interface Definition Language) on-chain. This serves as both documentation and a machine-readable interface description:
- The IDL describes all instructions, accounts, types, and errors
- Published on-chain at a deterministic PDA
- Clients can auto-generate TypeScript bindings from the IDL
- **However:** IDL publishing ≠ source verification. You can publish an IDL without verified builds

**Open/Closed Source Norms:**

Solana has a **weaker social norm** around open source compared to Ethereum:
- Many prominent protocols keep source code private or partially open
- There's no Etherscan-equivalent that makes verification visible to end users
- The culture is shifting toward more openness, especially post-FTX, as trust became a priority
- Programs like Jupiter (open IDL, closed source code for some components) operate in a gray area

**Fork Culture:**

Less pronounced than Ethereum:
- Solana programs are harder to fork (Rust complexity, account model specifics, often need accompanying off-chain infrastructure)
- The effort to fork a Solana program is significantly higher than forking an EVM contract
- Notable forks exist (OpenBook from Serum) but are less common

### Key Comparison

| Dimension | Ethereum (EVM) | Solana (SVM) |
|---|---|---|
| Verification standard | Etherscan (strong norm) | OtterSec/Ellipsis verified builds (emerging) |
| User visibility | Clear verified/unverified badge | No widely-used equivalent UI |
| License trend | BSL gaining adoption | Less standardized |
| Fork difficulty | Low (Solidity is simple to fork) | High (Rust complexity + off-chain infra) |
| Open-source expectation | Strong (unverified = red flag) | Moderate (improving post-FTX) |
| IDL/ABI | ABI auto-generated, standard | Anchor IDL (optional publication) |

---

## 4. Dependencies & Composability

### Ethereum (EVM)

**Standard Libraries:**
- **OpenZeppelin Contracts** — the de facto standard library. ERC-20, ERC-721, ERC-1155, access control, governance, proxies, utilities. Battle-tested across billions of TVL. Updated regularly
- **Solmate** (Rari Capital / transmissions11) — gas-optimized alternatives to OZ contracts. Less safe-by-default but more efficient
- **Solady** — ultra gas-optimized library, successor to Solmate in philosophy

**Package Management:**
- **Foundry:** `forge install` pulls from GitHub repos, uses git submodules. Manages remappings for imports
- **Hardhat:** npm packages (`@openzeppelin/contracts`, etc.)
- No on-chain package registry — dependencies are vendored or pinned to git commits

**Composability Model:**

EVM composability is *the killer feature.* Contracts share a global state space and can call each other freely — often called "money legos":

1. **Token Approvals (ERC-20 approve/transferFrom)** — the foundational composability primitive. User approves protocol to spend tokens → protocol can pull tokens atomically
   - **Permit2** (Uniswap) — a universal approval contract that improves the standard ERC-20 approve pattern. Users approve tokens to Permit2 once, then Permit2 manages granular approvals to individual protocols via ERC-712 signatures. Reduces gas costs and attack surface from over-approvals
   - **ERC-2612 (Permit)** — gasless token approvals via signed messages, built into newer ERC-20 tokens
2. **Flash Loans (ERC-3156)** — borrow any amount, use it, return it + fee, all in one transaction. All steps succeed or fail atomically. Enables capital-free arbitrage, liquidations, refinancing. Aave, Uniswap v3, Balancer all offer flash loans. Real example: Arbitrage DAO borrowed 3,137 DAI from Aave, swapped to SAI via MakerDAO, back to DAI on Uniswap — 3 protocols in one atomic tx
3. **Callbacks** — contracts can call back into the caller during execution. Enables patterns like Uniswap v3's `uniswapV3SwapCallback`. Also enables reentrancy attacks if not handled carefully
4. **Composable function calls** — any contract can call any public function on any other contract. No special registration or permission needed. Vaults, lenders, and traders can seamlessly integrate services in one workflow

**Proxy Patterns (Upgrade Dependencies):**

Upgradeability introduces dependency complexity:
- **Transparent Proxy** — separate admin and user entry points. ProxyAdmin contract controls upgrades
- **UUPS** — implementation controls its own upgrade. Simpler, cheaper, but one buggy implementation can brick the proxy
- **Beacon** — many proxies point to one beacon. Upgrade the beacon, all proxies update. Used for factory-deployed instances
- **Diamond (EIP-2535)** — modular upgrade pattern. Different function selectors route to different "facet" contracts. Complex but powerful for large protocols

**Trust implication:** Every upgradeable contract you depend on can change behavior. This creates a web of trust assumptions. Immutable contracts (Uniswap v2/v3 core) are safer composability targets.

### Solana (SVM)

**Standard Programs:**
- **SPL Token** — the equivalent of ERC-20. Immutable, deployed by Solana Labs. All fungible tokens use this one program (unlike EVM where each token deploys its own contract)
- **Token-2022 (Token Extensions)** — superset of SPL Token with programmable features: transfer fees, confidential transfers, permanent delegates, transfer hooks, metadata. Adoption growing — used by Paxos (USDP) and other regulated issuers
- **Associated Token Account (ATA)** — deterministic token account derivation. Ensures each wallet has one canonical token account per mint

**Anchor Framework:**

Anchor dominates Solana development like no single framework dominates EVM:
- Provides: IDL generation, account serialization/deserialization, constraint macros, CPI helpers, error handling
- Most new Solana programs use Anchor
- Native Rust (without Anchor) used for performance-critical programs: Phoenix, Jito tip distribution, some parts of Marinade
- **Key dependency:** `anchor-lang` crate. Version compatibility matters — programs built with different Anchor versions may have serialization differences

**Composability Model (CPI):**

Solana composability works through **Cross-Program Invocation (CPI)**:

1. Program A calls Program B, passing all required accounts explicitly
2. Program B executes with the provided accounts and returns
3. No re-entrant calls possible — CPI depth is limited and the runtime prevents re-entering the calling program (unlike EVM)
4. **PDA signing** — programs can "sign" for PDAs they own, enabling program-controlled accounts

Key differences from EVM:
- **No global state access** — you must pass every account a program needs. This makes composability more verbose but eliminates reentrancy
- **Account model** — data lives in accounts owned by programs, not in program storage. Programs are stateless; accounts are stateful
- **No token approvals** — the approval model exists (delegate authority on token accounts) but is used less. More commonly, tokens are transferred into program-owned PDAs

**Crate Ecosystem:**
- `solana-program` — core runtime types
- `anchor-lang` — Anchor framework
- `spl-token` — SPL Token interface
- `mpl-token-metadata` — Metaplex metadata standard
- Cargo manages dependencies; crate versioning and Solana version compatibility is a frequent pain point

### Key Comparison

| Dimension | Ethereum (EVM) | Solana (SVM) |
|---|---|---|
| Standard library | OpenZeppelin (mature, audited) | SPL programs + Anchor (less standardized) |
| Package manager | npm / forge install | Cargo (Rust ecosystem) |
| Composability primitive | approve/transferFrom, callbacks | CPI with explicit account passing |
| Reentrancy risk | Yes (major attack vector) | No (runtime prevents it) |
| Upgrade pattern | Proxy contracts (multiple patterns) | Upgrade authority (single mechanism) |
| Shared state | Global (any contract reads any storage) | Explicit (must pass accounts) |
| Token standard | One contract per token (ERC-20) | One program for all tokens (SPL Token) |

---

## 5. Program/Contract Design Philosophy

### Ethereum (EVM)

**Storage Model:**
- Contracts have persistent storage (256-bit key-value slots)
- Storage layout is implicit — slot 0, slot 1, etc. based on variable declaration order
- **Storage collisions** in proxies are a major risk (implementation and proxy must not overlap storage slots). The Audius protocol lost millions in 2022 due to a storage collision in an upgradeable proxy
- **ERC-7201 (Namespaced Storage Layout)** — adopted by OpenZeppelin Contracts v5 (2023). Solves storage collision by grouping storage variables into namespaced structs rooted at deterministic non-zero storage slots (using `keccak256(keccak256(id) - 1) & ~0xff`). Requires Solidity 0.8.20+. OpenZeppelin Upgrades plugins auto-detect namespaces and validate upgrade safety. This is now the recommended pattern for all upgradeable contracts
- `SSTORE` (write to storage) is the most expensive EVM opcode — drives gas optimization culture

**Gas Optimization Patterns:**
- Packing multiple values into single 256-bit storage slots
- Using `calldata` instead of `memory` for read-only function params
- Caching storage reads in local variables
- Minimizing state changes
- Using events/logs for data that doesn't need on-chain access
- Assembly (`Yul`) blocks for hot paths
- **Contract size limit:** 24KB (EIP-170). Large protocols must split logic across contracts or use Diamond pattern

**Design Patterns:**
- **Factory pattern** — deploy new contract instances from a template (Uniswap pair factory, Aave lending pool factory). A factory contract stores the template bytecode and deploys new instances with `create` or `create2`
- **Minimal Proxy Clones (EIP-1167)** — ultra-cheap deployment of clone contracts that delegate all calls to an implementation via `delegatecall`. The clone bytecode is only **55 bytes** (45 bytes runtime), making deployment dramatically cheaper than a full contract. Used extensively by: Uniswap (each trading pair is a clone), Gnosis Safe (each wallet is a clone of a master Safe), NFT collections, vault deployments. **Tradeoff:** each call costs more gas due to the `delegatecall` overhead, and clones cannot be upgraded — if the implementation has a bug, you must deploy new clones and migrate state
- **Singleton pattern** — one contract for all instances (Uniswap v4 uses a single PoolManager for all pools, unlike v3's one-contract-per-pool)
- **Contract size limit (24KB, EIP-170)** — the Spurious Dragon hard fork imposed a 24,576-byte bytecode limit. Large protocols must split logic across contracts using: Diamond pattern (EIP-2535) with multiple "facets," library contracts, or external helper contracts

### Solana (SVM)

**Account Model:**
- Programs are stateless — they contain only executable code
- State lives in **accounts** — data blobs owned by programs
- Each account has: owner (program), data (byte array), lamports (balance), and flags
- Programs define the data layout of their accounts (using Borsh serialization with Anchor, or raw bytes natively)

**PDAs (Program Derived Addresses):**
- Deterministic addresses derived from seeds + program ID
- Programs can "sign" for their PDAs without a private key
- Fundamental architectural primitive — used for: escrow accounts, config accounts, user state, pool vaults, authority derivation
- Seeds are the key design decision: what uniquely identifies this account? (e.g., `[b"user-position", user_pubkey, pool_pubkey]`)

**Rent & Account Creation:**
- Accounts must be **rent-exempt** — maintain a minimum SOL balance proportional to their data size
- ~0.00089 SOL per byte of account data (roughly)
- Creating accounts requires funding rent exemption upfront
- Account closure returns the rent SOL — incentivizing cleanup
- **This is a real cost consideration:** a program that creates many accounts (e.g., order book with many orders) must budget for rent

**Compute Units:**
- Each transaction has a compute budget (default 200K CU, max 1.4M CU per transaction)
- Optimization targets: minimize CPI calls (each costs ~base CU), minimize account data reads, batch operations
- Different optimization mindset from EVM: Solana optimizes for parallelism (transactions that touch different accounts can run in parallel) while EVM optimizes for storage access

**Design Patterns:**
- **One program, many accounts** — unlike EVM where you might deploy a contract per pool, Solana uses one program with different accounts per pool/user/position
- **Account hierarchy** — config account → pool account → position account → tick accounts (e.g., Orca Whirlpools)
- **Zero-copy deserialization** — for large accounts, Anchor supports `zero_copy` accounts that are read directly from memory without deserialization overhead

### Key Comparison

| Dimension | Ethereum (EVM) | Solana (SVM) |
|---|---|---|
| State model | Contract storage (key-value) | Accounts (data blobs) |
| Cost driver | Storage writes (SSTORE) | Account creation (rent) |
| Size limits | 24KB contract size | 10MB account max, 10KB init limit, 1,232B tx size |
| Parallelism | Sequential execution | Parallel (non-overlapping accounts) |
| Design unit | Contract (code + state) | Program (code) + Accounts (state) |
| Address derivation | CREATE/CREATE2 | PDA (deterministic from seeds) |

---

## 6. Testing & Deployment

### Ethereum (EVM)

**Frameworks:**
- **Foundry (Forge)** — now the leading framework (51.1% vs Hardhat's 32.9% per 2024 Solidity Developer Survey). Tests written in Solidity. Extremely fast (native Rust execution). Supports fuzz testing, property-based fuzzing, invariant testing, gas snapshots, fork testing. Widely regarded as the top choice for DeFi protocols in 2026 due to precise gas reporting and fuzzing capabilities
- **Hardhat** — JavaScript/TypeScript based. Most widely *deployed* framework despite losing ground in surveys. Robust plugin ecosystem. Flexible for integration testing, deployment scripts, and anything requiring JS. Better for teams needing extensive web technology integration
- Many teams use **both**: Foundry for unit tests + Hardhat for deployment scripts and integration tests
- **Emerging (2026):** AI-assisted smart contract testing plugins for both Foundry and Hardhat, using LLMs to auto-generate fuzz test seeds based on contract semantics
- **ConsenSys sunset Truffle and Ganache** (2024), pushing remaining teams toward Hardhat and Foundry

**Fork Testing:**
- `forge test --fork-url <mainnet_rpc>` — run tests against a fork of mainnet state. This is transformative: you can test your protocol's interaction with real Uniswap, real Aave, real USDC without mocks
- **Pin to a specific block** for deterministic caching: data is cached on disk per block number, yielding up to **20x speed improvements** on subsequent runs (`~/.foundry/cache/rpc/<chain>/<block>`)
- Hardhat also supports forking with its built-in Network Helpers library (manipulate time, balances, impersonate accounts)
- Requires archive node access (Alchemy, Infura both support this)
- This is a significant advantage EVM has over Solana

**Formal Verification:**
- **Certora Prover** — SMT-based verification. Write rules in CVL (Certora Verification Language), proves properties hold for ALL possible inputs. Used by Aave, Compound, MakerDAO and other top DeFi protocols as part of their security pipeline
- **Halmos** — symbolic testing framework from a16z crypto. Write tests in Solidity, uses symbolic execution to explore all possible execution paths
- **Kontrol** — Runtime Verification's K framework-based formal verification for Solidity. Mathematical proof of correctness
- Formal verification is increasingly expected for top-tier DeFi protocols but remains expensive and specialized

**Deployment:**
- **Deterministic deployment (CREATE2)** — deploy contracts to predictable addresses across chains. The address is determined by `keccak256(0xff, deployer, salt, initCodeHash)`. Used by Safe, Uniswap, and most multi-chain protocols
- **Keyless deployment** — Nick Johnson's method: a pre-signed transaction deploys a factory to the same address on any EVM chain without needing custody of the deployer's key. The deterministic deployment proxy lives at `0x4e59b44847B379578588920cA78FbF26c0B4956C` across Ethereum and many other chains
- **Safe Singleton Factory** — managed keyless deployment by the Safe team, widely used for cross-chain deployments
- **ERC-7955 (2025)** — permissionless CREATE2 factory using EIP-7702 (Set Code for EOAs) for universal deterministic deployment without preinstalls or secret keys
- **Deployment script best practices:** pin exact Solidity compiler version, set EVM version explicitly, disable metadata hash for deterministic bytecode, keep optimizer settings consistent across chains
- Deployment scripts in Foundry (`forge script`) or Hardhat (`hardhat deploy`)

**Auditing Ecosystem (Mature):**
- **Tier 1 Firms:**
  - **Trail of Bits** — invested in building open-source security tools (Slither static analyzer, Echidna fuzzer, Manticore symbolic executor). Known for the deepest technical analysis of contract security, Solidity language implications, and EVM behavior
  - **OpenZeppelin** — pioneered smart contract security in 2015 by establishing the industry's first professionalized audit group. Also maintains the de facto standard library
  - **Spearbit** — marketplace model connecting protocols with top independent security researchers
  - **Consensys Diligence** — part of ConsenSys, strong EVM-specific tooling
- **Competitive audits:**
  - **Code4rena** — contest-based model where Wardens compete to find vulnerabilities. Operates at **zero platform fee** (100% of sponsor funds go to wardens and judges). Standard split: 96% conditional pool + judging fee. Judge triages and assesses bug validity/severity independently
  - **Sherlock** — targeted at expert security researchers. Emphasis on exploit development and experienced participants. Offers audit coverage (insurance-like protection)
  - **Cantina** — newer competitive audit platform
  - **Hats Finance** — on-chain bug bounty and audit competitions
- **Bug bounties:** Immunefi (dominant platform), covering most major DeFi protocols
- **Costs:** Top-tier firms charge **$80K-200K** for enterprise audits. Mid-tier: $25K-70K. Simple ERC-20 audits: $10K-20K. Complex DeFi protocols: $50K-150K
- **Timelines:** 2-8 weeks typical, with **several month wait times** for popular firms. Plan audits early in development

### Solana (SVM)

**Frameworks:**
- **Anchor test suite** — uses Mocha (JS/TS) test runner with `@coral-xyz/anchor` bindings. Spins up a local validator
- **solana-test-validator** — local Solana cluster for testing. Slower than EVM local nodes but functional
- **Bankrun (solana-bankrun / litesvm)** — fast, lightweight testing without spinning up a full validator. Growing in adoption for unit testing. Significantly faster than test-validator
- **Trident** — fuzz testing framework for Anchor programs (by Ackee Blockchain)

**Fork Testing (Limitation):**
- Solana does not have a mature equivalent to EVM fork testing. You can clone accounts from mainnet into test-validator, but it's manual and less ergonomic
- This is a meaningful gap — testing composability with live protocol state is harder

**Deployment:**
- Programs are deployed as BPF bytecode into **buffer accounts**, then the buffer is assigned to the program address
- Deployment costs: ~2-5 SOL for typical programs (rent for the program account + buffer)
- Upgrade process: deploy new bytecode to buffer → upgrade authority signs → program address now points to new bytecode
- **No deterministic deployment addresses** like CREATE2 — program addresses are keypairs or derived from deploy keypair

**Auditing Ecosystem (Growing):**
- **OtterSec** — leading Solana audit firm, also maintains the verified builds infrastructure
- **Neodyme** — German-based, strong Solana focus
- **Sec3 (now Ackee Blockchain Security)** — automatic vulnerability detection + manual audits
- **Halborn** — multi-chain but strong Solana presence
- **Competitive audits:** Less mature than EVM. No equivalent to Code4rena/Sherlock scale for Solana
- **Bug bounties:** Immunefi covers Solana protocols but fewer programs listed than EVM
- Fewer firms, less competition → potentially longer wait times and less price competition

### Key Comparison

| Dimension | Ethereum (EVM) | Solana (SVM) |
|---|---|---|
| Leading framework | Foundry (51% adoption) | Anchor (dominant) |
| Test speed | Very fast (Foundry native) | Moderate (Bankrun fast, test-validator slow) |
| Fork testing | Mature (native in Foundry) | Immature (manual account cloning) |
| Formal verification | Certora, Halmos, Kontrol | Limited (emerging) |
| Deployment cost | Variable (gas-dependent) | ~2-5 SOL (rent-based) |
| Deterministic deploy | CREATE2 (standard) | No equivalent |
| Audit ecosystem | Mature (many firms, competitive audits) | Growing (fewer firms, less competition) |
| Typical audit cost | $50K-500K+ | $30K-300K+ |

---

## 7. Security Model & Attack Surfaces

### Ethereum (EVM)

**Common attack vectors:**
1. **Reentrancy** — the #1 historical attack vector. A contract calls an external contract, which calls back before the first call completes. The DAO hack (2016), Curve exploit (2023). Mitigated by: checks-effects-interactions pattern, reentrancy guards, Solidity 0.8+ default checks
2. **Storage collision** — in proxy patterns, implementation and proxy storage can overlap. Devastating if exploited
3. **Front-running / MEV** — public mempool allows attackers to see pending transactions and insert their own before/after. Mitigated by Flashbots, private mempools, commit-reveal schemes
4. **Oracle manipulation** — price feeds manipulated via flash loans. Mitigated by TWAP oracles, Chainlink
5. **Access control errors** — missing `onlyOwner` modifiers, improper role management
6. **Integer overflow/underflow** — largely mitigated by Solidity 0.8+ built-in checks
7. **Proxy initialization** — uninitialized implementation contracts can be taken over

### Solana (SVM)

**Common attack vectors:**
1. **Missing signer checks** — program doesn't verify that the expected authority signed the transaction. The #1 Solana-specific vulnerability
2. **Account confusion / substitution** — passing a malicious account where a legitimate one is expected. Anchor's account constraints (`#[account(has_one, constraint)]`) mitigate this
3. **Missing owner checks** — not verifying that an account is owned by the expected program
4. **CPI privilege escalation** — a program invokes another program with accounts it shouldn't have access to
5. **Arithmetic errors** — especially in fixed-point math for DeFi. Rust doesn't overflow by default in release mode (it wraps), unlike Solidity 0.8+
6. **PDA seed collision** — poorly chosen seeds allow creating accounts that conflict with other users' accounts
7. **Closing account vulnerabilities** — improperly closed accounts can be "revived" and reused

### Major Historical Exploits (Shaping Current Conventions)

| Exploit | Chain | Loss | Root Cause |
|---|---|---|---|
| Wormhole (Feb 2022) | Solana | ~$326M | Missing verification of `account.owner`; deprecated function allowed forged signatures |
| Mango Markets (Oct 2022) | Solana | ~$116M | Oracle price manipulation via leveraged futures (economic exploit) |
| The DAO (2016) | Ethereum | ~$60M | Reentrancy via fallback function |
| Curve Finance (Jul 2023) | Ethereum | ~$70M | Vyper compiler reentrancy lock bug |
| Euler Finance (Mar 2023) | Ethereum | ~$200M | Flash loan exchange rate manipulation |

**Bug Bounties:** Immunefi is the dominant platform for both ecosystems, protecting $190B+ in user funds across 300+ projects. Notable: Firedancer validator client has a $1M bounty on Immunefi.

**Key insight:** Solana's account model eliminates reentrancy but introduces account validation as the primary security surface. Anchor's constraint system (`#[account(...)]` macros) is the primary defense — most Solana vulnerabilities come from insufficient account constraints.

### Application-Level MEV Mitigation (Builder Concern)

Beyond infrastructure-level MEV protection (Flashbots, Jito), builders are increasingly *internalizing* MEV at the application layer — designing protocols that capture value that would otherwise leak to external searchers:

**EVM approach:**
- **Order-flow auctions (OFAs):** UniswapX routes trades through a Dutch auction among fillers, capturing MEV surplus for swappers rather than letting sandwich bots extract it. CoW Protocol's batch auction model similarly eliminates MEV by settling at uniform clearing prices
- **MEV-Share / MEV Blocker:** Users opt into MEV redistribution — searchers pay back a portion of extracted value to the originating user
- **Intent-based architectures:** By moving execution off-chain and settling on-chain, intents reduce the MEV surface area entirely

**Solana approach:**
- **Jito bundle integration:** Protocols directly submit bundles via Jito to atomically execute arbitrage alongside user transactions, capturing the spread for the protocol treasury rather than letting external searchers extract it
- **On-chain order books (Phoenix, OpenBook):** By matching orders on-chain at Solana speed, these reduce the MEV surface compared to AMMs (no sandwich-able slippage pool)
- **Priority fee bidding:** Applications tune priority fees to ensure their keeper/liquidation transactions land reliably, effectively competing in the MEV auction themselves

**Builder takeaway:** On both chains, sophisticated builders now treat MEV as a *design parameter* — not just a risk to mitigate, but a revenue stream to capture. The protocols that internalize MEV effectively (UniswapX, CoW Protocol, Jito-integrated Solana protocols) gain a structural economic advantage.

---

## 8. Oracle Integration Patterns

### This is foundational infrastructure — every DeFi app needs price feeds, and the oracle model fundamentally shapes your architecture.

### Ethereum (EVM): Push-Dominant, Shifting to Hybrid

**Chainlink Data Feeds (Push Model — The Standard):**

Chainlink secures over **$100 billion** in Total Value Secured (TVS) as of late 2025, commanding **~68-70% of the total oracle market** by TVS. On Ethereum specifically, Chainlink secures **83-84%** of all oracle-secured value.

How it works:
- A Decentralized Oracle Network (DON) of independent node operators aggregates off-chain data and pushes updates on-chain
- Updates trigger on two conditions: **deviation threshold** (e.g., 0.5% for ETH/USD, BTC/USD) or **heartbeat interval** (e.g., 1 hour for major pairs on mainnet). Whichever fires first triggers an on-chain update
- Contracts consume prices via `AggregatorV3Interface.latestRoundData()` — returns price, timestamp, round data. **Code is identical across all EVM chains** — just swap the feed address
- **Best practice:** Always validate `updatedAt` for staleness and check `answer > 0`

**Cost model:** Shared-cost sponsor model — multiple protocols using the same feed aggregate fees to fund node operators. **Chainlink SCALE** program subsidizes L2 oracle costs; **Chainlink BUILD** offers early-stage projects services in exchange for token allocation. **Payment Abstraction** (March 2025) enables paying in ETH/USDC instead of LINK.

**Chainlink Data Streams (Pull Model — For Low-Latency):**
- Newer product for latency-sensitive apps (perpetual DEXes). DON nodes sign reports and deliver to an Aggregation Network
- Clients connect via **WebSocket** for real-time sub-second streaming
- Price data is fetched off-chain and verified on-chain within the user's transaction — data only hits the chain when needed
- Gives Chainlink coverage of both lending (push) and perps/trading (pull) markets

**Chainlink SVR (Smart Value Recapture):**
- 2025 innovation: oracle updates are routed through a dual aggregator with an **MEV auction** for the right to backrun liquidations
- Recaptures **80%+ of eligible liquidation MEV**, splitting revenue 65% to the protocol / 35% to Chainlink
- Aave was the first integrator — SVR has processed **$32M+ in liquidations** and recaptured **$1.1M+ in MEV** on Ethereum
- Aave DAO expanded SVR coverage from 3% to 27% of its Ethereum TVL

**Other EVM Oracles:**
- **Chronicle** (formerly MakerDAO oracles) — exclusive oracle for MakerDAO/Sky. Uses **Schnorr multi-signature aggregation** (Scribe), reducing gas costs by **60%+ on L1** and **68% on L2**. 22 validator nodes run by entities like Infura, Etherscan, Gnosis
- **RedStone** — modular pull-based oracle. Signed data packages are injected directly into user transactions at execution time. **70%+ gas savings** vs push oracles. Specialized in yield-bearing collateral for lending markets. Supports 50+ chains
- **UMA Optimistic Oracle** — fundamentally different: data assumed correct unless disputed within 48h. Only ~1.5% dispute rate. Used by Polymarket (prediction markets) and Across Protocol (~$20B in cross-chain settlements). Not suitable for real-time price feeds
- **API3** — first-party oracles where API providers run their own nodes (Airnode). 200+ live price feeds across 40+ blockchains

### Solana (SVM): Pull-Native

**Pyth Network (Dominant on Solana):**

Pyth's TVS on Solana is **~$2.5 billion**. In February 2024, Pyth oracle transactions accounted for **~20% of all Solana transactions**.

How it works:
- **90+ first-party data publishers** (institutional firms: Jane Street, CTC, Raydium) submit price and confidence data directly
- Data is aggregated on **Pythnet**, a dedicated appchain built on Solana's codebase, producing aggregated prices every **400 milliseconds** (every Solana slot)
- Prices are **not automatically pushed** to destination chains. Applications **pull** price updates from Pythnet on-demand
- A user/keeper fetches the latest signed price update from Pythnet off-chain, includes it as an instruction in the Solana transaction, and the on-chain Pyth program verifies signatures and makes the price available — all in one atomic transaction
- Rust SDK provides `get_price_no_older_than()` with configurable staleness threshold (default 30 seconds)

**Confidence Intervals (Unique Pyth Feature):**
Every price update includes a confidence interval representing publisher uncertainty. Protocols use this to:
- Widen spread on derivatives platforms during volatile periods
- Reject prices with unreasonably wide confidence (indicating publisher disagreement or thin markets)
- Set oracle-aware risk parameters in lending (e.g., `max_confidence_ratio`)

**Pyth Lazer & Pyth Pro (2025):** Single-millisecond updates — 400x faster than base Pyth — targeting high-frequency trading. 2,200+ price feeds. Pyth has expanded to **100+ blockchains** including EVM chains.

**Switchboard (Permissionless Alternative):**

Switchboard TVS on Solana: **~$1.2-1.5 billion**, with **51+ production protocols**.

| Dimension | Pyth | Switchboard |
|---|---|---|
| Permissioning | Curated institutional publishers | **Permissionless** — anyone can create a feed |
| Data types | Financial price feeds | Arbitrary: NFT floors, sports, custom APIs |
| VRF (randomness) | Only on EVM (Entropy) | **Sole VRF provider on Solana** |
| Latency | 400ms (base), 1ms (Lazer) | 8-25ms (Surge, co-located nodes) |

Switchboard launched **Surge** in 2025: sub-100ms oracle latency at ~1/100th the cost of existing providers. First Solana oracle to integrate with **Jito's restaking platform** for economic security.

### Architecture Implications for Builders

**Push vs. Pull — How It Shapes Your Code:**

| Dimension | Push (Chainlink Feeds) | Pull (Pyth, RedStone) |
|---|---|---|
| Contract complexity | Simple: call `latestRoundData()` | More complex: caller must fetch and pass signed price data |
| Freshness guarantee | Depends on heartbeat/deviation | Always fresh at moment of use |
| Gas cost bearer | Oracle network (sponsors) | User/protocol (included in tx) |
| Congestion resilience | Risk of delayed updates | Unaffected (updates only when transacting) |
| Staleness handling | Check `updatedAt` timestamp | SDK enforces `max_age` parameter |

**Oracle Manipulation — Different Attack Surfaces:**

- **EVM flash loan attacks:** Exploiting on-chain DEX spot prices as oracles. Attacker borrows via flash loan → manipulates DEX pool reserves → exploits protocol reading that pool price. Notable: bZx ($954K), Cream Finance ($130M), PancakeBunny ($45M). **Mitigation:** Use off-chain aggregated feeds (Chainlink), TWAPs, multiple oracle sources
- **Solana manipulation:** No native flash loans in the same atomic pattern, but thin-market manipulation is the risk. Mango Markets ($117M) used real capital ($10M USDC) to pump MNGO on thin-liquidity markets, inflating oracle-reported collateral value. **Mitigation:** Consumer-side circuit breakers, TWAP deviation checks (`max_deviation_from_twap`), confidence interval limits

**How Lending Protocols Consume Oracles (Real Examples):**

- **Aave + Chainlink (EVM):** `AaveOracle` contract maps each asset to its Chainlink feed. `getAssetPrice(asset)` calls `latestRoundData()`. Staleness checks enforced. Now integrated with Chainlink SVR for liquidation MEV recapture
- **Kamino/MarginFi + Pyth (Solana):** Configure `max_staleness_slots`, `max_confidence_ratio`, and `max_deviation_from_twap`. Liquidators must include a fresh Pyth price update instruction in their liquidation transaction — ensuring the price is provably from the moment of liquidation. Some use Switchboard as secondary oracle

**TWAP Implementations:**
- **EVM (Uniswap V3):** Built-in TWAP oracle using tick accumulators. Resistant to flash loan manipulation because time-weighting minimizes single-block spikes. Used as secondary/fallback oracle alongside Chainlink
- **Solana (Pyth):** Provides exponentially-weighted moving average (EMA) alongside spot price. Lending protocols compare spot vs EMA using `max_deviation_from_twap` as a circuit breaker

### Key Comparison

| Dimension | Ethereum (EVM) | Solana (SVM) |
|---|---|---|
| Dominant oracle | Chainlink (~$100B TVS, 83% ETH share) | Pyth (~$2.5B TVS, dominant on Solana) |
| Default model | Push (shifting to hybrid) | Pull-native |
| Update frequency | Heartbeat (1h) + deviation (0.5%) | Every 400ms (Pyth base), 1ms (Lazer) |
| Unique feature | SVR (liquidation MEV recapture) | Confidence intervals |
| Permissionless feeds | API3, UMA | Switchboard |
| VRF (randomness) | Chainlink VRF | Switchboard VRF |
| Cross-chain | CCIP (60+ chains) | Pyth on 100+ chains |
| Flash loan manipulation risk | High (if using DEX spot prices) | Lower (no native flash loans, but thin-market risk) |

---

## 9. SDK & Client Ecosystem

### Ethereum (EVM)

- **viem** — modern, TypeScript-first Ethereum library. Smaller bundle (27KB vs ethers.js 130KB), tree-shakeable, type-safe. Rapidly gaining adoption
- **wagmi** — React hooks for Ethereum, built on viem + TanStack Query. The standard for React dApp frontends
- **ethers.js** — the incumbent. v6 available but viem is capturing new projects. Still has the largest community and most tutorials
- **web3.js** — the original, now less used for new projects
- **RainbowKit / ConnectKit / Web3Modal** — wallet connection UI libraries built on wagmi

### Solana (SVM)

- **@solana/web3.js** — official Solana JavaScript SDK. Recently rewritten (v2) to be more modular and tree-shakeable
- **@coral-xyz/anchor** — TypeScript client for Anchor programs. Auto-generates typed clients from IDLs
- **@solana/wallet-adapter** — wallet connection library (equivalent to wagmi for Solana). Supports Phantom, Backpack, Solflare, etc.
- **Solana Mobile Stack** — SDK for Android (and iOS via Expo). Includes Mobile Wallet Adapter for connecting to wallets on mobile. Saga phone (2023) and **Seeker phone (2025, more affordable)** with built-in secure enclaves and native dApp Store. Solana Mobile announced SMS for OEM partners, expanding beyond their own hardware

### Key Difference

EVM clients are more mature and have more choices. The viem/wagmi stack is notably better in terms of DX (type safety, caching, error handling) than the Solana equivalent. Solana's web3.js v2 rewrite is closing the gap.

---

## 10. State Management & Data Availability

### Ethereum (EVM)

- **State stored in contract storage** — persistent, expensive (SSTORE ~20K gas for new slot)
- **Events/logs** — cheap way to record data that doesn't need on-chain access. Standard for frontends and indexers. Well-structured (topics + data)
- **Calldata** — transaction input data, cheapest storage layer on L1
- **EIP-4844 (blobs)** — L2s post data to L1 as blobs, much cheaper than calldata. Drives L2 cost reduction
- **State growth** — major long-term concern. Ethereum full nodes store approximately 1TB+ of data and growing. The Ethereum Foundation has identified statelessness as a long-term solution, allowing validators to verify blocks without holding full state. Gas limit increases amplify state growth since they allow more writes per block

### Solana (SVM)

- **State stored in accounts** — rent-exempt model ensures minimum balance
- **State compression** — Solana's killer feature for large-scale data. Uses Merkle trees to store data proofs on-chain while keeping actual data on the ledger (available but not in expensive account storage)
  - **Compressed NFTs (cNFTs):** Minting 1M traditional NFTs costs ~24,000 SOL. With compression: **~10 SOL or less** (2,400-24,000x cheaper)
  - **Bubblegum** (Metaplex) — the protocol for cNFTs. Routinely mints **3M+ cNFTs weekly**
  - Used for: large-scale airdrops, gaming assets, loyalty programs, social tokens
- **Account data limits** — individual accounts can be up to 10MB, but account initialization is capped at 10,240 bytes (10KB) per instruction; larger accounts require `realloc`. In practice, most accounts are well under 10KB. Transaction size is limited to 1,232 bytes (IPv6 MTU - headers), with SIMD-0296 proposing an increase to 4,096 bytes
- **Ledger vs. state** — Solana distinguishes between state (accounts, expensive) and ledger (transaction history, cheaper). State compression bridges this gap

### Key Comparison

| Dimension | Ethereum (EVM) | Solana (SVM) |
|---|---|---|
| Event system | Rich (topics, indexed fields) | Weak (string logs, Anchor events improving) |
| Compression | L2 blobs (EIP-4844) | State compression (cNFTs, Merkle trees) |
| Cost for 1M records | High on L1, cheap on L2 | Cheap with compression (~10 SOL) |
| Indexing requirement | Moderate (events help) | High (no structured events) |

---

## 11. Cost Structure for Builders

### Ethereum (EVM)

| Cost Category | Estimated Monthly |
|---|---|
| RPC (production) | $49-499 |
| Indexing (The Graph / custom) | $100-1,000 |
| Keeper/automation | $100-500 |
| Monitoring (Tenderly, OZ Defender) | $100-500 |
| **Total infra** | **$500-2,500/mo** |
| Contract deployment (one-time) | $500-50,000+ (gas dependent) |
| Audit | $50K-500K (one-time) |

User costs: ~$0.44 average per transaction on L1 (mid-2025); ~$0.01-0.012 on L2s post-Dencun.

### Solana (SVM)

| Cost Category | Estimated Monthly |
|---|---|
| RPC (production, Helius/Triton) | $999-3,000 |
| Indexing (Geyser/DAS) | $500-2,000 |
| Crank/keeper bots (self-hosted) | $200-1,000 |
| Transaction fees (for keeper ops) | $100-500 |
| **Total infra** | **$1,500-5,000+/mo** |
| Program deployment (one-time) | ~2-5 SOL ($200-500) |
| Audit | $30K-300K (one-time) |

User costs: $0.001-0.01 per transaction (including priority fees).

**Key insight:** Solana shifts costs from users to builders. Your users pay almost nothing, but you pay significantly more for infrastructure. Ethereum (especially on L2) is approaching Solana's user costs while maintaining lower builder infrastructure costs.

---

## 12. Hiring & Team Composition

### Developer Population (Electric Capital 2025 Report)

- **Ethereum:** 31,869 total active developers; added 16,181 new developers Jan-Sep 2025; 5.8% YoY full-time growth
- **Solana:** 17,708 total active developers; added 11,534 new developers in same period; **83% YoY growth** with 29.1% full-time developer growth — much faster momentum despite smaller base

### Ethereum (EVM)

- **Solidity developers:** Large and growing pool. JavaScript developers can learn Solidity in weeks. Most bootcamps teach Solidity
- **Typical team:** 2-3 Solidity engineers + 1-2 frontend (React/wagmi) + 1 infra/DevOps + 1 security researcher
- **Hiring advantage:** Easier to find and replace team members. Larger contractor/freelancer market
- **Foundry adoption** means Solidity devs are increasingly writing tests in Solidity too (full-stack Solidity)

### Solana (SVM)

- **Rust developers:** Smaller pool, harder to hire. ~4,366 open Rust blockchain positions with limited experienced supply. Rust blockchain developers command a **20-30% salary premium**, with Staff/Principal roles reaching **$380K-$550K+**
- **Typical team:** 2-4 Rust engineers (more needed per feature due to complexity) + 1-2 frontend (React + @solana/web3.js) + 1-2 infra (critical — someone must manage RPC, cranks, indexing) + 1 security researcher
- **Hiring challenge:** Rust + blockchain + DeFi domain knowledge is a rare combination. Companies report "unprecedented willingness" to hire and upskill over 3-6 months
- **Cross-training:** Some teams hire Solidity devs and train them on Anchor, which works but has a 2-3 month ramp

### Cross-Chain Builder Trends

Increasingly, major protocols build on both ecosystems:
- **Wormhole:** Originally the first Ethereum-Solana bridge (2020), now connecting 40+ chains with $60B+ in bridged value
- **Pyth Network:** Oracle born on Solana, now serves data across EVM chains
- **MetaMask:** Added native Solana support in 2025, signaling ecosystem convergence
- **Circle (USDC):** Native on both, using Wormhole NTT for cross-chain transfers

---

## 13. Token Launch & Liquidity Bootstrapping

### Solana (SVM)

**Pump.fun (Dominant Launchpad):**

Pump.fun is the first Solana app to surpass **$1 billion in cumulative revenue** ($321M in 2024, $664M in 2025, $98M early 2026). Cumulative volume exceeds **$150 billion**. It accounts for **70-77% of all Solana token launches** and up to **56% of Solana DEX transactions**.

How it works:
- Anyone creates a token for ~0.02 SOL (~$3). 1B tokens minted, 800M placed on a bonding curve
- Price rises algorithmically with buys. When the curve reaches ~$69K market cap, the token "graduates"
- At graduation, ~$12K of liquidity auto-deposits to **PumpSwap** (pump.fun's own DEX, launched March 2025, replacing Raydium as the default migration target)
- **Graduation rate: only ~1.1-1.4%** of tokens ever graduate. Out of 20,000-30,000 tokens created daily, only 100-200 make it to DEX listing
- 13+ million tokens created total

Fee evolution: 0.02 SOL creation fee. "Project Ascend" (Sept 2025) introduced dynamic fees — 0.95% under $300K mcap, scaling to 0.05% above $20M. Creator Fee Sharing (Jan 2026) distributes fees to up to 10 wallets. **PUMP token** raised ~$500M in under 12 minutes (July 2025) at $4B FDV.

**Raydium — AMM & CLMM Pools:**
- **Standard AMM (CPMM):** Classic x*y=k pools. Preferred for new token launches — simple, no range management. Permissionless pool creation
- **Concentrated Liquidity (CLMM):** LPs choose price ranges for capital efficiency. Better for established pairs
- **Permissionless Farms:** Any project can create reward farms to bootstrap liquidity via incentive emissions
- Lost significant market share after PumpSwap launched

**Meteora — DLMM and Launch Pools:**
- **DLMM (Dynamic Liquidity Market Maker):** Concentrates liquidity into discrete price bins. Auto-adjusts trading fees based on real-time volatility
- **DLMM Launch Pool:** Specifically designed for new token launches. Auto-listed on Jupiter and aggregators
- **Alpha Vault (Anti-Snipe):** Supporters deposit SOL/USDC before trading starts — all get tokens at the same average price. Configurable lock-up and vesting. Blocks sniper bots

**Jupiter LFG Launchpad:**
- Community-governed: projects apply via Jupiter Research Forum, JUP DAO votes on acceptance
- Uses Meteora's DLMM as the underlying mechanism
- 78 projects launched, $1.2B TVL, 780K users
- Notable launches: JUP (airdropped to ~1M wallets), WEN

### Ethereum (EVM)

**Uniswap — Initial Liquidity Provision:**
- **Uniswap v2 (50/50 pools):** Still the most common pattern for new token launches. Deposit equal value of TOKEN + ETH/USDC. Simple, set-and-forget
- **Uniswap v3 (concentrated liquidity):** Multiple fee tiers (0.01%-1%). More capital efficient but requires active management. Less common for initial volatile launches
- **Uniswap v4 (Jan 2025):** Hooks enable custom launch mechanics (custom fee structures, oracles). Reduced gas costs

Common launch pattern: Deploy ERC-20 → create Uniswap v2 pool with initial liquidity → lock or burn LP tokens → list on aggregators.

**Balancer LBP (Liquidity Bootstrapping Pools):**
- Two-token pool with **dynamic weight shifting** over time (e.g., 95/5 → 50/50)
- Creates constant downward price pressure — price drops if there's no demand
- Buyers incentivized to **wait** rather than FOMO, preventing whale manipulation
- Minimal starting capital needed (10-20% collateral instead of 50%)
- Dutch auction-style price discovery

**Fjord Foundry (formerly Copper Launch):**
- Leading LBP platform across EVM chains, built on Balancer's mechanism
- Supports "zero liquidity LBPs" — projects don't need significant upfront collateral
- Cross-chain support via Connext — users participate from any chain
- Deployed on: Ethereum, Arbitrum, Base, Berachain, Mantle, zkSync, Monad, and more

**Vesting Contract Conventions:**
- **Sablier:** Live on 27 EVM chains + Solana. Token streaming by the second — continuous vesting. Supports linear, cliff, exponential, and custom curves
- **Hedgey:** "Vesting NFTs" — vesting rights as NFTs enabling governance voting with locked tokens. 5+ audits by ConsenSys Diligence
- **Typical schedules:** Team/investor: 6-12 month cliff, 18-48 month linear unlock. Community: shorter vesting or immediate

**Token Deployment Costs:**
- Ethereum mainnet: $100-$500 gas for basic ERC-20
- L2s (Base, Arbitrum): often under $1-$5 post-Dencun
- No-code platforms (TokenMint, etc.) available; OpenZeppelin contracts remain gold standard for custom deployments

### LP Lock/Burn Conventions

| Dimension | Ethereum (EVM) | Solana (SVM) |
|---|---|---|
| Lock platforms | Unicrypt (invented in 2020), Team Finance, PinkLock | Less standardized; Streamflow, custom contracts |
| Burn convention | Send LP to 0x000...dead (permanent) | Less common; PumpSwap auto-deposits |
| Community expectation | Lock 6-12mo minimum; burn = strongest signal | Growing but less rigid |
| Verification | DexScreener padlock icon for burned LP | Less standardized UI indicators |

### Airdrop Mechanics

**Solana — ZK Compressed Airdrops:**
- Traditional airdrops require creating a token account per recipient (~0.002 SOL each)
- **ZK Compression** (Light Protocol): Bundles all data into a single Merkle root with zero-knowledge proofs. **95%+ cost reduction** — 10,000 recipients drops from ~20 SOL to ~0.01 SOL
- **Helius AirShip:** Free, open-source tool for ZK-compressed airdrops (Web UI + CLI)
- **Notable:** Jupiter "Jupuary" (Jan 2024) — 1B JUP to 1M+ wallets. Solana TPS jumped from ~1,900 to 3,000+

**EVM — Merkle Distributors:**
- Build Merkle tree off-chain from recipient list, deploy claim contract storing only the root hash
- Users submit address + amount + Merkle proof to claim. Gas-efficient (one hash on-chain vs all addresses)
- **Notable:** Arbitrum airdrop (March 2023) — 1.275B ARB to eligible users. Massive congestion at launch, claiming portal overloaded for hours

### Key Comparison

| Dimension | Ethereum (EVM) | Solana (SVM) |
|---|---|---|
| Dominant launch platform | Uniswap v2 + Fjord Foundry LBP | Pump.fun (70-77% of launches) |
| Token creation cost | $100-500 (mainnet), <$5 (L2) | 0.02 SOL (~$3) |
| Daily new tokens | Thousands (varies by chain) | 20,000-30,000 (pump.fun alone) |
| Fair launch mechanism | Balancer LBP (Dutch auction) | Bonding curve → auto-migration |
| Anti-snipe tooling | LBP high-start pricing | Meteora Alpha Vault |
| Airdrop cost optimization | Merkle distributor (claim-based) | ZK Compression (95%+ savings) |
| LP lock convention | Strong norm (Unicrypt, Team Finance) | Emerging |

---

## 14. Network Reliability & Uptime

### Solana (SVM): Improving but Historically Challenged

Solana has experienced **87+ outages** over ~4 years (StatusGator), though the majority were concentrated in 2021-2022.

**Major Outage Timeline:**

| Date | Duration | Root Cause |
|---|---|---|
| Sep 14, 2021 | ~17 hours | Bot trading (~400K TPS) overwhelmed validators |
| Jan 6-12, 2022 | Intermittent (days) | Severe network congestion |
| Apr 30, 2022 | Hours | NFT mint (Candy Machine) caused validators to run out of memory |
| Jun 1, 2022 | Hours | Durable nonce transaction bug caused validator state disagreement |
| Sep 30, 2022 | Hours | Hot-spare validator produced duplicate blocks; fork selection bug |
| Feb 25, 2023 | Hours | Malfunctioning validator broadcast oversized block, overwhelming Turbine |
| Feb 6, 2024 | ~5 hours | `LoadedPrograms` cache bug triggered consensus failure |

**Unreported disruptions:** StatusGator detected **9 distinct disruptions** between Oct 2024 - Feb 2025 impacting transactions, some lasting ~13 hours, never officially acknowledged.

**How stability has improved:**

1. **Firedancer Validator Client** (Jump Crypto): Ground-up C rewrite. Networking layer demonstrated **1M+ TPS** packet ingress in tests. Hybrid version (Frankendancer) live on mainnet since Sept 2024 — **207 validators (~20.9% of staked SOL)** run it as of Oct 2025. Full voting deployment expected in 2025
2. **QUIC Protocol:** Replaced UDP for transaction ingestion. Provides flow control, DDoS protection, better spam filtering. Firedancer implements custom QUIC with kernel-bypass techniques
3. **Client Diversity:** Agave (Labs client) + Firedancer (Jump client) means a bug in one implementation doesn't necessarily halt the network

**Builder contingency patterns (from official Solana docs + Helius guides):**
- Set `maxRetries = 0` on `sendTransaction` during congestion; implement custom rebroadcasting
- Use `confirmed` commitment for recent blockhash (better time window vs `finalized`)
- Blockhashes expire after ~60 seconds — implement refresh and re-sign logic
- Use `simulateTransaction` before sending to catch errors early
- **Never assume a sent transaction landed.** Poll for confirmation status; show users pending/retry states

### Ethereum L1: Never Down

Ethereum L1 has **never experienced a full network outage** — continuous block production since 2015 launch, including through The Merge (Sept 2022).

**Finality delays (not outages):**
- May 11-12, 2023: Beacon chain finality delayed (4 epochs, then 9 epochs next day; ~149 blocks missed). Cause: Prysm cache overflow from stale attestations. Network continued producing blocks — transactions processed, just not finalized immediately. Client diversity prevented a full outage

### Ethereum L2: Sequencer Risk

| Date | L2 | Duration | Root Cause |
|---|---|---|---|
| Jun 2023 | Arbitrum | ~2 hours | Sequencer ran out of ETH for batch posting |
| Dec 15, 2023 | Arbitrum | ~1.5-3 hours | Consensus client desync + inscription spam surge |

**Builder mitigations:**
- **Chainlink L2 Sequencer Uptime Feed:** On-chain oracle reporting sequencer status. Critical for price oracle consumers — without this check, stale prices during downtime can be exploited (flagged in BendDAO audit)
- **Grace periods:** After sequencer restart, wait before acting on price data to prevent stale-oracle arbitrage
- **Forced inclusion (censorship resistance):** Arbitrum's Delayed Inbox and Optimism's `OptimismPortal` let users submit transactions directly to L1, forcing inclusion even if the sequencer is down or censoring

### Key Comparison

| Dimension | Ethereum (EVM) | Solana (SVM) |
|---|---|---|
| L1 full outages | **Zero** (since 2015) | 87+ incidents (improving) |
| Most recent major incident | Finality delay (May 2023, non-stop) | LoadedPrograms bug (Feb 2024, ~5h) |
| Client diversity | 5 consensus clients, 3+ execution clients | 2 clients (Agave, Firedancer) |
| L2 sequencer risk | Real (hours-long outages) | N/A (no L2 equivalent) |
| Forced inclusion fallback | Yes (Delayed Inbox, OptimismPortal) | N/A |
| Builder must handle | L2 sequencer liveness | Transaction retry, blockhash expiry |

---

## 15. Production Monitoring & Debugging

### Ethereum (EVM): Mature Tooling

**Tenderly — Full-Stack Debugging Platform:**
- **Transaction Simulation:** Preview exact outcomes across 90+ EVM chains before submitting. Full control over inputs, state overrides, and contract code
- **Debugger:** Step-by-step execution with state changes, event emissions, gas consumption, stack traces with line-by-line Solidity source mapping, state diff visualization, revert reason detection
- **Gas Profiler:** Per-opcode and per-function consumption analysis
- **Monitoring & Alerting:** 12+ on-chain triggers; notifications to Slack, Discord, Telegram, PagerDuty. Generated **330M+ alerts** in 2025
- **Virtual TestNets:** Forked mainnet environments (510K+ created in 2025)
- Scale: 225B+ Node RPC calls, 134M+ simulations, 970K+ transactions debugged in 2025

**Forta Network — Decentralized Threat Detection:**
- Thousands of community-developed detection bots scan transactions in real-time
- Range from simple threshold monitors to ML-powered models detecting phishing, rug pulls, address poisoning
- **Forta Firewall:** Can block malicious transactions before execution
- Covers Ethereum, Polygon, Arbitrum, Optimism, BSC

**How teams debug a reverted production transaction (EVM):**
1. Get tx hash from monitoring/alerting (Tenderly, Forta, user report)
2. Paste into Tenderly → instant revert reason, stack trace, state diffs, gas profile with source-mapped Solidity lines
3. If unavailable: call `debug_traceTransaction` with `callTracer` against an archive node
4. Check revert string or custom error selector to identify which `require`/`revert` was hit
5. Use Tenderly simulation to replay with modified parameters to test fixes
6. Deploy fix via OpenZeppelin Defender managed workflow

### Solana (SVM): Significant Gap

**No Tenderly equivalent exists for Solana.** This is the single biggest developer tooling gap between the ecosystems.

**Available tools:**
- **`simulateTransaction` RPC:** Execute transaction in simulated environment, returns logs and state changes. Known limitation: simulation failures can drop the logs needed for debugging
- **Program logs:** Programs emit via `msg!()` macro. No equivalent of EVM stack traces — you get flat log output and must infer execution path from log messages and CPI boundaries. Anchor emits structured error codes mappable to error variants
- **Helius Webhooks:** Monitor up to **100,000 addresses** per webhook across **80+ transaction types**. Dynamic address management via API. Parsed, enriched transaction data
- **Orb (Helius):** Newest developer-focused explorer. 2-10x faster queries than BigTable-backed alternatives. Supports verified program IDLs, inner instruction inspection
- **Solscan:** Most popular for daily use — wallet monitoring, token pages, transaction breakdowns
- **Solana Watchtower:** Monitors validator health and delinquency. Alerts via Slack, Discord, Telegram, Twilio

**How teams monitor program health in production (Solana):**
1. Helius Webhooks/WebSockets for real-time program event streaming
2. Prometheus + Grafana with Telegraf collecting validator metrics every 30s
3. RPC monitoring: alert on 429 rate-limiting and 5xx errors, track CU consumption
4. Custom indexers via Geyser plugins streaming account changes to databases

### Key Comparison

| Dimension | Ethereum (EVM) | Solana (SVM) |
|---|---|---|
| Step-by-step debugger | Tenderly (line-by-line Solidity) | **None** |
| Transaction simulation | Tenderly, `eth_call` with state override | `simulateTransaction` (less capable) |
| Threat detection | Forta (decentralized, ML-powered) | No equivalent |
| Monitoring | Tenderly (330M+ alerts/yr), Defender, Forta | Helius Webhooks, custom Geyser indexers |
| Production debugging flow | ~2 minutes to root cause (Tenderly) | Manual log parsing, often 10x slower |
| Explorer quality | Etherscan (gold standard) | Solscan, Orb (improving) |

---

## 16. Regulatory & Compliance-Aware Design

### Solana Token-2022: Purpose-Built Compliance Primitives

Token-2022 (Token Extensions) provides built-in regulatory features at the token program level — no custom contract logic needed.

**Freeze Authority:**
- Mint authority can freeze any token account, preventing all transfers
- Used by regulated stablecoins (PYUSD) for court orders and sanctions enforcement
- Same capability existed in original SPL Token, carried forward to Token-2022

**Permanent Delegate:**
- Assigns a delegate with **unlimited, irrevocable authority** over all token accounts for that mint
- Unlike regular delegates (which holders can revoke), permanent delegates **cannot be changed or removed** by token holders
- Can **transfer or burn tokens from any account** — enables regulatory clawback, seizure under court orders, recovery from lost wallets
- **PYUSD uses this extension**

**Confidential Transfers:**
- Encrypts transaction amounts while keeping token type and sender/receiver visible
- Optional third-party auditor can be designated to view encrypted amounts (critical for regulators)
- **PYUSD initialized this extension but has not yet enabled it**
- **Incompatibility:** Cannot combine with Transfer Hooks (hooks need to read amounts, which are encrypted)

**Transfer Hooks:**
- A program defined by the mint authority is called on **every transfer**
- Enables: per-transfer sanctions screening, jurisdiction-based restrictions, fee collection, compliance logging
- **PYUSD uses transfer hooks** for fine-grained wallet interaction control

**Real Example — PYUSD (PayPal):**
First major stablecoin to fully leverage Token-2022 (launched on Solana May 2024). Uses: Permanent Delegate, Transfer Hooks, Transfer Fees, Confidential Transfers (initialized), Metadata, and Mint Close Authority.

### Ethereum (EVM): Mature Compliance Patterns

**Sanctions Screening (OFAC):**
- **Chainalysis Sanctions Oracle:** Deployed smart contract on most EVM chains. Returns whether an address is on OFAC/EU/UN sanctions lists. **Free to use** — no Chainalysis customer relationship required. DeFi protocols call it in transfer/interaction functions to block sanctioned addresses
- **TRM Labs:** Competing provider with address screening, risk scoring, and compliance APIs

**Token-Level Blacklists:**
- **USDC:** Contract includes `blacklist(address)` and `unBlacklist(address)` functions. Only Circle's `blacklister` role can modify. Blacklisted addresses **cannot send or receive USDC**. In 2022, Circle froze >75,000 USDC in addresses tied to Tornado Cash after OFAC sanctions

**Permissioned DeFi:**
- **Aave Arc:** Separate Aave V2 deployment exclusively for KYC-verified institutions. Fireblocks served as sole whitelister using FATF-based KYC framework. 30 licensed institutions whitelisted at launch (Jan 2022) including GSR, Wintermute, CoinShares. **Note:** Saw limited traction post-launch, effectively dormant — highlighting permissioned DeFi demand was aspirational in 2022
- **KYC-gated pools:** Standard pattern of `address => bool` allowlist checked via modifiers. Often integrated with off-chain KYC providers (Fireblocks, Synaps, Persona)

**ERC-3643 (T-REX Protocol for Security Tokens):**
- Purpose-built standard for tokenized securities with embedded compliance
- **ONCHAINID:** On-chain identity system — transfers only execute when both parties pass verification rules
- **Compliance Module:** Automated transfer restrictions based on jurisdiction, accreditation, holding period — checked before every transfer
- **Recovery:** Built-in token recovery for lost wallet access (critical for regulated securities)
- **Adoption:** Over **$32 billion** in real-world assets tokenized using ERC-3643
- **Institutional recognition:** SEC Chairman Paul S. Atkins cited ERC-3643 in his speech announcing Project Crypto. ISO standardization proposal submitted

### Key Comparison

| Dimension | Ethereum (EVM) | Solana (SVM) |
|---|---|---|
| Freeze capability | Per-token `blacklist()` functions | Freeze Authority (program-level) |
| Clawback/seizure | Custom contract logic needed | Permanent Delegate (built-in) |
| Sanctions screening | Chainalysis Oracle (on-chain, free) | Transfer Hooks (custom logic) |
| Confidential transfers | Limited (Tornado Cash sanctioned) | Token-2022 Confidential Transfers |
| Security token standard | ERC-3643 ($32B+ tokenized) | No equivalent standard |
| Permissioned DeFi | Aave Arc (dormant), allowlist patterns | Token-2022 Transfer Hooks |
| Regulatory recognition | ERC-3643 cited by SEC, ISO proposal | PYUSD adoption validates approach |

---

## 17. Additional Considerations

### Account Abstraction

- **EVM (ERC-4337 + EIP-7702):**
  - **ERC-4337:** Smart contract wallets using UserOperation objects, a decentralized alt-mempool, and an on-chain EntryPoint contract. Key components: Bundlers (aggregate UserOperations and submit to EntryPoint), Paymasters (sponsor gas — vast majority of UserOperations leverage paymasters, with tens of millions in sponsored gas fees). Supported by Safe, ZeroDev, Biconomy, Pimlico, Alchemy
  - **EIP-7702 (Pectra upgrade, May 7, 2025):** EOAs can temporarily execute smart contract code during a transaction without deploying a smart wallet. Enables transaction batching, gas sponsorship, periodic subscriptions, social recovery — all natively in the protocol. **Unlike ERC-4337, requires no external bundler/paymaster infrastructure.** Now LIVE on Ethereum and Optimism
  - **Complementary, not competing:** EIP-7702 EOAs can still leverage ERC-4337 bundlers and paymasters. Together they create a comprehensive account abstraction stack
  - Industry projections anticipated **200M+ smart accounts** by late 2025
- **Solana:** Natively supports some AA features (transaction fees can be paid by any signer, not just the sender). No ERC-4337 equivalent needed because the account model is more flexible. Wallet-level innovation happens at the wallet layer (Phantom, Backpack)

### L2 Considerations (EVM-Specific)

The rise of L2s fundamentally changes the EVM builder calculus:
- **Post-Dencun (EIP-4844) cost reductions:** Arbitrum dropped from ~$0.37 to ~$0.012/tx; Optimism from ~$0.32 to ~$0.009/tx — approaching Solana-level user costs
- **Arbitrum, Optimism, Base, zkSync** — near-Solana costs with EVM compatibility
- **OP Superchain:** A network of **34 OP Chains** making up **over 50% of all L2 activity** and over 10% of all crypto activity (H1 2025). Vision: shared sequencing, pooled liquidity across member chains, collective governance. Full interoperability is a core roadmap priority for 2025-2026
- **Fragmentation problem:** The biggest UX failure of 2024-2025 — users with ETH on Base can't buy an NFT on Optimism without bridging. Solutions: shared sequencing, native interoperability protocols, intent-based cross-chain execution
- **OP Stack vs Arbitrum Orbit:** Two competing L2 framework stacks. OP Stack (open-source, used by Base, Zora, Mode) vs Arbitrum Orbit (used by Xai, Sanko, others)
- **Cross-chain messaging** — LayerZero, Axelar, Wormhole enable cross-L2 communication
- **Ethereum's biannual upgrade cycle** — major protocol changes roll out every ~6 months (Pectra May 2025, Glamsterdam, then Hegota). L2 builders must track L1 changes that affect their stack
- Building on L2 means: same Solidity tooling, lower costs, but added bridging/messaging complexity
- Many teams deploy to 3-5 L2s simultaneously (deterministic deployment via CREATE2)

### EIP Process & How It Shapes Building

The EIP process is the unit of governance for Ethereum protocol development:
- **Categories:** Core EIPs (protocol changes, included in hard forks), ERCs (smart contract and token standards like ERC-20, ERC-721, ERC-4337), Meta (process), Informational (guidelines)
- **Lifecycle:** Idea -> Draft -> Review -> Last Call -> Final. Any EIP inactive for 6+ months moves to Stagnant
- **Impact on builders:** ERCs define the interfaces your contracts must implement. Core EIPs change what opcodes are available and how gas costs work. Builders must track the EIP pipeline to plan for upcoming changes (e.g., EIP-7702 changing account abstraction patterns, EIP-4844 changing L2 data costs)
- **Pectra upgrade (May 2025)** was the largest fork in Ethereum's history by number of EIPs included

### Event/Log Conventions for Off-Chain Consumption

EVM events are a first-class design primitive for bridging on-chain state to off-chain systems:
- **Structure:** Events emit log entries with up to 4 indexed topics (32 bytes each, searchable via Bloom filters) + unlimited unindexed data (cheaper to emit)
- **Topic 0** is always `keccak256` of the event signature (e.g., `Transfer(address,address,uint256)`)
- **Standard convention:** Emit an event for every consequential state change. Addresses should be indexed; amounts generally should not be (cheaper as data, and amounts rarely need filtering)
- **ERC-20 Transfer event** (`event Transfer(address indexed from, address indexed to, uint256 value)`) is the most emitted event on Ethereum — the backbone of all token indexing and wallet tracking
- **Off-chain consumers:** The Graph subgraphs, custom indexers, block explorers, wallets, and analytics platforms all build on event logs
- Events are significantly cheaper than storage writes — the recommended way to store data that only needs off-chain access

### Token Standards

- **EVM:** ERC-20 (fungible), ERC-721 (NFT), ERC-1155 (multi-token), ERC-4626 (vault standard). Each token deploys its own contract
- **Solana:** SPL Token (one program for all tokens), Token-2022 (extensions for fees, confidential transfers, etc.), Metaplex for NFT standards. New token features don't require new program deployments — just new account configurations with Token Extensions

### Versioned Transactions & Lookup Tables (Solana-Specific)

- Solana transactions are limited in the number of accounts they can reference (~64 in legacy, 256 with versioned transactions + address lookup tables)
- **Address Lookup Tables (ALTs)** compress account references, enabling more complex transactions
- This is a real constraint when building protocols that interact with many accounts (DEX aggregators, liquidation engines)

---

## Methodology & Confidence Notes

**Web-verified data (high confidence):**
- Squads Protocol metrics ($10B+ secured, formal verification, named users)
- Safe metrics ($50B+ secured, $10M revenue, $600B volume in 2025, $189.6B Q1 2025)
- Safe revenue target: break-even 2026, $100M ARR by 2030
- Foundry vs Hardhat adoption (51.1% vs 32.9%, 2024 Solidity Developer Survey)
- Uniswap BSL timeline (v3 expired April 2023, v4 BSL until 2027)
- PancakeSwap forked Uniswap v3 within 1 day of BSL expiry ($140M TVL)
- CoW Protocol all-time-high $9B monthly volume, 34.3% DEX aggregation market share
- Ethereum Foundation Open Intents Framework (OIF) launched Feb 2025, 30+ teams
- Flashbots BuilderNet migration (Dec 2024), v1.2 (Feb 2025)
- Infura + Alchemy handle ~90% of Web3 RPC traffic
- The Graph: 15,087 active subgraphs Q3 2025 (7.6% QoQ growth)
- OP Superchain: 34 chains, 50%+ of all L2 activity
- EIP-7702 live with Pectra upgrade (May 7, 2025)
- ERC-7201 namespaced storage adopted in OZ Contracts v5
- ERC-7955 permissionless CREATE2 factory proposal
- Deterministic deployment proxy address (0x4e59b44847...)
- Solidity 87% DeFi TVL vs Vyper 8%
- Verified builds pipeline (Ellipsis Labs + OtterSec)
- State compression costs (24,000 SOL traditional vs ~10 SOL compressed for 1M NFTs)
- Token-2022 features and early adoption (Paxos USDP)
- The Graph Solana support (via Substreams)
- Helius infrastructure (LaserStream, DAS, pricing)
- SDK landscape (viem adoption surge, bundle size comparison)
- Electric Capital developer counts (ETH 31,869 / SOL 17,708, Solana 83% YoY growth)
- Historical exploit amounts (Wormhole $326M, Mango $116M, Curve $70M, Euler $200M)
- Audius storage collision exploit (2022)
- Rust developer salary premium (20-30%, Staff roles $380K-550K+)
- Post-Dencun L2 costs (Arbitrum $0.37→$0.012, Optimism $0.32→$0.009)
- Immunefi coverage ($190B+ protected, Firedancer $1M bounty)
- Solana account init limit (10,240 bytes), tx size (1,232 bytes, SIMD-0296 → 4,096)
- Solana Mobile Seeker phone (2025), MetaMask Solana support (2025)
- Wormhole cross-chain ($60B+ bridged value)
- Audit cost ranges (top-tier $80K-200K, mid-tier $25K-70K)
- Code4rena zero platform fee model, Sherlock expert focus
- Fork testing 20x speed improvement with block pinning
- EIP-1167 minimal proxy: 55 bytes total, 45 bytes runtime
- Pectra: largest Ethereum fork by number of EIPs

**Training knowledge (high confidence, pre-mid 2025):**
- Proxy patterns (UUPS, Transparent, Beacon, Diamond)
- Security attack vectors for both ecosystems
- OpenZeppelin as standard library
- Anchor framework patterns and CPI model
- Testing frameworks (Foundry, Hardhat, Bankrun)
- Audit firm landscape (Trail of Bits tooling: Slither, Echidna, Manticore)
- Cost structure estimates
- Governance patterns (Governor, Timelock, Realms)
- Permit2 and ERC-2612 approval patterns
- Flash loan composability patterns (ERC-3156)
- ERC-2771 meta-transaction standard
- Event/log design conventions

**Areas where current data may have evolved since mid-2025:**
- Specific pricing for RPC providers
- Adoption percentages for verified builds on Solana
- New tooling releases
- L2 ecosystem changes
- Account abstraction adoption numbers
- Superchain interoperability progress

**Sources:**
- [Safe Global](https://safe.global/)
- [Four Pillars: Safe Ownership Infra](https://4pillars.io/en/articles/safe-ownership-infra-layer-for-onchain-applications)
- [TodayOnChain: Safe Revenue](https://www.todayonchain.com/news/article/01KGHWZ3421ZEPXYS7P8BXTK9J/)
- [Flashbots MEV-Boost](https://boost.flashbots.net/)
- [Flashbots Docs](https://docs.flashbots.net/)
- [Eco: Intent-Based DEX Guide 2025](https://eco.com/support/en/articles/11852634-best-intent-based-dex-platforms-complete-2025-comparison-guide)
- [CoW Protocol Docs](https://docs.cow.fi/cow-protocol/concepts/introduction/intents)
- [Ethereum.org: Smart Contract Verification](https://ethereum.org/developers/docs/smart-contracts/verifying/)
- [MetaMask: Hardhat vs Foundry](https://metamask.io/news/hardhat-vs-foundry-choosing-the-right-ethereum-development-tool)
- [Nadcab: Web3 Frameworks 2026](https://www.nadcab.com/blog/web3-frameworks)
- [Chainlink Automation Docs](https://docs.chain.link/chainlink-automation)
- [OpenZeppelin Defender: Meta-TX Guide](https://docs.openzeppelin.com/defender/guide/meta-tx)
- [Alchemy: What is Account Abstraction](https://www.alchemy.com/overviews/what-is-account-abstraction)
- [Turnkey: Account Abstraction ERC-4337 to EIP-7702](https://www.turnkey.com/blog/account-abstraction-erc-4337-eip-7702)
- [Circle: Pectra Upgrade & EIP-7702](https://www.circle.com/blog/how-the-pectra-upgrade-is-unlocking-gasless-usdc-transactions-with-eip-7702)
- [Coinbase: Pectra Upgrade Guide](https://www.coinbase.com/blog/the-ultimate-guide-to-ethereums-pectra-upgrade)
- [EIPs Insight: Top 10 EIPs 2025](https://eipsinsight.com/Blogs/top-ten-eips-2025)
- [ERC-7201: Namespaced Storage Layout](https://eips.ethereum.org/EIPS/eip-7201)
- [RareSkills: ERC-7201 Explained](https://rareskills.io/post/erc-7201)
- [EIP-1167: Minimal Proxy Contract](https://eips.ethereum.org/EIPS/eip-1167)
- [Foundry: Deterministic Deployments](https://www.getfoundry.sh/guides/deterministic-deployments-using-create2)
- [ERC-7955: Permissionless CREATE2 Factory](https://eips.ethereum.org/EIPS/eip-7955)
- [GetBlock: Best RPC Providers 2025](https://getblock.io/blog/best-rpc-node-providers-2025-the-practical-comparison-guide/)
- [Messari: State of The Graph Q3 2025](https://messari.io/report/state-of-the-graph-q3-2025)
- [Ormi Labs: Best Blockchain Indexers 2026](https://blog.ormilabs.com/best-blockchain-indexers-in-2025-real-time-web3-data-and-subgraph-platforms-compared/)
- [Optimism: Superchain & Native Interoperability](https://optimism.mirror.xyz/9vAhFFTaxaaA2mfzRJUrPYuP6M687ffC74v7DCpsLEw)
- [Cryptopolitan: L2 Adoption 2026 Predictions](https://www.cryptopolitan.com/layer-2-adoption-2026-predictions/)
- [SoluLab: Smart Contract Audit Cost 2026](https://www.solulab.com/smart-contract-audit-cost/)
- [Trail of Bits: Smart Contract Audits](https://www.smartcontractaudits.com/audit-provider/trail-of-bits)
- [Code4rena: Free Audit Contests](https://www.zellic.io/blog/code4rena-free-contests/)
- [HackenProof: Code4rena vs Sherlock](https://hackenproof.com/blog/for-business/code4rena-vs-sherlock-crowdsourced-audits-comparison-guide)
- [Token Metrics: Solidity vs Vyper 2025](https://www.tokenmetrics.com/blog/solidity-vs-vyper-difference-2025-guide)
- [Chainlink: Composability](https://chain.link/education-hub/permissionless-composability)
- [RareSkills: Solidity Events](https://rareskills.io/post/ethereum-events)
- [Foundry: Fork Testing](https://getfoundry.sh/forge/tests/fork-testing/)
- [Ethereum.org: EIPs](https://ethereum.org/eips)

---

*Report compiled March 17, 2026. Sources cited inline. Builder-oriented analysis — not financial or investment advice.*
