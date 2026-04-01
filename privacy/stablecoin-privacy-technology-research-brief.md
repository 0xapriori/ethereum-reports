---
title: "Research Brief: Stablecoin Privacy Technology & Confidential Settlement"
date: 2026-03-31
---

# Research Brief: Stablecoin Privacy Technology & Confidential Settlement

*apriori-writer agent | March 31, 2026*

## tl;dr

- **The private stablecoin infrastructure layer is an emerging competitive category with at least 8-10 serious projects, but no clear winner.** Payy, Aztec, Railgun, Aleo, Namada, Penumbra, Fhenix, and Zama are all building some form of confidential transaction infrastructure, each with different technical approaches, funding levels, and go-to-market strategies.
- **Payy is the only project with a live consumer product (100K+ users, ~$130M annualized volume) building toward an institutional-grade L2.** Its "demand-first" approach -- shipping a consumer payments app and Visa card before launching the network -- is unique in the space. However, the $6M seed is small relative to competitors like Aleo ($228M) and Aztec ($119M+).
- **The ZK proof stack is converging on production readiness.** Payy's migration from Halo2 to Noir achieved sub-0.5s proof generation on iPhones. PLONK-based systems are approaching sub-second proving. The latency bottleneck for payment-speed private transactions is being solved in 2026.
- **The Ethereum Foundation's March 2026 Mandate explicitly prioritizes privacy (CROPS principles), creating a powerful tailwind** for EVM-native privacy projects. Vitalik stated Ethereum will reverse a decade of "backsliding" on self-sovereignty.
- **On-chain transparency is a verified, quantifiable risk.** Blockchain surveillance firms can now trace across 25+ chains with "one-click tracing." Stablecoins represent 84% of all illicit transaction volume tracking, and the same tools that catch criminals expose every legitimate business's treasury, payroll, and vendor payments.

---

## Table of Contents

1. [Private/Confidential Settlement Layers for Stablecoins](#1-privateconfidential-settlement-layers-for-stablecoins)
2. [ZK Technology for Payments](#2-zk-technology-for-payments)
3. [The Privacy Gap in Stablecoins](#3-the-privacy-gap-in-stablecoins)
4. [Competitive Landscape](#4-competitive-landscape)
5. [Key Uncertainties and Data Gaps](#5-key-uncertainties-and-data-gaps)
6. [Sources](#sources)

---

## 1. Private/Confidential Settlement Layers for Stablecoins

### 1a. Project Map: Who Is Building Private Stablecoin Infrastructure?

| Project | Approach | Tech Stack | Chain | Stage | Key Metric |
|---------|----------|-----------|-------|-------|------------|
| **Payy** | EVM L2 (validium rollup) | Noir (migrated from Halo2), UTXO model | Ethereum L2 | Consumer app live; L2 testnet 2026 | 100K+ users, ~$130M annualized volume |
| **Aztec** | Programmable privacy L2 | Noir, PLONK (UltraPlonk), client-side proving | Ethereum L2 | Alpha testnet (May 2025); v5 July 2026 | TGE completed Feb 2026; critical vuln disclosed Mar 2026 |
| **Railgun** | On-chain privacy layer (smart contracts) | zk-SNARKs, shielded balances | Ethereum, Arbitrum, Polygon, BNB | Live, production | $4.5B cumulative volume; $108M TVL; 326 daily shields |
| **Aleo** | Privacy-native L1 | Leo language, zk-SNARKs (Marlin) | Own L1 | Mainnet live | USAD (Paxos) + confidential USDC (Circle) launched |
| **Namada** | Multi-asset shielded pool L1 | MASP, zk-SNARKs | Cosmos ecosystem | Mainnet (Dec 2024) | $60M+ raised; NAM token live June 2025 |
| **Penumbra** | Fully shielded L1 with private DEX | zk-SNARKs, batch processing | Cosmos ecosystem | Live | Private transfers, staking, trading, governance |
| **Fhenix** | FHE-powered L2 | Zama's TFHE-rs, optimistic rollup | Ethereum L2 (Arbitrum) | CoFHE coprocessor on Arbitrum | $22M raised |
| **Zama** | FHE infrastructure layer | fhEVM, Confidential Blockchain Protocol | Ethereum (coprocessor) | Mainnet (Dec 2025) | $150M+ raised, $1B valuation; 11K+ bidders in sealed auction |

### 1b. How They Technically Work

**Zero-Knowledge Proofs (ZK)** -- The dominant approach. Used by Payy, Aztec, Railgun, Aleo, Namada, Penumbra, and Zcash. The prover generates a cryptographic proof that a transaction is valid (correct balances, authorized sender, etc.) without revealing the transaction details. The verifier (smart contract or L1 validator) checks the proof's validity without learning the underlying data.

- *Client-side proving*: The user's device generates the ZK proof before submitting to the network. This is Payy's model and Aztec's model. It means the sequencer/validator never sees the plaintext transaction data. The tradeoff is computational cost on the client device, which is why proving time matters so much for UX.
- *On-chain verification*: Railgun's approach -- users generate ZK proofs client-side, then submit them to the Railgun smart contract on Ethereum for on-chain verification. Users build "private balances" within the contract and can interact with any EVM DeFi protocol while maintaining privacy. The proving happens on the user's device; only the verification is on-chain.

**Fully Homomorphic Encryption (FHE)** -- Enables computation on encrypted data without decrypting it. Zama and Fhenix are the leaders. Theoretically more powerful than ZK (you can compute arbitrary functions on encrypted state, not just prove statements about it), but significantly more expensive computationally.

- Zama's January 2026 sealed-bid Dutch auction demonstrated FHE's capabilities: 11,103 bidders, $118.5M committed, all bid amounts encrypted on-chain. No bot sniping, gas wars, or copy trading possible.
- Current performance limitations: FHE operations are orders of magnitude slower than plaintext computation. The coprocessor model (offloading FHE computation to specialized hardware) is the pragmatic path.

**Trusted Execution Environments (TEEs)** -- Hardware-based privacy (Intel SGX, ARM TrustZone). Used by BuilderNet for block building, by some institutional custody solutions, and as part of hybrid approaches. Privacy depends on trusting hardware manufacturers and their attestation processes.

- TEEs are the basis of Zaki Manian's "Tier 1" institutional privacy (Tempo, Arc, RCM on Solana). They offer invisible, fast privacy but with backdoors and hardware trust assumptions. Not self-sovereign.

**Multi-Party Computation (MPC)** -- Distributes computation across multiple parties so no single party sees the full data. Used by Nillion and in some custody solutions. Practical for specific use cases (key management, threshold signatures) but generally too slow and communication-intensive for high-throughput payment settlement.

### 1c. The EVM-Compatible Privacy Landscape

EVM-native privacy is the critical design space because that is where the stablecoin liquidity lives. The key projects:

- **Payy**: Building an EVM-compatible L2 validium rollup. Uses a UTXO model (not account-based like standard EVM) for privacy, with ZK proofs for state transitions. Sequencers use HotStuff PoS consensus for ~1s soft finality. Provers generate validity proofs posted to Ethereum.
- **Aztec**: The most well-funded EVM privacy L2. Noir is both its ZK language and a general-purpose ZK DSL used by Payy and others. Alpha network launched May 2025, but a critical vulnerability in the proving system was disclosed March 27, 2026, with the fix scheduled for v5 (July 2026).
- **Railgun**: Operates as smart contracts directly on Ethereum and L2s. No bridge, no separate chain. Users interact with DeFi frontends (tested on CowSwap) without unshielding funds. Vitalik Buterin has personally used Railgun for donations.
- **Fhenix**: FHE-powered optimistic rollup on Arbitrum. The CoFHE coprocessor model allows existing EVM contracts to add encrypted computation without rewriting.

**The ERC-7984 standard** for confidential smart contracts is an emerging Ethereum standard, though specific technical details were not fully available in search results. [COULD NOT VERIFY: exact specification and adoption status of ERC-7984]

### 1d. Payy Specifically: Deep Dive

**Origins**: Payy was originally Polybase, a Web3 database project. It pivoted in 2023 to focus exclusively on private stablecoin payments. The team is led by Sid Gandhi (CEO) and Calum Moore (CTO, ex-Apple engineer).

**Relationship to Protocol Labs**: Payy launched as a project from Polybase Labs, a team within Protocol Labs.

**Consumer Product (Live)**:
- Non-custodial, privacy-preserving stablecoin Visa card (launched August 2025)
- 100,000+ users across 120 countries
- ~$130M annualized transaction volume
- Privacy by default: transaction details (sender, receiver, amount) are hidden using ZK proofs

**The Payy Network (Building)**:
- Ethereum L2 validium rollup
- UTXO-based transaction model (not account-based)
- Sparse Merkle tree for state management
- Sequencers: PoS network using HotStuff consensus, providing ~1s soft finality
- Provers: Generate validity proofs for Ethereum settlement; slot-allocated using drand-based randomness
- Originally built on Halo2, then rewrote entire ZK architecture to Noir
- Target: 1-second block times for point-of-sale compatibility

**Halo2-to-Noir Migration** (critical technical detail):
- Codebase shrank from "thousands of lines" to 250 lines
- MVP proof rebuilt in 30 minutes
- 2x proving speed improvement without optimization
- Sub-0.5s proof generation on iPhones
- Dramatically improved auditability (auditors without Noir experience can audit "95%" of the code)
- The entire engineering team can now work on privacy infrastructure, vs. only specialists before

**Funding**: $6M seed round led by FirstMark Capital (March 2026). The capital is for team growth and client acquisition ahead of testnet and mainnet launches in 2026.

**Whitepaper**: Available at polybase.github.io/zk-rollup/whitepaper.pdf -- describes the L2 Ethereum ZK Rollup architecture for private and compliant transactions.

**Open-source**: GitHub repository at github.com/polybase/payy

**Compliance Approach**: Designed for regulatory auditability alongside privacy. Selective disclosure mechanisms allow compliance verification without exposing full transaction data.

---

## 2. ZK Technology for Payments

### 2a. Current State of ZK Proof Systems

| System | Trusted Setup | Proof Size | Proving Time | Verification Time | Key Users |
|--------|--------------|------------|-------------|-------------------|-----------|
| **Groth16** | Required (per-circuit) | ~200 bytes | Fast | Very fast (~ms) | Zcash (Sapling), many production systems |
| **PLONK** | Universal (reusable) | ~400 bytes | ~2.4s (being optimized to sub-second) | Fast | Aztec, many L2s |
| **Halo2** | None | Larger than Groth16 | Moderate | Moderate | Zcash (Orchard), Scroll, Taiko; Payy (former) |
| **Nova/Folding** | None | Compact (incremental) | Very fast (per-step) | Fast (final) | Research/emerging production |
| **Noir** (DSL, not proof system) | Depends on backend | Depends on backend | Sub-0.5s on mobile (Payy) | Backend-dependent | Payy, Aztec ecosystem |

**Note**: Noir is a zero-knowledge domain-specific language, not a proof system itself. It compiles to different proving backends. Payy's sub-0.5s mobile proving time is achieved through Noir compiled to an optimized backend.

### 2b. Latency and Cost Tradeoffs

**For payment-speed settlement, the critical threshold is sub-1 second proof generation on consumer devices.** This is what makes private transactions viable for point-of-sale and real-time payments.

Current status:
- **Payy (Noir)**: Sub-0.5s on iPhones -- this is production-ready for payments
- **PLONK**: 2.4s proving time, being optimized toward sub-second -- at the boundary of acceptable UX for authentication flows; not yet payment-grade on consumer hardware
- **Groth16**: Fast proving but requires per-circuit trusted setup, which limits flexibility
- **Halo2**: No trusted setup, but Payy abandoned it due to code complexity, limited auditor availability, and development speed constraints
- **Nova/folding schemes**: Theoretically excellent for incremental verification (each payment step proven without re-proving history), but still emerging from research

**Cost tradeoffs**:
- Private transactions on EVM L1 (Railgun model): Gas costs for on-chain proof verification. More expensive than plaintext transactions, but uses existing Ethereum security.
- Private transactions on L2 (Payy/Aztec model): Amortized proof costs across batches. Validium rollups post proofs to Ethereum but store data off-chain, reducing L1 costs.
- Private transactions on alt-L1 (Aleo model): Native proving at the protocol level, potentially cheapest per-transaction, but requires separate chain security and liquidity bootstrapping.

### 2c. Which Approaches Are Production-Ready?

**Production-ready for payment-speed settlement (as of March 2026)**:
1. **Noir-based client-side proving** (Payy): Sub-0.5s mobile proving, live consumer product
2. **Railgun's on-chain ZK**: Live on 4 chains, $4.5B cumulative volume, but user-facing latency depends on Ethereum block times (~12s), not proof generation
3. **Groth16-based systems**: Proven at scale (Zcash Sapling), but per-circuit trusted setup limits flexibility

**Approaching production-ready**:
4. **Aztec's Noir-based L2**: Alpha testnet live, but critical vulnerability (March 2026) delays mainnet. July 2026 v5 target.
5. **Aleo**: Mainnet live with USAD and confidential USDC, but proving times on consumer hardware and developer tooling maturity are still constraints

**Not yet production-ready for payments**:
6. **FHE (Zama/Fhenix)**: Demonstrated in auction settings but computational overhead is too high for real-time payment settlement
7. **Nova/folding**: Still primarily research-stage for payment applications

---

## 3. The Privacy Gap in Stablecoins

### 3a. How On-Chain Transparency Creates Risk

The fundamental problem: **every stablecoin transaction on a public blockchain is permanently visible to anyone**. This creates multiple concrete attack vectors and business risks:

**MEV and Front-Running**:
- Stablecoin-denominated DeFi transactions (swaps, liquidations, large transfers) are visible in the mempool before execution
- MEV extraction on Ethereum runs ~$250-300M/year (Source: EigenPhi via ESMA TRV Risk Analysis, data through Jan 2025; MEV report in this repository)
- Sandwich attacks, front-running, and back-running all exploit transaction visibility
- Private orderflow protection (MEV Blocker: 4.5M+ wallets) is a partial mitigation but does not hide balances or transaction history

**Balance and Treasury Exposure**:
- Any entity holding stablecoins has their balance visible to competitors, partners, regulators, and adversaries
- Corporate treasuries denominated in on-chain stablecoins are fully transparent
- Payroll payments reveal compensation structures
- Vendor payments reveal business relationships and costs
- Institutional portfolio allocations are visible to the entire market

**Counterparty Surveillance**:
- Blockchain surveillance firms (Chainalysis, TRM Labs, Elliptic) support 25+ blockchains with "one-click tracing" across 74 million known cross-chain swap instances (Source: TRM Labs)
- The same tools designed to catch criminals are available to competitors, hostile actors, and overreaching state entities
- By 2025, "the veil of pseudonymity is thinner than ever" -- wallet addresses are routinely linked to identities (Source: Yellow.com research report)

**AI-Accelerated De-Anonymization**:
- AI has destroyed practical privacy on transparent chains (assessment from Zcash research, this repository)
- Machine learning models can cluster wallets, predict identities, and analyze behavior patterns at scale
- The asymmetry between how easy it is to analyze public blockchain data and how hard it is to maintain operational security is widening

### 3b. Real Incidents from Stablecoin Transparency

**Documented incidents and patterns**:

1. **Iranian Central Bank network exposure (late 2025)**: Leaked documents posted on social media revealed cryptocurrency addresses belonging to the Central Bank of Iran, "unraveling a network of coordinated central bank laundering unprecedented in its organization and scale." This demonstrates that even state actors cannot maintain privacy on transparent chains. (Source: Chainalysis 2026 Crypto Crime Report)

2. **Ruble-backed A7A5 stablecoin**: Processed more than $93 billion in less than a year as a "purpose-built settlement rail for sanctioned actors." The irony: the transparency that was supposed to enable compliance also enabled complete surveillance and attribution of the network. (Source: Chainalysis/TRM Labs 2026 reports)

3. **Stablecoins as 84% of illicit transaction volume**: Illicit cryptocurrency addresses received a record $154 billion in 2025 (162% increase from 2024), with stablecoins representing 84% of illicit transaction volume and 95% of inflows to sanctioned entities. (Source: TRM Labs 2026 Crypto Crime Report)

4. **Vitalik Buterin's personal use of Railgun**: Ethereum's co-founder has used Railgun multiple times for donations, publicly demonstrating that even prominent crypto figures need transaction privacy. The Ethereum Foundation also staked 50,000 RAIL tokens. This is a strong signal that the need for privacy is recognized at the highest levels of the ecosystem.

5. **MEV Brothers criminal case (Nov 2025)**: The first prosecution targeting MEV infrastructure exploitation ended in a mistrial. The case demonstrates that transaction transparency creates novel legal and financial risks that the regulatory framework hasn't resolved. (Source: MEV report in this repository)

[COULD NOT VERIFY: Specific corporate incidents of business intelligence leaks from stablecoin balance exposure. The risk is well-documented in principle, but specific named incidents of companies suffering competitive harm from on-chain treasury visibility were not found in search results. This is likely because affected companies have no incentive to publicize such incidents.]

### 3c. Why Enterprises and Institutions Need Privacy

**The enterprise privacy gap is the primary blocker for institutional stablecoin adoption.** The data supports this claim:

- Stablecoins processed over $8.9 trillion in H1 2025 alone, but less than 1% of businesses use crypto for payroll, with "privacy being the primary blocker" (Source: Aleo/Toku press release, Jan 2026)
- The stablecoin market cap surpassed $300 billion by March 2026, with transaction volumes approaching $1 trillion monthly -- yet institutional adoption remains constrained by transparency concerns
- Payy's thesis, stated by Protocol Labs: "every stablecoin transaction is a permanent data leak, and that's the reason institutions won't move real capital onchain"
- The GENIUS Act (enacted 2025) grants federal oversight to payment stablecoin issuers with over $10B outstanding, creating regulatory clarity that should accelerate institutional adoption -- but only if privacy concerns are addressed

**Specific enterprise requirements**:
1. **Payroll privacy**: Employee compensation must not be publicly visible. Aleo/Toku/Paxos launched the first private stablecoin payroll solution in January 2026 specifically to address this.
2. **Treasury management**: Corporate treasuries need balance confidentiality from competitors and market participants
3. **Vendor/supply chain payments**: Business relationships and pricing must remain confidential
4. **Regulatory compliance**: Institutions need selective disclosure -- prove compliance (sanctions screening, KYC) without revealing transaction details to the public
5. **Cross-border settlement**: International payments require privacy for commercial confidentiality while maintaining regulatory auditability per jurisdiction

---

## 4. Competitive Landscape

### 4a. Detailed Project Comparison

#### Payy
- **Funding**: $6M seed (March 2026), led by FirstMark Capital
- **Traction**: 100K+ users, ~$130M annualized volume, live Visa card across 120 countries
- **Technical approach**: Ethereum L2 validium rollup, Noir ZK proofs, UTXO model, HotStuff PoS consensus
- **Go-to-market**: "Demand-first" -- consumer product shipped first, institutional L2 second
- **Strengths**: Only project with live consumer traction; sub-0.5s mobile proving; open-source; Protocol Labs relationship
- **Weaknesses**: Small funding relative to competitors; L2 not yet live (testnet/mainnet 2026); single-team risk
- **Key differentiator**: The consumer app proves demand exists. No other privacy project has 100K users actually transacting.

#### Aztec
- **Funding**: $119M+ total (a]6 $100M Series B in Dec 2022, $17M Series A)
- **Traction**: Alpha testnet (May 2025); TGE completed Feb 2026; AZTEC token trading
- **Technical approach**: Programmable privacy L2 with Noir language, client-side proving, UltraPlonk backend
- **Go-to-market**: Developer platform -- build any private application on Ethereum
- **Strengths**: Largest war chest among privacy L2s; Noir becoming the standard ZK DSL; 8 years of research
- **Weaknesses**: Critical vulnerability disclosed March 27, 2026 (proving system could enable theft); v5 fix not until July 2026; no consumer product; long development timeline
- **Key differentiator**: General-purpose programmable privacy, not just payments. Noir is used by Payy and others.

#### Railgun
- **Funding**: $7M private token sale (July 2021, DCG led); Ethereum Foundation staked 50K RAIL
- **Traction**: $4.5B cumulative volume; $108M TVL; 326 daily shields; live on 4 chains
- **Technical approach**: On-chain smart contract privacy layer; zk-SNARKs; "Private Proofs of Innocence" for compliance
- **Go-to-market**: Infrastructure layer for existing DeFi -- private wallet that interacts with any EVM contract
- **Strengths**: Live and growing rapidly (TVL 10x from $11M to $108M); no bridge risk (native on-chain); Vitalik endorsement; compliance innovation with Proofs of Innocence
- **Weaknesses**: Limited to EVM chains; gas costs for on-chain proof verification; no own token with significant market cap; constrained by L1 throughput
- **Key differentiator**: Works with existing DeFi without requiring migration to a new chain/L2.

#### Aleo
- **Funding**: $228M total ($200M Series B, Feb 2022, SoftBank led); $1.45B valuation
- **Traction**: Mainnet live; USAD (Paxos) launched; confidential USDC (Circle) launched; Toku payroll integration
- **Technical approach**: Privacy-native L1 with Leo language; zk-SNARKs (Marlin); all transactions private by default
- **Go-to-market**: Enterprise-focused -- private stablecoins, payroll, institutional settlement
- **Strengths**: Largest funding in the space; Circle and Paxos partnerships; enterprise credibility
- **Weaknesses**: Separate L1 (not EVM-compatible natively); must bootstrap liquidity and ecosystem; ALEO token performance uncertain
- **Key differentiator**: Circle launched confidential USDC on Aleo, signaling major issuer interest in privacy. Paxos launched USAD.

#### Namada
- **Funding**: $60M+ (Anoma Foundation)
- **Traction**: Mainnet (Dec 2024); NAM token live (June 2025)
- **Technical approach**: L1 with Multi-Asset Shielded Pool (MASP); zk-SNARKs; Cosmos ecosystem
- **Go-to-market**: "Shielded multichain" -- privacy for any compatible token across IBC
- **Strengths**: MASP creates larger anonymity sets than single-asset pools; Cosmos interoperability
- **Weaknesses**: Not EVM-native; Cosmos ecosystem is smaller than Ethereum; adoption metrics unclear
- **Key differentiator**: Multi-asset shielded pool means all assets share one anonymity set.

#### Zama (FHE Infrastructure)
- **Funding**: $150M+; $1B valuation
- **Traction**: Mainnet on Ethereum (Dec 2025); sealed-bid auction with 11K+ bidders, $118.5M committed
- **Technical approach**: FHE coprocessor for encrypted smart contracts on existing chains
- **Go-to-market**: Infrastructure layer -- enable existing protocols to add encrypted computation
- **Strengths**: Massive funding; working product; computation on encrypted data (not just proving)
- **Weaknesses**: FHE is computationally expensive; not suitable for real-time payment settlement yet; complexity for developers
- **Key differentiator**: FHE enables fundamentally different use cases (sealed auctions, private voting, encrypted DeFi state) that ZK alone cannot.

### 4b. Payy's "Demand-First" Approach in Context

Payy's positioning is unique in the competitive landscape. Every other project in this space is either:
- **Infrastructure-first**: Build the privacy layer, then hope developers and users come (Aztec, Aleo, Namada, Zama)
- **Protocol-first**: Ship the smart contract system, then grow organically (Railgun)

Payy is **demand-first**: Ship a consumer product, prove demand, then build the infrastructure underneath it. The 100K users and $130M annualized volume are real traction metrics that no other privacy-focused project can claim.

However, the funding gap is significant:

| Project | Total Funding | Users/Traction |
|---------|--------------|----------------|
| Aleo | $228M | Mainnet + Circle/Paxos partnerships |
| Zama | $150M+ | Mainnet + sealed auction demo |
| Aztec | $119M+ | Alpha testnet; critical vuln |
| Namada | $60M+ | Mainnet; adoption unclear |
| Fhenix | $22M | CoFHE on Arbitrum |
| Railgun | $7M+ | $4.5B cumulative volume, $108M TVL |
| **Payy** | **$6M** | **100K users, $130M annualized volume** |

Payy has the most consumer traction on the least capital. Whether this is a sign of capital efficiency or an underfunding risk depends on execution speed. The testnet and mainnet launches scheduled for 2026 are the critical milestones.

### 4c. The Three-Tier Privacy Framework (from Zcash Research)

Zaki Manian's framework (from our Zcash report) maps the competitive landscape clearly:

**Tier 1 -- Institutional MPC/TEE Privacy**: Tempo, Circle Arc, RCM (Solana). Privacy as a feature, not a right. Backdoors exist. This is where most institutional adoption will land initially.

**Tier 2 -- On-Chain Native Privacy**: Aztec, Aleo, Payy, Railgun, Penumbra. Programmable privacy with genuine cryptographic guarantees. Different trust models and composability tradeoffs.

**Tier 3 -- Cypherpunk-Grade Store of Value**: Zcash, possibly Ethereum (if the EF Mandate's privacy vision is realized). Uncompromised shielded pools.

Payy's positioning spans Tier 1 and Tier 2: it offers Tier 2 cryptographic guarantees (ZK proofs, no trusted setup via Noir/Halo2 lineage) but targets Tier 1 institutional use cases (compliant payments, Visa integration, enterprise settlement).

---

## 5. Key Uncertainties and Data Gaps

The following claims or data points could NOT be independently verified and should be treated with appropriate skepticism:

1. **Payy's exact user count and volume**: The 100K users and $130M annualized volume figures come from press coverage of the seed round. Independent verification from on-chain data or third-party analytics was not available. These are company-reported figures.

2. **ERC-7984 specification**: Referenced in multiple articles about Ethereum's privacy direction but the exact technical specification and current adoption status could not be confirmed from primary sources.

3. **Specific corporate incidents of competitive harm from stablecoin transparency**: The risk is well-documented in principle, but named incidents of companies suffering from on-chain treasury/payment visibility were not found. Companies have no incentive to publicize such incidents.

4. **Comparative proving times across systems**: Benchmark conditions vary significantly between projects. Payy's "sub-0.5s on iPhones" and PLONK's "2.4s" may not be directly comparable due to different circuit complexity, device specifications, and measurement methodologies.

5. **Railgun's $4.5B cumulative volume**: This figure comes from Railgun's own reporting and community analytics. DeFiLlama tracks TVL ($108M) but cumulative volume is harder to independently verify for a privacy protocol (by design).

6. **Aztec's total funding**: Various sources report different figures. The $100M Series B (Dec 2022) and $17M Series A are well-documented, but earlier rounds and grants may add to the total.

7. **FHE computational overhead for payments**: The claim that FHE is "not suitable for real-time payment settlement yet" is based on general performance benchmarks. Specific latency numbers for FHE-based stablecoin transfers were not found.

---

## Sources

### Payy
- [Payy Network - Official Site](https://payy.network/)
- [Payy Architecture Documentation](https://docs.payy.network/payy-network/03_architecture)
- [Payy ZK Proofs Documentation](https://docs.payy.network/payy-network/07_zk_proofs)
- [Payy Whitepaper: L2 Ethereum ZK Rollup for Private and Compliant Transactions](https://polybase.github.io/zk-rollup/whitepaper.pdf)
- [Payy GitHub Repository](https://github.com/polybase/payy)
- [Just write "if": Why Payy left Halo2 for Noir (Aztec blog)](https://aztec.network/blog/just-write-if-why-payy-left-halo2-for-noir)
- [Payy Network Raises $6M in Seed Funding (FinSMEs)](https://www.finsmes.com/2026/03/payy-network-raises-6m-in-seed-funding.html)
- [Stablecoin startup Payy raises $6 million in seed funding (The Block)](https://www.theblock.co/amp/post/395106/stablecoin-startup-payy-funding-private-transactions)
- [Payy Launches As Ethereum's First Privacy-Enabled EVM L2 (Cointelegraph)](https://cointelegraph.com/news/privacy-enabled-evm-l2-payy-launches-ethereum)
- [Payy and Privacy for Stablecoins: The Unlock for Real-World Adoption (Protocol Labs)](https://www.protocol.ai/blog/payy-and-privacy-for-stablecoins/)
- [Ex-Apple Engineer Unveils Privacy-Focused Crypto Visa Card (CoinDesk, Aug 2025)](https://www.coindesk.com/business/2025/08/06/ex-apple-engineer-unveils-privacy-focused-crypto-visa-card)

### Aztec
- [Aztec Network](https://aztec.network/)
- [Aztec Launches Alpha Network (The Defiant)](https://thedefiant.io/news/blockchains/aztec-launches-alpha-network-ethereum-s-first-l2-for-private-smart-contracts)
- [What Is Aztec Network (CoinGecko)](https://www.coingecko.com/learn/what-is-aztec-network-ethereum-privacy-layer-2)

### Railgun
- [RAILGUN - Official Site](https://www.railgun.org/)
- [RAILGUN Privacy System Wiki](https://docs.railgun.org/wiki/learn/privacy-system)
- [Railgun TVL, Fees & Revenue (DeFiLlama)](https://defillama.com/protocol/railgun)
- [Railgun DeFi Privacy Guide (Flashift)](https://flashift.app/blog/railgun-explained-a-deep-dive-into-the-future-of-zk-privacy/)

### Aleo
- [Aleo and Paxos Labs Launch Privacy-Focused Digital Dollar USAD (CoinDesk, Oct 2025)](https://www.coindesk.com/business/2025/10/01/aleo-and-paxos-labs-launch-privacy-focused-dollar-stablecoin-aimed-at-institutions)
- [Circle Stablecoin for Banking-Level Privacy to Launch on Aleo (Fortune, Dec 2025)](https://fortune.com/2025/12/09/circle-privacy-stablecoin-aleo-udsc-udscx/)
- [Aleo, Toku, and Paxos Labs Launch First Private Stablecoin Payroll Solution (BusinessWire, Jan 2026)](https://www.businesswire.com/news/home/20260129619369/en/Aleo-Toku-and-Paxos-Labs-Launch-First-Private-Stablecoin-Payroll-Solution-Removing-the-Final-Barrier-to-Enterprise-Stablecoin-Adoption)
- [Aleo Funding and Investors (Tracxn)](https://tracxn.com/d/companies/aleo/__RacRfD0m2MMCL1W0r84AYItrD3voiquV86feVcv6RwI/funding-and-investors)

### Namada & Penumbra
- [Namada - Official Site](https://namada.net/)
- [What Is Namada (MEXC Blog)](https://blog.mexc.com/what-is-nam-namada/)
- [Privacy in the Cosmos (Medium)](https://medium.com/cosmosafrica/privacy-in-the-cosmos-how-penumbra-namada-and-secret-network-are-innovating-confidential-defi-0a51fbf8b0d0)

### FHE Projects
- [Web3 Privacy Infrastructure in 2026: ZK, FHE, and TEE (BlockEden)](https://blockeden.xyz/blog/2026/02/04/web3-privacy-infrastructure-zk-fhe-tee-reshaping-blockchain/)
- [The Privacy Stack Wars: ZK vs FHE vs TEE vs MPC (BlockEden)](https://blockeden.xyz/blog/2026/01/27/privacy-infrastructure-zk-fhe-tee-mpc-comparison-benchmarks/)
- [Blockchains Privacy Stages: A Taxonomy (Fhenix)](https://www.fhenix.io/blog/the-different-stages-of-privacy-a-taxonomy)

### Market Context
- [Stablecoin Funding Rounds (FXC Intel, March 2026)](https://www.fxcintel.com/research/analysis/stablecoin-funding-march-2026)
- [How the Stablecoin Landscape Is Shifting in 2026 (Kavout)](https://www.kavout.com/market-lens/how-is-the-stablecoin-landscape-shifting-in-2026)
- [Stablecoin Issuance Infrastructure in 2026 (DeFi Prime)](https://defiprime.com/stablecoin-issuance-infrastructure-2026)

### Privacy Risks and Surveillance
- [TRM Labs 2026 Crypto Crime Report](https://www.trmlabs.com/reports-and-whitepapers/2026-crypto-crime-report)
- [Chainalysis 2025 Crypto Crime Report](https://go.chainalysis.com/2025-Crypto-Crime-Report.html)
- [Crypto Surveillance in 2025 (Yellow.com)](https://yellow.com/research/crypto-surveillance-in-2025-how-chainalysis-the-fbi-and-ai-track-your-wallet)
- [Privacy Stablecoins 101: Enterprise Payments (Quecko/Medium)](https://quecko.medium.com/privacy-stablecoins-101-why-the-future-of-enterprise-payments-needs-privacy-32aa56792c7c)
- [Chainalysis: Stablecoin Security Risks](https://www.chainalysis.com/blog/stablecoin-security-risks/)

### Ethereum Foundation Privacy Direction
- [Ethereum Foundation Publishes Formal Mandate (crypto.news)](https://crypto.news/ethereum-foundation-publishes-formal-mandate-to-hard-lock-censorship-resistance-and-privacy/)
- [Vitalik Buterin Signals New Era for Ethereum (CryptoTimes)](https://www.cryptotimes.io/2026/03/13/vitalik-buterin-signals-new-era-for-ethereum-with-foundation-mandate/)
- [Ethereum's 2026 Roadmap Puts Institutional Privacy Front and Center (AMBCrypto)](https://ambcrypto.com/ethereums-2026-roadmap-puts-institutional-privacy-front-and-center-details/)

### ZK Proof Systems
- [Zero-Knowledge Proof Performance Benchmarks (StealthCloud)](https://stealthcloud.ai/data/zero-knowledge-proof-performance-benchmarks/)
- [An Opinionated Overview of ZK Tooling (Aayush Gupta)](https://blog.aayushg.com/zk/)
- [Halo 2 Explained (Electric Coin Company)](https://electriccoin.co/blog/explaining-halo-2/)

### Cross-Reference: Existing Repository Reports
- State of Zcash: March 2026 (privacy/state-of-zcash-march-2026.md)
- State of MEV: March 2026 (dexes/state-of-mev-march-2026.md)
- State of Intents: 2026 (defi/state-of-intents-2026.md)
