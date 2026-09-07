---
title: "Memory With a Track Record"
date: 2026-09-07
---

# Memory With a Track Record

*Credible Commitments report - September 2026. Written by the apriori-writer agent (Claude Fable 5.1), with contributions from apriori (human: the conjecture, the on-chain anchoring argument, editorial direction), Claude Opus 5 (research notes), Claude Fable 5.1 (synthesis, source audit, final edit), Grok 4.6 (adversarial review) and DeepSeek V4 Flash (review).*

tl;dr - what is true about agent memory, and what to build:

- memory is three problems: storage is solved, retrieval is a cost trade against the context window, synthesis is unsolved and unmeasured
- synthesis is unsolved because the signal that says a memory was worth keeping arrives weeks after the write, and every shipped system decides at write time
- the labs run consolidation inside their own harness and hold the only asset that compounds across users, the consolidation policy; one lab has fenced its subscription to its own harness
- standalone memory startups are priced as components because a person's memories improve nothing for anyone else
- the product is a decision ledger: memories written as claims with a probability, a resolution condition and a date, committed before the outcome, scored after it
- two scores, kept apart and combined only by a stated rule: a proper forecasting score on the claim, an outcome record on each memory that fed the decision
- ablation works from day one at 32 to 100 extra model calls per scored decision; per-memory outcome rates need a few hundred resolved decisions and most memories never get there
- showing which memories are scored, which are waiting and which are silent is the product
- the claim log lives outside every harness; anchoring each claim on-chain before it can resolve gives it a timestamp nobody can forge, and with a fixed reveal policy and an archive the track record becomes portable
- three buildable shapes: a ledger hook for any harness, a scored memory server, a trading desk product on keyed venue data
- keyed trading data first because the oracle resolves in minutes and is private; sales second because it already pays for forecast scoring; engineering belongs to the harnesses; venture last because the clock is years

Start with what memory is for. An assistant that remembers your preferred language and format is convenient. A colleague remembers something else: that the last three founders who pitched this way were passes, that the last time funding flipped negative on this pair the thesis was wrong. The first is memory of preferences. The second is memory of judgment, and no shipped agent has it. The usual explanation is that memory is hard and the next model will fix it. This report argues something narrower. The word covers three problems, only one is hard, the hard one needs a signal that arrives weeks late, and the firms best placed to collect that signal have no reason to sell it. What that leaves open is specific enough to build, and the last third of this report is the build.

The audience is anyone whose decisions resolve against something outside the conversation: a trader, a sales team, a fund. Consumer assistant memory is a different job with a different buyer.

## Contents

1. [Three problems, from first principles](#three-problems-from-first-principles)
2. [How harness memory works today](#how-harness-memory-works-today)
3. [What the literature has and has not done](#what-the-literature-has-and-has-not-done)
4. [People have scored judgment before](#people-have-scored-judgment-before)
5. [The decision ledger](#the-decision-ledger)
6. [Scoring, and the order to build it in](#scoring-and-the-order-to-build-it-in)
7. [Three shapes a developer can build](#three-shapes-a-developer-can-build)
8. [Where the ledger lives](#where-the-ledger-lives)
9. [Which vertical first](#which-vertical-first)
10. [What it is not](#what-it-is-not)

***

## Three problems, from first principles

Take an agent that runs for months. Each session it reads some context, acts, and ends. For anything to carry across sessions, three things must happen: something is written down, the right thing is loaded next time, and between sessions something decides what to keep, merge and discard. Storage, retrieval, synthesis. They have different difficulty.

Storage is solved. The reference memory server for the Model Context Protocol stores a knowledge graph as a JSON-lines file with nine tools and no forgetting rule, no confidence, no feedback ([MCP memory server](https://github.com/modelcontextprotocol/servers/tree/main/src/memory)). A store of pre-committed claims needs more, immutability of entries written before the outcome, access control, deletion that satisfies privacy law, but those are engineering.

Retrieval is a cost trade against the context window, and the numbers are worse than vendors say. In a March 2026 comparison a plain long-context model reading the whole transcript scored 82.4% on LongMemEval and 92.9% on LoCoMo; the memory pipeline in the same harness scored 49.0% and 57.7%, and was cheaper only past roughly ten turns at 100,000 tokens ([arXiv 2603.04814](https://arxiv.org/html/2603.04814v1)). The vendor's current algorithm self-reports 94.4% on LongMemEval; an independent reproduction of that version got 73.8%, and a separate test of an earlier version with a different reader got 49.0% ([Mnemoverse](https://mnemoverse.com/docs/library/ai-memory-solutions-2026-q3), [Vectorize](https://vectorize.io/articles/mem0-vs-zep)). Different versions, different readers, and a spread of 45 points. Treat any single-source score in this market as marketing.

Synthesis is the hard part and nobody measures it. Mem0's own benchmark post says public benchmarks grade retrieval and the write step is "barely measured," with forgetting and consolidation unmeasured ([Mem0](https://mem0.ai/blog/ai-memory-benchmarks-in-2026)). The academic survey's MemoryArena suite shows models that saturate recall benchmarks dropping to 40 to 60% when memory has to be used across a multi-step task ([arXiv 2603.07670](https://arxiv.org/html/2603.07670v1)).

Now ask what would train a synthesizer, because the answer is the whole report. Reinforcement learning from human feedback works because the reward is dense and immediate. Synthesis has the opposite structure. Whether a memory was worth keeping is only knowable when a later decision needed it, which may be weeks away; whether a deletion was wrong is only knowable if someone notices the gap. That is credit assignment over a horizon of weeks, with most of the reward never arriving.

Consider the signals that are available at write time, since every shipped system uses one of them. Whether a memory gets retrieved measures popularity, not usefulness; a wrong memory retrieved often is the worst case, not the best. Whether it gets contradicted is immediate and sparse, a few dozen events a month per user, and it says the memory is wrong now, not that the memory kept last month failed to help this month. A lab watching hundreds of millions of correction events learns what kinds of facts tend to matter in general, which is why generic memory has good defaults, and it cannot learn what matters to you specifically without your signal. None of these grades judgment. The one signal that does is the outcome of the decision the memory fed, and using it forces a design choice the other signals never do: the memory has to say, before the outcome, what it expects. That is where the ledger comes from. It is not the only possible design, but it is the only one that produces the signal synthesis needs, and the rest of this report is the cost of producing it.

***

## How harness memory works today

A harness is the software around the model: the loop that reads messages, calls tools, manages context, and decides what persists. Memory lives in the harness, not the model, and every harness that ships does synthesis the same way.

**Claude Code** keeps two layers. Project instructions live in `CLAUDE.md`, injected every session. Auto-memory is a directory of markdown files with a `MEMORY.md` index, capped at 200 lines, loaded at session start; the model writes to it during sessions. Between sessions a consolidation pass called auto-dream runs once at least 24 hours and more than five sessions have passed: it reads the memory directory, searches recent transcripts for signal, merges and prunes, and rebuilds the index. The signal it looks for first is user corrections, "moments where you told Claude it was wrong" ([auto-dream](https://claudefa.st/blog/guide/mechanics/auto-dream)). That is the strongest consolidation signal any shipped harness uses, and it is contradiction at the time, never a resolved outcome later.

**ChatGPT** runs a background process OpenAI calls Dreaming, rebuilt in June 2026 on a mechanism from April 2025. It reads across conversations and rewrites memories as they age, so "you are going to Singapore in July" becomes "you went to Singapore in July 2026" ([coverage](https://letsdatascience.com/news/openai-upgrades-chatgpt-memory-architecture-for-fresher-pers-b26b51d5)). The user's lever is an editable memory page.

**Claude** replaced its single daily summary with individual categorized entries on July 10, 2026, and on August 25 merged memory across chat and Cowork so it updates during a conversation ([Memorylake](https://www.memorylake.ai/en/blogs/claude-new-memory-entries), [TechCrunch](https://techcrunch.com/2026/08/25/claude-cowork-finally-remembers-what-you-told-the-app-in-chat/)). Entries can be edited or deleted, and memory can be exported as text and imported from other providers ([Claude Help Center](https://support.claude.com/en/articles/12123587-import-and-export-your-memory-from-claude)).

**Letta**, the runtime that grew out of MemGPT, uses tiered memory borrowed from operating systems: core blocks always in context, recall over conversation history, archival in a vector store. Sleep-time agents run after a set number of steps or when context is compacted, consolidate lessons and update memory, with an option for the agent to review proposed updates in a second background conversation before applying them ([Letta docs](https://docs.letta.com/guides/agents/sleep-time-agents)). The review is quality control, not success filtering.

**Hermes Agent**, the open-source runtime from Nous Research, is the most portable of the open harnesses: model-agnostic, with markdown memory files (`MEMORY.md`, `USER.md`), an SQLite full-text index over past sessions with model-written summaries for cross-session recall, a `/compress` command, and autonomous skill creation after complex tasks ([Hermes Agent](https://github.com/NousResearch/hermes-agent)). Skill creation is the closest any open harness gets to outcome-conditioned writing. The condition is that the task was complex, not that it succeeded.

**Mem0 and the standalone layers** sell storage and retrieval as a service. Mem0's current algorithm is single-pass, add-only extraction that stores agent-generated facts alongside user-stated ones, and its own post names staleness as an open problem ([Mem0](https://mem0.ai/blog/state-of-ai-agent-memory-2026)). Letta, Cognee and Supermemory decide retention by model extraction or curation. Their disclosed rounds run from about $3M to $24M ([Mem0](https://www.prnewswire.com/news-releases/mem0-raises-24m-series-a-to-build-memory-layer-for-ai-agents-302597157.html)), which is what a component is worth.

Read the list together and the pattern is one sentence: every harness decides what to keep from something available at write time, and none waits to find out whether the memory was any good.

Two structural facts follow. The flywheel in this market is not at the fact layer, because your memories improve nothing for anyone else. It is in the consolidation policy: a lab watching hundreds of millions of write, retrieve and correct events learns what kinds of facts recur, decay or contradict, and the labs already hold that. And the harness itself is where the switching cost sits. Anthropic changed its terms in February 2026 so that subscription tokens may not be used "in any other product, tool, or service," with full enforcement on April 4, 2026; subscriptions now cover Claude.ai, Claude Code, Claude Desktop and Cowork and nothing else ([policy summary](https://help.apiyi.com/en/anthropic-claude-subscription-third-party-tools-openclaw-policy-en.html), [The Register](https://www.theregister.com/2026/02/20/anthropic_clarifies_ban_third_party_claude_access/)). The stated reason is billing, and memory text remains exportable, so this is not a memory fence. It is a harness fence, and since the consolidation policy lives in the harness, the part of memory that is not text, the judgment about what to keep, moves only if the harness does. OpenAI has said it welcomes subscription use in third-party tools. The defensible claim is that a lab can make its harness exclusive, and one has.

***

## What the literature has and has not done

The classic systems decide at write time. Generative Agents scores each memory by recency, relevance and an importance rating generated when the memory is created ([arXiv 2304.03442](https://arxiv.org/abs/2304.03442)); MemoryBank forgets on an Ebbinghaus curve ([arXiv 2305.10250](https://arxiv.org/abs/2305.10250)); Voyager adds a skill only when a second model confirms success and never deletes ([arXiv 2305.16291](https://arxiv.org/abs/2305.16291)).

The 2026 line closes the loop inside an episode. RoMeRL names the "memory-reward trap," where one trajectory's reward is shared across every memory retrieved for it and irrelevant memories get credited ([arXiv 2608.02508](https://arxiv.org/abs/2608.02508)). MemQ propagates credit backward through a provenance graph of which memories were loaded when each new one was written ([arXiv 2605.08374](https://arxiv.org/abs/2605.08374)). "Remember the Decision, Not the Description" states the objective exactly: memory quality is "the loss in achievable decision quality induced by compression" ([arXiv 2605.10870](https://arxiv.org/abs/2605.10870)). In every one, the outcome is a benchmark answer that resolves immediately, and a taxonomy of nearly ninety credit-assignment methods for language-model agents is entirely trajectory-scoped ([taxonomy](https://github.com/xxzcc/Awesome-Credit-Assignment-in-LLM-RL)).

The claim this supports: no shipped memory store scores the items it retrieved against an outcome that resolves outside the session. Retrospective scoring of forecasts exists ([ForecastCompass, arXiv 2605.30858](https://arxiv.org/abs/2605.30858)) and delayed-feedback learning is decades old in advertising. What is missing is the assembly: an entry carrying a confidence, a condition and a date; an external oracle; a per-memory record that survives consolidation; and a distinction between a memory that was never useful and one whose decisions have not resolved yet.

***

## People have scored judgment before

Each precedent has one piece of the product. Metaculus scores forecasters with strictly proper rules and refuses to score questions whose resolution is unclear; "ambiguous" and "annulled" close unscored ([Metaculus scoring](https://forum.effectivealtruism.org/posts/FodvZaiKftDCHPTub/metaculus-introduces-new-forecast-scores-new-leaderboard-and), [FAQ](https://www.metaculus.com/faq/)). The Good Judgment Project shows a person's track record is signal: about 70% of superforecasters kept the status year over year ([AI Impacts](https://aiimpacts.org/evidence-on-good-forecasting-practices-from-the-good-judgment-project/)). Fatebook is the nearest existing thing to a decision ledger: log a claim, attach a probability, set a date, get scored ([Fatebook](https://fatebook.io/)); the human reads the score and no retriever does. Decision journals have the fields and no engine ([Farnam Street](https://fs.blog/decision-journal/)), and Duke's "resulting" is the argument for storing the probability before the outcome and not scoring a good decision that lost as a bad one ([Annie Duke](https://www.annieduke.com/article-decision-making-by-thinking-in-bets-annie-duke/)). TipRanks and StarMine rank analysts on success rate and significance so one lucky call cannot lift a ranking ([TipRanks](https://www.tipranks.com/glossary/h/how-analysts-are-ranked), [StarMine](https://www.fidelity.com/trading/research-firms/lseg-starmine)). Trading journals have perfect resolution and no claim: TradeZella imports fills from over 500 brokers and none scores what the trader believed at entry ([TradeZella](https://www.tradezella.com/pricing)). Venture has no loop; the one rigorous study reconstructed a fund's ledger by hand and found its team score predicted small outcomes and nothing large ([Jang and Kaplan](https://www.aeaweb.org/conference/2026/program/paper/frfaaakt)). Sales is the one place a stated belief is scored for money: Clari and Gong score the rep's committed number against closed-won, and have moved toward replacing the rep's forecast with a pipeline model rather than calibrating the rep ([Gong](https://www.gong.io/platform/revenue-forecasting-software)).

One line each: forecasting has scored claims no retriever consumes, journals have fields and no engine, trading has the oracle and no claim, venture has the need and no loop, sales scores the number and not the reason.

***

## The decision ledger

The product is memory whose entries have a track record. Four parts and one property.

**Capture as commitments.** When an agent or its user makes a consequential call, the record is structured: the context loaded, the claim, a probability, what resolves it, and by when. A claim is a proposition that resolves true or false. Continuous outcomes are stated as thresholds, "markout at five seconds is non-negative," "up more than 2% by Friday's close," so that a proper score applies; scoring full distributions comes later and needs a different rule. Capture happens before the outcome, at the order ticket, the memo, the pipeline stage change. If the human declines to state a claim, the decision is logged as unclaimed and never scored; the agent does not back-fill it after the fill. Probability is recorded twice, the model's and the human's, because the gap between them is information.

**Oracles that resolve claims.** A connector to wherever the outcome lives: a test suite, an exchange or broker, a CRM, a funding database. The labs already ship connectors, so connector access is not the moat. What they have not shipped is a scoring loop on private outcomes, because their memory has to work for everyone and cannot depend on a broker login.

**Two scores, one stated rule.** The claim gets a proper score, Brier or log, on the stated probability against the resolution. Each retained memory separately accumulates how often it was retrieved and how the decisions it fed resolved. The rule that connects them is written down once: a memory's outcome record moves only on decisions whose claims were also poorly calibrated, so a calibrated thesis retrieved into a trade that lost on execution is a good claim and a bad outcome and does not decay the memory. Consolidation uses both as inputs. Following Metaculus, a memory whose decisions have not resolved shows as unscored.

**Synthesis graded on lift.** When the consolidator merges or rewrites, the test is a rerun: take the resolved decisions that loaded the originals, rerun them with the merged entry substituted, and check whether the claims come out the same and score as well. The merged memory was never actually retrieved into those decisions, so scoring it on their outcomes without the rerun would be invalid. Every memory carries a lineage identifier that survives merges and rewrites, so the resolved claims that cited the originals still bind to the successor. That turns the consolidation pass every harness runs on faith into something with a number.

**The property: the claim is a commitment.** Who writes the resolution condition, before the outcome, in a form the writer cannot revise? An agent that writes its own conditions and grades them is the same shape as a token-weighted resolution vote where the voters hold positions. The fix: at write time, serialize the claim, probability, condition and timestamp as canonical bytes, append a 256-bit random nonce, and hash the result into an append-only log; treat any later edit as a new claim that supersedes but does not erase; give resolution a dispute path with a human decider whose overrides are logged. Deletion for privacy law is a tombstone that removes the content and keeps the hash, so the record shows that a claim existed and was removed without showing what it said. A risk desk, a limited partner or a second model can replay the log.

The architecture splits into a spine and a shell. The spine is the claim schema, the two scoring rules, and credit assignment from a resolved decision back to the memories it loaded; it is shared engineering, not a shared dataset, and there is no evidence attribution weights learned on a trading book transfer to a pipeline. The shell is per vertical: the oracle connector, the write-ahead capture hook, and the taxonomy of claims worth logging. Defensibility is the private oracle plus the accumulated claim log.

***

## Scoring, and the order to build it in

Three definitions, once. Ablation means rerunning a decision with some loaded memories removed to see how the output changes. Off-policy evaluation means estimating how a different retrieval rule would have done using only logged decisions. Censoring means a claim whose outcome has not arrived, which is most of them at any moment.

Ablation works from day one and is not free. ContextCite reruns a decision on random subsets of its context and fits a sparse linear model, getting a faithful attribution in 32 extra passes over 98 sources ([arXiv 2409.00729](https://arxiv.org/abs/2409.00729)); on a ten-document benchmark Kernel SHAP with 100 calls correlates with exact Shapley values above 0.95, an approximation on that benchmark rather than a guarantee ([arXiv 2507.04480](https://arxiv.org/abs/2507.04480)). Frontier APIs mostly do not expose log-probabilities, so the shippable version scores each rerun with a judge, and every rerun breaks the prompt cache, so each costs close to a full input. At ten to a hundred scored decisions a day on a 20,000-token context that is a real line item, which is why it is gated on stake and not run on everything.

The per-memory outcome rate is what needs volume. Detecting a ten-point lift over a 50% baseline at conventional power needs about 194 resolved decisions in which that memory was retrieved ([NIST](https://www.itl.nist.gov/div898/handbook/prc/section2/prc242.htm)); the real two-sample estimator against the user's actual base rate, with a correction for testing thousands of memories at once, pushes it higher. Take a few hundred as the order of magnitude. At thirty decisions a day, 40% resolving, ten memories per decision over a library of two thousand, each memory sees about 22 resolved retrievals a year. Retrieval follows a power law: a small head crosses the bar in months, the tail never does.

They measure different things. Ablation says the decision's text would have differed without the memory. Only a resolved outcome says the decision would have gone better. Using ablation weights to split an outcome across memories assumes the outcome depends on the memory only through the text it changed; state the assumption or keep the numbers apart. Every gradient-based attribution method is out by construction, because the memories are not in the weights and the weights belong to the lab; the datamodels formulation, regressing outcome on which items were included, is the one that transfers ([arXiv 2202.00622](https://arxiv.org/abs/2202.00622)).

Three failure modes matter more than confounding. Censoring: a claim with a deadline simply resolves at the deadline, true, false or unresolvable. The delay model is for claims whose outcome is observed with a lag after the event, where an unresolved claim is evidence weighted by how long it has waited, the delayed-conversion model from advertising ([Chapelle](https://doi.org/10.1145/2623330.2623634)). Small samples: a beta-binomial posterior with the prior fitted across the library, so a memory with no data returns the prior ([Brown](https://arxiv.org/abs/0803.3697)); shrinkage is not a multiple-testing correction, so a false-discovery-rate rule gates any memory before it is labeled a proven performer, or thousands of memories will produce winners by chance. Co-retrieval: two memories that always load together have identical statistics at any sample size, and only an intervention separates them, which is ablation's unique job.

Logging cannot wait and live randomization must. Off-policy estimators need each logged retrieval to have had some probability of not happening; a deterministic top-k retriever makes them undefined ([Dudík, Langford, Li](https://arxiv.org/abs/1103.4601)). Randomizing retrieval on a live book is PnL, not a tax. So: from day one log the full candidate pool, each item's score and a shadow retrieval, understanding that the shadow is a record for later comparison and not an estimator, since a decision that was never randomized has no propensity to weight; randomize only on paper decisions, which estimates the retrieval policy's value on paper decisions and nothing more; enable it for real decisions only where the user has priced the cost.

| Stage | What it does | When |
|---|---|---|
| 0. Logging | Per decision: candidate pool, retrieved set, scores, shadow retrieval, claim, both probabilities, condition, date, salted hash | Day one. Nothing below is retrofittable without it |
| 1. Proper score per claim; shrunk, censoring-aware rate per memory | Brier or log score per resolved claim; beta-binomial outcome rate per lineage with a delay model; interval and unscored count always shown | Day one; individually informative at a few hundred resolved retrievals |
| 2. Stake-triggered ablation | 32 to 100 reruns scored by a judge, on high-stakes decisions and co-retrieval clusters | Day one, gated on stake |
| 2b. Live exploration | Randomized retrieval with logged probabilities on paper and low-stakes decisions | Past about a thousand resolved decisions a year |
| 3. Learned attribution | Regress resolved outcome on the retrieval-indicator vector plus context features, on decisions with logged propensities only; without randomization it is association, not attribution | Roughly ten thousand resolved decisions |

Most memories in any library will never accumulate enough resolved outcomes to be scored alone. A ledger that shows which are scored, which are waiting and which have been silent is not admitting a limitation. That display is the product.

***

## Three shapes a developer can build

The ledger is a schema, a log, a scorer and a display. They compose into three products of increasing scope, and each is a subset of the next.

**Shape 1: the ledger hook.** A command-line tool and a local append-only log that bolts onto an existing harness, so the harness keeps its own memory and gains a track record for it. No retrieval changes.

- Storage: `ledger.jsonl`, one claim per line. Fields: `id`, `ts`, `lineage_ids` (stable identifiers of the memories loaded when the claim was made), `claim`, `p_model`, `p_human`, `resolution_condition`, `resolve_by`, `event_start_ts` (the fill or event the horizon runs from, set when it happens), `oracle`, `oracle_params`, `nonce` (256-bit random), `hash` (SHA-256 over the preceding fields plus the previous line's hash), `status` (`open`, `resolved`, `unresolvable`, `superseded`, `tombstoned`), `outcome`, `score`.
- `oracle_params` is typed per oracle so `ledger poll` can actually resolve a row: `manual` takes nothing; `github` takes `repo`, `pr`, `window_days`, `resolves_on` (`merged`, `reverted`, `reopened`); `price` takes `venue`, `instrument`, `direction`, `threshold_pct`, `horizon_s`, and resolves against a mark or mid series the poller captures itself from `event_start_ts` plus horizon, never from claim creation time.
- Commands: `ledger claim` (refuses to write without a condition, a date and typed params), `ledger resolve <id> <outcome>` (manual; logs who and when), `ledger poll` (automatic oracles against open claims), `ledger score` (Brier per claim, shrunk outcome rate per lineage), `ledger show` (per memory: n resolved, n open, n silent, Brier, interval), `ledger tombstone <id>`.
- Capture: `ledger claim` is a tool the model calls during the turn, at the moment it submits an order, opens a pull request or writes a memo. The harness hook is a rule, not a model call: on turn end it checks the transcript for those actions and flags any that have no claim. In Claude Code that is a `Stop` hook running a small script; in Hermes Agent it is the same script on the tool. If a team wants the model to judge whether a turn contained a claim, that is one extra inference per turn and should be budgeted as such.
- Oracles for week one: manual, GitHub, and a price oracle on public market data for direction claims. Hyperliquid's public endpoints are a free development oracle for that; they are not the product's moat.
- What to measure in the first month: claims per day, fraction whose condition the oracle could actually check, fraction resolved by the date, and whether `p_model` beats `p_human` on Brier. That last number is the first thing anyone will ask.
- Not in scope: embeddings, retrieval changes, ablation, hosting.

**Shape 2: the scored memory server.** A new MCP server whose entries carry track records and whose retrieval logs what the scorer will later need. It is not a drop-in for the knowledge-graph server; it sits beside it and works with any client that speaks MCP.

- Tools: `remember` and `search` over entries keyed by `lineage_id`, plus `claim`, `resolve`, `record_decision` (called at decision time with the candidate pool, the retrieved set and scores, a stake field, and a shadow retrieval from an alternative policy), and `track_record(lineage_id)`.
- Retrieval returns each entry with its scored metadata (n resolved, Brier, interval, last resolved date) so the model can weigh a memory by record rather than recency.
- Scorer as a separate process reading the log: stage 1 on a schedule; stage 2 ablation when `stake` crosses a threshold, rerunning the decision through the same model with subsets of the retrieved set removed, scoring each rerun with a judge, fitting the sparse linear model.
- Consolidation graded on lift: a proposed merge creates a new entry whose lineage points at its parents; the resolved decisions bound to those parents are rerun with the merged entry substituted, and the merge is rejected if the reruns diverge or score worse.
- Hosting split: the log and index are local files; the scorer runs locally or as a hosted service reading a synced copy, because ablation costs model calls the user's machine may not want to make.
- Not in scope: vertical connectors beyond GitHub and public prices, live randomized retrieval.

**Shape 3: the desk product.** The trading shell on shape 2, for a desk or fund running agents on its own book.

- Insertion point: the write-ahead capture lives in the order path, as a pre-trade check alongside risk limits in the OMS, not in the ledger CLI. An order without a claim is rejected the way an order over a limit is.
- Three claim types with three resolutions. Execution claims resolve on markout, the mid or mark at one, five and sixty seconds after the fill, which the connector must capture itself from the venue feed; a fill's realized-PnL field does not give it. Direction claims resolve on a mark at the stated horizon. Thesis claims resolve on a data series.
- Oracles: keyed connectors to centralized venues and brokers, handled per venue and per module. Binance's spot trade endpoint returns commission and quantity and no PnL, so position accounting is rebuilt from fills and fees ([Binance spot API](https://developers.binance.com/docs/binance-spot-api-docs/rest-api/account-endpoints)); its futures user-trades endpoint carries a realized-PnL field per fill, with funding still charged outside the fill stream. Hyperliquid's public `userFills` by wallet, with `closedPnl` on position-reducing fills ([Hyperliquid API](https://hyperliquid.gitbook.io/hyperliquid-docs/for-developers/api/info-endpoint)), serves realized-PnL claims and development.
- Display: per trader and per agent, a calibration curve on stated probabilities, the memory index with scored, waiting and silent counts, and the model-versus-human Brier gap. Team view weights each contributor's input by record.
- Paper lane: every strategy runs a shadow book where randomized retrieval is on. That is where off-policy estimates for the retrieval policy come from, on paper decisions, without touching live PnL.
- Buyer: a desk or fund already running agents on its own book. Retail journals cost tens of dollars a month and professional desks already run position systems, so willingness to pay is the leg to prove first, with one design partner rather than a launch.

Build order: shape 1 in a week, on a developer's own Claude Code or Hermes setup, because the first question is whether anyone writes claims at all. Shape 2 once claims exist and the retrieval log has a month of data. Shape 3 only with a design partner who has said what they would pay.

***

## Where the ledger lives

The log lives outside every harness, because the harness is the thing whose memory is being audited. Local files are the right first answer: portable, readable, deletable. They fail one test. A self-kept log is not a credible commitment to anyone but its owner. A limited partner or a counterparty has no way to know the file was not rewritten last night, and "the vendor promises it wasn't" is the trust the ledger exists to remove.

A public chain fixes exactly one thing: it gives a hash a timestamp nobody can forge. The rule that follows is that every claim is anchored before its earliest possible resolution, which is a property of the claim, not a schedule. For claims that cannot resolve for days, a Merkle root over the period's hashes, posted to Ethereum or a rollup that settles there, is enough, and the on-chain payload carries the leaf count, the ordering rule and the period alongside the root, so a partial reveal later is detectable from the chain alone. For a trading claim that can resolve in minutes, a periodic root proves nothing, and the commitment is per claim, to a rollup with second-scale blocks or a timestamping service whose own anchor is on-chain, before the order goes. The chain never sees a claim, a probability or a position; it sees a hash, and the 256-bit nonce is what keeps "ETH long, 60%, one hour" from being brute-forced out of it.

The timestamp alone does not make a track record portable. Anyone checking a calibration score still needs the claims themselves, and the owner could reveal the winners and forget the rest. Two more things are needed: a reveal policy fixed in advance, all claims in a period or none, checked against the leaf count on-chain; and an archive of the preimages held by a third party or by the counterparty, so the record survives the owner's incentive to lose it. With those, and only with those, a calibration score becomes something that follows a trader, an analyst or an agent across employers and harnesses, verifiable by anyone with the root and the archive. Resolution stays off-chain with the chosen oracle and a logged dispute path, because token-weighted resolution is the failure the prediction markets keep demonstrating. Retrieval logs stay local; they are a firm's activity and the scorer's training data, not commitments.

Two things follow that the local-only design cannot claim. The flywheel this market lacked appears at the track-record layer rather than the fact layer: one person's memories still improve nothing for another, but a verified record is worth something to whoever hires them next. And the lock-in argument inverts. A claim ledger outside the harness is the portability layer the labs have not built and have no incentive to.

***

## Which vertical first

The spine is generic. The shell is chosen by how fast the oracle resolves, how clean the resolution is, and whether the data is private, not by who pays most.

| Vertical | Cleanliness | Clock | Oracle cost | Willingness to pay | No incumbent | Total |
|---|---|---|---|---|---|---|
| Trading, keyed venues | 4 | 5 | 4 | 3 (unverified) | 4 | 20 |
| B2B sales | 3 | 4 | 4 | 5 | 2 | 18 |
| Software engineering | 5 | 5 | 5 | 2 | 1 | 18 |
| Venture capital | 1 | 1 | 1 | 5 | 5 | 13 |

Trading first, on keyed data. Public perpetuals venues have a free oracle that anyone, including a lab, can read, so there is no privacy moat there and the labs will take it if they want it. The wedge is centralized exchanges and brokers, where the data is keyed, private and harder to reconstruct. Volume has to be counted honestly: a trader generates hundreds of fills a month but tens of pre-committed theses, and it is theses the scoring math counts. Trading is the fastest clock by a wide margin, not the volume machine it first appears to be.

Sales second, on the same spine. A closed-won transition is the same object as a fill: timestamped, attributable, with an amount. A fifty-rep organization resolves thousands of claims a quarter, and the price is proven: third-party buyer data puts Clari's core tier at roughly $1,200 to $1,500 per user per year and Gong at $1,200 to $1,920 ([Clari](https://marketbetter.ai/blog/clari-pricing-breakdown-2026/), [Gong](https://marketbetter.ai/blog/gong-pricing-breakdown-2026/)). The wedge is that incumbents score the forecast number and none attaches a calibration curve to the reason a rep gave.

Software engineering has the cleanest oracle and the harnesses already own it. Claude Code's best-practices page describes a hook that blocks a turn from ending until a pass-or-fail check passes ([Claude Code](https://code.claude.com/docs/en/best-practices)); Agent-RLVR used unit tests as the training reward ([arXiv 2506.11425](https://arxiv.org/abs/2506.11425)). The labs will add outcome scoring here themselves. The seam that remains, "this design decision will hold up," resolves on reverts and reopened incidents months later and is the most ambiguous oracle in the table. It belongs later.

Venture last despite the highest willingness to pay, because the clock decides it. Among seed companies that do raise a Series A, the median wait is 2.2 years, and only about 15% of the 2022 seed cohort reached an A within two years ([Carta via SaaStr](https://www.saastr.com/carta-the-average-time-from-seed-to-series-a-has-hit-2-2-years-and-longer-from-series-a-to-series-b)); the ones that never raise generate no event at all, which is also the strongest signal for a passer, recorded worst. The oracle is bad: Crunchbase has retired its Basic API ([Crunchbase](https://data.crunchbase.com/docs/crunchbase-basic-getting-started)), Harmonic and Dealroom publish no prices, and unpriced SAFEs make "marked up" unobservable. Venture is the third vertical, entered with a first screen that is honest about how many claims are still open.

***

## What it is not

Oracle coverage will be partial and resolution is where ledgers fail. In April 2026 a US senator wrote to the CFTC about a Polymarket contract on whether a jacket and pants counted as a suit, with nearly a quarter of a billion dollars of volume, resolved through a token-weighted vote that participants with positions could influence ([Senate letter](https://www.blumenthal.senate.gov/imo/media/doc/20260421_-_cftc_-_market_resolutions_-_finalpdf.pdf)). Conditions are written before the outcome and unresolvable claims are quarantined, not scored.

The delayed-outcome regime is outside the published evidence base. No verified paper shows an agent's memory learning from an outcome that resolves in weeks. The mitigations come from advertising and survival analysis, where they are well tested, and the ledger is operating where the literature has not been.

The labs will take the public-oracle verticals. Anywhere the outcome is visible to the harness, they can add outcome scoring to native memory and will. The defensible ground is where the oracle is private: a keyed account, a book, a pipeline, a pass list.

Where this is most likely wrong is the buyer, not the mechanism. If no desk will pay for a ledger on its own book and sales incumbents extend forecast scoring to the reason on their own, this is a feature the harnesses absorb and not a company, and that is better learned from shape 1 on a developer's machine than from a launch. The claim the evidence supports is smaller than "memory solved." It is that memory becomes auditable: entries that carry a probability, a resolution and a record, in a log that cannot be quietly rewritten, is the first version of agent memory a firm can inspect, argue with, and let an agent act on.

*Primary sources: arXiv 2603.04814 (memory vs long context), arXiv 2603.07670 (agent memory survey, MemoryArena), Mem0 benchmark and state-of-memory posts, Claude Code auto-dream mechanics, Anthropic Help Center on memory import and export, Letta sleep-time agents docs, Nous Research Hermes Agent README, Anthropic third-party tool policy (Feb–Apr 2026), arXiv 2608.02508 / 2605.08374 / 2605.10870 (outcome-conditioned memory), arXiv 2409.00729 (ContextCite), arXiv 2507.04480 (RAG Shapley), Chapelle KDD 2014 (delayed feedback), Dudík–Langford–Li 2011 (off-policy evaluation), Metaculus scoring FAQ, Good Judgment Project persistence data, Jang and Kaplan 2025, Hyperliquid and Binance API docs, Carta seed-to-A data, Senate letter to the CFTC (April 2026). Numbers, dates and quotations were re-fetched from their sources before publication; figures that could not be verified from a primary source were excluded.*
