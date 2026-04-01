# The Critical Unsolved Problems in Crypto Payments

**Deep Dive Research Brief | March 31, 2026**

**tl;dr**

- **Fiat on/off ramps remain the largest friction point**, with fees ranging from 1% (bank transfer) to 4.5% (credit card) and KYC/licensing requirements creating a moat that only well-capitalized players can cross. Stripe/Bridge's federal banking charter is a paradigm shift.
- **Travel Rule compliance is technically solvable but operationally fragmented** -- 85 of 117 FATF jurisdictions have passed or are drafting legislation, but interoperability between messaging protocols (TRISA, OpenVASP, Notabene) remains incomplete. The compliance tech market is projected to hit $4.87B by 2028.
- **Merchant acceptance is accelerating but still niche** -- 75% of US retailers plan to accept crypto by 2026 (Deloitte), yet actual integration requires solving settlement, accounting, tax, and chargeback problems simultaneously.
- **Accounting and tax treatment of stablecoins is unresolved at the standards level** -- FASB excluded stablecoins from ASU 2023-08 and is only now (late 2025) studying whether they qualify as cash equivalents. This is a bigger blocker than most people realize.
- **The chargeback problem is existential for consumer adoption** -- no standardized dispute resolution exists, and smart contract escrow remains experimental and fragmented.
- **Cross-chain fragmentation is being addressed through intents (ERC-7683) and native burn/mint (CCTP V2)**, but the user experience is still poor for non-technical users.
- **Privacy is the missing layer for payments legitimacy** -- Payy, Railgun, Aztec, and Circle Arc represent four distinct architectural approaches, none yet proven at scale.

---

## Table of Contents

1. [The Fiat On/Off Ramp Problem](#1-the-fiat-onoff-ramp-problem)
2. [The Compliance and Travel Rule Problem](#2-the-compliance-and-travel-rule-problem)
3. [The Merchant Acceptance Problem](#3-the-merchant-acceptance-problem)
4. [The Accounting and Tax Problem](#4-the-accounting-and-tax-problem)
5. [The Dispute Resolution / Chargeback Problem](#5-the-dispute-resolution--chargeback-problem)
6. [The Interoperability Problem](#6-the-interoperability-problem)
7. [Privacy -- Where It Fits in the Stack](#7-privacy----where-it-fits-in-the-stack)
8. [Synthesis: The Interconnected Nature of These Problems](#8-synthesis-the-interconnected-nature-of-these-problems)

---

## 1. The Fiat On/Off Ramp Problem

### What It Actually Takes

Converting fiat to stablecoins (and back) requires navigating a stack of financial regulation, banking relationships, payment processing, and blockchain infrastructure. The provider must:

- Obtain money transmitter licenses (in the US, this means 50 state-level licenses plus federal FinCEN registration) or partner with a licensed entity
- Establish banking relationships (increasingly difficult -- "debanking" of crypto firms has been a persistent problem)
- Implement KYC/AML procedures compliant with BSA, FATF, and local regulations
- Build or integrate payment rails (ACH, SEPA, Faster Payments, card networks)
- Manage liquidity across fiat and crypto
- Handle fraud and chargebacks on the fiat side while managing irreversible transactions on the crypto side

### Major Providers and Their Fee Structures

| Provider | Credit/Debit Card Fee | Bank Transfer Fee | Off-Ramp Fee | Model | Key Differentiator |
|---|---|---|---|---|---|
| **MoonPay** | 4.5% (min ~$3.99) | 1% (min ~$3.99) | Varies | B2C + B2B API | 30M+ customers, 180 countries, AI agent wallets (Feb 2026) |
| **Transak** | ~1%+ (varies) | ~1% | 1% of withdrawal | B2B API | MetaMask exclusive partner (Sep 2025), $16M raise from Tether (Aug 2025) |
| **Ramp Network** | 0.49-2.9% | 0.49-2.9% | Varies | B2B API, non-custodial | 150+ countries, min fee of ~2.49 EUR |
| **Sardine** | Varies | Varies | Varies | B2B infrastructure | Full-stack: MoR, KYC, fraud liability, payment auth |
| **Zero Hash** | N/A (B2B2C) | N/A (B2B2C) | N/A | B2B2C infra | 0.146% implied take rate, crypto-as-a-service |
| **Bridge (Stripe)** | Stripe pricing | Stripe pricing | Stripe pricing | B2B API + enterprise | $1.1B acquisition, OCC conditional national trust bank charter (Feb 2026) |

*Note: Fees vary by region, payment method, transaction size, and partnership terms. The figures above represent published rates as of early 2026.*

### Why This Is Still the #1 Bottleneck

1. **Regulatory moat**: The licensing burden means only well-capitalized companies can play. A full US money transmitter license portfolio takes 12-24 months and millions in legal/compliance costs.
2. **Banking relationship fragility**: Even licensed providers lose banking partners regularly. The fiat side of the equation is controlled by traditional banks who remain skeptical of crypto.
3. **Fee compression is slow**: Credit card on-ramps at 3-5% make stablecoins uncompetitive with traditional payment methods for most use cases. Bank transfers at 1% are more competitive but slower.
4. **Off-ramp friction is worse than on-ramp**: Getting money back into a bank account remains slower, more expensive, and more compliance-heavy than buying crypto.
5. **Geographic fragmentation**: A provider might work seamlessly in the US and EU but have zero coverage in Southeast Asia or Latin America, where stablecoin demand is highest.

### How Different Teams Are Solving It

**Stripe/Bridge** represents the most significant structural shift. The $1.1B acquisition of Bridge by Stripe (completed Feb 2025), combined with Bridge's conditional OCC national trust bank charter (Feb 2026), means Stripe can now offer stablecoin on/off ramps as a native feature of its payment infrastructure -- which already processed $1.9T in 2025. Bridge's transaction volume quadrupled in 2025. Stripe launched Open Issuance (Oct 2025), letting any business launch its own stablecoin. This is not just another on-ramp; it is the integration of stablecoin rails into the dominant internet payments platform.

**Transak's Tether investment** ($16M, Aug 2025) and exclusive MetaMask partnership (Sep 2025, access to 100M+ users) represent a bet on wallet-native on-ramping at near 1:1 rates for stablecoins.

**MoonPay's enterprise stablecoin services** (Nov 2025) and AI agent integration (Feb 2026) show a pivot from consumer crypto purchases toward institutional stablecoin infrastructure.

**Zero Hash's embedded model** allows any fintech to offer crypto without becoming a crypto company -- the "Stripe for crypto" before Stripe itself entered.

### Key Insight

The on/off ramp problem is converging toward two models: (a) embedded infrastructure (Zero Hash, Bridge/Stripe) where the ramp disappears into existing financial products, and (b) wallet-native ramps (Transak/MetaMask) where users buy stablecoins as naturally as topping up a prepaid card. The standalone "buy crypto" widget is becoming a commodity.

---

## 2. The Compliance and Travel Rule Problem

### How FATF Travel Rule Works in Practice

The FATF Travel Rule (Recommendation 16, revised June 2025) requires Virtual Asset Service Providers (VASPs) to collect, verify, and transmit originator and beneficiary information for virtual asset transfers. In practice:

- **Threshold triggers**: Transfers above $1,000/EUR 1,000 require full Travel Rule data (name, address, date of birth for individuals)
- **Data exchange**: The originating VASP must transmit this information to the beneficiary VASP *before or simultaneously with* the transaction
- **Unhosted wallets**: Transfers to/from non-custodial wallets create a compliance gap -- there is no counterparty VASP to send data to
- **Chain-specific challenges**: Different chains have different transaction models, confirmation times, and metadata capabilities

### Who Is Building Compliance Infrastructure

| Provider | Role | Key Capability | Integration Model |
|---|---|---|---|
| **Notabene** | Travel Rule messaging | VASP-to-VASP data exchange, counterparty discovery | API, integrates with Chainalysis |
| **Chainalysis** | Blockchain analytics | KYT (Know Your Transaction), wallet attribution, risk scoring | API + dashboard |
| **TRM Labs** | Blockchain analytics | Transaction monitoring, VASP due diligence, sanctions screening | API + dashboard |
| **Elliptic** | Blockchain analytics + Travel Rule | Holistic screening, wallet risk scoring, Travel Rule messaging | API + dashboard |
| **Sygna** | Travel Rule messaging | VASP-to-VASP information exchange, sanctions screening | API, 4-6 week integration |
| **CipherTrace (Mastercard)** | Blockchain analytics | Transaction monitoring, acquired by Mastercard 2021 | Enterprise integration |

### The Messaging Protocol Fragmentation Problem

Three dominant Travel Rule messaging protocols exist, and they do not fully interoperate:

- **TRISA** (Travel Rule Information Sharing Architecture) -- open-source, decentralized
- **OpenVASP** -- open protocol, community-driven
- **Notabene** -- commercial solution, largest network

According to Notabene's 2025 State of Crypto Travel Rule Compliance Report, 100% of surveyed VASPs stated they would be Travel Rule compliant by end of 2025. However, "compliant" means different things in different jurisdictions, and the interoperability problem means a VASP on TRISA cannot necessarily exchange data with a VASP on OpenVASP.

### What Travel Rule Compliance Actually Costs

Specific per-transaction cost data is not publicly available from most providers. What is known:

- The blockchain analytics/compliance tech market was worth **$2.14 billion in 2025** and is projected to reach **$4.87 billion by 2028**
- Integration typically takes **4-6 weeks** (Sygna) to several months depending on complexity
- Ongoing costs include per-transaction screening fees (typically charged per API call to analytics providers), annual platform licensing, and dedicated compliance staff
- For smaller VASPs, compliance costs can represent a significant percentage of revenue, creating a barrier to entry that favors larger, established exchanges

### How Different Chains Handle This

- **Ethereum**: Most mature compliance tooling ecosystem. Chainalysis, TRM, Elliptic all have deep Ethereum coverage. However, smart contract interactions create complexity for transaction monitoring.
- **Tron**: Carries ~50%+ of USDT volume. Compliance tooling coverage is less mature than Ethereum despite higher stablecoin throughput. Tron's association with illicit finance (USDT on Tron has been flagged repeatedly by compliance firms) creates additional risk.
- **Solana**: Growing compliance coverage, but the high transaction throughput and account model (vs. UTXO) create different challenges for analytics providers.

### The Gap

The fundamental gap is this: **stablecoin transfers on public blockchains are pseudonymous by default, but compliance requires identity attribution.** The Travel Rule assumes a model where both sides of a transaction are VASPs with KYC'd customers. In reality:

- Peer-to-peer transfers between non-custodial wallets have no VASP on either side
- DeFi protocol interactions have no VASP on the receiving side
- Cross-chain transfers may involve VASPs in different jurisdictions with different thresholds and data requirements
- The June 2025 FATF revision expanded the Travel Rule's scope to include fraud prevention and proliferation financing, adding new data requirements

This creates a structural tension: the more compliant the system becomes, the more it resembles traditional finance -- and the less clear the value proposition of using blockchain rails in the first place.

---

## 3. The Merchant Acceptance Problem

### Why Merchants Don't Accept Stablecoins (Yet)

Despite the headline statistic from Deloitte (2024) that 75% of US retailers plan to accept crypto by 2026, actual merchant acceptance of stablecoins remains limited. The reasons are interconnected:

1. **No compelling economic incentive**: Card network interchange fees are 1.5-3.5%, which is painful but understood. Stablecoin payment processing fees are potentially lower, but the total cost of acceptance (integration, accounting, compliance, training) eliminates the savings.
2. **Settlement complexity**: Merchants need fiat in their bank accounts to pay suppliers, rent, and employees. Accepting stablecoins means adding an off-ramp step unless the entire supply chain accepts stablecoins (which it doesn't).
3. **Accounting nightmare**: Current FASB rules do not clearly classify stablecoins (see Section 4), making bookkeeping for stablecoin-accepting merchants significantly more complex.
4. **No chargeback protection**: Consumers are accustomed to card network protections. Merchants accepting stablecoins take on the burden of handling disputes manually (see Section 5).
5. **POS integration gap**: Existing point-of-sale systems (Square, Clover, Toast, etc.) do not natively support stablecoin acceptance. Integration requires additional hardware/software and staff training.
6. **Consumer demand is thin**: The percentage of consumers who *want* to pay with stablecoins at a retail store is still small relative to card users.

### What a Merchant Needs to Accept Stablecoin Payments

A complete merchant acceptance stack requires:

- **Payment gateway**: Convert stablecoin payment into a merchant's preferred settlement currency (NOWPayments, Coinbase Commerce, BitPay, Flexa)
- **POS integration**: Hardware/software that can generate payment requests, display QR codes, confirm on-chain settlement
- **Instant settlement or conversion**: Most merchants want fiat settlement. This requires real-time off-ramping.
- **Accounting integration**: Transactions must flow into the merchant's accounting system (QuickBooks, Xero, NetSuite) with proper categorization
- **Tax reporting**: Every stablecoin transaction may be a taxable event (IRS treats all crypto as property)
- **Fraud/dispute handling**: Some mechanism for refunds and dispute resolution
- **Compliance**: Depending on jurisdiction, the merchant may have reporting obligations for crypto transactions

### Who Is Building Merchant Infrastructure

| Provider | Model | Merchant Base | Stablecoin Support | Settlement |
|---|---|---|---|---|
| **Flexa** | Collateralized payment network (AMP token) | 660+ Bealls stores, expanding | Yes | Instant fiat settlement |
| **BitPay** | Payment gateway | Established, thousands of merchants | USDC, BUSD, others | Fiat settlement (bank deposit) |
| **Coinbase Commerce** | Payment gateway (merging into Coinbase Business by Mar 2026) | Mid-market, e-commerce focused | USDC native | Fiat or crypto settlement |
| **NOWPayments** | Payment gateway | SMB-focused, 200+ coins | USDT, USDC, others | Fiat or crypto settlement |
| **Strike** | Bitcoin/Lightning focused | Retail partnerships | Limited stablecoin focus | Fiat settlement via Lightning |
| **Stripe (via Bridge)** | Native payment integration | Millions of existing Stripe merchants | USDC, others via Bridge | Native fiat settlement |

### What Stripe's Stablecoin Play Means

This is the most consequential development in merchant crypto payments. Stripe already powers payments for millions of merchants. By integrating Bridge's stablecoin infrastructure directly into its payment platform:

- Merchants do not need to "accept crypto" -- they accept payments via Stripe, and Stripe handles stablecoin-denominated flows behind the scenes
- Settlement happens in fiat, in the merchant's existing Stripe account
- Bridge's Visa card partnership means stablecoin balances can be spent anywhere Visa is accepted, completing the loop
- Stripe launched stablecoin payments for subscriptions in 2025, signaling SaaS/recurring revenue businesses as an early target

**The key metric**: Stripe processed $1.9T in 2025 (up 34% YoY). If even 1% of that shifts to stablecoin rails, that is $19B in stablecoin payment volume flowing through a single provider -- more than most crypto payment processors combined.

### Adoption Data Points

- B2B stablecoin payments grew from <$100M/month in early 2023 to >$3B/month by 2025
- Bitcoin accounts for ~42% of merchant crypto transactions; USDT accounts for ~30-35%
- US crypto payment adoption projected to surge 82.1% between 2024 and 2026
- The global crypto payments market is projected to grow from $1.35B (2025) to $9.86B (2033) at 27.8% CAGR
- 81% of crypto-aware SMBs expressed interest in using stablecoins (2025 survey cited by Stripe)

---

## 4. The Accounting and Tax Problem

### The Current State of Stablecoin Accounting

This is a bigger blocker than most people in crypto realize, because it affects every business that touches stablecoins, not just merchants.

**FASB ASU 2023-08 (effective December 2024)**: This was the first dedicated crypto accounting standard. It requires fair value measurement for crypto assets. However, **stablecoins are explicitly excluded** from its scope because they are designed to maintain a stable value relative to a fiat currency, and the FASB determined they did not fit the definition of "crypto assets" under the standard.

**So how do businesses account for stablecoins?** There is no clear answer:

- They could be classified as **intangible assets** (carried at fair value or subject to impairment)
- They could be classified as **financial instruments** (if they represent a claim on the issuer)
- They could potentially qualify as **cash equivalents** (if they meet the definition of being readily convertible to known amounts of cash with insignificant risk of value change)

**The GENIUS Act (signed July 2025)** complicates and clarifies simultaneously. By requiring 1:1 backing with highly liquid assets and clear redemption rights, regulated payment stablecoins may more readily qualify as cash equivalents. But FASB has not yet issued guidance confirming this treatment.

**FASB's current work (late 2025 - 2026)**: FASB launched a project in late October 2025 to study whether stablecoins can be classified as cash equivalents. Bloomberg Tax has noted that defining stablecoins is one of the most important things FASB needs to do in 2026. As of March 2026, no final guidance has been issued.

### IRS Treatment

The IRS classifies **all virtual currencies, including stablecoins, as property** (Notice 2014-21, reinforced by subsequent guidance). This means:

- Every stablecoin transaction is potentially a taxable event
- If a business receives USDC as payment and the USDC's value at the time of receipt differs from its value when later converted to fiat (even by fractions of a cent due to minor depegs), there is a reportable gain or loss
- Businesses must track cost basis for every stablecoin unit
- Payments in stablecoins to contractors or employees have withholding and reporting obligations

For a business doing thousands of stablecoin transactions per month, this creates an enormous record-keeping burden that does not exist for fiat transactions.

### ERP Integration Landscape

| Provider | Target Market | ERP Integrations | Key Capability |
|---|---|---|---|
| **Bitwave** | Enterprise | NetSuite, SAP, QuickBooks, Xero | 200+ blockchain/exchange/DeFi integrations, GAAP/IFRS compliant |
| **TaxBit** | Enterprise + government | Major ERPs via API | SEC-grade, Big Four validated, high-volume multi-chain |
| **Lukka** | Enterprise | Enterprise ERPs | Proprietary data taxonomy, GAAP/IFRS compliance |
| **CoinTracker** | SMB to enterprise | QuickBooks, others | Portfolio tracking + tax reporting, multi-jurisdiction |
| **SAP Digital Currency Hub** | Large enterprise | SAP ERP natively | B2B stablecoin payments, smart contract-triggered settlement |

### Why This Is a Bigger Blocker Than People Realize

1. **CFOs will not approve stablecoin adoption without clear accounting treatment**. The ambiguity in FASB guidance means that a company's auditors may disagree with their accounting treatment, creating audit risk.
2. **Tax compliance costs can exceed the savings from using stablecoins**. The per-transaction tax tracking burden for property-classified assets is significant.
3. **ERP integration is immature**. While Bitwave and SAP Digital Currency Hub exist, most mid-market accounting systems have zero native stablecoin support.
4. **The GENIUS Act may resolve this** by creating a regulatory framework where compliant stablecoins can be treated as cash equivalents, but this depends on FASB issuing corresponding accounting guidance -- which has not happened as of March 2026.
5. **International complexity**: IFRS treatment of stablecoins differs from US GAAP, creating additional complexity for multinational businesses.

---

## 5. The Dispute Resolution / Chargeback Problem

### The Core Problem

Blockchain transactions are final and irreversible. There is no central authority that can reverse a stablecoin transfer after confirmation. This is by design -- immutability is a feature, not a bug, from a technical perspective. But from a commerce perspective, it creates a fundamental gap in consumer protection.

Traditional card networks (Visa, Mastercard) provide:
- **Chargeback rights**: Consumers can dispute unauthorized or fraudulent charges
- **Purchase protection**: Coverage for goods not received, not as described, or defective
- **Zero liability policies**: Consumers are not liable for unauthorized transactions
- **Merchant accountability**: Card networks can fine, penalize, or delist merchants who abuse the system

None of these protections exist natively in stablecoin payments.

### Current State of Solutions

**Smart Contract Escrow**: Funds are held in a smart contract that releases payment only when predefined conditions are met (e.g., delivery confirmation, time-lock expiry). This is the most technically mature approach but:
- Requires both parties to agree on escrow terms upfront
- Smart contract bugs can lock or lose funds
- No standardized escrow contracts exist across protocols
- Dispute resolution still requires a human or algorithmic arbiter

**Decentralized Arbitration**: Projects like Kleros provide decentralized dispute resolution through juror staking and voting mechanisms. However:
- Adoption is minimal
- Response times are slow compared to card network disputes (which resolve in days to weeks)
- The "jury" has limited ability to investigate facts (no subpoena power, no access to shipping records, etc.)

**Platform-Level Refund Policies**: Some crypto payment processors (e.g., BitPay, NOWPayments) offer voluntary refund mechanisms, but these depend on the merchant's willingness to cooperate and the processor's policies, not on enforceable consumer rights.

**The GENIUS Act Impact**: The GENIUS Act (July 2025) classified stablecoin issuers as financial institutions, introducing new consumer protection standards. However, the Act primarily addresses *issuer-level* protections (reserve requirements, redemption rights) rather than *transaction-level* dispute resolution.

### Why This Matters for Enterprise Adoption

Enterprises considering stablecoin payments face a fundamental question: **who bears the liability for fraud and disputes?**

- In card payments, the card network and issuing bank absorb significant fraud liability
- In stablecoin payments, the merchant or the consumer absorbs all risk
- For B2B payments, this is less critical (businesses have existing dispute resolution mechanisms through contracts and courts)
- For B2C payments, this is a dealbreaker for most mainstream commerce

**The gap**: There is no equivalent of Visa's dispute resolution system for stablecoin payments. No one is building this at scale. The closest analogs are platform-specific policies (like Stripe's buyer protection, which could theoretically extend to stablecoin transactions processed through Stripe) and smart contract escrow systems that remain experimental.

### What Would Need to Exist

A viable stablecoin dispute resolution system would need:
1. **Standardized escrow contracts** that are audited, composable, and widely adopted
2. **An arbitration network** with binding authority and reasonable response times
3. **Insurance or reserve pools** to cover losses from fraud
4. **Integration with existing legal frameworks** so that dispute resolution has enforceable real-world consequences
5. **Consumer-facing UX** that makes the process as simple as filing a credit card dispute

This is a massive unsolved problem and a significant opportunity.

---

## 6. The Interoperability Problem

### Are USDC on Ethereum, Solana, and Base the Same?

No, and yes. They represent the same claim on Circle's reserves, but they are technically different tokens on different blockchains. A merchant who accepts USDC on Ethereum cannot directly accept USDC on Solana -- the tokens are not interchangeable without a bridging or transfer mechanism.

This creates real problems:
- A user holding USDC on Arbitrum cannot pay a merchant accepting USDC on Base without a cross-chain operation
- Liquidity is fragmented across chains, meaning exchange rates and fees can differ
- Applications built on one chain cannot natively interact with stablecoins on another chain

### Cross-Chain Transfer Protocols

| Protocol | Mechanism | Supported Chains | Speed | Fees | Status |
|---|---|---|---|---|---|
| **Circle CCTP V2** | Native burn/mint (USDC only) | Expanding (V1 had 11 chains) | Minutes | Minimal (gas only) | Canonical standard, V1 legacy phase-out begins July 31, 2026 |
| **Chainlink CCIP** | Oracle-validated messaging + CCTP integration | 20+ chains | Minutes | Per-message + gas | Production, supports USDC via CCTP |
| **Wormhole** | Guardian network validation | 30+ chains | Seconds to minutes | <$0.01 typical | Production, largest chain coverage |
| **Across Protocol** | Intent-based execution with UMA optimistic oracle | Ethereum + L2s primarily | <1 minute typical | Sub-dollar for stablecoins | Production, dominant on Ethereum L2 routes |
| **deBridge** | Intent-based, decentralized | Multi-chain | ~2 seconds | Competitive | Production |

### ERC-7683: Cross-Chain Intents Standard

Developed by Uniswap and Across Protocol, ERC-7683 is a standardized way to express cross-chain intents. The core idea:

- **Users declare what they want** (e.g., "I want to send 100 USDC from Arbitrum to Base") rather than specifying how
- **Solvers compete to fill the intent**, finding the best route, price, and speed
- **A standardized order structure** means any solver can fill any intent, creating competition and better prices
- **Settlement is handled by the protocol**, with solvers fronting capital and being reimbursed on the source chain

What this solves:
- Users do not need to know which chain their counterparty is on
- Liquidity fragmentation is partially addressed by solver competition
- The UX can be abstracted to "send money to this address" without chain selection

What this does not solve:
- Non-EVM chains (Solana, etc.) are not natively supported
- The solver network must have sufficient liquidity and geographic coverage
- Settlement finality depends on the underlying chains
- Gas costs on Ethereum L1 still make small transfers expensive (though L2s mitigate this)

### Chain Abstraction vs. Bridge Thesis

These are converging rather than competing:

**Bridge thesis**: Build better, faster, cheaper bridges between specific chain pairs. Winners are defined by liquidity depth and route coverage. This is the Across, Wormhole, Stargate approach.

**Chain abstraction thesis**: Make chains invisible to the user. Wallets and applications route transactions automatically across chains using intents, account abstraction, and smart routing. By 2026, this includes tools like Particle Network, Aarc, Socket, and wallet-native routing in MetaMask, Coinbase Wallet, etc.

**The convergence**: ERC-7683 and intent-based systems are the bridge between these theses. Bridges become the execution layer; chain abstraction becomes the UX layer. The user never sees a "bridge" -- they just send money.

**The remaining problem**: This works well within the EVM ecosystem. Cross-ecosystem transfers (EVM to Solana, EVM to Bitcoin, etc.) remain significantly more complex, slower, and more expensive. Circle's CCTP V2 is the closest to a universal stablecoin solution, but it only covers USDC.

---

## 7. Privacy -- Where It Fits in the Stack

### At Which Layer(s) Does Privacy Matter Most?

Privacy is relevant at every layer of the payments stack, but the problems are different at each:

| Layer | Privacy Problem | Why It Matters for Payments |
|---|---|---|
| **Transaction layer** | Sender, recipient, amount are publicly visible on-chain | Competitors can see a business's payment flows; salary payments are exposed; purchase history is public |
| **Identity layer** | Wallet addresses are pseudonymous but linkable to real identities through on/off ramps, ENS, etc. | Once one transaction is linked to an identity, the entire wallet history is exposed |
| **Application layer** | DeFi interactions, payment metadata, and behavioral patterns are visible | Payment patterns reveal business relationships, pricing strategies, and financial health |
| **Network layer** | IP addresses and transaction broadcast patterns can be correlated | Sophisticated attackers can link transactions to physical locations |

**For payments specifically**, the transaction layer is the most critical. A business paying suppliers in USDC on a public blockchain is broadcasting its entire supply chain, payment amounts, and timing to anyone who can read a block explorer. This is unacceptable for most enterprises.

### Who Is Building What

**Payy (L2 on Ethereum)**
- Architecture: Privacy-focused L2 using Halo2 ZK proofs (same framework as Zcash)
- Approach: Default privacy -- all transactions shield sender, recipient, and amount
- Compliance: KYC at wallet level; on-chain activity not correlatable to identity by analytics firms
- Status: 100,000+ users across 120 countries, ~$130M annualized transaction volume, mainnet planned for summer 2026
- Funding: $6M seed (Dec 2025, led by FirstMark Capital)
- Key innovation: Privacy-focused Visa card (Aug 2025, ex-Apple engineer founder)

**Railgun (L1 smart contract on Ethereum)**
- Architecture: On-chain privacy system using ZKPs, deployed on Ethereum, Arbitrum, Polygon, BNB Chain
- Approach: Users "shield" tokens into a shared privacy pool, then transact privately within the pool
- Compliance: "Privacy Pools" / "Proofs of Innocence" -- users can cryptographically prove funds did not originate from known illicit sources without revealing transaction history
- Status: Production. Vitalik Buterin moved $2.6M through Railgun (Jun 2025). Ethereum Foundation staked 50,000 RAIL (May 2025)
- Key innovation: Composable with DeFi -- private swaps, private lending, etc. on existing chains

**Aztec Network (Programmable Privacy L2)**
- Architecture: Ethereum L2 with full private execution environment using ZK-SNARKs
- Approach: Three privacy pillars -- data, identity, and compute. Developers choose what is public vs. private.
- Status: Ignition Chain launched on Ethereum mainnet (Nov 2025). 185+ operators across 5 continents, 3,400+ sequencers. Token sale completed Dec 2025. Transactions expected to go live early 2026.
- Compliance: Programmable -- developers can build selective disclosure into applications
- Key innovation: First L2 with a full execution environment for private smart contracts. Not just private transfers but private *computation*.

**Circle Arc (Stablecoin-Native L1)**
- Architecture: Purpose-built L1 for stablecoins using TEEs (Trusted Execution Environments) with path to MPC and ZK proofs
- Approach: Selective privacy -- confidential transfers shield amounts while keeping addresses visible. "View keys" for authorized auditors/regulators.
- Status: Private testnet (2025), public testnet fall 2025, mainnet beta 2026
- Compliance: Built by the USDC issuer -- compliance is architecturally native
- Key innovation: Built-in RFX (Request-for-Quote) foreign exchange engine for atomic stablecoin-to-stablecoin swaps (e.g., USDC to EURC)

**Stripe Tempo (Rumored/In Development)**
- Architecture: Purpose-built blockchain for stablecoin payments, reportedly 100,000+ TPS with sub-second finality
- Status: Details limited; reported as Stripe's answer to Circle Arc
- Approach: High-throughput payment chain; privacy details unclear

### What "Compliant Privacy" Actually Looks Like

The emerging consensus model has several components:

1. **KYC at the on-ramp**: Identity verification happens when fiat enters the system (wallet creation, exchange deposit)
2. **Privacy in transit**: Transactions between verified participants are shielded on-chain
3. **Selective disclosure**: Authorized parties (regulators, auditors, counterparties with consent) can verify specific transactions using view keys or proofs
4. **Proof of innocence/exclusion**: Users can prove their funds are NOT from a specific bad set (e.g., OFAC-sanctioned addresses) without revealing the actual source

This is the model that Payy, Railgun (via Privacy Pools), and Circle Arc are all converging on, from different directions.

### The Anonymity Set Problem

For privacy in payments, the anonymity set -- the number of possible senders/recipients that a transaction could belong to -- is critical.

- **Too small**: If only 10 people use a private payment system, de-anonymization through statistical analysis is trivial
- **Too large and diverse**: If the anonymity set includes known illicit actors, legitimate users are "tainted by association" from a compliance perspective
- **The bootstrapping problem**: New privacy systems start with small anonymity sets, making them less private, which discourages adoption, which keeps the anonymity set small

**Railgun** addresses this through its Privacy Pools model, where the anonymity set is filtered -- users can prove inclusion in a "clean" subset. **Payy** addresses it through wallet-level KYC, so the entire anonymity set is pre-vetted. **Aztec** leaves it to application developers to design appropriate privacy boundaries.

For payments specifically, the ideal is a large anonymity set of KYC'd participants -- which requires significant adoption before the privacy guarantees become meaningful. This is a chicken-and-egg problem that none of these projects have fully solved yet.

---

## 8. Synthesis: The Interconnected Nature of These Problems

These seven problems are not independent. They form a dependency graph:

```
Merchant Adoption
    |
    +-- requires --> Fiat On/Off Ramps (for settlement)
    +-- requires --> Accounting/Tax Clarity (for bookkeeping)
    +-- requires --> Dispute Resolution (for consumer trust)
    +-- requires --> Interoperability (to accept from any chain)
    |
Fiat On/Off Ramps
    +-- requires --> Compliance/Travel Rule (for licensing)
    +-- requires --> Banking Relationships (for fiat rails)
    |
Enterprise Adoption
    +-- requires --> Privacy (for business confidentiality)
    +-- requires --> Compliance (for regulatory approval)
    +-- requires --> Accounting/Tax Clarity (for CFO sign-off)
    +-- requires --> Dispute Resolution (for risk management)
```

**No single problem can be solved in isolation.** A merchant cannot adopt stablecoin payments unless the accounting is clear, the off-ramp works, disputes can be resolved, and the privacy is sufficient for business use. This is why, despite years of effort, stablecoin payments remain a fraction of total payment volume.

**The Stripe/Bridge thesis** is significant because it is the first serious attempt to solve the entire stack simultaneously -- ramps, compliance, merchant integration, settlement -- within a single platform that already has distribution.

### The Biggest Remaining Gaps (Opportunity Map)

1. **Dispute resolution / consumer protection**: The least-addressed problem with the highest impact on consumer adoption. No serious, scaled solution exists.
2. **Stablecoin accounting clarity**: Dependent on FASB action, but tooling to make compliance easy once standards are set is still immature for mid-market businesses.
3. **Privacy for business payments**: Enterprise adoption is blocked until businesses can make payments without broadcasting their financial operations publicly. The solutions exist architecturally (Payy, Railgun, Arc) but are not yet at scale.
4. **Cross-ecosystem interoperability**: EVM-to-EVM is converging on solutions (ERC-7683, CCTP). EVM-to-Solana-to-Tron remains fragmented.
5. **The compliance UX gap**: Travel Rule compliance works at the exchange-to-exchange level but breaks down for any transaction involving non-custodial wallets, DeFi, or peer-to-peer transfers.

---

## Sources

### Section 1: Fiat On/Off Ramps
- [MoonPay Pricing Disclosure](https://www.moonpay.com/legal/pricing_disclosure)
- [MoonPay Fees Support](https://support.moonpay.com/customers/docs/moonpay-fees)
- [MoonPay Business Ramps](https://www.moonpay.com/business/ramps)
- [Transak Developer Integration](https://transak.com/)
- [Transak Fiat Currency Coverage and Fees](https://docs.transak.com/docs/fiat-currency-country-payment-method-coverage-plus-fees-and-limits)
- [Ramp Network Fee FAQ](https://support.rampnetwork.com/en/articles/10415-what-fees-are-charged-when-buying-crypto)
- [Bridge Stablecoin Infrastructure](https://www.bridge.xyz/)
- [Stripe Stablecoin Strategy - insights4vc](https://insights4vc.substack.com/p/stripes-stablecoin-strategy)
- [Stripe Newsroom: Tour New York 2025](https://stripe.com/newsroom/news/tour-newyork-2025)
- [Stripe/Bridge Stablecoin Volume Quadruple - CoinDesk](https://www.coindesk.com/business/2026/02/24/stripe-s-bridge-sees-stablecoin-volume-quadruple-as-utility-insulates-from-crypto-winter)
- [Architect Partners: Stripe Bridge Acquisition Analysis](https://architectpartners.com/stripe-is-acquiring-bridge-for-1-1-billion-the-most-strategically-important-transaction-since-the-emergence-of-crypto/)
- [Zero Hash Infrastructure](https://zerohash.com/)
- [Sardine Payments Documentation](https://docs.payments.sardine.ai/products/onRamp)
- [Best Crypto On-Ramps 2026 - Bleap Finance](https://www.bleap.finance/en-us/blog/best-crypto-on-ramp-off-ramp-compared)
- [Top Fiat Onramps 2026 - QuickNode](https://www.quicknode.com/builders-guide/best/top-9-fiat-onramps)
- [Best Fiat On-Ramp Providers 2026 - Seamless Chex](https://www.seamlesschex.com/blog/best-fiat-on-ramp-providers-in-2026)

### Section 2: Compliance and Travel Rule
- [Crypto Travel Rule Guide 2026 - InnReg](https://www.innreg.com/blog/crypto-travel-rule-guide)
- [FATF 2025 Targeted Update on VA/VASPs (PDF)](https://www.fatf-gafi.org/content/dam/fatf-gafi/recommendations/2025-Targeted-Upate-VA-VASPs.pdf.coredownload.pdf)
- [Stablecoins Travel Rule Explainer - FXC Intelligence](https://www.fxcintel.com/research/analysis/stablecoins-travel-rule-explainer)
- [Notabene State of Crypto Travel Rule Compliance Report 2025](https://notabene.id/state-of-crypto-travel-rule-compliance-report)
- [Travel Rule Compliance Surges - CoinDesk](https://www.coindesk.com/business/2025/04/23/travel-rule-compliance-surges-on-new-regs-stablecoin-payments-notabene-says)
- [Chainalysis-Notabene Travel Rule Integration](https://www.chainalysis.com/blog/chainalysis-notabene-travel-rule-integration/)
- [Chainalysis-Notabene Interoperability](https://www.chainalysis.com/blog/chainalysis-notabene-crypto-travel-rule-interoperability/)
- [Elliptic Travel Rule Solutions](https://www.elliptic.co/travel-rule)
- [Sygna VASP Travel Rule Solution](https://www.sygna.io/)
- [Sumsub: Crypto Regulation 2026](https://sumsub.com/blog/global-crypto-regulations/)
- [FATF Crypto Guidance 2026](https://midlandsinbusiness.com/fatf-crypto-guidance-travel-rule-vasp-rules-and-aml-compliance-in)

### Section 3: Merchant Acceptance
- [Oliver Wyman: Stablecoin Revolution in Merchant Payments](https://www.oliverwyman.com/our-expertise/insights/2025/jun/how-stablecoins-transform-merchant-transactions.html)
- [Cryptocurrency Payment Adoption by Merchants Statistics 2025 - CoinLaw](https://coinlaw.io/cryptocurrency-payment-adoption-by-merchants-statistics/)
- [Bealls Inc. Partners with Flexa](https://www.businesswire.com/news/home/20251020035820/en/Bealls-Inc.-Partners-With-Flexa-to-Offer-Digital-Currency-Payment-Options-at-Its-Stores-Nationwide)
- [Stripe: Stablecoin Payments for Subscriptions](https://stripe.com/blog/introducing-stablecoin-payments-for-subscriptions)
- [Stripe: How Businesses Are Adopting Stablecoins](https://stripe.com/en-se/resources/more/how-businesses-are-adopting-stablecoin-payments)
- [Crypto Payment Gateway Comparison 2026 - Aurpay](https://aurpay.net/aurspace/crypto-payment-gateway-comparison-2026/)
- [American Banker: Payment Fintechs Push Stablecoin Tech for 2026](https://www.americanbanker.com/news/payment-fintechs-push-stablecoin-tech-for-2026)

### Section 4: Accounting and Tax
- [Bloomberg Tax: Accounting Board Needs to Define Stablecoins 2026](https://news.bloombergtax.com/tax-insights-and-commentary/accounting-board-needs-to-define-stablecoins-clarify-ai-in-2026)
- [FASB Crypto Accounting Rules - Gordon Law](https://gordonlaw.com/learn/fasb-announces-crypto-accounting-rules/)
- [Stablecoin Accounting Treasury Perspective - TMI](https://treasury-management.com/blog/understanding-stablecoin-accounting)
- [Deloitte FASB Crypto Assets FAQ](https://dart.deloitte.com/USDART/home/publications/deloitte/heads-up/2024/faq-fasb-crypto-assets-standard-asu-2023-08)
- [FASB Overhaul Crypto Accounting Rules - CryptoNews](https://cryptonews.com/news/fasb-overhauls-crypto-accounting-rules/)
- [GENIUS Act Text - Congress.gov](https://www.congress.gov/bill/119th-congress/senate-bill/1582/text)
- [GENIUS Act Overview - Latham & Watkins](https://www.lw.com/en/insights/the-genius-act-of-2025-stablecoin-legislation-adopted-in-the-us)
- [Bitwave Enterprise Accounting](https://www.bitwave.io/solutions/crypto-and-defi-tax-tracking-software)
- [TaxBit Enterprise Platform](https://www.taxbit.com/products/enterprise-accounting)
- [SAP Digital Currency Hub B2B Payments](https://community.sap.com/t5/financial-management-blog-posts-by-sap/b2b-payments-with-stablecoins-integrating-sap-erp-systems-with-sap-digital/ba-p/13885927)

### Section 5: Dispute Resolution
- [Crypto Chargebacks - Chargebacks911](https://chargebacks911.com/crypto-chargebacks/)
- [Stablecoin Chargebacks - Plasma](https://plasma.to/learn/stablecoin-chargebacks)
- [Irrevocable Crypto Transactions Consumer Protection - Bulldog Law](https://www.thebulldog.law/irrevocable-cryptocurrency-transactions-consumer-protection-challenges)
- [Chargebacks and Disputes in Crypto Payments - CoinRemitter](https://blog.coinremitter.com/understanding-chargebacks-refunds-and-disputes-in-cryptocurrency-payments/)
- [GENIUS Act Reshaping Disputes - JAMS ADR](https://www.jamsadr.com/insight/2025/how-the-genius-act-is-reshaping-stablecoin-regulation-and-emerging)

### Section 6: Interoperability
- [Circle CCTP Documentation](https://developers.circle.com/cctp)
- [Circle CCTP Overview](https://www.circle.com/cross-chain-transfer-protocol)
- [Chainlink CCIP Documentation](https://docs.chain.link/ccip)
- [ERC-7683 Crosschain Intents](https://www.erc7683.org/)
- [ERC-7683 Deep Dive - Rango Exchange](https://rango.exchange/learn/market-trends/erc7683-crosschain-intent-standard)
- [Across Protocol](https://across.to/)
- [Cross-Chain Stablecoin Bridges Comparison 2026 - Stablecoin Insider](https://stablecoininsider.org/cross-chain-stablecoin-bridges-2026/)
- [Wormhole CCTP Bridge Documentation](https://wormhole.com/docs/products/cctp-bridge/overview/)
- [Chain Abstraction Relevance 2025 - Particle Network](https://blog.particle.network/is-chain-abstraction-relevant-in-2025/)
- [Rise of Stablechains - Across Protocol](https://across.to/blog/stablechains)

### Section 7: Privacy
- [Payy $6M Seed Round - Crypto.news](https://crypto.news/payy-raises-6m-seed-to-build-private-stablecoin-payments-on-zero-knowledge-rails/)
- [Payy Ethereum L2 Launch - CoinMarketCap](https://coinmarketcap.com/academy/article/payy-launches-ethereum-layer-2-with-default-privacy)
- [Payy Privacy for Stablecoins - Protocol Labs](https://www.protocol.ai/blog/payy-and-privacy-for-stablecoins/)
- [Payy Privacy Visa Card - CoinDesk](https://www.coindesk.com/business/2025/08/06/ex-apple-engineer-unveils-privacy-focused-crypto-visa-card)
- [Railgun Privacy Ecosystem](https://www.railgun.org/)
- [Railgun Privacy System Wiki](https://docs.railgun.org/wiki/learn/privacy-system)
- [Vitalik Uses Railgun - CoinDesk](https://www.coindesk.com/tech/2025/06/04/vitalik-buterin-uses-privacy-tool-railgun-again-signaling-ongoing-embrace-of-on-chain-anonymity)
- [Pragmatic Privacy 2025 - The Block](https://www.theblock.co/post/383680/aztec-zcash-year-pragmatic-privacy-root)
- [Aztec Road to Mainnet](https://aztec.network/blog/road-to-mainnet)
- [Aztec Ignition Chain Launch - CoinDesk](https://www.coindesk.com/markets/2025/11/20/privacy-focused-aztec-network-s-ignition-chain-lights-up-on-ethereum)
- [Aztec Ignition Mainnet Launch - The Block](https://www.theblock.co/post/379592/aztec-launches-ignition-chain-mainnet)
- [Circle Arc Introduction](https://www.circle.com/blog/introducing-arc-an-open-layer-1-blockchain-purpose-built-for-stablecoin-finance)
- [Circle Arc Network](https://www.arc.network/)
- [Stablechains Comparison: Plasma, Arc, Tempo - Across Protocol](https://across.to/blog/stablechains)
- [Tempo vs Stable vs Arc Guide - PayRam](https://www.payram.com/blog/tempo-vs-stable-vs-arc)

### Market Data
- [Stablecoin Market Cap $317B - MEXC](https://www.mexc.co/news/421705)
- [How Stablecoins Reached $300B - Arkham](https://info.arkm.com/research/how-stablecoins-reached-a-300-billion-market-cap-in-2025)
- [USDT vs USDC Market Share Analysis - Crystal Intelligence](https://crystalintelligence.com/thought-leadership/usdt-maintains-dominance-while-usdc-faces-headwinds/)
- [Stablecoin Market Cap - DefiLlama](https://defillama.com/stablecoins)
