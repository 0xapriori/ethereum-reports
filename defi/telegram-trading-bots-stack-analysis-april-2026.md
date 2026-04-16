---
title: "Competing with the Bots: Stack Analysis and Entrant Playbook for Telegram Trading, April 2026"
date: 2026-04-14
---

# Competing with the Bots: Stack Analysis and Entrant Playbook for Telegram Trading, April 2026

*A synthesis report. | April 2026*

## tl;dr

- **The Telegram trading bot category is already past its frontier-product phase and well into its rent-extraction phase.** Protocol revenue concentrates in six operators — Photon ($438.7M lifetime), Axiom ($401.0M), Trojan ($221.1M), BullX ($202.9M), Maestro ($137.8M), GMGN ($125.6M) — but top-three market share has fallen from ~70% in 2024 to ~45% TTM, and the 1% flat fee that used to look like a schelling point is now the most obvious competitive surface.

- **The product is a stack, not a chat window.** Wallet custody, intel, execution, routing, social graph, and monetization are separable layers — and incumbents are strong in at most two of them. Axiom won by replacing the Telegram UI with a purpose-built terminal while keeping everything else standard. GMGN won by treating wallet intelligence as the product and execution as a commodity. There is nothing special about Telegram as a substrate.

- **Every publicly disclosed loss traces to custody.** Unibot, Maestro, Banana Gun, Solareum/BonkBot adjacency, Polycule — the architecture is "non-custodial in name, operationally hot-wallet in practice." Server-side key custody is the single most exploitable seam in the stack and the one where marketing claims diverge most sharply from reality. A credible non-custodial replacement is the single largest unclaimed wedge.

- **Referrals have absorbed the gross margin and are now the distribution layer.** Trojan has paid out $57M+ in referral rewards and runs a 45% Titan tier. A new entrant cannot out-bribe Trojan or Photon in cash, but can rebuild the layer entirely — referrer-as-equity (tokenized affiliate positions), KOL-as-operator (white-label stack), or flow-auction models (sell the orderflow, rebate 100%).

- **Unibot is the category's ghost.** $3/day revenue, -87% MoM, from a 2023 category leader. The lesson is not that tokens don't work — Banana Gun is healthy — but that **chain-of-choice risk is catastrophic**. A bot that bets on the wrong chain, wrong UX substrate, or wrong asset class has no capacity to reallocate because the product *is* the bet. New entrants should plan for the transition they need to survive, not the market they want to enter.

- **Four displacement vectors are open:** (1) custody — a credibly non-custodial signer with competitive latency; (2) interface — mobile-native, Discord-native, Farcaster-native, or terminal-native replacements for the Telegram envelope; (3) social graph — trader reputation and copy-trade as the primary object, execution as a service; (4) asset class — perps, prediction markets, and RWAs are adjacent markets where the bot form factor is barely present.

- **The bear case is that none of this matters.** The category's revenue is memecoin-cycle-correlated. Axiom's growth is decelerating (-41% MoM). Photon is down 10%. Seven of the top 15 bots are down 20%+ MoM. In a world where Solana memecoins stay quiet, the sector's TAM compresses faster than any entrant can scale. The displacement windows below assume the category holds the floor it established in 2024–2025; if it does not, the right move is to build adjacent, not compete directly.

---

## Table of Contents

1. [Prologue — What "Bot" Actually Means](#i-prologue-----what-bot-actually-means)
2. [The Product Stack](#ii-the-product-stack)
3. [Where Incumbents Are Weak](#iii-where-incumbents-are-weak)
4. [The Moat Map](#iv-the-moat-map)
5. [Four Displacement Vectors](#v-four-displacement-vectors)
6. [Case Studies — What Worked, What Didn't](#vi-case-studies-----what-worked-what-didnt)
7. [A Credible New-Entrant Playbook](#vii-a-credible-new-entrant-playbook)
8. [Bear Cases](#viii-bear-cases)
9. [Data Sources & Methodology](#ix-data-sources--methodology)
10. [Sources](#x-sources)

---

# I. PROLOGUE --- What "Bot" Actually Means

The phrase "Telegram trading bot" is a category name that has outlived its definition. What started in 2023 as BonkBot and Unibot — command-driven Telegram handles that let users buy memecoins by typing a contract address — is now a category that includes web terminals (Axiom, Photon, BullX), hybrid mobile+desktop products (GMGN), and multi-chain execution platforms (Maestro) that happen to *also* be addressable through Telegram. The Telegram envelope is the weakest part of the product. Several of the most successful operators in the category generate the majority of their volume outside it.

The correct unit of analysis is not the chat interface. It is the **stack of services** that sit between a user's intent to buy a token and the final on-chain settlement. That stack has at least seven distinct layers, each of which is in principle separately competable. This report treats the category as a stack, asks which layers incumbents have actually won, and identifies where new entrants can build without trying to beat Photon's RPC cluster or Trojan's referral tree head-on.

A clarifying reframe before the analysis begins: **these are not trading bots. They are custodial hot wallets with an on-chain execution layer bolted to a social interface.** The trading is the demo. The custody is the architecture. The social interface is the distribution. Once the category is seen in those terms, the competitive questions sharpen considerably.

---

# II. The Product Stack

A Telegram trading bot — and, increasingly, its web terminal cousin — is a composition of seven layers. Each layer can be built in-house, outsourced, or commoditized. Each layer has different margin economics, different defensibility, and different exposure to technical risk.

**1. Interface.** The envelope through which the user sends commands. Today this is Telegram for BonkBot, Trojan, Banana Gun, Maestro, Shuriken, and Nova; a purpose-built web terminal for Axiom, Photon, BullX; a hybrid for GMGN. The interface is the most commodity layer in the stack and also the one where competitive displacement has already happened twice (Telegram → web terminal, and increasingly web terminal → mobile-native).

**2. Wallet & custody.** How the user's keys are generated, stored, and authorized for signing. This is uniformly the weakest layer across the category. Almost every operator generates an in-backend keypair on user signup, stores the private key in an operator-controlled database, and signs transactions server-side. "Non-custodial" marketing language means only that the user *can* export a seed phrase. In practice, the operator is the custodian.

**3. Intel & discovery.** What the user trades. This layer includes new-pair detection, whale-wallet tracking, honeypot and rug-pull scoring, on-chain analytics, token screeners, and copy-trade target lists. GMGN is the category leader here; Photon and BullX have invested heavily; Trojan and Maestro treat intel as secondary.

**4. Execution.** How orders are routed and confirmed. For Solana: RPC selection (Helius dedicated nodes, QuickNode, Jito, Yellowstone/LaserStream), Jito bundle tipping, priority fee optimization, failure-handling policy. For EVM: Flashbots/MEV-Share access, private mempool routing, gas-pricing policy. Sub-300ms execution is table stakes. The marginal gains beyond that are compounding — and expensive.

**5. Routing & settlement.** The on-chain router contract, DEX aggregation logic, cross-chain bridging integrations, and the operator's approval model. This is where Unibot and Maestro's 2023 exploits happened; it is also where most new operators make unforced errors.

**6. Social & growth.** Referrals, affiliate tiers, KOL partnerships, copy-trade social graphs, group-sniping features, cashback mechanics. Trojan and Photon run the most aggressive referral programs in the category (up to 35–45% rebates). This layer absorbs a huge fraction of gross margin — and, critically, it is where operators compete hardest because it is the layer most visible to users comparing bots.

**7. Monetization.** Fee model, token revshare, subscription tier, treasury policy. 1% flat is the dominant model. Banana Gun's 0.5% ETH-manual rate, Maestro's $200/month Premium tier, and Unibot's (suspended) loyalty discount are the visible exceptions. Token revshare (Banana Gun 40%, BONKbot 100%-to-buyback) is a separable design choice.

None of these layers are unbuildable. None of them are monopolized. The question is where an entrant can win a layer decisively enough to build a wedge into the others.

---

# III. Where Incumbents Are Weak

A layer-by-layer read of the incumbent set:

**Interface — weak everywhere, already contested.** Telegram was never a good trading interface. It has no charting, no portfolio view, no limit-order visualization, no position management, no alerts that aren't bot-posted messages in chat history. Axiom's rise is the proof — the web terminal captured ~74% peak Solana terminal share not because its execution was better but because its interface was. The interface layer has been displaced once already in the category's short history, which means it can be displaced again.

Specifically: mobile-native is the next open interface. None of the top-tier operators has a first-class mobile app. Telegram works on phones, but the bot paradigm is hostile to mobile UX — scrolling through chat history to find a position, typing contract addresses by hand, missing alerts because Telegram is muted. A real iOS/Android app with push notifications, native charts, biometric authentication, and passkey-based signing is an open product.

**Custody — weak everywhere, and it matters more than the industry admits.** The pattern is uniform. BonkBot, Trojan, Photon, Maestro, Banana Gun, GMGN, Bloom, Shuriken, Nova, Sigma — every major operator generates keys backend-side and signs server-side. Users are told to treat these wallets as burners. Every publicly disclosed loss in the category traces to this design: Unibot's router exploit compromised approvals on user-held EOAs, but Banana Gun's $3M loss was a direct compromise of the custodial Telegram-message oracle, and Polycule's $230K loss in January 2026 was a textbook SSRF-plus-forged-event attack against a backend with reversible-encryption keys.

The custody layer is weak for three structural reasons. First, all operators concentrate risk at a single server — one breach drains every user. Second, authentication is tied to Telegram account control, which is defeated by SIM-swap at scale. Third, operator opsec is invisible to users and uniformly opaque. The Hacken writeup is blunt: "almost all trading bots store users' private keys on their own servers with transactions signed directly by the backend."

Note the gap between what the category sells and what it runs. The branding is "non-custodial." The reality is "custodial with seed export." An entrant that actually closed the gap — not by marketing, but by architecture — would be selling a materially different product.

**Intel & discovery — weak except at GMGN.** GMGN is the only operator that has built a clear moat on this layer. Photon and BullX have made intel investments but treat it as a feature. The rest of the category has commodity screeners. The weakness is that on-chain data is fundamentally public: any indexer can build a honeypot scorer, a whale tracker, or a new-pair feed. The real moat at the intel layer is not the data but the *curation* — which wallets are worth copying, which signals are real, which scores are calibrated. That is a social problem, and it is solvable by a social entrant.

**Execution — strong, but ceiling is close.** Sub-300ms execution is table stakes on Solana. Jito bundle access, dedicated Helius nodes, and co-located infrastructure are expensive ($50K–$300K/month for a top-five operator) but buyable. There is no proprietary tech at the execution layer. The gap between Photon and a well-capitalized new entrant is budget, not engineering secrets. This layer is a moat against undercapitalized entrants, not against serious ones.

**Routing & settlement — weak in history, less weak now.** The 2023 Unibot and Maestro router exploits both originated from unverified contracts with insufficient input validation. Post-incident, most serious operators have tightened router hygiene. But the basic pattern — a long-lived router contract holding blanket approvals from every user — persists. An entrant using ephemeral approvals, or Permit2-only flows, or fully stateless routing, has a structural security advantage that compounds over time.

**Social & growth — strong but expensive.** Trojan's $57M in lifetime referral distributions is an absorbing moat: any new entrant trying to win on cash rebates is competing against an incumbent with a seven-figure monthly outflow. But the layer is expensive precisely because it is undifferentiated. "Pay referrers 35%" is not a strategy; it is a cost. An entrant that restructures the economics of the social layer — tokenized affiliate positions, equity-for-KOL deals, flow-auction rebates — is not competing on the same axis.

**Monetization — strong by convention, weak by design.** The 1% flat fee is a schelling point, not a cost basis. The actual cost to execute a swap is dominated by network fees (gas, priority, Jito tip), which are passed through. The bot's marginal cost per trade after infrastructure is close to zero. 1% is rent. The only operator that has materially broken from it is Banana Gun with 0.5% on ETH manual trades and 0% on stable swaps. The fee compression window is wide open.

---

# IV. The Moat Map

For each layer, what is actually defensible and how it compounds:

**Interface.** No durable moat. UX is copyable, and switching costs are low once a user's wallet is portable. Axiom showed that a year of good UX work can capture a majority of a chain's volume; the same can happen in reverse.

**Custody.** Moat is trust built over time without incidents — and the incidents everyone has had make that moat shallow. Incumbent trust is a liability, not an asset, because it can be revoked by a single breach. A credibly different architecture is structurally advantaged.

**Intel & discovery.** Moat is the curated social graph of which traders, wallets, and signals to follow. GMGN has a thin version of this; nobody has a deep version. The first operator to build a Twitter/Farcaster-style *reputation graph* of on-chain traders wins a durable moat, because the graph has network effects the incumbents cannot replicate cheaply.

**Execution.** Moat is capital and relationships (dedicated validator access, Jito priority, custom infrastructure deals). Sustainable for incumbents at scale, prohibitive for early-stage entrants. But the gap narrows as infrastructure providers productize — Helius and Jito sell to anyone with a budget.

**Routing & settlement.** Moat is smart-contract hygiene and the absence of historical incidents. Compounding — every day without an exploit raises the trust floor. But asymmetric: one incident destroys years of accumulated trust. Banana Gun survived a $3M exploit by refunding from treasury; a smaller operator might not.

**Social & growth.** Moat is the accumulated referral tree. Trojan's and Photon's trees have five-year-old leaves that an entrant cannot time-machine. But the trees are extractive — referrers are paid for acquisition, not for retention. An operator that flips to retention-based growth (the user is acquired once, kept forever) sidesteps the moat entirely.

**Monetization.** No moat. 1% is a convention, not a cost basis. The first operator to credibly commit to lower-than-1% pricing with sustainable economics forces the category to follow.

The pattern: the layers with the weakest moats (interface, custody, monetization) are the layers where incumbents are most exposed. The layers with the strongest moats (execution, social growth, routing history) are the ones where a serious entrant still has to match table stakes. A displacement strategy should lead with the weak layers and pay the entry tax on the strong ones.

---

# V. Four Displacement Vectors

Four strategies are open to a serious new entrant. They are ranked by how much of the stack they replace and how defensible the resulting position is.

## a. The Custody Play — "Actually Non-Custodial"

The thesis: replace backend-signed hot wallets with a credibly non-custodial signer that preserves competitive latency. The user's key never touches the operator's servers. Signing happens on the user's device (passkey, secure enclave, TEE) or in an MPC threshold that requires user participation for every transaction.

The technical path is not novel. Phantom, Rainbow, and Rabby have shipped device-side signers. Privy and Turnkey have productized MPC with passkey auth. Session-key models on Solana (via program-derived authority delegation) and EIP-7702 / account abstraction on EVM allow a user to authorize a bot for a limited scope and time without handing over the master key. The question is not whether it is possible — it is whether anyone has packaged it into a trading-bot product with Photon-grade execution.

Why incumbents cannot just copy: the custody model is load-bearing for their UX. Instant trades, reply-to-message sniping, mobile-Telegram convenience — all depend on the operator holding the key. A retrofit to a non-custodial model degrades the experience for existing users. A new entrant has no existing users to degrade.

Why this is a moat: once a user has funded a non-custodial wallet through the product, the switching cost to a custodial competitor is asymmetric. Users who have tasted actual self-custody do not go back to giving their keys to a Telegram backend. The category's next breach — and there will be one — will drive migration into whichever non-custodial alternative exists.

Risk: the UX penalty may be too large. Every passkey prompt, every MPC roundtrip, every signing confirmation is friction. If the penalty is 500ms per trade, the product loses on speed. The work is in making the friction invisible — cached session authority, batched approvals, progressive-trust models.

## b. The Interface Play — Post-Telegram

The thesis: replace the Telegram envelope with a purpose-built interface for a specific trader archetype. Axiom did this for Solana memecoin power users with a web terminal. The open variants:

- **Mobile-native.** A first-class iOS/Android app with push notifications, biometric auth, and passkey signing. Target the user who trades from their phone and finds Telegram bots hostile on mobile.
- **Discord-native.** Multi-server alerts, role-gated copy trading, guild-level intel sharing. Discord's crypto population is distinct from Telegram's and is largely unserved by the category.
- **Farcaster-native.** Frames as trading surfaces, cast-level copy trading, on-chain social graph as the primary discovery mechanism. Small TAM today, but the on-chain-native social graph is structurally differentiated.
- **Twitter DM / X-native.** If and when X's payments and DMs meaningfully integrate — unclear as of this writing — the trader-on-timeline audience is large and underserved.

Why incumbents cannot just copy: interface work is expensive and path-dependent. Telegram-first operators have years of Telegram-specific UX accumulated — bot commands, inline keyboards, group-chat features — that do not translate. A full rebuild is a new product, not a feature.

Why this is a moat: interface-layer wins tend to be sticky. Axiom users do not go back to Photon once they have charts; they switch when the next better interface appears. The question is whether the entrant's interface is *decisively* better for a specific archetype, not marginally better for everyone.

## c. The Social Graph Play — Copy-Trade as Primary

The thesis: make the trader the product, not the trade. GMGN has a shallow version of this — wallet tracking with copy-trade grafted on. A deeper version treats the social graph as the primary object: traders have reputations, followers, histories, and monetization. Execution is a service provided to the social layer, not the other way around.

Concrete design surface: **tokenized trader positions**. A successful trader's copyable strategy is issued as an asset. Followers stake into the strategy. The trader earns a performance fee; the operator takes a platform fee; the copy-trade execution is commoditized across any bot the follower prefers. This is the asset-manager-as-product pattern — Bitclout for on-chain traders, or, more usefully, the crypto-native equivalent of eToro.

Why incumbents cannot just copy: the incumbent model rewards the operator for each trade. A social-graph model rewards the operator for *hosting the graph*, not executing the trades. The incentives are opposed. An incumbent that added this layer would be cannibalizing its own fee stream.

Why this is a moat: social graphs have network effects. The first operator to reach a critical density of copyable traders is strictly better than the second. Once a trader's follower base lives on one platform, moving them is expensive — the trader loses their audience.

Risk: regulatory exposure is real. Tokenizing trader strategies is, in many jurisdictions, securities issuance. The design must sidestep this or accept the compliance burden. The compliance burden is, notably, a deeper moat than a social graph, if the entrant is willing to build it.

## d. The Asset-Class Play — Not Memecoins

The thesis: apply the bot form factor to asset classes where it is barely present. Three adjacent markets are open:

- **Perpetuals.** Hyperliquid, dYdX, Aevo, Paradex, and ~20 smaller perp DEXes move $5–20B+ daily. Telegram bot coverage is sparse — a handful of small operators, none of them category leaders. The perps user is the closest analogue to the memecoin trader in behavior but has 10x the volume and 10x the sophistication.
- **Prediction markets.** Polymarket's 2024–2025 volume surge created a bot-sized market that existed for five minutes before Polycule exploded ($230K hack, January 2026) and the category retreated. The product is still open. The Polymarket user wants fast, mobile, alert-driven execution across hundreds of thin markets — exactly what a bot does well.
- **RWAs and onchain equities.** Ondo, Superstate, and the coming wave of tokenized securities on Ethereum L2s are a different kind of market — lower frequency, larger size, compliance-heavy. A bot targeting this user is a different product from a memecoin bot, but the execution primitives (fast settlement, portfolio management, push alerts) are the same. The user base is also categorically different: institutional-adjacent, not degen-adjacent.

Why incumbents cannot just copy: each asset class has domain-specific requirements — margin management for perps, market-resolution logic for predictions, compliance rails for RWAs. An incumbent whose product is tuned for memecoin sniping is not three weeks away from a perps product. It is three quarters away.

Why this is a moat: asset-class leadership compounds through liquidity and orderflow. The first serious bot for Hyperliquid becomes the default bot for Hyperliquid, and the default bot's orderflow attracts market makers, which attracts more users. This is the winner-take-most pattern that defined Telegram bots in Solana memecoins. It repeats, once per asset class.

---

# VI. Case Studies --- What Worked, What Didn't

Three natural experiments in the recent history of the category are worth examining closely. Each tested a hypothesis about where competition happens.

**Axiom vs. Photon.** Axiom launched into a market where Photon had a year-plus head start, lifetime-revenue leadership, and superior execution infrastructure. Axiom won significant share anyway, reaching ~74% peak Solana terminal share (self-reported, unverified). The only layer Axiom contested was the interface. The thesis — that interface alone is enough to displace — was validated for power users. The thesis is decaying (Axiom is down 41% MoM as of April 2026) but that appears to be category-level decline, not Axiom losing to Photon. The lesson: a narrow wedge on a weak layer can take meaningful share without out-engineering the incumbents on anything else.

**GMGN vs. the field.** GMGN entered a crowded category by deprioritizing execution speed and overinvesting in wallet intelligence, copy trading, and a web UI. The result: $125.6M in lifetime revenue, $305K/day in current daily revenue, and one of only two operators still growing month-over-month. The thesis — that the intel layer can anchor a product that wins more broadly — was validated. GMGN's users do not come for the fastest trade; they come for the best wallet intel and stay because the execution is good enough. The lesson: a deep wedge on an underweighted layer can produce a durable second-tier position even in a concentrated market.

**Unibot's collapse.** Unibot was the 2023 category-defining bot. Its lifetime revenue is $7.4M; its current daily revenue is $3; its trailing-month decline is -87%. There is no evidence of an operator failure — no breach, no mismanagement, no exodus of the team. Unibot simply bet on Ethereum at the moment the category's center of gravity moved to Solana, and its product architecture was too tightly coupled to that bet to pivot. The lesson: the thing that made Unibot defensible as an Ethereum bot — Ethereum-specific router contracts, Ethereum-specific token economics, an Ethereum-specific user base — became the thing that killed it when the memecoin market moved elsewhere. **Chain and asset-class specificity are not moats. They are bets that cannot be hedged.**

A fourth natural experiment is happening in slow motion: **the token-aligned operators vs. the equity-funded operators.** Banana Gun (40% revshare to $BANANA) is the healthiest token-based operator by a wide margin — $86.7M lifetime revenue, continuous weekly distributions, treasury accrual in the $30–50M range. Unibot is the cautionary tale. Photon, Trojan, and GMGN are all equity-funded and retain 100% of post-referral fees. The comparison is not yet conclusive — Banana Gun survived an exploit and a user-rescue, which is real history — but the equity-funded operators are currently outperforming the token-aligned ones on revenue, with much more operational flexibility.

---

# VII. A Credible New-Entrant Playbook

Synthesizing the stack analysis and the displacement vectors, a credible entrant playbook:

**1. Pick one weak layer and lead with it.** Do not try to beat Photon on execution, Trojan on referrals, or GMGN on intel. Pick custody, interface, or social graph — and be decisively better on that layer. "Marginally better across the board" is the category's loser pattern.

**2. Match table stakes on the strong layers from day one.** Sub-300ms execution, Jito bundle access, router-contract audits, multi-chain coverage at least on Solana and one EVM. A beautiful interface with bad execution loses to Photon within a week. The entry tax is not optional.

**3. Restructure the economics of the layer that absorbs margin.** The referral layer is the operator's largest variable cost and the category's hardest displacement problem. An entrant that rebuilds it — tokenized affiliate positions, KOL equity, flow-auction rebates, or a retention-based rather than acquisition-based growth model — reduces the competitive axis where incumbents are strongest. This is the second wedge beyond the leading one.

**4. Plan for the transition, not the market.** Unibot's collapse is the central warning. Design the product so that chain, asset class, and social substrate are *parameters*, not foundations. The product should be able to move from Solana memecoins to Hyperliquid perps to Polymarket to RWAs without a rewrite. The operators who survive the next cycle are the ones whose architecture is loosely coupled to the market they happen to be serving.

**5. Price below 1% from day one, and mean it.** The monetization layer has no moat. An entrant that commits to 0.5% — or to a flow-auction model that effectively prices at 0.25% net — forces the category to follow or accept the loss of price-sensitive users. Banana Gun already demonstrated this works for ETH manual trades; nobody has done it as a platform-level commitment. The entrant who does will capture the category's price-sensitive segment.

**6. Treat the first breach as inevitable and design for it.** Banana Gun's $3M treasury refund is the category's reference pattern for operator accountability. An entrant that builds treasury reserves into the product economics from day one — a publicly verifiable insurance fund, automatic user reimbursement triggers, bug-bounty budget — converts what is currently a legal-and-PR problem into a product feature. Users would pay extra for insured trades. Nobody offers it.

**7. Distribution through the weakest incumbent layer, which is retention.** Incumbent referral programs are optimized for acquisition (new user signs up → referrer gets 30%+ forever). An entrant whose unit economics favor retention (existing user's lifetime value grows without paying a rebate on every trade) can outlast the incumbents in a cyclical market. This is the same shift newsletter subscriptions made from ad-supported to subscriber-supported: worse peak revenue, much better survival.

**Sequence.** In practice: ship the custody-plus-interface wedge first (the hardest technical work, the largest moat). Match execution table stakes. Layer intel and social graph on top — those are product extensions, not product definitions. Save asset-class expansion for after the first wedge has compounded. Do not raise a token until there is a treasury to distribute from; an entrant token issued as a growth hack is Unibot's epitaph.

---

# VIII. Bear Cases

The playbook above assumes the category is worth entering. Three bear cases deserve sustained attention.

**The category is cyclical and the cycle is turning.** Of the top 15 bots, 11 are down 15%+ MoM as of April 2026. Photon is down 10%, Axiom 41%, Trojan 17%, BullX 21%, BONKbot 23%, Banana Gun 15%, Bloom 41%, Padre 27%. The growth is concentrated in GMGN (+10%) and Maestro (+51%), both of which have multi-chain diversification the rest lack. If this is the top of the Solana memecoin cycle rather than a brief pause, the category's TAM is compressing faster than any entrant can scale into it. Entering at the top of a cycle has the worst risk/reward profile in the sequence.

**The custody problem may not be as large a wedge as it looks.** The argument above assumes users will migrate to a non-custodial alternative after the next breach. But the historical evidence is mixed. BonkBot retained users through the Solareum adjacency incident. Banana Gun retained users through its own $3M exploit. Users appear to price convenience higher than custody risk, and the industry's refund-from-treasury pattern has trained users to expect rescue rather than demand architecture. A non-custodial entrant may be building for a user preference that does not exist at the scale the market needs.

**The moat that matters is orderflow and it already flows to Jito / Flashbots / block builders, not bots.** The real rent in on-chain trading sits with whoever sees the transaction first and can extract from it — which is the block builder, not the bot. Bots are retail distribution into a market whose economics are captured upstream. A new bot might win the distribution layer and still not capture the economics, because the economics were never at the bot layer. If this is true, the entire category is a commodity-distribution game with no durable advantage, and the right move is to build at the block-builder or intent-solver layer, not the bot layer.

**The regulatory environment is not priced in.** The category is almost entirely unregulated and almost entirely serving US users (via VPN) or users in jurisdictions where token trading is a gray zone. The Solareum drain, the Banana Gun exploit, and the Polycule hack have not yet triggered coordinated enforcement action, but a single high-profile incident — particularly one where a large US retail population is clearly affected — could end the category's operating environment overnight. An entrant that builds compliantly from day one is both slower to market and structurally more durable; the path-dependence is brutal.

The honest read: the playbook above is a strategy for winning the category as it exists today. The bear cases are reasons the category may not exist in the form that makes the playbook work. A serious entrant should run both scenarios in parallel and hedge — build the custody-plus-interface wedge because it is the best-available opportunity, but structure the product so it can serve adjacent markets (perps, predictions, RWAs) if the memecoin cycle does not return.

---

# IX. Data Sources & Methodology

This report synthesizes findings from four parallel research streams: landscape and market share, security model and incident history, fee structures and take rates, and revenue and cost economics.

Revenue figures are taken from DefiLlama's Telegram Bot category fees/revenue API, pulled 2026-04-14. DefiLlama reports **protocol revenue** (fees retained by the operator), not gross trading volume. Where this report discusses volume, the figures are secondary (aggregator listicles, self-reported totals) and are flagged as unverified.

Incident details (Unibot, Maestro, Banana Gun, Solareum, Polycule) are sourced from post-mortems by QuillAudits, Rekt News, Neptune Mutual, Revoke.cash, and CertiK, and corroborated against project announcements and coverage by Cointelegraph, The Block, and Decrypt.

Fee structures are sourced from operator documentation (docs.bananagun.io, docs.maestrobots.com, learn.unibot.app, docs.gmgn.ai, docs.bloombot.app, docs.trojanonsolana.com) and cross-referenced against third-party reviews (solanatradingbots.com, CoinGecko learn, DEXTools tutorials).

Cost estimates are directional. Operators do not publish P&Ls. RPC and infrastructure cost ranges are derived from public Helius, QuickNode, and Chainstack pricing plus industry reporting (Dysnix, Chainstack, SecureBlitz). Referral and treasury distribution figures where verifiable are from operator docs and DefiLlama.

"Achilles" — which appeared in the initial brief — did not surface in any primary source examined during this research and is not included in the analysis.

Three caveats for any downstream use:
1. DefiLlama adapters occasionally undercount, particularly for operators that route through multiple contracts. Treat lifetime revenue figures as minimums.
2. User-count and volume figures are almost universally self-reported. No operator publishes audited DAU or MAU.
3. The category's revenue is memecoin-cycle-correlated. The April 2026 snapshot above captures a market that is cooling from the 2024–2025 peak; any projection forward must assume either a further decline or a new cycle, and the analysis differs materially between the two.

---

# X. Sources

**Revenue and market data**
- [DefiLlama — Telegram Bot category fees/revenue](https://defillama.com/protocols/Telegram%20Bot)
- [DefiLlama — Photon](https://defillama.com/protocol/photon)
- [DefiLlama — Axiom](https://defillama.com/protocol/axiom)
- [DefiLlama — Trojan](https://defillama.com/protocol/trojan)
- [DefiLlama — GMGN](https://defillama.com/protocol/gmgn)
- [DefiLlama — BonkBot](https://defillama.com/protocol/bonkbot)
- [DefiLlama — Banana Gun](https://defillama.com/protocol/banana-gun)
- [DefiLlama — Maestro](https://defillama.com/protocol/maestro)
- [DefiLlama — BullX](https://defillama.com/protocol/bullx)
- [DefiLlama — Unibot](https://defillama.com/protocol/unibot)
- [Trojan — Solana Compass](https://solanacompass.com/projects/trojan)
- [Banana Gun — Delphi Digital](https://members.delphidigital.io/projects/banana-gun)
- [Following Up on Telegram Bots — Delphi Digital](https://members.delphidigital.io/feed/following-up-on-telegram-bots-cryptos-new-cash-cows)

**Security incidents and analysis**
- [Neptune Mutual — Unibot exploit analysis](https://medium.com/neptune-mutual/taking-a-closer-look-at-unibot-exploit-7c53e25031ee)
- [Revoke.cash — Unibot exploit](https://revoke.cash/exploits/unibot)
- [Revoke.cash — Maestro exploit](https://revoke.cash/exploits/maestro)
- [Cointelegraph — Maestro refund](https://cointelegraph.com/news/telegram-maestro-bot-610-ether-refund)
- [CertiK — Maestro & Unibot writeup](https://www.certik.com/resources/blog/maestro-and-unibot)
- [QuillAudits — Banana Gun exploit analysis](https://www.quillaudits.com/blog/hack-analysis/banana-gun-exploit)
- [Rekt News — Banana Gun](https://rekt.news/bananagun-rekt)
- [Cointelegraph — Banana Gun refund](https://cointelegraph.com/news/banana-gun-crypto-bot-refunds-3m)
- [Decrypt — Solareum shutdown](https://decrypt.co/224371/solana-telegram-trading-bot-shut-down-users-drained-523k)
- [KuCoin — Polycule hack](https://www.kucoin.com/news/flash/telegram-trading-bot-polycule-on-polymarket-hacked-230k-stolen)
- [Hacken — Telegram trading bot risks](https://hacken.io/discover/telegram-crypto-trading-bots-risks/)
- [Cointelegraph — Should Telegram bots be trusted](https://cointelegraph.com/news/should-telegram-trading-bots-be-trusted)
- [The Block — Telegram bot security vulnerabilities](https://www.theblock.co/post/241386/telegram-crypto-bots)

**Operator documentation and fee structures**
- [BonkBot Docs](https://docs.bonkbot.io/)
- [Trojan — Rewards Program docs](https://docs.trojanonsolana.com/telegram-bot-user-guide/rewards-program)
- [Banana Gun — Banana Bonus docs](https://docs.bananagun.io/banana-token/banana-bonus)
- [Banana Gun — Rewards](https://docs.bananagun.io/banana-token/rewards)
- [Maestrobots docs — Fees](https://docs.maestrobots.com/faq/fees)
- [Maestrobots docs — Premium](https://docs.maestrobots.com/premium-subscription)
- [Maestrobots docs — Security](https://docs.maestrobots.com/faq/security)
- [Unibot — Revenue Sharing](https://learn.unibot.app/faq/revenue-sharing)
- [Unibot — Loyalty Program](https://learn.unibot.app/faq/loyalty-program)
- [GMGN — Fees & Settings](https://docs.gmgn.ai/index/gmgn-fees-settings)
- [GMGN — Copy Trade](https://docs.gmgn.ai/index/copy-trade-copy-smart-money-automatically-earn-sol)
- [Bloom — Pricing & Fees](https://bloombot.net/fees-pricing/)
- [Nova Bot docs](https://docs.tradeonnova.io/)
- [Photon fees docs](https://pies-organization.gitbook.io/photon-trading/photon-on-sol/photon-fees-sol)
- [Shuriken docs](https://docs.shuriken.trade)

**Category reviews and competitive context**
- [CoinGecko — Top Telegram Trading Bots](https://www.coingecko.com/learn/top-telegram-trading-bots)
- [CoinGecko — Top Solana Telegram Trading Bots](https://www.coingecko.com/learn/solana-telegram-trading-bots)
- [Backpack Exchange — Best Solana Telegram Trading Bots 2026](https://learn.backpack.exchange/articles/best-telegram-trading-bots-on-solana)
- [DEXTools — Telegram Trading Bots in 2026](https://www.dextools.io/tutorials/telegram-trading-bots-2026-guide)
- [DEXTools — Axiom Solana Terminal tutorial](https://www.dextools.io/tutorials/how-to-use-axiom-solana-trading-bot-tutorial-2026)
- [crypto-reporter — Best Telegram Trading Bots 2026](https://www.crypto-reporter.com/press-releases/best-telegram-trading-bots-in-2026-7-platforms-tested-for-speed-fees-and-mev-protection-124600/)
- [AMBCrypto — Top 7 Telegram Trading Bots April 2026](https://ambcrypto.com/top-7-telegram-trading-bots-of-april-2026/)
- [SecureBlitz — On-chain trading terminals comparison](https://secureblitz.com/banana-pro-best-on-chain-trading-terminals/)
- [DLNews — Telegram bots and memecoin trading](https://www.dlnews.com/articles/defi/telegram-bots-gain-in-popularity-with-more-memecoin-trading/)

**Infrastructure and execution**
- [Dysnix — Complete stack for competitive Solana sniper bots](https://dysnix.com/blog/complete-stack-competitive-solana-sniper-bots)
- [Chainstack — Helius RPC provider overview](https://chainstack.com/helius-rpc-provider-a-practical-overview/)
- [QuickNode — Build a Telegram trading bot on Base](https://www.quicknode.com/guides/defi/bots/build-a-telegram-trading-bot-on-base)
- [Blocknative — MEV protection](https://www.blocknative.com/blog/mev-protection-sandwiching-frontrunning-bots)
- [Greythorn — Banana Gun profitability analysis](https://0xgreythorn.medium.com/banana-gun-leading-web3-bot-profitability-ec0590c48622)
