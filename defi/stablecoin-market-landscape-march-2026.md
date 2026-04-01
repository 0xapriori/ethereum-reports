# Stablecoin Market Landscape & Ecosystem Players

**Research Brief -- March 31, 2026**

---

**tl;dr**

- **The stablecoin market cap hit ~$318B in early 2026**, up from $205B at the start of 2025 -- a 55%+ annual growth rate that makes it arguably the fastest-growing financial primitive in crypto
- **$33 trillion in raw transaction volume in 2025** (72% YoY growth), but McKinsey/Artemis found only ~$390B (~1%) represents real-world payments -- the rest is trading, arbitrage, and smart contract mechanics
- **B2B payments are the breakout vertical**, accounting for $226B (60% of real payment volume), up 733% YoY -- this is where the money is moving
- **Infrastructure M&A is accelerating**: Stripe acquired Bridge ($1.1B), Mastercard acquired BVNK ($1.8B), and the card networks are racing to own stablecoin settlement rails
- **The GENIUS Act (enacted July 2025)** has created the first federal stablecoin framework; OCC, FDIC, and NCUA are all in active rulemaking with a July 2026 deadline
- **Tether launched USAT for US compliance**, Circle IPO'd at $32B market cap, and 10+ banks have issued or are developing stablecoins -- the issuer landscape is fragmenting rapidly

---

## Table of Contents

1. [Market Size & Growth](#1-market-size--growth)
2. [Chain Dominance & Distribution](#2-chain-dominance--distribution)
3. [Ecosystem Players & Strategies](#3-ecosystem-players--strategies)
4. [Use Cases by Vertical](#4-use-cases-by-vertical)
5. [Key Distribution Channels](#5-key-distribution-channels)
6. [Regulatory Landscape](#6-regulatory-landscape)
7. [Potential Payy Partners & High-Volume Teams](#7-potential-payy-partners--high-volume-teams)
8. [Source Index](#8-source-index)

---

## 1. Market Size & Growth

### Total Market Cap

| Period | Total Stablecoin Market Cap | Notable |
|--------|---------------------------|---------|
| Jan 2025 | ~$205B | Start of year baseline |
| Dec 2025 | ~$300B | Per Arkham Research |
| Jan 6, 2026 | $317.94B | MEXC News reporting |
| March 2026 | ~$320B+ (est.) | Extrapolating trend |

Source: [Arkham Research](https://info.arkm.com/research/how-stablecoins-reached-a-300-billion-market-cap-in-2025), [MEXC News](https://www.mexc.co/news/421705), [DefiLlama](https://defillama.com/stablecoins)

### Individual Stablecoin Supply (as of early 2026)

| Stablecoin | Market Cap | Market Share | Issuer |
|-----------|-----------|-------------|--------|
| USDT | $187.0B | ~60.4% | Tether |
| USDC | $75.7B | ~24.4% | Circle |
| USDS (fka DAI) | ~$8-10B (est.) | ~3% | Sky Protocol (fka MakerDAO) |
| PYUSD | Data not confirmed | <1% | PayPal |
| USAT | Newly launched Jan 2026 | Nascent | Tether (US entity) |

Source: [CoinMarketCap](https://coinmarketcap.com/view/stablecoin/), [Crystal Intelligence](https://crystalintelligence.com/thought-leadership/usdt-maintains-dominance-while-usdc-faces-headwinds/)

### Transaction Volume

| Metric | Value | Source |
|--------|-------|--------|
| 2025 raw on-chain transaction volume | $33 trillion | [Bloomberg](https://www.bloomberg.com/news/articles/2026-01-08/stablecoin-transactions-rose-to-record-33-trillion-led-by-usdc) |
| 2025 YoY growth (raw volume) | 72% | [Yahoo Finance](https://finance.yahoo.com/news/stablecoin-transactions-soared-72-2025-054951388.html) |
| 2025 actual payment volume (McKinsey/Artemis adjusted) | ~$390B | [McKinsey](https://www.mckinsey.com/industries/financial-services/our-insights/stablecoins-in-payments-what-the-raw-transaction-numbers-miss) |
| USDC 2025 transaction volume | $18.3T (raw) | Bloomberg |
| USDT 2025 transaction volume | $13.3T (raw) | Bloomberg |
| Q4 2025 alone | $11T (raw) | Yahoo Finance |

**Critical distinction (per CLAUDE.md rules and McKinsey):** The $33T raw volume and the $390B actual payment volume are fundamentally different metrics. The $33T includes trading, arbitrage, smart contract interactions, and liquidity cycling. Only ~1% represents real-world payments. Any analysis of the "payments opportunity" must use the $390B figure as the baseline.

### Is "Fastest Growing Financial Primitive" Defensible?

**Yes, with caveats.** The evidence:

- Market cap: $205B to $318B in 12 months (55%+ growth)
- Raw volume: 72% YoY growth to $33T
- Actual payments: Doubled from ~$195B (2024) to ~$390B (2025)
- B2B payments specifically: Up 733% YoY
- US Treasury Secretary Bessent projected $3T supply by 2030
- Bloomberg Intelligence projected $56T in payment flows by 2030

The claim is strongest when scoped to "fastest-growing financial primitive by total capital deployed and transaction throughput." DeFi lending and DEX volumes have not grown at comparable rates in the same period.

---

## 2. Chain Dominance & Distribution

### Stablecoin Supply by Chain

| Chain | Stablecoin Supply | % of Total | Primary Use |
|-------|-------------------|-----------|-------------|
| Ethereum (L1) | ~$184.8B | ~56% | DeFi, institutional settlement |
| Tron | ~$78.5B | ~24% | Remittances, small-ticket transfers |
| Solana | ~$14.4B | ~4.5% | Trading, retail payments |
| Base (Coinbase L2) | ~$4.53B | ~1.4% | Consumer apps, card settlement |
| Arbitrum | Data not confirmed | Est. 1-2% | DeFi, enterprise |
| Other EVM L2s | Combined ~$15B+ | ~5% | Various |

Source: [DefiLlama Stablecoins by Chain](https://defillama.com/stablecoins/chains), [Tatum](https://tatum.io/blog/stablecoins-across-blockchains), [CryptoRank](https://cryptorank.io/news/feed/ebc47-ethereum-leads-the-stablecoin-market)

### Stablecoin Transaction Volume by Chain (different from supply)

The volume picture tells a very different story than supply:

- **Solana** overtook Ethereum in stablecoin transaction volume for the first time in early 2026, commanding 37-74% of total volume in February 2026
- **Tron** consistently handles 30-40% of total volume, processing $600B+ monthly in transfers (mostly small-ticket remittances under $1,000)
- **Ethereum + L2s** retain supply dominance but have lost volume share to faster/cheaper chains

Source: [ETHNews](https://www.ethnews.com/solana-took-the-lead-in-stablecoin-volume-for-the-first-time/), [Alchemy](https://www.alchemy.com/blog/the-stablecoin-landscape-across-different-chains)

**Key insight:** Supply lives on Ethereum (institutional trust, DeFi composability). Volume lives on Solana and Tron (speed, cost). Any stablecoin payments product must be multi-chain.

---

## 3. Ecosystem Players & Strategies

### 3A. Stablecoin Issuers

#### Tether

- **Market position:** $187B market cap, 60.4% dominance, largest stablecoin by far
- **2026 strategy:** Launched **USAT** (January 27, 2026) -- a new US-regulated, GENIUS Act-compliant stablecoin targeting American financial institutions
- **Leadership move:** Appointed Bo Hines (former White House Crypto Council Executive Director) as CEO of Tether USA
- **Transparency push:** Hired KPMG for full USDT audit, PwC for internal systems -- preparing for US expansion
- **Diversification:** Invested $200M in Whop (digital marketplace) and $50M in Eight Sleep -- signaling a platform strategy beyond pure stablecoin issuance
- **Risk factor:** USDT remains less likely to be the compliant stablecoin that Visa, Western Union, and European banks standardize on for mainstream retail payments

Source: [CoinDesk](https://www.coindesk.com/markets/2026/03/27/tether-hires-kpmg-for-usdt-audit-brings-in-pwc-as-it-gears-up-for-u-s-expansion/), [FXC Intelligence](https://www.fxcintel.com/research/reports/ct-usat-stablecoin-launch)

#### Circle (USDC)

- **Market position:** $75.7B market cap, ~24% market share
- **IPO:** Went public on NYSE June 4, 2025 (ticker: CRCL). Market cap ~$32B as of March 2026
- **FY2025 revenue:** $2.7B (64% YoY increase), adjusted EBITDA $582M (21.5% margin)
- **Revenue model:** Hybrid asset manager + SaaS
  - Reserve interest income (yield on $75B+ in short-term Treasuries backing USDC)
  - Transaction/platform fees via Circle Mint and developer APIs
  - Programmable Wallets (subscription + usage-based billing)
- **Transaction volume leadership:** USDC captured 64% of stablecoin transaction volume by March 2026 (vs. USDT's market cap dominance) -- USDC is the preferred settlement stablecoin
- **OCC approval:** Circle received conditional approval for national trust bank charter (December 2025)
- **Balance sheet:** $1.2B+ in corporate cash, zero long-term debt

Source: [FinancialContent/Finterra](https://markets.financialcontent.com/stocks/article/finterra-2026-3-18-the-regulated-dollar-a-deep-dive-into-circle-internet-groups-crcl-post-ipo-surge), [Bitget Academy](https://www.bitget.com/academy/how-does-circle-generate-revenue-in-2026-usdc-reserves-saas-global-financial-infrastructure), [SimplyWall.St](https://simplywall.st/community/narratives/us/software/nyse-crcl/circle-internet-group/s8t34ret-circle-internet-group-crcl-the-programmable-dollar-powerhouse-post-ipo-momentum-and-stablecoin-dominance)

#### PayPal (PYUSD)

- **Strategy:** Leveraging PayPal + Venmo's massive consumer base (~430M accounts) for distribution
- **CEO Alex Chriss** committed to expanding PYUSD in 2026, scaling the digital wallet
- **Positioning:** Inside access to existing consumer payment network gives PYUSD a distribution advantage no other stablecoin has
- **Limitation:** PYUSD market cap remains small relative to USDT/USDC; adoption is primarily within the PayPal ecosystem

Source: [American Banker](https://www.americanbanker.com/news/payment-fintechs-push-stablecoin-tech-for-2026)

#### Sky Protocol (fka MakerDAO) -- USDS

- **Rebranded from MakerDAO in 2025**, DAI upgraded to USDS
- **Strategy:** Yield-bearing stablecoin via Sky Savings Rate and sDAI
- **Differentiator:** Decentralized, overcollateralized, DeFi-native -- positioned for users who want yield + decentralization vs. fiat-backed stability
- **Niche:** Treasury management and DeFi composability rather than payments

Source: [Transak](https://transak.com/blog/stablecoin-playbook-2026)

### 3B. Infrastructure Layer

#### Bridge (Stripe-owned)

- **Acquisition:** Stripe acquired Bridge for $1.1B (closed February 2025) -- Stripe's largest acquisition ever
- **OCC charter:** Received conditional approval for national trust bank charter (February 17, 2026), enabling Bridge to issue stablecoins, custody digital assets, and manage reserves under federal supervision
- **Product expansion:** Visa + Bridge announced stablecoin-linked Visa cards expanding to 100+ countries by end of 2026 (live in 18 countries currently)
- **Win:** Secured contract to issue USDH stablecoin for Hyperliquid, beating Paxos
- **Strategic significance:** Bridge gives Stripe native stablecoin issuance and settlement infrastructure

Source: [CNBC](https://www.cnbc.com/2025/02/04/stripe-closes-1point1-billion-bridge-deal-prepares-for-stablecoin-push-.html), [CoinDesk](https://www.coindesk.com/business/2026/02/17/stripe-s-stablecoin-firm-bridge-wins-initial-approval-of-national-bank-trust-charter), [The Defiant](https://thedefiant.io/news/tradfi-and-fintech/visa-and-stripe-owned-bridge-roll-out-stablecoin-linked-cards-to-100-countries)

#### BVNK (Mastercard-acquired)

- **Acquisition:** Mastercard agreed to acquire BVNK for up to $1.8B -- the biggest stablecoin-focused transaction on record
- **Scale:** Processing ~$30B in annual transaction volume
- **Valuation context:** Previously valued at $750M+
- **Strategic role:** Gives Mastercard native stablecoin settlement infrastructure to compete with Stripe/Bridge

Source: [Finovate](https://finovate.com/mastercard-acquires-stablecoin-infrastructure-bvnk-for-1-8-billion/), [Blockhead](https://www.blockhead.co/2026/03/18/mastercard-acquires-bvnk-to-expand-stablecoin-settlement-infrastructure/), [CoinDesk](https://www.coindesk.com/opinion/2026/03/27/why-mastercard-paid-double-for-stablecoin-infrastructure-it-could-have-built)

#### Paxos

- **OCC approval:** Received conditional national trust bank charter (December 2025), alongside Circle, Ripple, BitGo, and Fidelity Digital Assets
- **Products:** Issues USDP (Pax Dollar), powers stablecoin for various partners
- **Competition:** Lost Hyperliquid contract to Bridge

Source: [CoinDesk](https://www.coindesk.com/business/2026/02/17/stripe-s-stablecoin-firm-bridge-wins-initial-approval-of-national-bank-trust-charter)

#### Fireblocks

- **Scale:** Processes over $200B in stablecoin transaction volume per month -- significantly larger than other infrastructure providers
- **Role:** Enterprise custody and transaction infrastructure, not an issuer
- **Clients:** Institutional-grade infrastructure serving banks, fintechs, and exchanges

Source: [Fireblocks](https://www.fireblocks.com/report/state-of-stablecoins)

#### Conduit, Brale

- **Data gap:** I was unable to find specific current volume data or detailed strategy updates for Conduit or Brale in my search results. These are known as stablecoin issuance-as-a-service platforms (Brale enables companies to launch their own branded stablecoins), but specific 2026 metrics were not surfaced. **This is a gap that should be filled with direct outreach or deeper research.**

### 3C. Banks Exploring Stablecoin Issuance / Tokenized Deposits

| Institution | Product | Status | Notes |
|-------------|---------|--------|-------|
| JPMorgan (Kinexys) | JPM Coin (JPMD) | Live, expanding | Now on Base (Coinbase L2) and Canton Network. Interest-bearing deposit token. Clients: B2C2, Coinbase, Mastercard |
| Societe Generale (SG-FORGE) | EURCV (EUR), USDCV (USD) | Live | MiCA-compliant since July 2024. USDCV launched June 2025 with BNY Mellon as custodian. Expanding to Solana, Stellar, XRP Ledger |
| ING, UniCredit, KBC, DekaBank | Euro stablecoin (joint venture) | In development | Formed a company in the Netherlands, expected to launch in 2026 |
| NAB (Australia) | AUDN | Discontinued | Pilot ended |
| ANZ Bank (Australia) | A$DC | Pilot | AUD stablecoin |
| Bancolombia | COPW | Live | Colombian peso stablecoin |
| Sumitomo Mitsui | Details not confirmed | Exploration stage | Mentioned in bank-issued stablecoin lists |

**Notable:** JPM Coin (JPMD) is a deposit token, not a stablecoin -- it is interest-bearing, which is explicitly prohibited for stablecoin issuers under the GENIUS Act. This distinction matters: banks can offer yield on tokenized deposits; stablecoin issuers cannot.

Source: [CoinDesk](https://www.coindesk.com/tech/2026/01/07/jpmorgan-to-issue-its-jpm-stablecoin-directly-on-privacy-focused-canton-network), [J.P. Morgan](https://www.jpmorgan.com/kinexys/digital-payments/jpm-coin), [StablecoinInsider](https://stablecoininsider.org/bank-issued-stablecoins-complete-list-for-2025/), [CoinGlass](https://www.coinglass.com/news/741334)

### 3D. Fintechs Adding Onchain Stablecoin Rails

| Company | Activity | Scale/Detail |
|---------|----------|-------------|
| Stripe | Acquired Bridge ($1.1B), stablecoin-linked Visa cards to 100+ countries | Stripe's largest acquisition; Bridge pursuing national bank charter |
| PayPal | Issuing PYUSD, scaling via PayPal/Venmo ecosystem | ~430M account distribution advantage |
| Revolut | $1.2B+ cumulative on-chain tx volume on Polygon alone; 156% YoY stablecoin volume growth in 2025 (~$10.5B across chains) | Selected for UK FCA sandbox to pilot GBP stablecoin |
| Wise | Routes internal liquidity through stablecoins (not branded as crypto) | Specific volume data not confirmed |
| SoFi | Partnership with Mastercard to enable bank-issued stablecoin settlement | Details of specific stablecoin not confirmed |
| Nium | Launched dual-network stablecoin card platform on Visa and Mastercard (March 2026) | Cross-border payments infrastructure |
| Bitso | $6.5B in US-Mexico crypto remittances in 2024 (~10% of corridor) | Latin America's largest crypto exchange |

Source: [Cryptonomist](https://en.cryptonomist.ch/2026/03/26/revolut-crosses-1-2b-onchain-transactions-polygon/), [Blockhead](https://www.blockhead.co/2026/03/31/nium-launches-dual-network-stablecoin-card-platform-on-visa-mastercard/), [CoinDesk](https://www.coindesk.com/business/2026/01/16/crypto-card-spending-hits-usd18-billion-annualized-as-stablecoin-use-shifts-to-everyday-payments)

**Key trend:** 8 out of 10 top neobanks reportedly use stablecoin rails internally for treasury settlement, liquidity routing, or cross-border corridors -- most without branding it as "crypto" to consumers.

Source: [StablecoinInsider](https://stablecoininsider.org/the-neobank-disruption-report/)

---

## 4. Use Cases by Vertical

### 4A. Cross-Border Payments & Remittances

- **Total stablecoin payment volume (adjusted):** ~$390B in 2025, doubled from 2024
- **B2B payments:** $226B (60% of total), up 733% YoY
- **Monthly B2B volume:** From under $100M in early 2023 to over $3B by 2025 (30x in 2 years)
- **Asia-Pacific dominance:** $245B (60% of total payment volume). Singapore-China corridor is the most active route
- **Latin America:** Bitso processed $6.5B in US-Mexico crypto remittances in 2024 (~10% of entire corridor). Fees under 1% vs. 5-7% traditional
- **Cost advantage:** Stablecoin remittances settle near-instantly at fees below $1.00, vs. traditional fees exceeding 6%
- **2030 projection:** Stablecoins could handle 5-10% of all cross-border payments ($2.1-4.2T annually)

Source: [McKinsey](https://www.mckinsey.com/industries/financial-services/our-insights/stablecoins-in-payments-what-the-raw-transaction-numbers-miss), [Reap](https://reap.global/newsroom/b2b-stablecoin-payments-surge-30x-to-3-billion-monthly-volume-in-2025), [Polygon Blog](https://polygon.technology/blog/latam-corridor-economics-why-enterprises-are-betting-on-stablecoins-for-cross-border-payments)

### 4B. Payroll on Stablecoins

- **Status:** Market has crossed the adoption threshold in 2026; six major platforms have live or actively rolling-out features
- **Key player:** Rise is the most prominent, shipping yield on idle payroll balances via Rise Earn (built on Arbitrum, powered by Aave)
- **Acquisition activity:** Paystand acquired Bitwage (November 3, 2025), integrating cross-border payroll infrastructure into AP/AR automation
- **Emerging dynamic:** As companies prefund larger balances on-chain for payroll, the ability to earn yield on idle capital is becoming a material treasury consideration
- **Use case:** Primarily international contractors and remote workforce (not yet mainstream for domestic W-2 payroll)

Source: [StablecoinInsider](https://stablecoininsider.org/stablecoin-payroll-2026/), [Convera](https://convera.com/blog/cross-border-payments/stablecoin-use-cases/)

### 4C. B2B Settlement

- **Largest segment:** $226B in 2025, 60% of all stablecoin payment volume
- **Growth rate:** 733% YoY -- the fastest-growing stablecoin use case
- **Use cases:** Invoice settlement, international payroll, inter-company treasury rebalancing
- **Speed advantage:** Minutes vs. days for international settlement
- **7-day availability:** Eliminates weekend and holiday liquidity gaps

Source: [McKinsey](https://www.mckinsey.com/industries/financial-services/our-insights/stablecoins-in-payments-what-the-raw-transaction-numbers-miss)

### 4D. Treasury Management

- **Yield-bearing stablecoins:** Grew from $9.5B (start of 2025) to $20B+ (early 2026), with average yields ~5%
- **Key products:** sDAI/sUSDS (Sky Protocol), various structured products
- **Corporate adoption:** Businesses using stablecoins to rebalance treasury positions across regions in minutes rather than days
- **GENIUS Act constraint:** Stablecoin issuers cannot directly offer interest. Banks can offer interest on tokenized deposits (JPM Coin). This creates a structural advantage for bank-issued deposit tokens in treasury management

Source: [AlphaPoint](https://alphapoint.com/blog/stablecoin-treasury-management-for-institutions-the-definitive-2026-guide/), [PwC](https://www.pwc.com/us/en/tech-effect/emerging-tech/stablecoin-for-treasurers.html)

### 4E. Card Settlement Programs

- **Annualized crypto card spending:** ~$18B (as of late 2025/early 2026), up from ~$100M in early 2023 (~15x growth)
- **Stablecoin-linked card spending specifically:** $4.5B in 2025, 673% increase from prior year
- **Monthly run rate:** Over $100M/month by early 2026
- **Visa dominance:** >90% of on-chain crypto card volume, despite both Visa and Mastercard supporting 130+ crypto card programs
- **Visa settlement run rate:** $3.5B by November 2025, expanded to 40+ countries

Source: [CoinDesk](https://www.coindesk.com/business/2026/01/16/crypto-card-spending-hits-usd18-billion-annualized-as-stablecoin-use-shifts-to-everyday-payments), [Insights4VC](https://insights4vc.substack.com/p/the-state-of-stablecoin-cards)

---

## 5. Key Distribution Channels

### 5A. Crypto Card Programs

| Player | Role | Scale | Notes |
|--------|------|-------|-------|
| Visa | Network, settlement | >90% of on-chain crypto card volume, 175M merchant locations | Bridge partnership: stablecoin cards to 100+ countries |
| Mastercard | Network | 130+ crypto card programs | Acquired BVNK ($1.8B); SoFi partnership for stablecoin settlement |
| Rain | Issuer/processor | $3B+ annualized, 200+ partners | Direct Visa member; 38x growth in past year. Partners: Western Union, Nuvei, KAST |
| Reap | Issuer/processor | $6B+ annualized | Corporate card focus, skewing toward enterprise spend |
| Nium | Infrastructure | Dual-network platform (launched March 2026) | Cross-border payments infrastructure on both Visa and Mastercard |

Source: [Rain](https://www.rain.xyz/resources/rain-raises-250m-series-c-to-scale-stablecoin-powered-payments-infrastructure-for-global-enterprises), [Insights4VC](https://insights4vc.substack.com/p/the-state-of-stablecoin-cards), [Blockhead](https://www.blockhead.co/2026/03/31/nium-launches-dual-network-stablecoin-card-platform-on-visa-mastercard/)

### 5B. Crypto Neobanks & Fintechs

| Company | Type | Stablecoin Activity |
|---------|------|---------------------|
| Revolut | Neobank | $10.5B+ stablecoin volume (2025), $1.2B on Polygon alone, piloting GBP stablecoin in FCA sandbox |
| SoFi | Neobank/bank | Mastercard partnership for stablecoin settlement |
| PayPal/Venmo | Fintech | PYUSD issuer, ~430M user base |
| Bitso | Exchange/fintech | $6.5B US-Mexico corridor (2024), dominant LATAM player |
| Bleap | Web3 neobank | Stablecoin-native banking |
| Various LATAM neobanks | Neobanks | Route internal liquidity through stablecoins without branding it as crypto |

Source: [Bleap](https://www.bleap.finance/en-us/blog/best-neobanks-web2-web3-and-hybrid), [Cryptonomist](https://en.cryptonomist.ch/2026/03/26/revolut-crosses-1-2b-onchain-transactions-polygon/)

### 5C. Stablecoin Infrastructure Providers (by volume tier)

**Tier 1: $100B+/month**
- Fireblocks -- $200B+/month in stablecoin transaction volume (enterprise custody + settlement infrastructure)

**Tier 2: $10-50B/year**
- BVNK (now Mastercard) -- ~$30B annual transaction volume
- Bridge (now Stripe) -- Volume not publicly disclosed but significant given Visa partnership to 100+ countries

**Tier 3: $1-10B/year annualized**
- Rain -- $3B+ annualized (card volume)
- Reap -- $6B+ annualized (card volume, corporate focus)

**Other notable infrastructure:**
- Paxos -- Stablecoin issuance infrastructure, OCC-approved
- Circle Mint -- Institutional USDC minting/redemption
- Conduit -- Stablecoin issuance-as-a-service (specific volume not confirmed)
- Brale -- Branded stablecoin issuance platform (specific volume not confirmed)
- Routefusion -- Stablecoin payment routing (compared to Bridge and BVNK in marketing materials)

Source: [Fireblocks](https://www.fireblocks.com/report/state-of-stablecoins), [Rain PR](https://www.prnewswire.com/news-releases/rain-raises-250m-series-c-to-scale-stablecoin-powered-payments-infrastructure-for-global-enterprises-302657084.html), [Routefusion](https://www.routefusion.com/blog/bridge-vs-bvnk-vs-routefusion-comparison)

---

## 6. Regulatory Landscape

### GENIUS Act (US)

- **Enacted:** July 18, 2025 -- first federal stablecoin legislation in the US
- **Rulemaking deadline:** July 18, 2026 (one year from enactment)
- **Effective date:** Earlier of 18 months from enactment OR 120 days after final regulations
- **Key requirements:**
  - 1:1 reserve backing with specified assets (USD, short-term Treasuries)
  - Stablecoin issuers cannot directly offer interest (deposit tokens from banks can)
  - Federal supervision pathway via OCC national trust bank charters
- **Regulatory progress:**
  - OCC: Issued notice of proposed rulemaking (February 25, 2026)
  - FDIC: Proposed framework by end of 2025, finalizing early 2026
  - NCUA: Submitted rulemaking to OMB, pending review
- **OCC approvals granted:**
  - December 2025: Circle, Ripple, BitGo, Fidelity Digital Assets, Paxos (conditional)
  - February 2026: Bridge/Stripe (conditional)

Source: [Congress.gov](https://www.congress.gov/bill/119th-congress/senate-bill/394/text), [OCC](https://www.occ.treas.gov/news-issuances/bulletins/2026/bulletin-2026-3.html), [FDIC](https://www.fdic.gov/news/press-releases/2025/fdic-approves-proposal-establish-genius-act-application-procedures-fdic), [Latham & Watkins](https://www.lw.com/en/insights/the-genius-act-of-2025-stablecoin-legislation-adopted-in-the-us), [Gibson Dunn](https://www.gibsondunn.com/the-genius-act-a-new-era-of-stablecoin-regulation/)

### Europe (MiCA)

- MiCA framework live since mid-2024
- Societe Generale EURCV is MiCA-compliant since July 2024
- ING/UniCredit/KBC/DekaBank forming joint euro stablecoin venture for 2026 launch

### Key Regulatory Insight for Product Design

The GENIUS Act's prohibition on interest-bearing stablecoins creates a structural gap: users hold stablecoins for payments but cannot earn yield from the issuer directly. This makes yield aggregation, sweep-to-DeFi, and treasury optimization a natural product layer on top of stablecoin rails.

---

## 7. Potential Payy Partners & High-Volume Teams

Based on the data above, teams doing billions in stablecoin volume monthly that could be Payy partners or integration targets:

### Card Issuers / Processors

| Company | Volume | Why Relevant |
|---------|--------|-------------|
| Rain | $3B+ annualized, 200+ partners | Direct Visa member, enterprise card infrastructure. Partners include Western Union, Nuvei, KAST. Just raised $250M Series C |
| Reap | $6B+ annualized | Corporate card focus, large enterprise clients |
| Nium | Not confirmed but dual-network | Cross-border payments infrastructure, just launched stablecoin card platform |

### Settlement / Infrastructure

| Company | Volume | Why Relevant |
|---------|--------|-------------|
| Fireblocks | $200B+/month | Institutional custody + settlement, processes more than anyone |
| BVNK (Mastercard) | ~$30B/year | Now owned by Mastercard, stablecoin settlement specialist |
| Bridge (Stripe) | Significant but undisclosed | National bank charter, Visa partnership to 100+ countries |

### Fintechs / Neobanks

| Company | Volume | Why Relevant |
|---------|--------|-------------|
| Revolut | $10.5B+ in 2025 stablecoin volume | 45M+ users, piloting GBP stablecoin, deep Polygon integration |
| Bitso | $6.5B in US-Mexico corridor (2024) | LATAM dominant, remittance focus |
| PayPal/Venmo | Not separately broken out | 430M accounts, PYUSD distribution |

### B2B / Payroll

| Company | Volume | Why Relevant |
|---------|--------|-------------|
| Rise | Not confirmed | Only platform shipping yield on idle payroll balances (Arbitrum/Aave) |
| Paystand/Bitwage | Not confirmed | Combined AP/AR + payroll automation |

### Unconfirmed / Gaps

- **Conduit:** Known stablecoin infrastructure player, specific volume not found. Needs direct research
- **Brale:** Branded stablecoin issuance platform, specific volume not found. Needs direct research
- **Wise internal stablecoin routing:** Confirmed as happening but volume not disclosed publicly
- **Southeast Asia invisible stablecoin payments:** CoinDesk reported stablecoin payments going "invisible" in Southeast Asia with surging card business (March 2026), but specific companies/volumes not detailed

---

## 8. Source Index

All data points in this report are sourced from the following. Items marked with an asterisk (*) are claims I found in search result summaries but could not independently verify against a second source.

**Market Data & Analytics**
- [DefiLlama Stablecoins](https://defillama.com/stablecoins)
- [DefiLlama Stablecoins by Chain](https://defillama.com/stablecoins/chains)
- [CoinMarketCap Stablecoin View](https://coinmarketcap.com/view/stablecoin/)
- [CoinGlass Stablecoin Data](https://www.coinglass.com/pro/stablecoin)
- [Visa On-Chain Analytics](https://visaonchainanalytics.com/supply)
- [Artemis Stablecoins](https://app.artemisanalytics.com/stablecoins)

**Research & Analysis**
- [McKinsey: Stablecoins in Payments](https://www.mckinsey.com/industries/financial-services/our-insights/stablecoins-in-payments-what-the-raw-transaction-numbers-miss)
- [Arkham: How Stablecoins Reached $300B](https://info.arkm.com/research/how-stablecoins-reached-a-300-billion-market-cap-in-2025)
- [Bloomberg: Stablecoin Transactions Record $33T](https://www.bloomberg.com/news/articles/2026-01-08/stablecoin-transactions-rose-to-record-33-trillion-led-by-usdc)
- [Artemis/CoinDesk: Stablecoin Payments at Scale](https://research.artemisanalytics.com/p/stablecoin-payments-at-scale-how)
- [Insights4VC: State of Stablecoin Cards](https://insights4vc.substack.com/p/the-state-of-stablecoin-cards)
- [Fireblocks: State of Stablecoins](https://www.fireblocks.com/report/state-of-stablecoins)

**News & Industry**
- [CNBC: Stripe Closes Bridge Deal](https://www.cnbc.com/2025/02/04/stripe-closes-1point1-billion-bridge-deal-prepares-for-stablecoin-push-.html)
- [CoinDesk: Bridge OCC Approval](https://www.coindesk.com/business/2026/02/17/stripe-s-stablecoin-firm-bridge-wins-initial-approval-of-national-bank-trust-charter)
- [CoinDesk: Crypto Card Spending $18B](https://www.coindesk.com/business/2026/01/16/crypto-card-spending-hits-usd18-billion-annualized-as-stablecoin-use-shifts-to-everyday-payments)
- [CoinDesk: Mastercard/BVNK](https://www.coindesk.com/opinion/2026/03/27/why-mastercard-paid-double-for-stablecoin-infrastructure-it-could-have-built)
- [CoinDesk: Tether KPMG Audit](https://www.coindesk.com/markets/2026/03/27/tether-hires-kpmg-for-usdt-audit-brings-in-pwc-as-it-gears-up-for-u-s-expansion/)
- [Rain: $250M Series C](https://www.rain.xyz/resources/rain-raises-250m-series-c-to-scale-stablecoin-powered-payments-infrastructure-for-global-enterprises)
- [Cryptonomist: Revolut on Polygon](https://en.cryptonomist.ch/2026/03/26/revolut-crosses-1-2b-onchain-transactions-polygon/)

**Regulatory**
- [Congress.gov: GENIUS Act Text](https://www.congress.gov/bill/119th-congress/senate-bill/394/text)
- [OCC: GENIUS Act Proposed Rulemaking](https://www.occ.treas.gov/news-issuances/bulletins/2026/bulletin-2026-3.html)
- [FDIC: GENIUS Act Procedures](https://www.fdic.gov/news/press-releases/2025/fdic-approves-proposal-establish-genius-act-application-procedures-fdic)
- [Latham & Watkins: GENIUS Act Analysis](https://www.lw.com/en/insights/the-genius-act-of-2025-stablecoin-legislation-adopted-in-the-us)
- [Gibson Dunn: GENIUS Act Analysis](https://www.gibsondunn.com/the-genius-act-a-new-era-of-stablecoin-regulation/)

---

## Data Limitations & Flags

1. **PYUSD market cap:** I could not find a confirmed Q1 2026 market cap for PYUSD. Multiple sources mentioned it but without specific numbers
2. **Conduit and Brale:** No specific volume data or detailed 2026 strategy updates were surfaced. These need direct research or outreach
3. **Wise stablecoin routing:** Confirmed qualitatively (they route internal liquidity through stablecoins) but no volume figures disclosed
4. **Solana volume spike:** The 37-74% volume share range for Solana in February 2026 is a wide range across sources; the exact figure may depend on methodology
5. **"8 out of 10 neobanks" claim:** Sourced from StablecoinInsider, which is an industry publication -- this should be treated as directional rather than precise
6. **McKinsey $390B adjusted payment volume:** This is the most rigorous estimate available but relies on Artemis Analytics methodology for filtering out non-payment transactions. The exact methodology may exclude some legitimate payment flows or include some non-payment flows
7. **Stripe/PayPal acquisition rumors:** One search result mentioned Stripe exploring a PayPal acquisition. This was from Crypto Valley Journal and should be treated as speculation unless confirmed by primary sources
