---
title: "The Phenomenology of Token Games"
date: 2026-04-13
author: "apriori (human)"
---

# This is just Vapor for your token

**tl;dr The crypto industry is stuck in a loop where VCs and founders collude to extract value while everyone else gets rekt. We can fix this broken game by moving beyond the core builder-trader dialectic towards a future where builders deliver high value products and traders allocate capital to projects that deserve it.**

### Contents

- Preface
- I. A Brief History of Crypto Culture
- II. The Trader vs. Builder Dialectic
  - Builders, Founders, Traders
  - Venture Capitalists, Performance Metrics, Compensation
  - The DPI Problem
  - Crypto VCs
- II.b What VCs actually made *(empirical MOIC panel, 22-token cohort)*
- II.c If VCs are the problem, is crowdfunding the answer? *(Echo / Legion / MetaDAO)*
- III. The Mechanics That Sustain the Game
  - Low Float / High FDV
  - The Founder's Dilemma
  - Mimetic Desires
  - Scapegoats
  - Pushback
- IV. The Synthesis: Towards VC Signal
- V. The equalizer was never the distribution mechanism *(AI and the dissolving constraint)*
- Closing
---

## Preface

Online culture wars have been around for some time, arguably since the inception of the internet. What is a culture war? A culture war is a form of cultural conflict (metaphorical "war") between different social groups who struggle to politically impose their own ideology (moral beliefs, humane virtues, and religious practices) upon mainstream society, or upon the other.

On the internet there are many of these. Some people see the world one way while others may see it another. There are multiple layers and dimensions to this. But typically micro culture wars if given enough time to linger evolve into bigger and badder wars that become all consuming for people. In fact these types of debates are quite common in crypto. They are battles which never seem to end. Example we see in crypto today is

- VC vs. anti- VC
- Trader vs. Builder
- My coin vs. Your coin

Some of these conflicts eventually resolve. Typically a sort of dialectic forms where the war demands being either pro A or not-A. And in this scenario the not-A is typically the antithesis of A. The synthesis only emerges once a resolution that subsumes both the A and not-A perspectives. This new synthesis may provide the basis for a new dialectic to form and so on and so forth until the singularity. Simply put, each smaller culture war sets the stage for the next one which is often higher stakes.

If it helps I like to think of the dialectic process as a tree structure where two children fold into a parent and two parents fold into a grandparent and so on up to the root of the tree. Sublation I like to think about holding directly conflicting ideas, feeling both of them deeply, and then finding a resolution which evolves to a new understanding.

Okay, back to crypto. Much of the crypto culture wars (and others) are driven by the fact that participants are terminally online engaging with viral content that is delivered and curated in a way to persist and create new dialectics. The apparent dialectic of the moment which has persisted since the tail end of 2021 is the VC coin vs. non-VC coin.

There is a belief that any and all venture capital backed projects that issue tokens are doing so as a pure grift to extract value from retail market participants to make insiders like founders and investors rich. These coins should not be purchased by retail under any circumstances. Promises made about delivering technology are so far out in advance that the tokens these projects represent are practically vaporware.

The non - venture backed coins are mostly pure modulo obvious grifts. Retail stands much more chance dealing in these non VC coins than the VC ones. New VC coins are parasitic to the entire ecosystem transferring value from the true believers to the tourists value extractors.

Is that the real tension though? No, its only a symptom. The real dialectic that needs a resolution is between traders and builders. Everything else builds on that foundation.

Note this essay does not touch on the relationships between startups, investors, exchanges, market makers, or token launch strategies. Much of this subject matter has been covered on Twitter and various podcasts over the last two years. I am assuming a high context audience. Nowadays people barely read anything, so if you are reading this, you're likely searching for answers. In this process you will find many threads to pull on, dark alleys to lurk through. Good luck with that. Take this essay with a grain of salt.

---

## I. A Brief History of Crypto Culture

Crypto culture is quite fragmented. In the beginning there was only Bitcoin. And to embrace Bitcoin early on you we're either a cypherpunk, an innovator (as defined by Geoffrey Moore in Crossing the Chasm), a speculator, or someone who just had deep distrust of the existing financial apparatus, always on the look out for something beyond the horizon. In fact, just read the fucking Bitcoin genesis block, its quite clear really.

> The Times 03/Jan/2009 Chancellor on brink of second bailout for banks

During the nadir of the global financial crisis (bottom March 2009 thereabout), a life raft emerged. That life raft was Bitcoin. Bitcoin solved the double spend problem. Money can be created and transferred without a centralized mint or intermediary tracking every transaction to ensure there is no double spend when a new coin is created. As a result, Bitcoin provided a new way to transact that does not require trusting an intermediary. It allowed end-users to participate in the network as first class citizens, both mining and validating the chain.

Bitcoin was really only designed for sending around bitcoin. Bitcoin script is programmable but not Turing-complete. Many early crypto enthusiasts attempted to build extensible systems on top of Bitcoin. Eventually though, technologists recognized that a new type of architecture would be necessary for general purpose compute - namely Ethereum.

Ethereum unlocked a new architecture for decentralized applications enabling the creation and exchange of ERC-20s, Ethereum Name Service (ENS) domains, Non-Fungible Tokens, and Decentralized Autonomous Organizations (DAOs). Ethereum, was founded by a small group of idealists working together with a shared set of goals and values to deliver a next generation smart contract and decentralized application platform technology to the world.

Over time more chains and tokens were launched. The next wave of innovation came on the heels of the so-called ICO era. And while there were some good projects that received funding during that era that are still around today, there was a copious amount of grift.

During the following bear market (2018-2020), Ethereum began to find product market fit with AMMs like Uniswap or borrow and lending protocols like Aave (Ethlend). And of course then you had the emergence of DeFi summer and Yield farming. Which later transitioned to DeFi summer 2.0 driven by NFT trading, rebasing coins, and later algorithmic stable coins.

During 2020-2021, a new cohort of chains entered the zeitgeist to directly compete with Ethereum. These newer chains like the three headed SoLunAvax dragon featured novel consensus mechanisms, crypto economic incentives, sharding, and WASM execution environments. Some of these newer chains were just cheaper faster versions of Ethereum, forking the EVM and strapping it to a new consensus protocol (Fantom, Avalanche C-Chain, BSC, Polygon POS). At this time, people believed cryptocurrency would be integrated into almost every aspect of life. As it turns out, such positive sentiment was only ephemeral.

After the blowups of the 2022-2023 bear market, there we're few believers left in the crypto industry. FTX was a go to for many crypto traders who lost significant portions of their holdings almost overnight. Terra's UST was marketed and shilled to retail as "basically risk free". CeFi also had other other headline wreckages including 3AC, Celsius and BlockFi. The industry had egg on its face.

Somewhere around this time, the Securities and Exchange Commission began to target crypto with regulation by enforcement. Operation chokepoint 2.0 has since been acknowledged as real. Perhaps the climax came with the banking crisis when Silicon Valley bank and Signature bank were taken into FDIC receivership. Under Gary Gensler's reign as SEC chair, we saw lawsuits filed against Coinbase and issuance of a Wells notice to Uniswap Labs.

However not all optimism was lost. Builders were still raising large rounds to build infrastructure. Many of these raises were for "modular" chains pitched as rollups or L2s. Other raises focused on ZK companies boasting prover marketplaces and highly performant ZK-VMs. Self-funded teams like Hyperliquid understood and took a distinctly different path from venture capital backed projects, but more on this later.

At the end of 2023, Solana found product market fit. In particular, there was the Jito airdrop, a major wealth creation event for the ecosystem, and the emergence of Pump.fun. Pump fun made it extremely easy for anyone to "become a dev" and launch their own meme coins. During this time, the Crypto x AI meta took off and people started building so called onchain agents. All of these projects had coins, and for a time people wanted them because they just kept going up. But eventually they ran into a wall. The market officially topped when Trump launched a meme coin and hasn't looked back since.

**Let's recap.** Cryptocurrency began in earnest with a vision of removing trust in intermediaries and the possibility of fundamentally changing the way we allocate trust on this planet. Later in concordance with technology improvements tokens became valuable in USD terms. More emphasis was placed on trading and decentralized finance use cases. Taken to the extreme memes live for five minutes at a time in the trenches. Focus on product market fit, building businesses that generate revenue, and finding use cases is the current meta for builders. On the surface, the values may not be in the room with us. But if you look deeper, they're still there.

---

## II. The Trader vs. Builder Dialectic

Two archetypes. They are distinct.

### 1. Builders

Builders make software or hardware. They "build" things so to speak. They are concerned with systems design, security, and product. In crypto the term typically refers to people working on a team or project that is building something. Software engineer is sometimes assumed, but researchers, product designers, and go-to-market teams are builders too.

Builders are accountable not only to their P&L (unlike traders) but to their colleagues, investors (people who backed the project), the community (everyone from airdrop farmers to peers who want to see them succeed), and critically their higher order goals. For founders this would be the vision or end-state of their project.

Builders often choose to work on projects with a multi-year development cycle. Many like and are obsessed with solving hard problems and want to commit to something for the duration of solving said hard problem. Builders also often do not evaluate success in terms of token price or bullshit adoption metrics that are highly game-able. Often they want people to use their technology.

Builders must also weight community dynamics. Do I work on a project that is highly polarizing even if I love it because I don't want to get harassed on all of my socials? What if the project fails, will I be scapegoated? If I receive compensation in this token related to what I am building, do I want to hold it? You may think these are silly questions, but these things happen in crypto when you succeed and when you fail. The mob will come after you at one point.

#### Founders

Founders are the first employees of a new company. An entrepreneur is someone who sees an opportunity to solve a particular problem or set of problems in a particular market segment and then builds a business around this solving a problem thesis. Entrepreneurship is challenging.

But crypto throws this in the trash. In crypto founders are the techno priests. They are the thought leaders on Crypto twitter (now X). Founders appear on all the in the know podcasts, present the key notes at the top conferences, are quoted in press releases about fundraising, and generally serve as the key opinion leader for their project. The Founder is the lifeblood of the project.

Founders are rational actors sometimes. For example a founder may have the ability to sell a portion of their locked token holdings to willing buyers at the current fair market price from private investors. The founder gets to de-risk long-term risk and realize future profits today. Sometimes they do engage in this behavior whilst other employees do not have this privilege. We saw this play out publicly with the former Eclipse founder. Cobie wrote a brilliant piece about this and related topics as well.

### 2. Traders

Traders evaluate the price of a particular asset based on where it is in the current moment and where they think it is going in the future, and either chooses to act or not act.

Crypto traders, generally, fall into the category of retail. Unless you are doing CEX-DEX arbitrage, offering RFQs, making markets, you are probably a retail trader. Crypto retail traders typically trade two instruments; spot and perps. Both instruments can be traded onchain or offchain.

Traders are often in the game for two reasons. Lifestyle and money. The rush of making multiples over your rent and living expenses in a single trade is as good as sex. As a trader you also learn how to manage risk. If you don't learn how to cut losers fast you'll be out of capital.

As a trader when you wake up in the morning the question is always, how am I going to make money today? The trader has his head on a swivel. He is ready to act from anywhere he can access the market at any time of day or night if need be. The trader must be accountable to his P&L. There is no free lunch for him. If he wants to earn he must learn and work hard building up his skills and intuition over many years and often several starts from zero.

All of these words to say that crypto traders in particular, have skin in the game. They live and die with their P&L. They can either eat dinner at the Mandarin Oriental or Chicken fried rice from the local takeout. It depends how well they do.

The takeaway here is to demonstrate clearly that profits obtained from trading can go to other high leverage activities, or simply provide the trader the means to pursue a life that they desire. This is important because traders, especially in crypto, are often villainized or scapegoated by both VCs and builders the moment a new speculative bubble that does not align with their existing bags or agenda emerges from the ether. But trading is just a means to an end. Sometimes that end is "the trader lifestyle", but other times its simply about freedom, self-sovereignty and autonomy.

Autonomy means self-lawed, you create a set of rules for yourself and obey them, impose them on yourself without the input or permission of others. There is no freedom without autonomy. And crucially, autonomy is one of the core value propositions of crypto because its multidimensional affordances speak directly to the foundational Ethos.

- Moral autonomy (a man has to have a code, from the Kantian tradition)
- Personal autonomy to pursue actions or decisions independent of moral content (what is your personal utility function)
- Political autonomy where decisions are made and respected within a political context (which fork do you follow)

Autonomy aligns with the original ethos. You self-custody your coins. You can enter and exit markets at will based on your set of rules. You can completely cut out the middle man. You no longer need a bank to store your wealth. You no longer need approval to send someone money for a good / service. You don't have to trust anyone, you can simply verify what happened.

Traders fundamentally represent the atomic units of cultural autonomy in crypto. Most of them are simply "just trying to make it", and in the process reclaim their autonomy from society. Builders have a variety of motivations. Traders have very few.

Now that we've established this let's talk about some uncomfortable topics. Founders are builders and venture capitalists are traders. But roles can switch.

### 3. Venture Capitalists

Venture capitalists are people who invest in entrepreneurs. VCs typically invest in the riskiest types, those building businesses that have likely not been tried or existed before and are too risky for traditional sources of capital like a bank loan. Also, venture capitalists look particularly for companies that have the potential to become Unicorns or Decacorns.

VCs are organized as limited partnerships. The structure involves a limited partner and a general partner. GPs run the firm and LPs contribute the capital. Early on in the fund's life cycle the GP spends time deploying capital during the investment period, and later shifts his focus to supporting his portfolio companies by helping generate exits via Initial Public Offering (IPO) or acquisition (M&A).

VCs lose money on their investments. In fact most of them will not work out. 75% of venture backed business fail. VCs can afford to lose money on most investments because of the power law distribution of returns. One investment in a portfolio of many can return the venture capitalist's fund five or even 10 times over. Imagine you have a portfolio of one hundred investments with $100M of capital deployed. Now assume ninety eight out of one hundred of the investments go to literal zero, are failed businesses. Two of the investments in your portfolio wind up returning $2B, the first returns $1.8B and the second $200M. In effect, as long as you can catch a couple unicorns or Decacorns (rare creatures), you win. Your LPs will be happy and you will earn carry.

The venture capitalist is often not investing their own personal funds. VCs raise money for their funds from limited partners. So effectively they are building an investment product for well-sourced capital allocators to invest in which create superior returns not found in other asset classes.

The LPs come from different places, but all of them have large amounts of money to put to work: institutional investors including university endowments (Harvard, MIT), public (CALPERS) and private (corporate) pension funds, sovereign wealth funds like Temasek (Singapore), Mubadala (UAE), PIF (Saudi Arabia), and Norway, foundations like the Ford, Gates, Hewlett, or MacArthur, insurance companies like Prudential, MetLife, AIG, and Allianz, fund of funds like Hamilton Lane, Horsley Bridge, and HarbourVest, family offices, and corporate investors like Google Ventures, Intel Capital, or Salesforce Ventures. The bulk of capital for venture funds comes from pension funds. Think about that for a moment: retirement savings of ordinary working people flow into VC funds, which fund crypto projects, which launch tokens, which extract from retail crypto participants — who are often the same demographic of ordinary working people trying to build wealth outside the traditional system. The money comes from the same class of people on both ends.

#### VC Performance Metrics

**IRR** — Internal rate of return is a financial metric used to measure how profitable an investment might be. IRR is essentially the break-even rate of return. In lehman's terms IRR shows the speed of value creation. It is the annual growth rate of your investment. It accounts for both the amount of money made and how quickly it was made. Think of IRR like looking at different scores of basketball games on the same night. Regardless of the teams playing you can see who is performing well at the end of the first quarter and who is not because points is the widely accepted metric. IRR works the same way — it lets you make apples to apples comparisons against your peers and across asset classes regardless of the time frame. Crucially, if you can show your LPs a better IRR than your peers you may have an edge in raising your next fund.

**TVPI** — Total value to paid-in measures the total value of realized and unrealized investments in a fund in proportion to the total contributions—the paid-in capital. TVPI shows the total multiple on invested capital including paper gains. Note that TVPI does not take time sensitivity into account. For example a 5x TVPI over three years is preferable to a 5x TVPI over 10 years.

**DPI** — Distribution to paid-in capital, measures the return multiple on the money paid into a VC or PE fund. DPI is a measure of the actual cash that a Limited Partner (LP) has received back from a fund relative to the capital they have contributed. Unlike IRR, DPI does not take into account the book value of unrealized investments in a fund. Over time you would expect TVPI and DPI to converge once the unrealized gains become realized as a result of investment exits via acquisition or IPO. When evaluating a fund at its terminal point DPI is the only metric that matters. Its not hypothetical or unrealized, it is the actual return LPs receive. Sticking with our basketball analogy you can think of DPI as the end of the game scoreboard. Nothing else matters, did you win the game or not.

- IRR shows the speed and efficiency of value creation (time-weighted returns)
- TVPI shows the total multiple on invested capital (including paper values)
- DPI shows how much cash has actually been returned to investors

#### Compensation

VCs are compensated with active management fees and carried interest also known as "carry". Management fees allow the venture capitalist to fund their operation. In particular they need to pay for a team, perhaps an office, subscription services, and travel expenses. The standard management fee is 2% per year. For example, a fund charging a 2% management fee on $100M fund would generate $2M annually. Note that the base upon which the annual percentage rate is applied typically changes during the fund's life. In the first three to five years a management fee is often calculated as a percentage of committed LP capital. After all capital is called a fee is typically calculated as a percentage of capital invested in portfolio companies.

Carried interest is the percentage of a fund's investment profits allocated to the General Partner (GP) as compensation, but only _after_ the Limited Partners (LPs) have received back their initial capital contributions, and often, after achieving a minimum rate of return (preferred return or hurdle rate). Many funds rely on a standard 2 and 20 compensation structure where 2% is paid out as a management fee and 20% paid out as carried interest on realized profits. VCs with strong track record of performance can command a higher carry, 25%-30%, while newer funds may offer lower carry rates like 15%.

Carry is paid out in what's called a distribution waterfall which is agreed on in the limited partnership agreement (LPA). There exist two types of waterfalls, American and European. In a European waterfall, carry is calculated on the entire fund's aggregate performance. GPs only receive carry once all LP funds are returned plus any preferred hurdle rate. In an American waterfall, carry is calculated on a per investment basis. GPs can profit before all capital is returned to LPs, but this structure typically requires a clawback provision to protect LPs should the remainder of the fund underperform.

Let's say there is an American waterfall fund we'll call Niagara fund. The Niagara fund is $100M and has two investments (let's assume all capital is called). Investment 1 is $50M and exits for $150M at time t. Investment 2 is $50M and exits for $0 at time t + 1. At time t the profit calculation is $150 - $50 = $100M profit. The LPs first get their $50M back. The $100M is split with $80M going to LPs and $20M going to the GP. LPs now get $50M back + $80M = $130M while GP gets $20M. At time t + 1, investment 2 returns $0. Total realized gains $100M - Total realized losses $50M = $50M profits. GP is entitled to $50M * 0.2 = $10M. Now $10M needs to be clawed back from the GP and paid to LPs. After the clawback LPs have $130M + $10M clawback = $140M. GP has $20M - $10M clawback = $10M.

The primary goal of the carry based compensation is to align the interests between LPs and GPs such that GPs are incentivized to focus on generating meaningful returns over the life of the fund.

#### The DPI Problem

Now that we have the basics squared away let's think about this for a second. If I am a newer fund manager raising a new fund I will certainly over-index on IRR and TVPI when pitching my potential LPs. The reason is that I probably don't have a DPI that says much about my abilities as a fund manager yet.

Let's walk through an example. Say I raise a $100M fund. I charge a management fee of 2% per year over a ten year fund life. That's 20% of the funds raised in management fees, leaving $80M to invest. Now assume that 75% of my investments fail. That leaves $20M of investments that need to generate the returns for the fund. My investors are expecting returns that outperform what they would have gotten in the public markets, where they pay no fee. If we invest $100M into public markets over ten years assuming 10% returns we'd have about $259M. The venture capitalist needs to generate $159M of returns at a minimum on the initial paid in capital. In reality though, we only have $20M to generate that return — roughly 8x on the surviving positions. If the VC cannot generate this return then they will lose out to peers and other asset classes.

There is a flaw here however. It is possible that the investments I have marked up in my first fund to the latest financing round are unlikely to exit at that valuation. However, I don't want to mark down the value of my investments as the other funds that have invested in these companies have all left their marks of the investment at the latest financing round. This means that both my IRR and TVPI are likely artificially inflated and not reflective of the potential DPI.

Indeed, the key vulnerability in relying too heavily on IRR and TVPI for newer fund managers is that both metrics can be materially influenced by paper valuations that may never materialize into actual returns. This creates a potential misalignment between reported performance and realistic outcomes.

Interestingly enough, this is what we are seeing play out in real life. DPI even on 2017 vintage funds, the 75th percentile would only return $0.64 for every $1.00 of paid in capital by LPs. IRR still looks fantastic and likely well above the hurdle rate for even the 50th percentile funds from 2017-2020 vintages. But the lack of DPI is telling. It means that these funds are holding their investments for longer and the companies that have been marked up (paper gains) have yet to find paths to an exit.

Unfortunately, what this chart implies may or can have a chilling effect on venture capital funding. If there are fewer exits then LPs are unable to get back their paid in capital. This means VCs have less capital to pull from when trying to raise new funds. If there is less capital available to raise new funds then the funds raised are smaller or don't get raised at all. This leads to less capital available to invest in high risk high reward startups. Its a negative feedback loop which may dampen innovation.

Indeed, venture capitalists want a competitive DPI. You can't eat IRR! In the long-run the dominant strategy would be to maximize DPI, its really the only number that matters.

> There is no fast term accountability in venture capital. Sometimes you have momentum and the valuations go up and that does give you a feel that things are working but in practice, five to ten years later you get liquidity on these businesses or even longer. It takes incredibly long and so people tend to over-index on these sociological factors, and what's popular. — Peter Thiel

#### Crypto VCs

Crypto VCs are relatively new phenomenon. Most have been around for about decade or less. To name a few OGs: Blockchain Capital (2013), Pantera Capital (2013), Consensys Ventures (2015), Fenbushi Capital (2015), Polychain Capital (2016), Coinbase Ventures (2018), Paradigm (2018), and Dragonfly Capital (2018). In addition traditional VC firms like Andreessen Horowitz and Sequoia Capital have both entered the crypto space as well.

There are three main differences between crypto funds and traditional venture funds.

The first is crypto VCs often invest in both equity and tokens depending on the particular deal. The second is that crypto investments have a clearer path to an exit than traditional venture capital. Crypto VCs are playing a game of regulatory arbitrage backing teams and projects which basically issue securities. The third difference is that the composition of LPs is less reliant on large institutional backers. Crypto LPs often consist of high net worth individuals, family offices, fund of funds, crypto native investment firms, sovereign wealth funds and perhaps a small amount of institutions. Crypto is far out on the risk curve. While there may be a handful of pension funds or endowments willing to allocate small amounts, the industry has yet to see an inflow of that type of capital as compared to traditional VC LPs. This difference in LP composition matters because it changes the accountability dynamics. Pension fund LPs have fiduciary obligations and regulatory oversight. Family office LPs don't. That difference partially explains why crypto VCs can get away with behavior that traditional VCs can't.

How is it that crypto projects can guarantee exits, don't they need an Initial Public Offering or M&A? Recall there is a thing called a token which is not in most cases considered a security. Crypto projects typically launch a token with their product offering. Tokens often have lock-ups and vesting schedules.

For a crypto token coming to market with a centralized exchange listing, there are no IPO underwriters. There is no SEC S-1 filing. There is no IPO roadshow so to speak (although you could argue the rotating conference circus, I mean circuit, is a proxy roadshow). The underwriters are not working with the project to understand what their forward revenue projections are. This is crucial because when an IPO hits the market the underwriter reserves a portion of the shares for retail traders to purchase. Retail often trusts that if Goldman Sachs or another underwriter is taking a company public then it is clearly not a scam because they needed the SEC's approval, they needed to secure buyers from mutual funds, pension funds, and other institutional investors, and they have clear insights into the profitability of the company they are bringing to market. Even if the company is not yet profitable, the underwriter is signaling this is by default a new shiny growth stock to pay attention to.

In effect, in crypto, the venture capitalists act as proxy underwriters. The market can take signal from their investment in a particular project to understand if it is worthy or not. However, the market must trust that these entities are only bringing the best projects to market at fair valuations, which is demonstrably not always the case.

If compensation is DPI based then there is an incentive to push founders to create exits faster. Faster exits means more carry but it also means that funds can be returned to LPs and gains realized. Crypto is perfect for this because a project if willing and able can launch a token whenever they want. In addition, once the token has a liquid market there will be more interest in purchasing locked tokens at a discount on secondary markets. If the price of the discount on the secondary market delivers an acceptable return well above your hurdle rate you pull the trigger and realize the gains.

One question you might ask. If most crypto projects "go to zero" or become zombie tokens until all unlocks are complete, perhaps you could manufacture a startup strictly for the purpose of launching a token! No one would do that, right? As it turns out this happens sometimes. These startups do just enough to generate a project with visible activity. They may also hire employees who are not in on the scam and work very hard as software engineers, researchers, marketers, business development and fully believe the project is legit rather than a shell company to launch a token.

Your fund may look great on paper. LPs may be impressed. But you can't eat IRR. At the end of the day all that matters is DPI.

*Bridge: before we get into the structural and cultural mechanics that keep this game running, it's worth settling one piece of the argument empirically. The DPI problem above tells us what healthy VC math is supposed to look like. Crypto breaks that math in a specific, measurable way — because the token is itself the exit, and the buyer at the exit is retail.*
---

## II.b What VCs actually made

When people say "VCs won, retail lost," they are usually pointing at a chart. Arthur Hayes ran a version of this. Binance Research ran a version. Cobie ran a version in May 2024. Earlier drafts of this essay ran a version too. All of them used the day-1 close as the retail entry baseline, which is the right shorthand if you want a quick number but the wrong baseline if you want to know what happened to VCs across the unlock schedule.

The cliff matters. Most 2024-era tokens unlock on a 12-month cliff with linear vesting after. The interesting question is not whether a token was up or down at TGE+1 — it's what the VC's position was worth at TGE+12mo, +18mo, +24mo, because that is when the realized-return clock starts. Day-1 close conflates "VCs are up on paper" with "VCs actually got out." They are different things.

So we replicate the exercise with a 22-token panel that has entry-valuation data of acceptable quality and price data at three post-TGE horizons. Prices are 7-day means around TGE+365, +545, +730 days, pulled from DefiLlama's historical API and cross-verified against CoinGecko on 23 of the 31 total horizons (max deviation 2.21%, mean <1%). MOIC is computed as **(price_at_horizon × total supply) / VC entry FDV** — i.e., what a VC's token allocation would be worth at the relevant horizon relative to the implied per-token price they paid at their last pre-TGE round. Entry FDVs are sourced in full in the underlying research file; only tokens with **high-confidence** entry valuations are included in the primary MOIC table. Medium- and low-confidence tokens are kept in a separate data-gaps table so the reader can see what we know versus what we don't.

Cobie got to this argument first. His May 2024 essay *New launches (part 1) — private capture, phantom pricing*¹ was the cleanest early statement of the asymmetry: price discovery has migrated into the private rounds, TGE is phantom pricing, retail buys at or near a local top while seed investors sit on 100x+ returns. The two illustrations everyone remembers from that piece are "OP seed $60M → $1.7B FDV, 183x" and "STRK seed $80M → $11B FDV, 138x." The numbers are rhetorical shorthand and deserve a footnote: the $1.7B in Cobie's OP comparison is a post-TGE open-market FDV low, not a private round valuation (Optimism's last pre-TGE round was a Series B at **$1.65B equity**, Paradigm + a16z, Mar 2022); the STRK "$80M at $11B" folds the seed amount together with the TGE *opening* FDV, and Starknet's last pre-TGE round was actually a **$100M Series D at $8B equity** (Greenoaks + Coatue, May 2022). This isn't a hit piece on Cobie's arithmetic — it's an invitation to restate the claim more carefully, because the directional point survives the correction. Seed investors in both projects received token allocations priced off those early equity rounds, and when those tokens unlocked against a TGE FDV that was multiples higher than the implied per-token cost basis of the seed, the 100x+ paper returns Cobie describes did accrue. The cleaner version is: *VCs entered at valuations orders of magnitude below TGE FDV, and retail entered at or near the top.* That claim holds under any reasonable recalculation — the corrected entries in the MOIC table below make it concrete.

¹ Cobie, "New launches (part 1) — private capture, phantom pricing," cobie.substack.com, 19 May 2024, https://cobie.substack.com/p/new-launches-part-1-private-capture.

### MOIC by token (high-confidence entry valuations only)

| Token | TGE | VC entry FDV | Price @ 12mo | MOIC @ 12mo | Price @ 18mo | MOIC @ 18mo | Price @ 24mo | MOIC @ 24mo | Notes |
|---|---|---|---|---|---|---|---|---|---|
| OP | 2022-05-31 | $1.65B equity | $1.49 | 3.88x | $1.76 | 4.58x | $2.51 | 6.53x | Series B; Paradigm/a16z |
| STG | 2022-03-17 | $250M (token FDV) | $0.77 | 0.36x | $0.48 | 0.22x | $0.80 | 0.37x | Low initial float (~1.2%); STG merged into ZRO 2025 |
| APT | 2022-10-12 | $4B equity (range $2-4B) | $4.94 | 1.48x | $13.01 | 3.91x | $9.04 | 2.72x | Series A FTX/Jump; $2B–$4B sources disagree |
| IMX | 2021-11-04 | $2.5B equity (Mar-2022) | $0.61 | 0.49x | $0.95 | 0.76x | $0.77 | 0.62x | Series C closed 4mo post-TGE; proxy |
| SUI | 2023-05-03 | ~$4.3B implied token FDV | $1.15 | 2.67x | $1.89 | 4.39x | $3.45 | 8.03x | Implied from Series B $0.43/SUI allocation |
| SEI | 2023-08-15 | $800M | $0.28 | 3.56x | $0.23 | 2.84x | $0.33 | 4.07x | Strategic round Apr-2023 |
| TIA | 2023-10-31 | $1B equity | $5.00 | 5.83x | $2.85 | 3.32x | $0.99 | 1.15x | Polychain exited remaining stake at $62.5M in 2025 |
| ARB | 2023-03-23 | $1.2B equity | $1.65 | 13.74x | $0.53 | 4.45x | $0.37 | 3.11x | Series B Lightspeed |
| STRK | 2024-02-20 | $8B equity | $0.23 | 0.28x | $0.13 | 0.17x | $0.05 | 0.06x | Series D Greenoaks/Coatue; contested entry |
| ENA | 2024-04-02 | $300M equity | $0.34 | 17.23x | $0.58 | 28.89x | $0.09 | 4.30x | Feb-2024 Dragonfly/Maelstrom; Dec-2024 post-TGE round re-priced at ~$6B |
| W | 2024-04-03 | $2.5B token warrants | $0.08 | 0.33x | $0.11 | 0.43x | $0.01 | 0.06x | Rare clean token-FDV case |
| ONDO | 2024-01-18 | $525M CoinList FDV | $1.27 | 24.19x | $0.95 | 18.06x | $0.38 | 7.26x | Retail CoinList tier — not a pure VC entry |
| OMNI | 2024-04-17 | $150M Series A token FDV | $1.85 | 1.23x | $2.50 | 1.67x | n/a | — | Direct per-token pricing at $1.50/OMNI |
| SCROLL | 2024-10-22 | $1.8B combined (equity+token) | $0.17 | 0.09x | n/a | — | n/a | — | 12mo elapsed only |
| ATH | 2024-06-12 | $150M Pre-A | $0.05 | 12.94x | $0.01 | 3.79x | n/a | — | Aethir; 24mo pending (60 days out) |

*Caption: MOIC = (7-day mean price at horizon × total supply) / last pre-TGE private round FDV. Only tokens with high-confidence VC entry valuations are shown. Equity valuations are used where token FDV was not disclosed at the round; this understates the VC's effective cost basis where the token warrant economics were richer than the nominal equity ticket. Totals and stats are in the cohort table below.*

### Data gaps — entry valuation not recoverable or too uncertain for MOIC

| Token | Reason excluded |
|---|---|
| DYDX | Series C valuation undisclosed; $2B rumored post-money, single-source and unverified |
| PYTH | No pre-TGE private round publicly documented; retrospective airdrop distribution |
| JUP | **No pre-TGE VC round — self-funded.** This is a control case: zero insider entry price. |
| ALT | Valuation withheld at Strategic ($14.4M / Polychain + Hack VC) |
| DYM | CEO explicitly declined to disclose valuation |
| PIXEL | Polygon → Ronin migration broke the paper trail; no clean source |
| PORTAL | Medium confidence only (implied ~$87.9M FDMC); included in table above with a flag |
| TNSR | Valuation withheld at $3M seed |
| PRCL | Valuation withheld at Series A |
| REZ | Only post-TGE Series A ($17M) is documented; pre-TGE was $3.2M seed at undisclosed valuation |
| ZK | Press reports suggest ~$2B equity at Series C but never confirmed |
| BLAST | $20M seed, valuation undisclosed |
| EIGEN | $100M Series B a16z; valuation undisclosed |
| AVAIL | "Several hundred million" — single-source, unattributed |
| WLD | Series C ($115M) valuation undisclosed; prior $3B equity stale |
| DIMO | $11.5M Series A pre-TGE, valuation undisclosed |

*Caption: 16 tokens where either the last pre-TGE round went undisclosed or the round didn't exist. JUP is the important one — it is the 2024-cohort control: a token with no insider entry price. Any "VCs beat retail" claim has to exclude JUP from the VC side because there is no VC side.*

### Cohort summary — retail-side drawdown by vintage

| Cohort | n (24mo) | Median retail drawdown @ 24mo | Median retail MOIC @ 24mo | Tokens above TGE @ 24mo |
|---|---|---|---|---|
| 2021–22 | 5 | **+6.4%** | 1.06x | 3 of 5 |
| 2023 | 7 | **−52.5%** | 0.48x | 2 of 7 |
| 2024 | 10 | **−97.8%** | 0.02x | 1 of 10 |
| **All (pooled)** | 22 | **−84.8%** | 0.15x | 6 of 22 |

*Caption: Retail MOIC = price_at_horizon / TGE_open. The 2024 cohort at 24 months is the headline number: the median token has lost 97.8% of its TGE value. Only ONDO is above TGE in that cohort at the 24-month mark.*

### What the data actually shows

**One.** VCs in the 2021–22 cohort mostly did fine. OP returned 6.53x on entry FDV by the 24-month mark. APT 2.72x. The 2021–22 retail cohort is the only one where the median token is above TGE at 24 months (+6.4%). Said differently: back when the VC and retail windows overlapped in price, both sides had outcomes. The 2024 cohort is where the wedge opens.

**Two.** In the 2024 cohort, VCs remain in the money at 12–18mo on almost every name where we can calculate MOIC — often by wide margins — while retail is down 90%+. ENA, ATH, ONDO, SUI all show this clearly. STRK is the exception in the 2024 cohort: VCs priced in at $8B equity and the token launched at $44B opening FDV, then proceeded to crater to a $469M FDV by month 24 — VCs are at 0.06x. The punchline there is not "VCs lost." It's that the TGE was priced so absurdly relative to the Series D that both sides got crushed in absolute terms, with retail crushed much harder.

**Three.** The 12-month cliff is not a recovery moment. It's a drawdown moment. Of 22 tokens with both 12mo and 24mo data, **seven rose between the two horizons — and all seven were 2021–22 or 2023 launches.** Every single 2024-cohort token with both data points (ALT, JUP, DYM, PIXEL, STRK, PORTAL, ENA, W, TNSR, ONDO) **fell** between the 12-month cliff and the 24-month mark. The unlock schedule is aligned with further supply overhang, not mean reversion. You would expect this if the cliff initiates a linear-vest sell pressure into thin liquidity. You do not expect it if you believe the narrative that "tokens recover post-cliff."

**Four.** There is a hidden variable and it's called the post-TGE strategic round. ENA is the cleanest example. Dragonfly/Maelstrom entered at a $300M equity valuation in February 2024. The token opened at ~$0.60 in April 2024, implying a ~$9B FDV. **Then, in December 2024, Franklin Templeton + F-Prime + Polychain + Pantera + Dragonfly bought $100M of ENA tokens at $0.40 — a price below TGE open, implying a ~$6B FDV, not $9B.** This was not publicly announced until February 2025. The pattern — and WLD and EIGEN have close analogues — is that post-TGE strategic rounds let sophisticated capital re-enter at discounts to TGE once the public price discovery is done. The VC cohort that matters to retail's loss is therefore not just the pre-TGE cap table; it is the pre-TGE cap table plus the post-TGE re-entry at lower prices. Retail cannot access either.

**Five.** JUP is a control case and should be treated as one. Jupiter raised zero VC capital pre-TGE. Meow's public position was explicit: "Horrible VC sales scarred Solana DeFi 1.0 — Solana DeFi 2.0 is changing that definitively." JUP still had a rough outcome for retail at the 24-month mark (−90.2%). So the "VCs cause retail losses" claim cannot be "VC presence is sufficient" — JUP had no VCs and fell anyway. The more careful claim the data supports is: **when VCs are present and priced meaningfully below TGE, the 24-month retail outcome is nearly always negative and the VC-retail return wedge is enormous.** When VCs are absent, the 24-month retail outcome can still be negative but the wedge claim doesn't apply because there's no other side.

**Six — the "75%" fail claim and the fund-level backdrop.** The earlier VC subsection asserts "75% of venture-backed businesses fail." The canonical source is Shikhar Ghosh's Harvard Business School research on 2,000+ U.S. venture-backed companies funded 2004–2010, reported by Deborah Gage in *The Wall Street Journal*, Sep 20, 2012.² Ghosh's own framing is more careful than the headline suggests: 30–40% of venture-backed companies liquidate and lose all investor money; ~75% fail to return invested capital to LPs (the "75%" figure most-cited); over 95% fail to hit the return projections that underwrote the original investment. The NVCA's lower ~25–30% figure and Ghosh's 75% figure are not in disagreement — they use different definitions of "failure." For the essay's purposes, the return-of-capital definition is the relevant one.

What matters more for the argument is the fund-level consequence. AngelList's *State of U.S. Early-Stage Venture 2024* reports that the median 2017-vintage fund on its platform has **0.29x DPI at year eight** against **3.57x TVPI** — eight years in, the median fund has returned 29 cents on the dollar in cash while carrying 3.57x of paper markups.³ Industry-wide, the DPI-to-TVPI ratio fell from 16% in 2019 to 11% in 2024. Carta's Q4 2024 fund performance data cross-confirms: **63% of 2019-vintage funds on Carta had returned zero capital to LPs by Q1 2025**; the median 2017 IRR fell from 16.8% (Q4 2021) to 12.0% (Q4 2024).⁴ The Kauffman Foundation's 20-year LP audit remains the sharpest single-source indictment: **62 of 100 funds failed to beat a public-market equivalent after fees and carry**; of 30 funds above $400M in committed capital, only 4 beat a small-cap index.⁵ Bill Gurley's summary line is still the cleanest: *"The only type of return that's guaranteed is excessive fee income."*⁶

**This is the setup for why crypto changes the math.** In traditional VC, a failed company returns zero to the fund and the DPI drought is a function of exits that never arrive. In crypto-VC, the token itself is the exit event — retail becomes the buyer of last resort for paper positions that would otherwise sit illiquid on a balance sheet waiting for an IPO or acquirer that may never come. A "failed" project — no product-market fit, abandoned roadmap, zombie DAO — can still generate 3–10x MOIC for the VC on a pre-TGE cost basis, because the TGE is a liquidity event underwritten by retail bid rather than by a diligent acquirer. The same 75% failure rate therefore generates wildly different fund-level economics in crypto than in traditional venture. This is the mechanism that turns the DPI drought above into the launchpad reality check that follows. Traditional VC's bad math gets papered over with crypto's retail-funded exit, which is why the fund-size-destroying-returns dynamic Kauffman documented in 2012 doesn't fully apply to the crypto-VC books that matter to this essay — and why the asymmetry between VC MOIC and retail MOIC in the 2024 cohort above is so stark.

² Shikhar Ghosh (Harvard Business School), reported in Deborah Gage, "The Venture Capital Secret: 3 Out of 4 Start-Ups Fail," *The Wall Street Journal*, Sep 20, 2012. HBS press summary: https://www.hbs.edu/news/Pages/item.aspx?num=487. Definitions: 30–40% total liquidation; ~75% fail to return invested capital (most-cited); >95% miss projected returns.

³ AngelList, *The State of U.S. Early-Stage Venture & Startups: 2024*, https://www.angellist.com/data-center/the-state-of-venture-2024.

⁴ Carta, *VC Fund Performance Q4 2024*, https://carta.com/data/vc-fund-performance-q4-2024/.

⁵ Diane Mulcahy, Bill Weeks, Harold S. Bradley, *We Have Met the Enemy… and He Is Us: Lessons from Twenty Years of the Kauffman Foundation's Investments in Venture Capital*, Kauffman Foundation, May 2012, https://www.kauffman.org/wp-content/uploads/2012/05/we_have_met_the_enemy_venture_capital_report_kauffman_foundation.pdf.

⁶ Bill Gurley, quoted in Connie Loizos, "Bill Gurley doesn't think these giant new funds are such a great idea," *TechCrunch*, Apr 14, 2016, https://techcrunch.com/2016/04/14/bill-gurley-doesnt-think-these-giant-new-funds-are-such-a-great-idea/. See also Gurley, "On the Road to Recap," *Above the Crowd*, Apr 21, 2016, https://abovethecrowd.com/2016/04/21/on-the-road-to-recap/ ("Unlike the 1999 bubble, everyone was 'successful' on paper, but there was little to show in real cash-on-cash returns").

### Methodology note

Prices are 7-day means of daily closes from DefiLlama's historical API (`coins.llama.fi/chart`), centered on TGE+365, +545, +730. DefiLlama aggregates CoinGecko and on-chain DEX prices; it does not expose per-day volume on this free endpoint, so the windows are arithmetic means rather than true VWAPs. 23 of 31 elapsed horizons were cross-verified against CoinGecko's free `market_chart/range` API (365-day rolling cap meant earlier horizons could not be cross-checked). Max absolute deviation between sources was 2.21% (JUP 18mo); all deviations were below the 5% threshold. Entry FDVs are the **last private round before TGE** where the round's valuation was disclosed with High confidence; where equity valuation was disclosed but token FDV was not, equity is used as a conservative proxy (this tends to *understate* VC MOIC because token warrant economics were typically richer than equity tickets, per ICO Analytics on SUI and similar). Total supply is the current fully-diluted supply per CoinGecko; where total supply drifted post-TGE due to emissions or burns, this introduces ~5% noise in FDV calcs but does not move the qualitative picture. EIGEN's 18mo print is 14 days past horizon as of pull date and will shift as more days accumulate; cross-verified within 0.5% on CoinGecko at pull.
---

## II.c If VCs are the problem, is crowdfunding the answer?

The clean response to everything above is: fine, build the launchpad that routes around VCs. Let retail enter at VC prices. Let reputation-based allocation replace logo-based signal. Let a fair launch decide the cap table. This has been the thesis animating Echo, Legion, and MetaDAO — three platforms that have absorbed most of the "ICO 2.0" oxygen over the last eighteen months.

It is worth walking through each one carefully, because the headline pitch and the empirical result disagree in instructive ways. This also sends us forward to Haseeb's line that will show up in the Pushback section below — he specifically named Echo and Legion at DAS New York when he said the idea of circumventing VCs is a bullshit narrative. The data mostly agrees with him. Not for the reasons he thinks.

**Echo (Cobie's platform, now owned by Coinbase after a ~$375M acquisition in October 2025).** Echo's stated mission is to give non-institutional angel-adjacent capital access to the same rounds VCs see at the same prices — no 3–5x markup for being downstream of the VC. The syndicate product has facilitated ~30+ private rounds (Ethena, Monad, MegaETH, Usual, Morph, and others), cumulatively raising ~$100M. The Sonar public-sale product, launched May 2025, ran the Plasma (XPL) sale in June 2025 — $500M FDV, $373M raised, 4,000+ participating wallets with a $12K median deposit. That median number is genuinely broader than any VC-only round. Non-US Plasma participants unlock before insiders. Price parity holds: Echo/Sonar participants buy at the same per-token price as the VC lead. All of that is real.

Two structural facts cut against the framing. First, the Echo private side is accredited-only. You need the SEC's $1M net worth / $200K income status to participate, plus an invite into the syndicate. That is angel democratization, not retail access. The addressable population is in the low tens of thousands of people worldwide — a large subset of existing VC LPs and HNW crypto natives, not the median user of the protocol. Second, on the Sonar side, even with 4,000+ participating wallets, **insiders still took 90% of Plasma's supply.** Retail got 10% of the cap table in exchange for $373M, while the other 90% went to team, investors, ecosystem, and Tether-aligned stakeholders. The unlock advantage — non-US retail unlocking before insiders — is real, but it is the *edges* of the distribution that got softened, not the distribution itself. The power structure remained what it was.

**Legion.** Self-described "world's first ICO underwriter," MiCA-compliant, built around a reputation-scoring allocation model (on-chain history, wallet age, dev activity, social signal — no $LEGION staking required). Kraken partnership September 2025. Backers include VanEck, Brevan Howard Digital, Delphi Labs, and CoinGecko. ~18–20 completed sales, ~$36M cumulative raised. The empirical track record is ugly: **Legion's own aggregate 6-month ROI is 0.63x** per CryptoRank — participants who hold have lost money on average. FRAG is down 98.8% from ATH per CoinMarketCap. RESOLV is down ~86%. FUEL pumped 3x at TGE and then sold off heavily because the 100% unlock at TGE was weaponized by participants front-running insider selling. Messari's late-2025 report stating only 6 of 41 token sales since January 2025 remain profitable (average loss 46%) almost certainly includes multiple Legion names given those price charts.

Legion's allocation mechanism is genuinely innovative — reputation-scoring is a real attempt to price participation by something other than ticket size. But the rest of the token stack is unchanged: tiny retail carveouts (1–5% of supply), insiders still taking 95%+, cliff + linear vesting after TGE. Retail gets a larger share of a smaller pie and the pie was always going to be dumped on them. If the token economics of the underlying launch are broken — low-float, high-FDV, aggressive insider allocation — no allocation fairness at the sale corrects them. Legion rearranged the deck chairs. The ship still sank.

**MetaDAO.** The outlier. Built on Solana, pseudonymous founder (Proph3t), $2.2M seed from Paradigm in August 2024. What makes MetaDAO different from Echo and Legion is that it attacks the *underlying incentive structure* rather than the allocation mechanism. Token holders get enforced cash-flow rights via a DAO LLC structure. Founder allocations are locked into performance packages — not cliff + linear — meaning you do not unlock on a calendar, you unlock on outcomes. Treasury is controlled by futarchy: prediction-market price signals rather than governance voting. IP is locked into the DAO LLC in the Marshall Islands. The "team dumps on retail" loop is mechanically harder to run because the team's allocation is contingent on the protocol being worth something, not on the calendar turning over.

mtnCapital, a MetaDAO project from early 2025, was **liquidated by a futarchy vote** after underperforming — USDC returned to token holders. That is the mechanism working exactly as designed. Umbra launched at $0.30 ICO price, ran to $2.47 ATH in October 2025 (8.2x), and trades ~$0.40–$0.57 in April 2026 — still 1.3–1.9x the ICO price, which is vastly better than the Legion or Sonar benchmarks. Messari's report claims 4 of the 6 profitable 2025 launches came from MetaDAO — if that holds, it's the strongest platform-level validation available.

And here is the punchline: **MetaDAO's total platform market cap is ~$34 million.** All of it. Every project that has launched on the platform, summed together. By contrast, Plasma alone raised ~$373M on Sonar in a single event, and MegaETH + Monad through Coinbase added hundreds of millions more. The market is rewarding access theater at roughly a hundred times the scale it is rewarding actual structural change. The platform that solved the problem is the smallest platform in the set.

This is not a product problem. It is not that MetaDAO's UX is worse or its tokenomics less legible. It is that the market does not, at the participant level, reward the structural change that would actually dilute insider power. Retail will rotate capital into Plasma at $500M FDV because Plasma is the narrative. The narrative is downstream of VC backing, which is downstream of the intermediation economics Haseeb described. The launchpads inherit the incentive structure they were designed to disrupt because the *attention* flows on the old graph. Echo is VC-backed. Legion is VC-backed. Even MetaDAO took a $2.2M Paradigm seed. You cannot route around the VC stack using infrastructure built inside it, because the distribution signal — which VC, which CT KOL, which podcast, which Twitter Space — is still the network that decides what retail buys.

The dialectic predicts exactly this outcome. The trader-builder frame says that as long as traders are mimetically coordinated on VC signal — following the meta, following the money, treating the logos as underwriter — the builder has no choice but to route through the intermediary whose signal the traders are tracking. The alternative is MetaDAO's path: refuse the signal, accept a smaller launch, attach cash flow rights and futarchy governance, and watch the majority of attention flow past you to the better-branded VC-backed competitor. This is a coordination problem masquerading as a product problem. Until traders start pricing in the structural features MetaDAO offers — founder-lock, treasury-lock, IP-lock, liquidation-by-market — the reward function points the wrong way and the signal keeps flowing through the old channel.

Crowdfunding is not a wrong answer. It is a right answer being drowned out by the persistence of the intermediation signal that the original promise of the space said we were going to eliminate. And until the signal problem is solved, the structural solution stays at $34M and the access-theater solution keeps raising hundreds of millions per launch.
---

## III. The Mechanics That Sustain the Game

The empirical frame above cuts against a story you may have seen elsewhere: the launch event as the crime — retail buys vapor, VCs dump, price collapses. That framing isn't quite right. The launch is where retail enters at a local top, but the real extraction plays out over the unlock horizons and in the post-TGE strategic rounds that retail can't access. VCs remain in the money at 24 months on almost every 2024-cohort token where MOIC is calculable, while retail is down 90%+. That's not a launch-day crime. It's a schedule-long asymmetry.

What follows is the machinery that keeps the game running even when every participant can see it's rigged: the supply-side tokenomics (Low Float / High FDV), the behavioral economics of the founder's choice, and the cultural machinery (Girardian mimesis, scapegoating, the libertarian-shield pushback) that prevents the game from ending. These are what you'd have to dismantle if you wanted a different outcome.
### Low Float High FDV

Low Float High Fully Diluted Value means that a token has a very high valuation, often in the billions of dollars but a low float of supply on the market. Many venture capital backed projects launch tokens with this configuration. Why is that?

When projects raise money they build out a capitalization table. Let's consider a startup that sold 25% of its tokens to BigBird Capital. The team has 75% of the tokens remaining. Lets say another 25% is given to employees, KOLs, former employees, founders, and any advisors. 10% of the supply is allocated to the parent foundation, 10% is allocated to the labs co, 25% is allocated to the community's ecosystem fund for future airdrops, strategic initiatives liquidity mining incentives, and 5% is airdropped to retail traders who met the airdrop criteria.

When the token launches assuming the airdrop claim is part of the launch, let's assume only 10% of the float of the token is tradable.

### The Founder's Dilemma

Founders who print tokens ex-hililio (from nothing) have the best returns of all. In some sense its a real life example of the marshmallow experiment. You stick a founder in the room and tell him you can have 8 figures now or spent the next ten years of your life making your project valuable delivering value for token holders and you'll receive a nine figure payout. Which do you prefer?

It's very tempting to take the money. Let's say you secure $10 million selling some locked tokens at a discount on the secondary market. This is less transparent than selling post TGE either on a CEX or DEX. This provides a way to at least temporarily side step any FUD and mask your actions. When asked you might even be able to plausibly convince yourself that you both deserve everything that is coming to you and that you did not sell one token. You only sold locked tokens which are not even real yet. You're just borrowing against the future value your protocol is creating, right?

Let's say you take half and pay your taxes, you then take most of it lets say $4.4 million and put it in an instrument that yields 7%. You don't touch it for three years. You take $600k and allocate it for three years of living expenses. At the end of three years you'll have around $5,028,342.20 assuming you paid your taxes at a 35% rate every year. From there on if you continue to withdraw your annual salary of $218,833 at this rate of return, you're set. But its not really enough to walk away with a damaged reputation. But everyone has a number, right? Would one order of magnitude be enough? How about two orders of magnitude more? For a handful of people no, its not. For some it is a lust for power and a greed for life that drives them towards acquiring more. Alternatively, other people are not driven by money and only need some minimal living conditions and amenities to thrive.

Not everyone is a grifter, some people have character. See Satoshi as an example. The identified Satoshi wallets remain unchanged. If that isn't a form of leading by example and altruism I don't know what is.

### Mimetic Desires

Much of the fundraising from venture capital in crypto requires accepting the notes, suggestions, and ideas of venture capitalists. While there are some brilliant investors out there who do contribute significant ideas to the space, most of the ideas are simply copy-cats based on a current meta of the moment.

The late Rene Girard has a theory about this called mimetic theory or sometimes called mimetic desire. We want what other people want because we imitate the desires of others.

> "Man is the creature who does not know what to desire, and he turns to others in order to make up his mind. We desire what others desire because we imitate their desires." — René Girard

How much of the investment behavior is driven by mimetic theory? I'd venture to guess a good deal. When one cohort of investors decides its time to start investing in a given category with a juicy narrative market fit, many follow. VCs need to explain to their LPs why they missed on a particular narrative that outperformed.

Let's take a look at VC investments by category since 2014. We'll use data from DeFi llama in the raises section. There are more than twenty categories in the dataset. We aggregated them to three:

- **Infrastructure** [Infrastructure, L1, L2, Bitcoin infrastructure, Blockchain-as-a-Service, Cross-chain, Lightning, Mining, Oracle, BRC20, Zero-knowledge, ZK, Security, Privacy, Interoperability, EVM, Staking, Liquid staking, Scaling Solution, PoS, Liquid Staking Protocol, Smart Contract Platform, Zero Knowledge Industry, L3, Trading, Analytics, Web3, Digital Identity, Wallet, AI, IoT, Storage, Hardware, Cloud Security, Information security, Insurance, Stablecoin, Stablecoin Infrastructure, Taxes, Investment, MEV, Cybersecurity, Education, Meme, Smart contract security, Smart contract audits, News, Healthcare, DePIN, Supply Chain, Crypto Intelligence, Betting, Information services digital assets, Real Estate, Voting, Travel, Incubator, Liquid Restaking Protocol, decentralized protocol for perpetual options]
- **CeFi & CEX** [CeFi, Centralized Exchange, CEX, Banking, CeDeFi, CeFi Yield, Custody]
- **Applications** [DeFi, Gaming, Game, GameFi, NFT, Social Platform, Social, DAO, DAO infrastructure, Metaverse, Payments, DeSci, Lending, DEX, SocialFi, Real World Assets, RWA]

Two things stand out:

- 83.6% of investment is not directly related to applications
- The Infra category represents 65.2% or $74.2B invested. The infra to app ratio says that since 2014 for every $1 invested in applications $3.98 was invested in infra

Investment by cycle:

- 2014-2016 ~ $1.792B
- 2016-2019 ~ $25.41B
- 2020-2022 ~ $63.38B
- 2023-2025 ~ $23.24B

Interestingly we can see that this current cycle to date has seen less investment than previous market cycles. In the middle two investment cycles (2016-2019 & 2020-2022) infrastructure was by far and away the most desired category. The 3.98:1 infra-to-app ratio isn't just VCs being dumb or copying each other. It's VCs rationally selecting the category where the token-as-exit strategy works best. You don't need product market fit to launch an L1 or L2 token — you need a whitepaper, a testnet, and a narrative.

Thought experiment: if we subtract Bitcoin's market cap, $1.855T, from the total crypto market cap, $3.027T, we are left with $1.172T. Remove ETH returns as well because investors could not participate in the ICO. Take out USDT and USDC from the market cap and you're at $748.6B. If you divide this by the total amount of venture investment $113.8B you're looking at 6.6x multiple of return on investment since 2014.

### Scapegoats

Mimetic desire has two parts the desire and the scapegoat.

> "René Girard saw that for thousands of years humans have had a specific way of protecting themselves in a mimetic crisis: they converge, mimetically, on one person or group, whom they expel or eliminate… Someone who is unable to fight back. A scapegoat." — Luke Burgis

Sometimes venture capitalists who lose money on their positions will blame the project's failure on founders and builders. Founders who fail to find PMF will often look to blame traders for poisoning the culture. Builders will scapegoat their founders for engaging in shady activities. Traders scapegoat founders, builders, and VCs for launching garbage projects that are literally vaporware with a coin attached. Do these players agree on who is the scapegoat? Yes, but what you believe largely depends in what position you sit.

### Pushback

Now I know what your thinking. I am clearly making a moral argument and crypto is inherently "Libertarian". No one is forcing anyone to buy any tokens. People should do their due diligence and take responsibility for their investment decisions.

I'm going to pushback on that line of argumentation.

The space was founded on the idea of eliminating intermediaries or middle men. From a recent panel at DAS New York titled "Do VCs matter" Haseeb from Dragonfly says:

> Obviously VCs have multiple different functions in the market. The reality is one of the functions of VC is kind of as an underwriter… The market takes that signal very seriously. You go look at something on Legion or Echo and the first thing you look at is who are the VCs? **Almost all these platforms are VC backed. The idea that there are all these things circumventing VCs entirely is kind of a bullshit narrative.** — Haseeb Qureshi

What he is saying is that the market takes signal from VCs. This is a reasonable take at first glance. But the engaged reader who read this far knows that the numbers tell a different story, and that often VC projects are counter-signal to astute market participants. The bolded text importantly acknowledges the function venture capitalists play, a function which intermediates investment and token creation. Again, this is not aligned with the core values of the space. We want to eliminate intermediaries where possible.

*And here's the thing about intermediaries: they have obligations. In traditional markets, underwriters face legal liability. They conduct due diligence not out of goodwill but because the SEC will come after them if they don't. Crypto VCs have assumed the underwriter's role — Haseeb says so himself — without assuming the underwriter's obligations. They take the economics of intermediation while disclaiming the responsibilities that come with it. If you're going to sit between capital and projects, collecting fees and carry for that privilege, you owe something to the people on both sides of you. This isn't a radical claim. It's the basic ethical framework that every functional market in history has converged on.*

Second, Immanuel Kant's Categorical Imperative says:

> There is only a single categorical imperative and it is this: act only in accordance with that maxim through which you can at the same time will that it become a universal law…

Another way of stating this is act as if your action were to become through your will a universal law. The combination of crypto's core values (eliminate the middle man) and the Categorical imperative which is rooted in western culture suggests you can't hide behind the anything goes argument. It's simply an incorrect reading of the crypto Geist.

---

## IV. The Synthesis: Towards VC Signal

The only way to resolve the conflict is moving to higher ground, where builders and traders trust each other, and speak openly—not to virtue signal but to learn from and be accountable to each other.

We need to break out of this messed up system where founders and VCs collude to extract value while everyone else gets screwed. In our ideal state, we kill the misaligned incentives and information asymmetry bullshit, replacing it with direct builder-trader connections where everyone's incentives actually align. Capital still flows, but it flows organically to deserving projects through traders who recognize value rather than via inflated token launches and marketing hype. Builders create actual useful products, traders allocate capital and attention to builders who aren't running scams, and if VCs want to stay in the game, they have to add legitimate value rather than just orchestrating exit liquidity events.

### The Problem with the Current Model

This did not always used to be the case. In a previous era, projects would turn directly to the "internet capital markets" (crypto) to raise money. Ethereum raised $18.6M at ~ $37M FDV. Ethereum's current FDV ~ $217B. Anyone who purchased $1000 of ETH in the ICO and held now has ~ $58M. There was no further fundraise beyond the ICO, retail was able to participate and buy at the same valuation as everyone else. Price discovery happened in the public crypto markets where anyone had an opportunity to participate.

Until retail can access coins backing credible teams at reasonable valuations, we will continue to see a bloodbath in price action for pure venture-backed coins (see all the Ethereum L2 charts as an example). More profit sharing between retail and investors is critical to the long-term sustainability of these projects. Without strong token distribution to community members who will trade, HODL, stake, participate in governance, re-stake, and use their receipt tokens in DeFi most of these VC projects will fail (the crypto economic flywheel needs to turn). But a continued failure of venture capital backed projects will push entrepreneurs to pivot to Silicon Valley backed startup product verticals like AI. If the crypto industry has fewer quality projects to back, we will see less innovation and disruption.

### VC Signal: A Concrete Proposal

From first principles, could we design a mechanism such that venture capital and retail investors can participate in funding projects together at fair valuations? Yes, we can. Both Echo and Legion today demonstrate examples of this. One key thing to note is that they require some form of intermediation. Projects need to be vetted before being listed and users may need to KYC or answer survey questions to participate in the platform's public sale offerings. One of the goals of crypto has been disintermediation. Where we can preserve these ideals and make our systems and applications verifiable, permissionless, and censorship-resistant, we should.

VC Signal is an application that aims to create fairer token distributions by allowing both retail investors and venture capitalists to participate in crypto project funding at reasonable valuations, while retail investors benefit from VC signal.

The basic idea is to build an intent-centric version of Kickstarter which, in addition to being supply-side driven, can be demand-side driven. In Kickstarter only the producers can interact with the platform, while the backer has no ability to express their preferences for what projects they want to see offered on the platform.

In this type of game, a startup could decide to reserve a specific portion of the token allocation for venture investors, an allocation for public sale, an allocation for team and foundation, or DAO. For reference, Ethereum sold 60 million/72 million tokens in its ICO. Think about the typical pizza pie. You try and split the pie in the optimal way. Perhaps the optimal split is 1/3 for team/dao/foundation, 1/3 private investors, 1/3 public investors. Perhaps the optimal split is 40% VC, 40% retail, and 20% team and foundation/DAO.

### Intent-Based Participation

One specific use case of this application could be to back projects that both retail and VC backs. Retail users could have an intent that says if this array of Venture Capital projects back this startup, I will invest $XXXXXX.

Intent Examples:

- **Identity based;** you submit an intent that says you will donate 1 ETH to any project Vitalik donates at least 10 ETH to.
- **Information based;** you submit an intent that says you will donate 1 ETH to any project doing superconductivity research.
- **Incentive based;** you submit an intent that says you will donate 10 ETH to any project offering a refund bonus.
- **Threshold based;** you submit an intent that says you will donate 10,000 NAM to any project that raises > 50% of their campaign goal in 1 week or less.

From the demand side, another feature could be users via voting aggregate their demand into one intent that says we will commit 1/3 of funding (pizza) up to $10M (arbitrary amount) to any project backed by a combination of this array [investor 1, investor 2, investor 3.... ] of investors. This would give retail the ability to signal their preferences credibly.

There are many ways to split the pie. The most important outcome is finding the right allocation between venture capital and public sale investors or retail. Discovering the optimal pie allocation will come as a positive side effect of VC signal.

*VC Signal users may also want the ability to filter out particular venture capitalists based on past behavior as a way to not reward them, which will tip founders not to accept investment from these firms in the future. This creates a market-based accountability mechanism: retail gets a lever it has never had before. If a VC consistently backs projects that extract from retail, traders can collectively blacklist that firm. The VC doesn't lose their fund — they lose deal flow, because founders know that accepting their money now means losing retail participation. This is shorter-term accountability than the decade-long DPI cycle, and it comes from the people who actually bear the cost of bad VC behavior rather than from LPs who may not even know their capital is in crypto.*

*VC Signal also functions as a credible information layer. Today, much of crypto's information environment — pay-to-play conferences, sponsored podcasts, KOLs with undisclosed compensation, press releases dressed up as news — is economically aligned with the extractive loop this essay diagnoses. VC Signal cuts through that noise because signal is expressed through capital commitment, not content. When a trader puts money behind a project conditional on specific VC participation, that's not an opinion — it's a bet. And bets, unlike tweets, have consequences.*

### Challenges

There are significant challenges in bootstrapping a new marketplace like this:

- Bootstrapping VC and trader participation (need both)
- Sourcing Deal Flow for the platform
- Pricing mechanism
- Game-theoretic challenges
- Regulatory or legal hurdles
- Reputation systems (slow game)

More thought should be given to product requirements for a minimal viable version. Thinking through dominant strategies and edge cases of the game will also be important.

### Why This Matters

If this happens, we can unlock alternative funding models. Traders are fantastic at allocating capital. They are a discerning bunch that can allocate capital to deserving projects effectively, but not projects that simply exist to launch a token. In this environment there will still be a need for plenty of infrastructure research, which also needs to be understood. We are not close to the final forms of blockchain architectures. The endgame that you think is the endgame is much closer to the beginning than the end. But the research doesn't need to conflict with shipping great product features that traders care about today.

In any game you play its good to have strategies for short term dynamics of game play whilst having an overall strategy for how to win in the long run. A delicate balance can be struck.

The objective of this application is to better align incentives with retail, venture capitalists, and teams building moonshot technology. With the era of regulation by enforcement coming to a close, it's time to try some experiments that have the potential to not only save crypto but the other malfunctioning industry called Venture Capital.

---

## V. The equalizer was never the distribution mechanism

One more move before we close. Everything in this essay so far — the intermediation argument, the VC return data, the launchpad reality check, the structural and cultural mechanics, the VC Signal proposal — has been arguing over *who gets the allocation*. That framing assumes the thing being allocated — a built product, a running network, an economy worth coordinating capital around — already exists, and the fight is over who shares in the upside.

Walk that assumption back one step. The reason tokens, launchpads, and VC rounds exist as coordination infrastructure at all is that historically, building a protocol required coordinating capital across many people because no individual or tiny team could afford to ship the thing alone. That was the binding constraint. Everything downstream of that constraint — the dialectic, the intermediation, the allocation games — is coordination machinery built on top of the binding constraint. If the constraint loosens, the entire layer of machinery above it becomes overbuilt.

Between 2020 and 2025 something quiet happened that changes the binding constraint structurally. The cost of producing the first production version of a pure-software company fell by roughly three orders of magnitude. Not because infra got cheaper — AWS was already cheap. Because the labor of the first several engineer-equivalents collapsed onto a ~$200/month Claude subscription and a driven operator. Base44 — Maor Shlomo, solo — spent $10–20K in personal capital, reached $1M ARR three weeks after launch, and sold to Wix for $80M cash after six months. Midjourney is 11 full-time people at inflection, $500M ARR, zero VC. Hyperliquid is 11 engineers, $1B+ annualized revenue, fully self-funded from the founders' trading P&L. These are existence proofs, not projections.

The documented shifts don't extend cleanly to every domain. Frontier model training still takes $1–40B rounds. Anduril and Saronic raised $2.5B and $600M in 2025 for defense hardware. Isomorphic Labs raised $600M for drug-candidate model training and wet lab infrastructure. Network-effect businesses still reward first-mover distribution capital. Regulated fintech cannot prompt-engineer its way past a state licensing queue. The right phrasing is: **capital was displaced as the constraint on software company formation, not on all company formation.** The subset of products that crypto token-distribution mechanisms were designed to serve — small software teams shipping protocols — sits squarely inside the subset where the constraint dissolved.

This cuts the dialectic at an angle the rest of the essay couldn't reach.

**First — the dependency inverts.** The trader-builder dialectic as developed earlier treats the builder as the dependent pole. The builder needs the trader's capital to ship. The launchpads, the VC rounds, the token mechanics, the underwriter-proxy role — all of that exists because builders cannot afford to build alone. The whole shape of the intermediation problem is that capital and production are separated by a coordination gap that the intermediary fills at rent. If the labor cost of the first ten engineer-equivalents goes from ~$2M/year to ~$2K/year, **the gap closes.** The builder no longer needs the trader to ship. The trader is still needed downstream for liquidity and exit, but is no longer structurally upstream of creation. This is not reform; it is sublation. The dialectic's dependent pole collapses, and what survives on the other side of the aufhebung is a world where shipping no longer requires permission from capital, which was the original claim of the space fifteen years ago.

**Second — the token's coordination function becomes local, not universal.** The essay frames tokens partly as a coordination technology: how do you pool capital and attention to get something built? If a solo founder can ship in six months on $20K, *what is the token coordinating*? Not labor — one person. Not capital — $20K. For the subset of products that were tokenized as a capital-formation hack, the hack is obsolete. For the products that genuinely need decentralized coordination — credibly neutral infrastructure, open networks, protocols that need to be owned by no one — the token still does real work. Hyperliquid shows the pattern: self-fund the product, let the token be downstream of an already-working thing rather than upstream funding of a promise. The token coordinates the network, not the team. That's a distinction the 2020–2024 token playbook blurred deliberately and AI makes sharp.

**Third — VC stack capture becomes a dead layer.** The historical VC value proposition was five things: capital, hiring network, distribution/BD, credibility, pattern-matching advice. AI cleanly commoditizes (1). It partially commoditizes (2) and (5). What remains valuable is (3) and (4) — distribution and credibility — which is exactly what Sequoia and a16z partners have been writing about in 2025 as they move "downstream" to seed and defend network-effect positions. Applied to our context: the integrated VC-launchpad-market-maker-exchange stack that the essay's critique has been dissecting was engineered for a world where builders needed all five things from one counterparty. When builders only need (3) and (4), the integrated stack is overbuilt and the parts that were valuable as leverage over founders become irrelevant. The stack capture strategy — own every layer between idea and liquidity, extract rent at each — becomes a dead layer because the builder isn't passing through it anymore. They're shipping direct.

So there's a Kantian point to land. The essay's earlier invocation of the categorical imperative — *act only in accordance with that maxim through which you can at the same time will that it become a universal law* — is sharper now than it was pre-AI. Imagine the VC-backed-token-launch maxim universalized in 2020: every builder raises, every project tokenizes at a premium, every exit is retail bid. Painful but the story had a pretense of necessity, because coordination of capital was genuinely the bottleneck. Universalize the same maxim in 2026 and it fails harder. The necessity claim is gone. You cannot say with a straight face "we needed the VC + launchpad + token to get this built" when Base44 went solo to $80M, Midjourney went zero-VC to $500M ARR, and Hyperliquid went self-funded to a $1B+ revenue run rate. The maxim that was defensible-under-constraint is indefensible-under-abundance. The rent-extraction model fails the universalizability test more sharply, not less, than before.

This changes what the question even is.

The **old question** was: given that builders need external capital to ship, how do we fairly distribute the upside across builders, early users, and capital providers? Launchpad reform, fair launches, retroactive airdrops — all answers to *that* question. All still worth doing for projects that are genuinely capital-coordinated networks. But also all increasingly localized to that shrinking subset.

The **new question** is: if a builder can ship without external capital, why tokenize at all? Three sub-cases. If the product doesn't need decentralization, it doesn't need a token — Base44 sold for $80M cash, full upside captured, no token-game machinery required. If the product genuinely needs decentralization — credibly neutral infrastructure, open protocols — the token still does real work, but downstream of a built product, not upstream of a promise. If the product is tokenized as extraction theater, it gets harder to defend in the AI-equalizer regime because the observed existence proofs make the "we needed the capital machinery" claim plainly false.

The launchpad debate solves an allocation problem from a world where capital was scarce relative to builders. In the AI-equalizer world, builders are scarce relative to capital — capital is chasing a smaller number of actual shippers, because the signal of "can ship" has become almost free to produce. The constraint moved. The game designed for the old constraint is being played on a changed board.

What to do with this, if you're shipping:

- If you're a builder with a pure-software product, the default should be *don't raise yet*. Ship the first cut on a subscription and rent. Raise if and only if the thing you're building has a capital-intensive moat — network effects, regulated integrations, compute — that a $200/month subscription cannot substitute for. The default inverted. It used to be "raise as soon as you can." It is now "ship as long as you can, then raise only what you can't substitute for."

- If you're a builder with a genuinely-decentralized product, separate the questions. The coordination question (do you need a network of participants?) is real and the answer can justify a token. The capital question (do you need external funding to build V1?) increasingly does not justify a token. Hyperliquid's order — ship, then token — is the template. Not the 2021 template.

- If you're a trader, the mimetic signal you're tracking — VC logos on the deck, launchpad backing, KOL alignment — is a signal about the *old* constraint. It tells you which project has successfully navigated the coordination-of-capital layer. In the new regime, that signal is less informative, because the best builders route around the coordination-of-capital layer entirely. You should be updating your priors toward teams that demonstrate output-per-person, not teams that demonstrate capital-coordination.

The synthesis the essay has been reaching for was never only in the distribution mechanism. The distribution mechanism was infrastructure built to route around a constraint. The constraint is now mostly gone, for the subset of products this essay cares about. The sublation isn't only "better launchpads." It's that the binding constraint the launchpads existed to address has quietly dissolved and the game is being played, for a little while longer, as if it hasn't. VC Signal remains the right answer for the projects that still need capital coordination. But the projects that don't need it have a cleaner option: build the product, then — and only then — decide whether a token is coordinating a network or coordinating nothing.
---

## Closing

Don't use the pretext of "innovation" to justify crime. You do a disservice to yourself, token buyers, and the crypto ecosystem at large.

Don't virtue signal to manipulate perceptions. Embrace the struggle and speak from the heart.

Don't pretend to care about things that contradict your revealed preferences.

If you want to play in the silicon sand box with taste masters start building real products that crypto users want.

If you want to build permissionless distributed applications with censorship resistance start building immutable applications with unstoppable code.

If you want to get rich then create value. If its not cash flows articulate the growth strategy with benchmarks for deliverables.

> Act only in accordance with that maxim through which you can at the same time will that it become a universal law

---

_Thanks for reading this far. You dear reader are a cognitive altruist. Cheers._
