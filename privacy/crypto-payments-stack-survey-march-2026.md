---
title: "The Crypto Payments Stack: A First-Principles Survey of What Works, What Doesn't, and What No One Has Solved"
date: 2026-03-31
---

# The Crypto Payments Stack: A First-Principles Survey of What Works, What Doesn't, and What No One Has Solved

*A synthesis report written by the apriori-writer agent | ethreportseth.xyz | March 2026*

## tl;dr

- **Crypto does not replace the payments stack -- it compresses it.** The traditional five-layer architecture (ledger, messaging, clearing, settlement, application) collapses messaging, clearing, and settlement into a single atomic operation. This compression -- not "speed" or "cost" in isolation -- is the fundamental value proposition.
- **The proven use case is cross-border settlement, and the numbers are no longer theoretical.** Bitso processes 10% of the US-Mexico remittance corridor ($6.5B in 2024). Stripe/Bridge quadrupled to $4.8B monthly. JPMorgan Kinexys settles $7B daily. The World Bank global average remittance cost is 6.49% for $200; stablecoin corridors achieve 0.3-1.5%.
- **Seven critical problems remain unsolved, and no single team has addressed all of them.** Fiat on/off ramps (1-4.5% fees, 38% of users cite this as the main barrier per Gemini), FASB accounting treatment (stablecoins excluded from ASU 2023-08, classification undetermined), dispute resolution (no chargeback equivalent exists at scale), compliance fragmentation (FATF Travel Rule at 52% adoption -- 85 of 163 jurisdictions), merchant acceptance ($18B annualized crypto card spending vs. Visa's ~$15T), interoperability (USDC on Ethereum is not USDC on Solana in practice), and privacy (no regulator has endorsed ZK-based compliance).
- **Stripe/Bridge is the closest thing to a full-stack solution, but it is centralized and lacks privacy.** Privacy protocols solve one problem but not the other six. Institutional networks (Kinexys, Canton) are permissioned and not interoperable with the open stablecoin ecosystem. The complete solution does not exist yet.
- **The business case for institutions is quantifiable: $1M-4M annual savings on $10M monthly cross-border volume, $20M/year in foregone yield on $500M in nostro accounts, 3 business days saved per payment cycle.** These are back-of-envelope estimates that do not account for implementation costs, but the directional magnitude is why Stripe paid $1.1B for Bridge and JPMorgan built Kinexys.
- **Actual stablecoin payments volume is approximately $390B annually -- not $33T.** The $33T figure includes DeFi, trading, and treasury movements. The distinction between transfer volume and payment volume is the most important analytical correction in the space.

---

## Table of Contents

1. [The Payments Stack From First Principles](#1-the-payments-stack-from-first-principles)
2. [The Problems Crypto Payments Actually Solve](#2-the-problems-crypto-payments-actually-solve)
3. [Who Is Building What -- A Survey of Approaches](#3-who-is-building-what----a-survey-of-approaches)
4. [What No One Has Solved -- The Seven Gaps](#4-what-no-one-has-solved----the-seven-gaps)
5. [The Observation -- No One Has a Complete Solution](#5-the-observation----no-one-has-a-complete-solution)
6. [Why Institutions Care -- The Business Case](#6-why-institutions-care----the-business-case)
7. [Data Sources and Methodology](#7-data-sources-and-methodology)

---

## 1. The Payments Stack From First Principles

To understand where crypto fits -- and where it doesn't -- you have to start with how payments actually work. Not the marketing version. The plumbing.

What follows is a five-layer analytical framework for decomposing the traditional payments stack. A disclosure: this is not an industry-standard taxonomy. No universally agreed-upon "five-layer model" exists in payments literature. Various sources describe payment stacks with different numbers of layers and different boundaries. This framework is constructed for this report because it maps cleanly to real infrastructure and, more importantly, because it makes the crypto value proposition (and its gaps) legible.

The five layers: Account/Ledger, Messaging/Instruction, Clearing/Netting, Settlement, and Application/Interface.

---

### Layer 1: Account/Ledger -- Who Holds What

**Traditional finance:** Your bank balance is an entry in your bank's proprietary database. You do not hold dollars -- you hold a claim on your bank for dollars. Banks hold reserve balances at the Federal Reserve. The Fed's ledger is the ultimate source of truth for interbank obligations. Securities are held through chains of intermediaries, with DTCC/DTC maintaining the master ledger in "street name."

The defining characteristic: account records are siloed, proprietary, and opaque. Your bank knows your balance. Another bank does not, unless they exchange information through messaging layers.

**Where crypto inserts itself:** Blockchains replace the proprietary, siloed ledger with a shared, cryptographically secured one. Stablecoin balances (USDC, USDT) on Ethereum, Solana, or other chains are the crypto equivalent of bank account balances -- visible to all participants on public chains and not dependent on any single institution's database. Smart contract wallets (ERC-4337, account abstraction) add programmable account functionality: multi-sig, spending limits, recovery.

**What crypto solves:** The reconciliation problem disappears. In TradFi, every institution maintains its own ledger and must reconcile against others. A shared ledger means everyone reads from the same source of truth. Single-institution dependency is removed -- your bank can freeze your account, go bankrupt (Synapse, 2024), or debank you; an onchain balance is self-custodied and institution-independent. And the ledger operates 24/7.

**What crypto breaks:** No FDIC insurance. Bank accounts are insured up to $250,000; stablecoin balances have no equivalent. Key management risk means "be your own bank" also means losing your private key means losing your funds. Public transparency on public chains is a non-starter for institutional use without privacy infrastructure. And FASB accounting treatment for stablecoin holdings remains undefined -- FASB voted 6-1 in October 2025 to study the question, with preliminary guidance expected mid-2026.

---

### Layer 2: Messaging/Instruction -- How Payment Instructions Move

**Traditional finance:** This layer transmits the instruction "move money from A to B." It does not move money -- it moves information about intended money movement.

- **SWIFT:** The dominant global messaging network for cross-border payments. Approximately 11,000 member institutions. SWIFT does not settle payments -- it sends standardized messages (MT and MX/ISO 20022 formats) between banks. As of March 2026, 75% of payments on the SWIFT network reach beneficiary banks within 10 minutes, though end-to-end delivery still varies.
- **ACH:** The US domestic batch payment system. In 2025, ACH processed 35.2 billion payments worth $93 trillion (up 4.9% and 7.9% YoY, respectively).
- **Card network messaging:** Authorization requests travel from merchant terminal to acquirer to card network to issuing bank and back in roughly 2 seconds. This is a messaging and authorization flow, not settlement.

**Where crypto inserts itself:** On a blockchain, messaging and settlement are not separate. When you send USDC to an address, the instruction and the settlement are the same transaction. There is no separate "tell the counterparty to expect a payment" step.

Cross-chain messaging adds complexity: Circle CCTP processes cross-chain USDC transfers ($110B+ cumulative by November 2025, CCTP V2 live on 17+ blockchains). Chainlink CCIP handles cross-chain messaging for tokens and arbitrary data ($18B monthly as of March 2026, connecting 60+ blockchains). LayerZero handles 75% of cross-chain bridge volume with 1.2 million daily messages.

**What crypto solves:** The messaging/settlement separation is eliminated. In TradFi, messages travel instantly but settlement lags by days. Crypto collapses both into a single atomic operation. Intermediary chains are removed -- a SWIFT payment from the US to Nigeria may traverse 3-4 correspondent banks, each adding latency, cost, and failure risk. Smart contracts enable programmable messaging: "pay this amount IF these conditions are met." SWIFT cannot do this natively.

**What crypto breaks:** Bridge risk introduces new attack surfaces (Ronin $625M, Wormhole $320M, Nomad $190M). No universal messaging standard exists -- SWIFT has ISO 20022, while crypto has competing protocols (CCTP, CCIP, LayerZero, Wormhole, Across) with incomplete interoperability. Finality differs across chains, meaning a payment "confirmed" on one chain may not be final on another for minutes or hours.

---

### Layer 3: Clearing/Netting -- Who Owes Whom

**Traditional finance:** Clearing calculates net obligations between parties, dramatically reducing the capital required for settlement.

- **CHIPS** processes roughly 500,000 payments totaling approximately $1.8 trillion daily. Together with Fedwire, CHIPS accounts for roughly 96% of US large-value payments.
- **Card network clearing:** Visa and Mastercard aggregate millions of daily transactions and net them at end-of-day for settlement between issuers and acquirers. This netting is why card settlement takes T+1 to T+2.
- **CLS Bank** clears FX transactions using payment-versus-payment, eliminating Herstatt risk (the risk that one leg of an FX trade settles while the other does not).

**Where crypto inserts itself:** Onchain transactions settle gross, in real-time. Every transaction is individually settled with finality. This eliminates the need for a separate clearing layer. When you send USDC, the full amount moves. There is no end-of-day netting.

**What crypto solves:** Clearing risk -- the period between trade execution and settlement where counterparty failure can prevent settlement -- is eliminated. Clearinghouses as trust intermediaries (DTCC, CLS Bank, LCH) become redundant for simple transfers, since the blockchain provides trustless settlement.

**What crypto breaks:** Capital inefficiency. Netting exists for a reason. If Bank A owes Bank B $1 billion and Bank B owes Bank A $900 million, netting means only $100 million moves. Gross settlement requires both full amounts to be available. For high-volume institutional flows, this is a real disadvantage. There is also no error correction layer -- clearing in TradFi provides a window to catch errors, apply compliance checks, and resolve disputes. Onchain settlement is immediate and irreversible. And the crypto ecosystem has not built robust netting infrastructure for institutional use.

---

### Layer 4: Settlement -- Final Transfer of Value

**Traditional finance:** Settlement is the actual, irrevocable transfer of value.

- **Fedwire Funds Service:** The Federal Reserve's RTGS system. Settles approximately $4.7 trillion daily. Transfers are immediate and final. Operates Monday through Friday, roughly 9:00 PM ET (prior evening) to 7:00 PM ET. *Note: The Federal Reserve announced in October 2025 that Fedwire will expand to include Sundays and weekday holidays, though implementation is targeted no earlier than 2028.*
- **Cross-border settlement** occurs through correspondent banking chains, where banks hold nostro (our money at their bank) and vostro (their money at our bank) accounts. An estimated $400 billion to $1+ trillion sits trapped in these accounts globally as pre-funded liquidity. This range is an industry estimate without authoritative primary sourcing from BIS or IMF -- the figure appears in Finextra and similar secondary sources. The conservative range is used here; some sources cite dramatically higher figures.

**Where crypto inserts itself:** Public blockchains are settlement layers. Ethereum settles $1-2 trillion monthly in native token and stablecoin transfers. Solana provides sub-second finality at fractions-of-a-cent gas costs. Layer 2 networks (Arbitrum, Optimism, Base) inherit Ethereum's settlement guarantees at lower costs.

Institutional settlement networks are emerging in parallel: JPMorgan Kinexys processes approximately $7 billion daily ($3T+ cumulative) using permissioned blockchain with tokenized deposits. Canton Network, a privacy-preserving L1, has partnered with DTCC to tokenize DTC-custodied US Treasuries, with a production MVP targeting H1 2026; Broadridge DLR (built on Canton) processed $339 billion in average daily repo transactions in September 2025. Fnality is building tokenized wholesale payments backed by central bank reserves ($136M Series C, September 2025).

**What crypto solves:** 24/7/365 availability. Speed -- seconds to minutes versus 1-5 business days for cross-border. Reduced pre-funding (the $400B+ locked in nostro accounts represents the cost of correspondent banking). Programmability -- settlement can be conditional on oracle inputs, milestone completion, or time constraints.

**What crypto breaks:** Settlement finality varies -- Ethereum's 12-second block time does not mean 12-second finality; true finality takes approximately 12 minutes. Stablecoin issuer risk means settlement depends on the issuer remaining solvent (the March 2023 USDC depeg, when $3.3B of reserves were at SVB, demonstrated this is not theoretical). Regulatory uncertainty persists around whether a stablecoin transfer constitutes "settlement" in a legal sense. And there is no lender of last resort -- the Federal Reserve backstops Fedwire; no one backstops a public blockchain.

---

### Layer 5: Application/Interface -- What Users See

**Traditional finance:** Bank apps, card terminals, payment buttons (Stripe Checkout, PayPal, Apple Pay). The abstraction layer that hides the plumbing from the end user.

**Where crypto inserts itself:** This layer is where crypto has historically been weakest, but 2025-2026 has seen significant progress. Stripe/Bridge launched stablecoin financial accounts in 101 countries, with Bridge volume quadrupling to approximately $4.8 billion monthly. Ingenico POS terminals now accept stablecoin payments (USDC, USDT, EURC) via WalletConnect integration. Payy offers a privacy-preserving stablecoin wallet with Visa card integration ($6M seed, FirstMark Capital, March 2026; claims $130M annualized volume and 100K+ users -- company-reported, not independently verified).

**What crypto solves:** Global-first design -- a stablecoin wallet works everywhere there is internet, without local banking partnerships. Programmable payment experiences (streaming payments, conditional payments, automated escrow). Self-custody without institutional intermediaries.

**What crypto breaks:** UX remains inferior for mainstream users. Wallet addresses, gas fees, network selection, bridge transactions are all foreign concepts. No universal standard for payment initiation exists -- Visa/Mastercard provide tap/swipe/insert; stablecoin payments offer fragmented options (WalletConnect, QR codes, NFC, direct transfer). And error irreversibility means sending to the wrong address results in permanent loss with no recourse.

---

### The Stack Compression Thesis

| Traditional Layer | Traditional Infrastructure | Crypto Equivalent | Status |
|---|---|---|---|
| **Account/Ledger** | Bank databases, central bank reserves | Blockchain addresses, smart contract wallets | Functional; lacks FDIC equivalent, accounting clarity |
| **Messaging** | SWIFT, ACH, card networks | Onchain transactions, CCTP, CCIP, LayerZero | Functional; fragmented standards, bridge risk |
| **Clearing/Netting** | CHIPS, CLS Bank, card network batch processing | Eliminated (gross settlement) or smart contract netting | Under-built for institutional scale |
| **Settlement** | Fedwire ($4.7T/day), correspondent banking | Public chains, Kinexys ($7B/day), Canton Network | Proven at scale for select use cases |
| **Application** | Bank apps, POS terminals, Stripe/PayPal | Wallets, Stripe/Bridge, Ingenico, Payy | Improving rapidly; UX gap narrowing |

The fundamental insight: crypto does not add a new layer to the stack -- it compresses the stack. Messaging, clearing, and settlement collapse into a single atomic operation. This is structurally disruptive because it eliminates the institutions whose business models depend on operating those intermediate layers: correspondent banks, clearinghouses, card network settlement operations.

This compression is the value proposition. Not "faster" in isolation. Not "cheaper" in isolation. The elimination of entire categories of intermediation.

---

## 2. The Problems Crypto Payments Actually Solve

### Cross-Border Settlement: The Proven Use Case

The World Bank global average remittance cost remains 6.49% for sending $200 (Q1 2025). Sub-Saharan Africa corridors average 8.78%. Banks charge 14.55% on average; money transfer operators charge 5.04%.

Stablecoin corridors achieve 0.3-1.5% in optimized routes, plus on/off ramp costs of 0.3-1% where fiat conversion is needed.

The evidence is no longer theoretical:

- **Bitso** processed $6.5 billion in US-Mexico crypto remittances in 2024, representing approximately 10% of the total US-Mexico corridor ($64.7 billion). This is company-reported data, but the 10% corridor share aligns with World Bank corridor figures.
- **Stripe/Bridge** quadrupled stablecoin volume in 2025, reaching approximately $4.8 billion monthly by early 2026. Stripe self-reported.
- **USDT in Nigeria** functions as a de facto parallel dollar. Nigeria ranks among the highest in global crypto adoption per capita (Chainalysis Global Crypto Adoption Index).

**Honest caveat:** The stablecoin cost advantage is most dramatic in high-cost corridors. In the US-EU corridor, traditional costs are already low (Wise charges 0.5-1.5%), and the stablecoin advantage narrows significantly. The comparison also depends on whether you include the full on/off ramp cost stack -- when you do, the savings in low-cost corridors can approach zero.

---

### Counterparty Risk and Trapped Capital

Cross-border correspondent banking requires pre-funding. A bank in Nigeria that wants to pay a bank in the US must hold a nostro account pre-funded in USD at a US correspondent bank. Industry estimates put global trapped liquidity in nostro/vostro accounts at $400 billion to over $1 trillion (no authoritative primary source exists for this figure -- it comes from Finextra and similar secondary sources).

This capital earns minimal yield (typically low-interest demand deposits), cannot be deployed for productive uses, and disproportionately burdens smaller banks and banks in emerging markets.

Stablecoin settlement is payment-versus-payment: the sender's stablecoins move to the receiver atomically. No pre-funding of intermediary accounts is required.

- **JPMorgan Kinexys** allows clients to move tokenized deposits across borders without pre-funding correspondent accounts. $7 billion daily volume demonstrates institutional viability.
- **Fnality** is building tokenized wholesale payment systems backed by central bank reserves, specifically designed to replace nostro/vostro pre-funding. $136M Series C (September 2025).

---

### 24/7 Settlement

Weekends, holidays, and overnight hours represent roughly 30-35% of total calendar time during which no settlement can occur on traditional rails. The math: weekends are 28.6% of the week; add federal holidays and Fedwire's non-operating overnight hours, and the figure reaches the 30-35% range.

Blockchains settle 24/7/365. No downtime, no batch processing windows, no business hours.

*Important context: The Federal Reserve announced in October 2025 that it will expand Fedwire and NSS to include Sundays and weekday holidays, though implementation is targeted no earlier than 2028. This will reduce the 30-35% figure over time, though weekends and overnight hours will still represent significant settlement gaps. The expansion acknowledges the competitive pressure from always-on digital settlement.*

---

### Programmable Money

Traditional payments are dumb pipes: money moves from A to B with metadata attached. Conditional logic, if it exists, lives in separate systems disconnected from the payment itself.

Smart contracts embed logic directly in the payment: escrow released automatically when conditions are met, streaming payments (pay per second via Sablier or Superfluid), conditional payments tied to oracle inputs, multi-party splits enforced at the protocol level.

The Circle Refund Protocol (launched April 2025) enables non-custodial dispute resolution and escrow for ERC-20 payments -- refunds, lockups, and mediated resolutions onchain. It is early-stage, but it represents the first serious attempt at programmable payment dispute handling.

**Honest caveat:** Most onchain payments today are still simple transfers. Programmable money is technically possible but not mainstream. Enterprise adoption of smart contract-based payments is in early stages.

---

### Financial Access

Approximately 1.4 billion adults globally remain unbanked (World Bank Findex, 2021). A smartphone plus internet connection equals a stablecoin wallet -- no bank account, credit check, or physical branch visit required. USDT on Tron is the most-used financial tool in many parts of sub-Saharan Africa and Southeast Asia, not because users are crypto enthusiasts but because it is the most accessible way to hold dollars.

**Honest caveat:** "Unbanked" does not mean "has a smartphone and understands wallet software." The last mile is still mobile money (M-Pesa) in much of Africa, not stablecoins. Crypto access requires digital literacy that many unbanked populations lack. The financial access argument is real but oversimplified when presented without the last-mile gap.

---

### Correspondent Banking Disintermediation

The global correspondent banking network has been contracting for over a decade. The number of active correspondents fell roughly 25% between 2011 and 2022 (BIS data). Banks in high-risk jurisdictions -- Africa, Central Asia, Pacific Islands -- are losing correspondent banking relationships entirely, a phenomenon called "de-risking."

De-risking means entire countries are losing access to the global dollar system. The remaining corridors become more concentrated, more expensive, and more fragile.

Stablecoins provide an alternative settlement rail that does not depend on having a correspondent banking relationship. This is not coincidence-driven: stablecoin usage is highest in exactly the countries experiencing the worst de-risking -- Nigeria, Turkey, Argentina, Philippines, Kenya.

---

## 3. Who Is Building What -- A Survey of Approaches

### Stablecoin Rails as Settlement

| Company/Protocol | What They Do | Stack Layer(s) | Traction | Status |
|---|---|---|---|---|
| **Stripe/Bridge** | Stablecoin orchestration API; financial accounts in 101 countries | Settlement + Application | ~$4.8B monthly (early 2026), 4x growth in 2025 | $1.1B acquisition; OCC conditional national trust bank charter (Feb 2026) |
| **Circle (USDC + CCTP)** | Stablecoin issuance + cross-chain transfer infrastructure | Ledger + Settlement | $110B+ cumulative CCTP volume; USDC processed $18.3T in onchain transfers in 2025 | Public company (IPO priced at $6.9B, trading at ~$32B market cap) |
| **Visa USDC settlement pilot** | Stablecoin settlement between acquirers and Visa | Settlement | Pilot with Crypto.com, Worldpay; joined Canton Network governance (March 2026) | Internal initiative |
| **Mastercard MTN** | Multi-Token Network for tokenized settlement; agreed to acquire BVNK for up to $1.8B | Settlement | Pilot/early production | Internal initiative + M&A |

---

### Cross-Chain Settlement Infrastructure

| Protocol | What They Do | Stack Layer(s) | Traction | Status |
|---|---|---|---|---|
| **Chainlink CCIP** | Cross-chain messaging + token transfers | Messaging + Settlement | $18B monthly (March 2026); 60+ chains; Coinbase, Lido, Base integrations | LINK token ($8B+ market cap) |
| **LayerZero** | Omnichain interoperability protocol | Messaging | 75% of cross-chain bridge volume (single source, may use favorable methodology); 1.2M daily messages | Raised $263M (Series B at $3B valuation) |
| **Wormhole** | Cross-chain messaging; institutional partnerships | Messaging + Settlement | BlackRock BUIDL ($18B AUM by Feb 2026), Apollo, Securitize integration | W token; raised $225M |
| **Across Protocol** | Intent-based bridge for ETH and stablecoins | Settlement | $35B+ bridged lifetime; 54% of daily active bridge users (Jan 2026); V4 with ZK proofs (July 2025) | Transitioning from DAO to C-corp (March 2026) |
| **Circle CCTP V2** | Native USDC cross-chain burns and mints | Settlement | $110B+ cumulative; 17+ chains; V1 phase-out begins July 31, 2026 | Circle infrastructure |

---

### Institutional Networks

| Network | What They Do | Stack Layer(s) | Traction | Status |
|---|---|---|---|---|
| **JPMorgan Kinexys** | Tokenized deposit payments for JPM clients | All layers (permissioned) | ~$7B daily, $3T+ cumulative; $300B in intraday repo | Production, expanding (GBP rollout 2025) |
| **Canton Network** | Privacy-preserving L1 for regulated finance | Ledger + Settlement | 600+ validators; DTCC Treasury tokenization MVP (H1 2026); $339B daily repo via Broadridge DLR | Production |
| **Fnality** | Tokenized wholesale payments backed by central bank reserves | Settlement | Partnered with Broadridge for repo settlement | $136M Series C (Sep 2025) |
| **DTCC ComposerX** | Tokenization platform for DTC-custodied securities | Ledger | Treasury tokenization pilot on Canton | Production MVP targeting H1 2026 |

---

### Privacy-Preserving Payments

| Protocol/Company | Architecture | Approach | Status |
|---|---|---|---|
| **Payy** | Privacy L2 on Ethereum (Halo2 ZK proofs) | Default privacy; KYC at wallet level; Visa card integration | $6M seed (March 2026); claims $130M annualized volume, 100K+ users (company-reported, unverified); mainnet planned summer 2026 |
| **Railgun** | ZK privacy on existing EVM chains | Users shield tokens into a shared privacy pool; "Proofs of Innocence" for compliance | Live on Ethereum, Arbitrum, BSC, Polygon; $2B+ lifetime volume; $94M TVL (late 2025); Vitalik Buterin moved $2.6M through Railgun (June 2025) |
| **Aztec Network** | Programmable privacy L2 on Ethereum (ZK-SNARKs) | Three privacy pillars: data, identity, compute; developers choose what is public vs. private | Alpha testnet launched November 2025 (not a production mainnet -- transactions expected to go live early 2026); critical vulnerability disclosed March 17, 2026, with fix targeted for v5 in July 2026; 185+ operators, 3,400+ sequencers |
| **Aleo** | Privacy L1 blockchain (ZK-SNARKs) | Full-stack privacy chain; private stablecoin payroll with Toku/Paxos | USAD stablecoin live on mainnet (Feb 2026); private payroll launched Q1 2026; raised $298M+ |
| **Circle Arc** | TEE-based stablecoin L1 with path to MPC and ZK proofs | Selective privacy: amounts shielded, addresses visible; "view keys" for auditors/regulators | Private testnet (2025), public testnet fall 2025, mainnet beta 2026 |

---

### Card Programs and POS Infrastructure

| Company | What They Do | Traction | Notes |
|---|---|---|---|
| **Marqeta** | Card issuer-processor powering crypto cards | Coinbase Card, Cash App; revenue $507M (2024) | Public company; JIT funding model suited to crypto liquidation |
| **i2c** | Card program manager | Crypto.com card; 200+ countries | Private |
| **Ingenico** | POS terminal stablecoin integration | Stablecoin payments on Android terminals via WalletConnect (2026) | Partnership with WalletConnect |
| **Burner Terminal** | Stablecoin-native POS hardware | Shipping early 2026; <$200 price point | Early stage |

---

### Payroll and B2B

| Company | What They Do | Traction | Status |
|---|---|---|---|
| **Toku** | Stablecoin payroll platform | $1B+ annual payroll volume (company-reported), 100+ countries; private payroll with Aleo/Paxos (Jan 2026) | Production |
| **Rise** | Hybrid fiat/stablecoin payroll | Purpose-built for stablecoin payroll | Production |
| **Deel** | Global payroll ($22B annual); stablecoin partnership with MoonPay | Stablecoin payroll announced Feb 2026 (UK/EU initially) | $12B+ valuation |

Over 225 businesses integrated stablecoins for payroll and operational payments in 2025. Yet less than 1% of businesses use crypto for payroll, indicating the market is very early.

---

### Remittance Corridors

| Company/Network | Focus | Traction |
|---|---|---|
| **Bitso** | US-Mexico, Latin America | $6.5B in US-Mexico crypto remittances (2024); ~10% of corridor; $82B annualized TPV (2025) |
| **Coins.ph** | Philippines (inbound from US, Middle East) | Major local platform; Philippines among top remittance-receiving countries |
| **Yellow Card** | Africa (Nigeria, Kenya, Ghana, South Africa) | Pan-African stablecoin on/off ramp; raised $33M Series B (2023) |
| **USDT P2P markets** | Nigeria, Turkey, Argentina, Venezuela | Organic, largely unmeasured; de facto dollarization |

---

### Merchant Infrastructure

| Provider | Model | Notes |
|---|---|---|
| **Stripe (via Bridge)** | Native payment integration for millions of existing Stripe merchants | USDC and others via Bridge; fiat settlement in merchant's Stripe account; stablecoin payments for subscriptions launched 2025 |
| **Coinbase Commerce** | Payment gateway (merging into Coinbase Business, March 2026) | USDC native; mid-market, e-commerce focused |
| **BitPay** | Payment gateway | Established, thousands of merchants; USDC and other stablecoins |
| **Strike** | Bitcoin/Lightning focused | Retail partnerships; limited stablecoin focus; fiat settlement via Lightning |
| **NOWPayments** | Payment gateway | SMB-focused; 200+ coins; USDT, USDC |
| **Flexa** | Collateralized payment network (AMP token) | 660+ Bealls stores; instant fiat settlement |

---

## 4. What No One Has Solved -- The Seven Gaps

These are not theoretical concerns. They are concrete blockers that explain why, despite years of effort and billions of dollars invested, stablecoin payments remain approximately 0.1% of Visa's annual volume.

---

### Gap 1: Fiat On/Off Ramps

**The bottleneck.** Converting fiat to stablecoins (and back) requires navigating money transmitter licenses (in the US: 50 state-level licenses plus federal FinCEN registration), banking relationships that can be revoked at any time, KYC/AML infrastructure compliant with BSA and local regulations, and payment rail integrations (ACH, SEPA, card networks).

Fees range from 1% for bank transfers (MoonPay, Transak) to 4.5% for credit card purchases (MoonPay). Off-ramp friction is worse than on-ramp: getting money back into a bank account remains slower, more expensive, and more compliance-heavy.

**38% of potential crypto users cite difficulty buying crypto with fiat as their main barrier.** 41% of existing users say fast and reliable crypto-to-fiat withdrawals are their biggest unmet need. These figures originate from the Gemini Global Survey.

**The structural shift:** Stripe/Bridge's conditional OCC national trust bank charter (February 2026) is the most significant development in the on/off ramp space. Stripe processed $1.9 trillion in 2025 (press-reported; Stripe is private and does not file public financials). By integrating Bridge's stablecoin infrastructure directly into Stripe's payment platform, the on-ramp can disappear into existing financial products -- merchants do not need to "accept crypto," they accept payments via Stripe, and Stripe handles stablecoin-denominated flows behind the scenes.

This is the right direction. But it is one company, operating under US regulatory jurisdiction, and it does not solve the problem in countries where Stripe does not operate.

---

### Gap 2: Compliance Infrastructure

The FATF Travel Rule (Recommendation 16, revised June 2025) requires VASPs to collect, verify, and transmit originator and beneficiary information for virtual asset transfers above $1,000/EUR 1,000.

**Adoption: 85 of 163 FATF-surveyed jurisdictions (52%) have passed or are drafting Travel Rule legislation.** This is the correct figure per the FATF's own 2025 Targeted Update. The commonly cited "73%" figure uses an outdated denominator of 117 (FATF membership alone) rather than the 163 jurisdictions actually surveyed, which includes FATF-style regional body members.

MiCA's Transfer of Funds Regulation, effective December 30, 2024, is the strictest major-market implementation -- it requires full originator and beneficiary identification for all crypto transfers with no minimum threshold in the EU.

Three dominant Travel Rule messaging protocols exist, and they do not fully interoperate: TRISA (open-source, decentralized), OpenVASP (open protocol, community-driven), and Notabene (commercial solution, largest network). A VASP on TRISA cannot necessarily exchange data with a VASP on OpenVASP.

The fundamental gap: stablecoin transfers on public blockchains are pseudonymous by default, but compliance requires identity attribution. The Travel Rule assumes both sides of a transaction are VASPs with KYC'd customers. In reality, peer-to-peer transfers between non-custodial wallets have no VASP on either side. DeFi protocol interactions have no VASP on the receiving side. Cross-chain transfers may involve VASPs in different jurisdictions with different thresholds.

**No jurisdiction has formally endorsed ZK-based compliance as satisfying Travel Rule or BSA requirements.** This is the single most important outstanding regulatory question for privacy-preserving payments. If FinCEN's forthcoming GENIUS Act rulemaking requires plaintext transaction data, the entire "compliant privacy" thesis collapses in the US.

---

### Gap 3: Merchant Acceptance

Crypto-linked card spending reached approximately $18 billion annualized (late 2025), growing from approximately $100 million monthly in early 2023 to approximately $1.5 billion monthly by late 2025. Compare to Visa's approximately $15 trillion in annual payment volume. Stablecoin card spending is approximately 0.1% of Visa's volume.

Note: this is card spending that liquidates crypto to fiat at the point of sale through Visa/Mastercard rails -- it is not native stablecoin acceptance. The distinction matters.

Merchants need five things solved simultaneously before stablecoin acceptance makes economic sense: (1) payment gateway and POS integration, (2) instant settlement or conversion to fiat, (3) accounting integration with QuickBooks/Xero/NetSuite, (4) tax reporting (every stablecoin transaction is potentially taxable under IRS property classification), and (5) dispute resolution and chargeback handling.

No provider solves all five today. Stripe is the only player attempting the full stack -- integrating ramps, settlement, merchant tools, and fiat conversion within a single platform that already has distribution. If even 1% of Stripe's $1.9T volume shifts to stablecoin rails, that represents $19 billion in stablecoin payment volume flowing through a single provider.

---

### Gap 4: Accounting and Tax

This is a bigger blocker than most people in crypto realize, because it affects every business that touches stablecoins.

**FASB:** ASU 2023-08, the first dedicated crypto accounting standard (effective December 2024), explicitly excluded stablecoins from its scope because they are designed to maintain a stable value relative to a fiat currency. So how do businesses account for stablecoins? There is no clear answer. They could be classified as intangible assets, financial instruments, or cash equivalents. FASB voted in October 2025 to study whether stablecoins can be classified as cash equivalents. As of March 2026, no final guidance has been issued. Preliminary guidance is expected mid-2026, with implementation potentially in 2027 financial statements.

The GENIUS Act complicates and clarifies simultaneously. By requiring 1:1 backing with highly liquid assets and clear redemption rights, regulated payment stablecoins may more readily qualify as cash equivalents. But the GENIUS Act also makes it illegal to treat non-compliant stablecoins as cash equivalents, creating a regulatory distinction that FASB has not yet codified in accounting standards.

**IRS:** All virtual currencies, including stablecoins, are classified as property (Notice 2014-21). Every stablecoin transaction is potentially a taxable event. If a business receives USDC and the value differs from $1.00 by even fractions of a cent at the time of later conversion, there is a reportable gain or loss. For a business doing thousands of stablecoin transactions per month, the record-keeping burden is enormous and does not exist for fiat transactions.

IRS Form 1099-DA requires brokers to report gross proceeds starting 2025, with cost-basis reporting arriving 2026. This is operationally absurd for a payment instrument.

**CFOs will not approve stablecoin adoption without clear accounting treatment.** The ambiguity means auditors may disagree with a company's treatment, creating audit risk. Tax compliance costs can exceed the savings from using stablecoins. Most mid-market accounting systems have zero native stablecoin support.

---

### Gap 5: Dispute Resolution and Chargebacks

This is the least-addressed problem with the highest consumer impact.

Blockchain transactions are final and irreversible by design. This directly conflicts with consumer expectations and legal requirements. Visa and Mastercard chargebacks return approximately $11 billion annually to consumers for fraud, merchant errors, and unauthorized transactions. Stablecoins have no native chargeback mechanism. Once sent, funds cannot be recalled without the recipient's cooperation.

The GENIUS Act requires stablecoin issuers to have technical capability to freeze, seize, and burn stablecoins when legally required (law enforcement, sanctions) -- but this is not a consumer dispute mechanism. It is an enforcement tool.

**Emerging solutions:**

- **Circle Refund Protocol** (launched April 2025): Non-custodial dispute resolution and escrow for ERC-20 payments. Enables refunds, lockups, and mediated resolutions onchain. Early-stage and not production-ready at scale.
- **Stablecoin Chargeback Protocol (SCP):** Academic proposal for smart contract-based escrow and decentralized arbitration (TechRxiv). Not implemented.
- **Kleros:** Decentralized arbitration through juror staking and voting. Minimal adoption, slow response times, no investigative capability.

None of these approaches is at a scale that could substitute for card network dispute resolution. Until this is solved, stablecoins cannot replace card payments for consumer transactions where chargeback protection is expected. For B2B payments, existing contractual dispute resolution mechanisms mitigate this gap -- but for B2C commerce, it is a dealbreaker.

---

### Gap 6: Interoperability

USDC on Ethereum and USDC on Solana represent the same claim on Circle's reserves, but they are technically different tokens on different blockchains. A merchant who accepts USDC on Ethereum cannot directly accept USDC on Solana. A user on Arbitrum cannot pay a merchant on Base without a cross-chain operation.

**Within the EVM ecosystem, convergence is happening.** ERC-7683 (developed by Uniswap and Across Protocol) standardizes cross-chain intents -- users declare what they want ("send 100 USDC from Arbitrum to Base") and solvers compete to fill the intent. CCTP V2 provides canonical USDC bridging. The user experience is approaching "send money to this address" without chain selection.

**Cross-ecosystem remains fragmented.** EVM to Solana, EVM to Bitcoin, EVM to Tron -- these transfers are significantly more complex, slower, and more expensive. Circle's CCTP V2 is the closest to a universal stablecoin solution, but it only covers USDC.

Delphi Digital predicts 60% of interoperability protocols will vanish by 2027 as the market consolidates around standards (IEEE 3221.01-2025, ERC-7683). Chainlink CCIP and LayerZero are positioned as likely survivors. The consolidation is healthy -- fragmentation is the enemy of payment rail adoption.

---

### Gap 7: Privacy

Privacy enters the analysis here -- as one of seven gaps, not the entire framing.

The core problem is straightforward: public blockchain transparency means every stablecoin transaction is permanently visible to anyone. Blockchain analytics firms (Chainalysis, TRM Labs, Elliptic) can trace across 25+ chains with one-click capabilities. AI-accelerated de-anonymization has destroyed practical pseudonymity. No CFO will accept a treasury system where competitors can see balances, vendor relationships, payment timing, and contract terms in real time.

Privacy is relevant primarily at the settlement and account layers of the payments stack. It is also relevant at the identity layer (wallet addresses are pseudonymous but linkable to real identities through on/off ramps and ENS) and the network layer (IP correlation).

**The approaches:**

- **Payy** (privacy L2): Default privacy for all transactions. KYC at wallet level. The Visa card integration is the most consumer-facing approach.
- **Railgun** (client-side proving on existing EVM chains): Users shield tokens into a shared privacy pool. "Proofs of Innocence" allow users to cryptographically prove funds did not originate from sanctioned sources without revealing transaction history. Composable with existing DeFi.
- **Aztec** (programmable privacy L2): Three privacy pillars -- data, identity, compute. Developers choose what is public vs. private. Alpha testnet launched November 2025, but a critical vulnerability was disclosed March 17, 2026 that could have enabled theft via the proving system. Fix targeted for v5 in July 2026. Transactions not yet live at publication date.
- **Aleo** (privacy L1): Full-stack ZK chain with private stablecoin payroll (Toku/Paxos, launched January 2026). USAD stablecoin live on mainnet.
- **TEE approaches** (Circle Arc): Selective privacy using Trusted Execution Environments. Amounts shielded, addresses visible. Built by the USDC issuer, so compliance is architecturally native.

**The "compliant privacy" convergence:** All major approaches are converging on a model: KYC at the on-ramp, privacy in transit, selective disclosure for authorized parties (regulators, auditors), and proof of innocence/exclusion (proving funds are NOT from a sanctioned source without revealing the actual source). Payy, Railgun (via Privacy Pools), and Circle Arc are all implementing variants of this model from different directions.

**The anonymity set bootstrapping problem:** For privacy to work, the anonymity set (the number of possible senders/recipients a transaction could belong to) must be large enough to resist statistical de-anonymization. New privacy systems start with small anonymity sets, making them less private, which discourages adoption, which keeps the anonymity set small. This is the chicken-and-egg problem none of these projects have fully solved.

**The regulatory overhang:** No regulator anywhere has endorsed ZK-based compliance. The Tornado Cash ruling (March 2025) established that privacy tools cannot be sanctioned per se. The Roman Storm conviction (August 2025) established that operating them without registration may be illegal. The legal framework is still forming, and the risk is asymmetric -- a single high-profile illicit finance event on a privacy protocol could collapse the "privacy enables compliance" narrative overnight.

---

## 5. The Observation -- No One Has a Complete Solution

The seven gaps form a dependency graph. They are not independent problems:

```
Merchant Adoption
    |
    +-- requires --> Fiat On/Off Ramps (for settlement)
    +-- requires --> Accounting/Tax Clarity (for bookkeeping)
    +-- requires --> Dispute Resolution (for consumer trust)
    +-- requires --> Interoperability (to accept from any chain)
    |
Enterprise Adoption
    +-- requires --> Privacy (for business confidentiality)
    +-- requires --> Compliance (for regulatory approval)
    +-- requires --> Accounting/Tax Clarity (for CFO sign-off)
    +-- requires --> Dispute Resolution (for risk management)
```

A merchant cannot adopt stablecoin payments unless the accounting is clear, the off-ramp works, disputes can be resolved, and payments can arrive from any chain. An enterprise cannot adopt unless privacy exists, compliance is satisfied, and the CFO can account for the holdings. No single problem can be solved in isolation.

Here is where every approach stands against the full problem set:

| | On/Off Ramps | Compliance | Merchant Tools | Accounting | Disputes | Interop | Privacy |
|---|---|---|---|---|---|---|---|
| **Stripe/Bridge** | Strong | Strong | Strong | Partial | Via Stripe (potential) | Limited | No |
| **Circle/CCTP** | Via partners | Strong | Via partners | FASB pending | Refund Protocol (early) | USDC only | Arc (beta) |
| **Kinexys** | JPM only | Strong (permissioned) | N/A (institutional) | JPM internal | N/A | JPM network only | Permissioned |
| **Canton Network** | N/A | Strong (permissioned) | N/A | DTCC internal | N/A | Canton only | By design |
| **Railgun** | No | ZK proofs (unendorsed) | No | No | No | EVM only | Strong |
| **Payy** | Visa card (partial) | KYC at wallet | Via Visa card | No | No | Ethereum only | Strong |
| **Aztec** | No | Programmable | No | No | No | Ethereum L2 | Strong (alpha, vulnerability) |

The pattern is clear. **Stripe/Bridge comes closest to a full-stack solution** -- ramps, compliance, merchant integration, settlement -- but it lacks privacy, is centralized (single company, single point of regulatory risk), and does not operate on permissionless rails.

**Privacy protocols** (Railgun, Payy, Aztec, Aleo) solve the privacy gap but do not address the other six problems. A private payment rail without on/off ramps, merchant tools, accounting integration, or dispute resolution is infrastructure without a complete application layer.

**Institutional networks** (Kinexys, Canton) are production-grade and solve privacy through permissioning -- but they are walled gardens, not interoperable with the open stablecoin ecosystem. A Canton-settled Treasury token cannot interact with USDC on Ethereum. Kinexys serves JPMorgan clients, not the broader market.

The complete solution requires: on/off ramps + compliance + settlement + privacy + accounting + dispute resolution + merchant tools. No single team has all of them. The most likely path to a complete stack is not one company building everything -- it is composability between specialized providers, connected through standardized interfaces (ERC-7683, CCTP V2, ISO 20022 mappings).

This is both the opportunity and the challenge. The opportunity because the total addressable problem is enormous and the components are being built. The challenge because the dependency graph means partial solutions have limited value -- and the coordination problem of assembling a complete stack from independent providers is itself unsolved.

---

## 6. Why Institutions Care -- The Business Case

The institutional case for crypto payment rails is not ideological. It is arithmetic.

### Cost Savings on Cross-Border (Quantified)

For a company sending $10 million monthly in cross-border payments:

| Method | Monthly Cost | Annual Cost | Settlement Time |
|---|---|---|---|
| **SWIFT wires** | $25K-75K wire fees + $100K-300K FX spreads = $125K-375K | $1.5M-4.5M | 2-5 business days |
| **Correspondent banking (emerging market corridors)** | 2-5% = $200K-500K | $2.4M-6M | 2-5 business days |
| **Stablecoin rails (optimized)** | 0.1-0.5% transfer + 0.3-1% on/off ramp = $40K-150K | $480K-1.8M | Minutes to same-day |

**Potential annual savings: $1M to $4M+ for a mid-sized company with significant cross-border flows.**

*Important caveat: These are back-of-envelope estimates. They do not account for the costs of implementing stablecoin infrastructure: compliance systems, key management, accounting overhead, treasury integration, staff training, and the ongoing operational complexity described in Gaps 2 and 4 above. Net savings after implementation costs will be lower. The magnitude is directionally correct, but the precise figure for any given company will depend on corridor mix, volume, and existing infrastructure.*

### Treasury Efficiency

If a bank maintains $500 million in pre-funded nostro accounts earning 0.5% (typical for demand deposits), and could instead deploy that capital at 4.5% (current US Treasury rate), the opportunity cost is:

$500M x (4.5% - 0.5%) = **$20 million per year in foregone yield.**

*This is a simplified calculation. In practice, some liquidity buffer would still be required even with stablecoin settlement, and the full $500M would not be freed. But the directional scale -- tens of millions of dollars per year for a mid-sized bank -- is why JPMorgan built Kinexys and why Fnality raised $136M.*

### Speed as Competitive Advantage

A US importer pays a Chinese manufacturer. Traditional flow: wire initiated Friday, arrives Tuesday (3 business days). Stablecoin flow: arrives and confirms within minutes.

Three business days saved per payment cycle. For a company with 10 payment cycles per month, that is 30 business days of cumulative settlement acceleration. This translates directly to faster inventory turns, reduced working capital needs, and improved supplier relationships.

### Competitive Pressure

This is the underappreciated driver. When one company in an industry adopts stablecoin rails and achieves 3-day faster settlement, 50-80% lower cross-border costs, and 24/7 treasury operations, its competitors face a choice: adopt or accept a structural disadvantage.

- Bitso's stablecoin-powered remittance corridor forced Western Union and MoneyGram to lower fees in the US-Mexico corridor.
- Stripe's stablecoin financial accounts (101 countries) create pressure on every other payment processor.
- JPMorgan's Kinexys gives JPM clients a treasury efficiency advantage that other banks' clients lack.

### Regulatory Push Factors

The regulatory environment has shifted from hostile to constructive:

- **GENIUS Act** (signed 2025): First US federal stablecoin framework. Implementation by January 2027.
- **MiCA** (EU): Fully in force. Comprehensive framework for crypto-asset service providers.
- **Hong Kong, Singapore, UAE:** All have functioning stablecoin frameworks.
- **SAB 121 rescission:** Removed barriers to bank custody of crypto assets.
- **SEC-CFTC Joint Guidance** (March 2026): Named 16+ tokens as digital commodities, providing clarity.

The business case for institutions is no longer "should we explore this?" It is "the regulatory framework now exists, and our competitors are already moving."

---

## 7. Data Sources and Methodology

### Source Categories

This report synthesizes data from the following source types:

**Official/Regulatory Sources:** World Bank Remittance Prices Worldwide (Q1 2025), Nacha (ACH 2025 statistics), Federal Reserve Services (Fedwire data), FATF 2025 Targeted Update on VA/VASPs, FASB ASU 2023-08 and related Deloitte/Bloomberg Tax analysis, IRS Notice 2014-21, GENIUS Act (Congress.gov), MiCA Transfer of Funds Regulation.

**Company-Reported Data (flagged as such):** Stripe/Bridge ($4.8B monthly), JPMorgan Kinexys ($7B daily), Bitso ($6.5B US-Mexico), Chainlink CCIP ($18B monthly as of March 2026), Circle CCTP ($110B+ cumulative), Payy ($130M annualized, 100K users -- unverified seed-stage claims), Toku ($1B+ annual payroll), LayerZero (75% bridge volume share).

**Financial Press and Analyst Reports:** CoinDesk, The Block, American Banker, Bloomberg Tax, PYMNTS, Artemis Research, CoinTelegraph, Morningstar.

### Key Figures Cross-Referenced

| Data Point | Value | Source | Verification Status |
|---|---|---|---|
| Stablecoin market cap (March 2026) | ~$300-320B | CryptoTicker, KuCoin, Macquarie/CoinDesk | Verified across 3+ sources |
| Onchain stablecoin transfer volume (2025) | $33T | Bloomberg, Yahoo Finance | Verified; 72% YoY growth |
| Actual stablecoin payments volume | ~$390B annualized | Plasma/Stablecoin Insider (McKinsey/Artemis methodology) | Single source; methodology-dependent |
| World Bank remittance cost (global avg) | 6.49% for $200 | World Bank RPW Q1 2025 | Official source |
| FATF Travel Rule adoption | 85 of 163 jurisdictions (52%) | FATF 2025 Targeted Update | Official source |
| Crypto-linked card spending | ~$18B annualized | CoinDesk (Jan 2026), Artemis Research | Verified |
| Visa annual payment volume | ~$15T | Visa fiscal 2025 data | Range of $14.2T-$16.7T depending on methodology |
| BlackRock BUIDL AUM | ~$18B (Feb 2026) | Multiple sources | Verified |

### What Could Not Be Independently Verified

- Payy's $130M annualized volume and 100K users (seed-stage self-report)
- The precise $400B-$1T nostro account estimate (no authoritative primary source)
- Stripe's $1.9T processing volume (private company, press-reported)
- Compliance tech market size ($2.14B in 2025, $4.87B by 2028 -- no named research firm cited)
- B2B stablecoin payment growth figures (<$100M to >$3B monthly -- directional claim supported but specific figures unattributed)

### Analytical Framework Disclosure

The five-layer payments stack model used in Section 1 is a custom analytical framework constructed for this report. It is not an industry-standard taxonomy. The layers map to real infrastructure (SWIFT = messaging, CHIPS = clearing, Fedwire = settlement) and the crypto equivalents are accurately identified, but the clean five-layer separation is an analytical idealization. In practice, these layers overlap -- card network authorization includes clearing elements, CHIPS performs both clearing and settlement.

### Audit Corrections Applied

This report incorporates corrections identified in QA audits of the underlying research:

- FATF Travel Rule: 85 of 163 jurisdictions (52%), not 85 of 117 (73%). The 117 figure uses an outdated FATF membership count; the actual survey covers 163 jurisdictions.
- Chainlink CCIP: $18B monthly as of March 2026, updated from the $7.77B 2025 snapshot.
- Aztec Network: Described as alpha testnet with critical vulnerability (March 17, 2026), not production mainnet.
- Marqeta revenue: $507M (2024), corrected from $700M+.
- BlackRock BUIDL: $18B AUM, corrected from $1B.
- Fedwire: Note added on October 2025 expansion announcement (Sunday operations, 2028+ implementation).
- Gemini survey attribution added for 38% barrier statistic.

No data has been fabricated. Where a specific figure could not be verified, it is either omitted or explicitly flagged.

---

*Research synthesis prepared March 31, 2026 | ethreportseth.xyz*
