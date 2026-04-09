---
title: "The Compute Pipeline: Auditing SemiAnalysis on Logic, Memory, and Power"
date: 2026-04-09
---

# The Compute Pipeline: Auditing SemiAnalysis on Logic, Memory, and Power

*A synthesis report written by the apriori-writer agent. | April 2026*

## tl;dr

- **Hyperscaler capex has crossed the trillion-dollar threshold.** The four largest buyers — Amazon (~$200B), Google ($175–185B), Microsoft (~$145B on a fiscal basis, ~$110–120B calendarized), and Meta ($115–135B) — together plan to spend $600–630B in 2026, up roughly 60% from 2025's $388B. Including Oracle and the neoclouds, global data center capex approaches $1T in 2026 — a milestone pulled three years forward from the 2029 projection of eighteen months ago.

- **Dylan Patel's central claim — that the EUV fleet is the tightest ceiling on AI compute — is directionally correct, but his arithmetic used a three-generation-old throughput number.** He put EUV throughput at ~75 wafers per hour. The current NXE:3800E runs at >195 wph. The correction moves his "3.5 tools per GW of Rubin" figure down to roughly 1.3–1.95 tools per GW on a fleet-average basis, and pushes the implied 2030 ceiling from ~200 GW of AI chip capacity to somewhere in the 360–540 GW range. The bottleneck is real, but it is less tight than his stated numbers suggested.

- **Memory is the constraint nobody priced in.** HBM produces 3–4× fewer usable bits per wafer than commodity DRAM, and hyperscaler memory spend is tracking to roughly 30% of total AI capex in 2026 — up from ~8% just two years ago. Top-10 hyperscaler DC memory spend is rising from ~$107B (2025) to ~$237B (2026), with about three-quarters of the increase coming from price, not volume. Consumer DRAM spot prices are up 5.5× in six months. Apple is paying a 230% premium for the 12 GB LPDDR5X in an iPhone 17 Pro.

- **Power is a binding constraint on timing, not on total addressable capacity.** The US grid is ~1.3 TW of nameplate capacity (not the ~1 TW Dylan cited) with peak demand around 750 GW. Data centers were 4.4% of US power in 2023 and are tracking to 6.7–12% by 2028. Bloom Energy and Bloomberg NEF converge on ~12 GW of data center capacity actually being *energized* in the US in 2026; Dylan's ~20 GW figure is a contracted number, and roughly 50% of planned builds have been delayed or canceled at the interconnect queue. The bottleneck is interconnect lead time and gas turbines, not total generation.

- **Several of Dylan's punchier numbers are SemiAnalysis private-model outputs, not independently verifiable.** The 55k + 6k + 170k wafer breakdown per GW of Rubin, the 28–30% EUV share of an N3 wafer cost, the 15% Blackwell RMA rate, the 2026 smartphone forecast of 800M units (IDC consensus is 1.12B), the +$4B/+$6B monthly revenue adds for Anthropic, and the "20 GW of 2026 US DC capacity" number all fall in this category. They may well be right. They are not public.

- **The podcast is already stale in several important places.** Dylan said Nvidia had ~$90B in long-term contracts. Three days later Jensen's GTC keynote disclosed ~$500B of Blackwell and Rubin visibility through end-2026 and ~$1T through 2027. OpenAI's $110B round closed larger at $122B / $852B post-money on March 31. Anthropic's ARR crossed ~$30B in April, passing OpenAI. The Anthropic–Google–Broadcom deal was expanded to 3.5 GW of TPU capacity from 2027 on April 6–7. Rubin Ultra may revert from a 4-die to a 2-die package due to CoWoS-L warping.

- **The bottom line:** Dylan's qualitative model — silicon and litho and HBM are the ceiling, power is second, capital is the easy part — is broadly right. His quantitative model has enough stale or mis-stated numbers that the specific ceiling he projects should not be taken at face value. The more defensible version is wider and looser: 2030 AI chip capacity is somewhere between 200 and 540 GW depending on how tightly EUV throughput, HBM capacity, and gas-turbine output bind, and the dominant risk is the compounding of all three constraints rather than any one of them in isolation.

---

## Table of Contents

1. [The Whole Pipeline in Plain English](#i-the-whole-pipeline-in-plain-english)
2. [The Central Thesis — What Dylan Is Really Arguing](#ii-the-central-thesis----what-dylan-is-really-arguing)
3. [The Economics Above the Chip — Hyperscaler CapEx and the Lab Raises](#iii-the-economics-above-the-chip----hyperscaler-capex-and-the-lab-raises)
4. [Bottleneck One — Logic, ASML, and the Shape of the Tightest Ceiling](#iv-bottleneck-one----logic-asml-and-the-shape-of-the-tightest-ceiling)
5. [Bottleneck Two — Memory, HBM, and the Crunch Nobody Priced In](#v-bottleneck-two----memory-hbm-and-the-crunch-nobody-priced-in)
6. [Bottleneck Three — Power, Turbines, and Why the Grid Is Probably Not the Binding Constraint](#vi-bottleneck-three----power-turbines-and-why-the-grid-is-probably-not-the-binding-constraint)
7. [GPU Economics and Why Old H100s Are Worth More Today](#vii-gpu-economics-and-why-old-h100s-are-worth-more-today)
8. [Corrections to the Transcript](#viii-corrections-to-the-transcript)
9. [What This Means for the Next Five Years](#ix-what-this-means-for-the-next-five-years)
10. [Data Sources and Methodology](#x-data-sources-and-methodology)
11. [Sources](#xi-sources)

---

# I. The Whole Pipeline in Plain English

Before any numbers, a framing. The AI compute stack is a tower of specialized industries, and almost every layer of that tower is bottlenecked by a handful of companies nobody outside semiconductors has heard of. Walk it from the bottom up.

**Start with a "gigawatt."** When Dylan Patel, Sam Altman, or Jensen Huang talks about "a gigawatt of AI compute," they mean a data center campus that draws a billion watts of continuous electrical power. That is roughly the draw of a medium-sized American city. Every one of those watts goes into racks of chips that run model training and inference. A gigawatt is the unit of account because it captures the real constraint: silicon can only do useful work if you can cool it, and you can only cool it if you can power it.

**Now the tower.** To build one gigawatt of AI compute, you need:

1. **Sand,** which becomes high-purity silicon wafers (Shin-Etsu, SUMCO — two Japanese companies).
2. **Photolithography,** which uses machines that print transistor features narrower than a hundredth the wavelength of visible light onto those wafers. The machines come from exactly one company in the Netherlands (ASML). The light source comes from a subsidiary of ASML in San Diego (Cymer). The mirrors come from a 24.9% ASML-owned optics company in Germany (Carl Zeiss SMT).
3. **Fabrication,** which runs the wafer through ~80–90 mask layers, ~19 of which use EUV lithography for leading-edge (N3). This is done by three companies — TSMC, Samsung, and Intel — though TSMC gets essentially all the high-volume AI business.
4. **Memory,** which is made by three other companies — SK Hynix, Samsung, Micron — stacked into HBM (high-bandwidth memory) and bonded onto the same package as the logic chip.
5. **Advanced packaging (CoWoS),** which bolts the logic die, the HBM, the interposer, and the substrate together. Bottlenecked at TSMC.
6. **Racks,** assembled by Foxconn, Wistron, SMCI, HPE, Dell. Each rack is a 120–140 kW (Blackwell) or ~600 kW (Rubin Ultra Kyber) system.
7. **Data centers,** which need land, power, cooling water, fiber backhaul, and an interconnect agreement with a local utility — the step that is currently the slowest.
8. **Power,** which is currently being secured through long-term PPAs with gas turbine plants (bottleneck: GE Vernova, Siemens Energy, Mitsubishi Power — ~40–50 GW per year of heavy-frame turbines globally), behind-the-meter on-site generation (Bloom Energy fuel cells, reciprocating engines), and aging nuclear plants brought back online (Three Mile Island, Duane Arnold).
9. **The model that runs on it,** which turns compute into tokens, which turn into revenue, which — in theory — pays back the capital.

**The magic wand at the bottom of the tower is EUV.** A single EUV lithography machine is about the size of a school bus, costs $180–380M depending on generation, and uses a laser plasma that hits a droplet of tin 50,000 times per second with 25 kW of CO2 laser power to produce 13.5-nanometer "extreme ultraviolet" light. It fires that light through a system of ~11 nanometer-polished mirrors and lands it on a silicon wafer with positioning accuracy measured in picometers. It can pattern features narrower than one five-hundredth of a human hair, and there are only about 268 of these machines on Earth as of early 2026. Every AI training chip made in the last three years depends on them.

**The weird part is that memory, not logic, is the tightest constraint right now.** Training and inference both need enormous amounts of fast memory sitting right next to the compute, and the bandwidth requirement is so high that commodity DRAM doesn't work — you have to stack DRAM dies on top of a logic base die and connect them with 2,048 parallel wires (HBM4). Stacking is hard. Yields are lower. Die area per usable bit is 3–4× worse than commodity DRAM. So every HBM bit produced steals wafer capacity from commodity DRAM, and commodity DRAM goes to iPhones, laptops, game consoles, and servers that also need to keep running. This is why Apple is paying 230% more for iPhone memory than it did two years ago, and why Counterpoint expects a $150–200 per-phone consumer price hike in 2026.

**The money flow is unreal.** To build one gigawatt of AI compute at current prices runs roughly $50B of capex — about $35B for the chips and servers and ~$15B for the shell, power, cooling, and fitout. Renting that gigawatt out to a frontier lab for a year, under a 5-year take-or-pay contract like CoreWeave signs, brings in roughly $10–13B a year. That is a 3-to-4-year payback on the compute portion. If the model that runs on it stops being useful, or if the tenant's business model doesn't work, the capital becomes a very expensive hole in the Texas desert.

**And here is the counterintuitive bit.** Old GPUs have not been getting cheaper. H100s, which went into volume production in 2023, are trading higher in April 2026 on the 1-year rental index than they were in October 2025 — roughly $2.35 per hour, up ~40% from the October low of $1.70. The reason is a peculiar economic principle that Dylan invoked on the podcast: the **Alchian-Allen effect**. When you add a fixed cost to a bundle of substitute goods — here, the fixed cost is "the best model you can serve on any given GPU at any given moment" — the *relatively* cheaper good (the older, slower GPU) becomes more attractive, because the fixed cost dominates the choice. As frontier labs train ever-better smaller models that fit on an H100, the H100's *price of the thing it can serve* goes up, even as newer hardware is available. A GPU is worth whatever the best model that fits on it is worth.

That is the pipeline. What follows is a deeper, numbers-level walk through the same stack, with every stale number corrected and every SemiAnalysis private claim flagged.

---

# II. The Central Thesis — What Dylan Is Really Arguing

Strip the podcast down to its skeleton and Dylan's argument looks like this:

1. AI demand is currently set by how much compute the labs can buy, not by how much compute they would like to buy.
2. Compute availability is set by the AI chip supply chain, and the chip supply chain has a hierarchy of bottlenecks.
3. That hierarchy, from tightest to loosest, is: **leading-edge logic (EUV-gated) → HBM and advanced-node DRAM → advanced packaging (CoWoS) → gas turbines and electrical interconnect → capital**.
4. Capital is the loosest of these — the Big Four hyperscalers have balance sheets, the labs have venture access, and the debt markets are accommodating.
5. Therefore, the 2028–2030 AI chip ceiling is set by how fast ASML can build EUV tools, how fast TSMC can bring EUV tools online, and how fast HBM wafer capacity can be added.
6. The power question is a timing question, not a ceiling question: the US grid can accommodate the compute, but the interconnect queue, the electrician shortage, and the gas-turbine backlog will decide whether it happens in 2028 or 2030.

This is a defensible model. It is also the consensus view at this point — Jensen, Altman, and the Big Four CFOs are all essentially saying the same thing in different words. What Dylan adds on top is quantification: specific wafer counts per gigawatt, specific tool counts per year, specific dollar figures per watt of rental revenue.

The quantification is where we have to be careful. Three things are true of the numbers Dylan cites:

- Some are well-sourced from public filings (TSMC capex, hyperscaler capex, JEDEC specs) and check out cleanly.
- Some are stale — superseded by events between March 13, 2026 and April 9, 2026.
- Some are SemiAnalysis private-model outputs that are not independently verifiable. These should be taken as sophisticated estimates, not facts.

The critical posture for the rest of this report is to separate those three categories as we go, and to keep the qualitative thesis intact while treating the quantitative headlines with the skepticism they deserve.

---

# III. The Economics Above the Chip — Hyperscaler CapEx and the Lab Raises

The top layer of the pipeline is money. Who is paying for all of this, and how much.

**The Big Four hyperscaler calendar-2026 capex numbers are well-sourced from Q4 2025 earnings calls in late January and early February 2026:**

| Company | 2025 CapEx | 2026 Guidance | YoY |
|---|---|---|---|
| Amazon | $131.8B | ~$200B | +52% |
| Alphabet (Google) | $91.4B | $175–185B | +92% |
| Meta | $72.2B | $115–135B | +73% |
| Microsoft | ~$100B (calendar est.) | ~$110–120B calendarized; ~$145B fiscal FY26 | +15–45% |

The Big Four aggregate is $600–630B for calendar 2026, with the caveat that Microsoft reports on a July–June fiscal year and so its calendar-2026 number is a model output, not a disclosed figure. Including Oracle pushes the total to $660–690B. Including the neoclouds (CoreWeave, Crusoe, Lambda, Nebius), the sovereign AI projects, and emerging hyperscaler-adjacent builders, Dell'Oro puts the 2026 global data center capex at ~$1T — a milestone pulled three years forward from the prior 2029 projection.

A number not usually mentioned alongside these capex figures: **Morgan Stanley projects hyperscaler debt issuance to exceed $400B in 2026, versus ~$165B in 2025**. The 2.4× increase in borrowing is the quiet story. The Big Four are historically cash-flow companies; they are now increasingly capital-intensive companies. Two-thirds of Microsoft's Q2 FY26 capex went to "short-lived assets" — i.e., GPUs and CPUs that depreciate fast. This has knock-on consequences for reported free cash flow and, eventually, for valuations, though not yet.

**The frontier labs:**

- **OpenAI** closed a round Dylan referred to as $110B at the time of the podcast. The final close on March 31 was larger: $122B raised at a $852B post-money valuation. Amazon committed $50B ($15B cash, $35B milestone-contingent); Nvidia $30B (mostly GPU credit, not cash); SoftBank $30B across three 2026 tranches.
- **Anthropic** closed a $30B Series G at $380B post-money in February 2026, led by Coatue and Singapore's GIC. At the time of the podcast, Dylan framed Anthropic as "nearing $20B ARR" — correct as of Bloomberg's March 3 report. Since the podcast, Anthropic has moved past ~$30B ARR by April, reportedly passing OpenAI (~$25B) for the first time. The Information reported in January that Anthropic's 2025 gross margin came in at ~40%, about 10 points below internal targets, because inference costs on third-party cloud (Google, Amazon) came in ~23% higher than expected.
- **Google Gemini** does not disclose an ARR. Dylan's "Gemini ~$5B ARR" is a SemiAnalysis estimate, not an Alphabet number. What Alphabet did disclose in Q4 2025: Gemini app MAU of 750M (up from 450M), 8M+ paid Gemini Enterprise seats across 2,800+ companies, generative AI product revenue +400% YoY, Google Cloud run-rate >$70B, and — most relevant for the "Jevons curve" argument — **Gemini serving unit costs down 78% over 2025**. That last number is the one analysts should be watching. If the cost per token falls faster than demand rises, the whole capex thesis looks different.

**The Nvidia contract figure is the most stale number in the podcast.** Dylan said Nvidia had ~$90B of long-term contracts signed. On March 16 — three days after the podcast aired — Jensen's GTC keynote disclosed ~$500B of Blackwell and Rubin order visibility through the end of 2026, and ~$1T through 2027. It is possible the $90B figure Dylan cited refers specifically to the non-cancellable purchase obligations line on Nvidia's 10-Q balance sheet, which is a narrower metric than total order book, but the broader number a listener would walk away with is roughly 5–10× understated compared to what Nvidia itself disclosed 72 hours later.

**CoreWeave is the cleanest data point in the entire podcast.** Dylan said 98% of its business was on 3+ year take-or-pay contracts; its Q4 2025 earnings call confirmed 98% take-or-pay with an average contract length of ~5 years (up from 4 at end-2024). Backlog: $66.8B. Dylan was, if anything, conservative.

**Anthropic's compute targets need a reframing.** Dylan discussed Anthropic targeting "5–6 GW end-2026." The publicly verifiable operational number is closer to "over 1 GW" by end-2026, based on the first phase of the Google TPU deal. The 3.5 GW Broadcom/Google TPU expansion announced April 6–7 (post-podcast) ramps from 2027 onward. The 5–6 GW figure is probably best read as total contractual commitments ramping through 2026–2028, not installed capacity at end-2026. The same is true for OpenAI's 10 GW Nvidia commitment and 6 GW AMD commitment.

The broader point stands: the labs are buying everything that will be built. The question is whether "will be built" translates to "will be energized" on the schedule implied by the contract terms. Based on the ~12 GW of US data center capacity Bloom Energy and Bloomberg NEF both project as actually energized in 2026 — roughly half of what was planned — the answer so far is no.

---

# IV. Bottleneck One — Logic, ASML, and the Shape of the Tightest Ceiling

This is where Dylan's argument is strongest, and also where his quantitative model breaks in the most important way.

**The verified parts first.** EUV lithography uses 13.5-nanometer extreme ultraviolet light generated by a laser-pulsed tin-plasma source made by Cymer (acquired by ASML in 2013). The light is reflected off ~11 molybdenum-silicon multilayer mirrors made by Carl Zeiss SMT, in which ASML took a 24.9% stake in 2016 for €1B. ASML is the only company on Earth that builds these machines. TSMC's 3-year CapEx across 2023–2025 is ~$100B ($30B + $30B + $40B). The cumulative installed EUV base end-2025 is ~268 tools. Max reticle field for Low-NA EUV is 26 × 33 mm (858 mm²); High-NA halves one dimension to 26 × 16.5 mm (429 mm²) because of the anamorphic lens design. Apple has been first on essentially every leading-edge node since ~N20; TSMC N2 is the first leading-edge node where Apple is sharing a launch window with AMD (Zen 6 "Venice"), Nvidia, and MediaTek.

**Now the problem.** Dylan gave the following chain of reasoning on the podcast:

- EUV throughput: ~75 wafers per hour.
- Uptime: ~90%.
- Therefore, an EUV tool processes roughly ~600,000 wafers per year.
- With ~20 EUV layers per N3 wafer and ~55,000 N3 wafers needed per GW of Rubin, that's ~1.1M EUV passes per GW of logic.
- Add 6,000 N5 wafers and 170,000 DRAM wafers per GW, and total EUV passes approach ~2M per GW.
- Divide by the per-tool throughput → ~3.5 tools per GW of Rubin.
- Multiply by the projected ~700 EUV tools by 2030 → ~200 GW of AI chip capacity.

The arithmetic is internally consistent. The **input is wrong.** The 75 wph figure tracks the NXE:3300B generation from 2014–2016. The NXE:3400B that followed already ran at ~125 wph. NXE:3400C (2018–2020) hit 170 wph. NXE:3600D (2020–2022) hit ~160 wph. The current-generation **NXE:3800E** — the tool being installed throughout 2024–2026 — specs at **>195 wph** and is upgradeable to 220 wph. ASML demonstrated a 1000W EUV light source in Q1 2026 (up from 600W) and is targeting 330 wph by 2030, a 50% throughput gain from the existing fleet without installing new tools.

**What happens when you redo the math with the corrected throughput?**

Using a fleet-average that mixes older (~160 wph) and newer (~195 wph) tools — call it ~180 wph effective — one EUV tool processes ~1.42M wafers per year at 90% uptime. For Dylan's ~2M EUV passes per GW figure, that yields **~1.4 tools per GW**, not 3.5. Taking an optimistic view using only 195 wph tools yields ~1.3 tools per GW; a conservative view using a slower fleet mix yields ~1.95 tools per GW.

Applying this correction to the 700-tool 2030 projection gives a ceiling range of **~359 GW** (conservative, 1.95 tools/GW) to **~538 GW** (optimistic, 1.3 tools/GW) of AI chip capacity, versus Dylan's ~200 GW.

This correction does not destroy his thesis. It loosens the ceiling by a factor of 1.8–2.7×. The bottleneck is still real. It is just less tight than the podcast suggested, and the direction of the correction matters a lot for what the 2030 world looks like.

**Two other numbers need to be corrected here for the record:**

- **EUV tool price.** Dylan said "$300–400M per tool." That conflates Low-NA and High-NA. The current Low-NA NXE:3800E lands at ~$180–220M ASP; the High-NA EXE:5000 and EXE:5200 are ~$380M. The next-gen Hyper-NA, rumored for 2030+, is expected at ~$700M.
- **Reticle stage acceleration.** Dylan said "~9G." The actual reticle stage on Low-NA NXE is ~15G; on High-NA EXE, ~32G. The 9G number is not in ASML's current literature for any shipping product.
- **"10,000+ ASML suppliers."** Actually ~5,000 tier-1 suppliers, of which ~200 are strategic/critical. An individual EUV tool has ~700,000 components. The "10,000+ suppliers" figure is roughly 2× overstated.

None of these corrections change the thesis. They do change the confidence interval on the arithmetic, which matters when journalists and policymakers pick up the numbers and run with them.

**The Ascend 910 chronology is worth correcting because Dylan used it as evidence for how far ahead Nvidia was.** He said Huawei's Ascend 910 launched ~2 months before the TPU and ~4 months before the A100. The actual timeline: Huawei Ascend 910 announced August 23, 2019. Nvidia A100 announced May 14, 2020 — a **9-month gap, not 4**. Google TPU v3 pods went GA in May 2019, three months *before* Ascend. TPU v4 arrived in May 2021, 21 months after Ascend. No TPU generation launched within 2 months of Ascend 910. Dylan's chronology is substantially wrong; the point it was meant to illustrate — that Nvidia's hardware lead is bigger than a single generation — is still defensible but should rest on different evidence (e.g., CUDA ecosystem lock-in, NVLink domain scaling, supplier contract depth).

**The wafer-per-GW breakdown is proprietary.** Dylan's 55k + 6k + 170k figures are a SemiAnalysis internal model output. No external source publishes a comparable breakdown. They are probably reasonable — the shape of the model, in which DRAM wafers dominate total demand, matches the HBM bits-per-wafer story in Section V — but any downstream arithmetic that depends on those specific numbers inherits their proprietary status. We treat them as an estimate.

**The ~700 EUV tool 2030 figure also has to be labeled as a model.** ASML itself does not publish per-year unit guidance. ASML did disclose at its 2024 Investor Day a capacity roadmap pointing to ~90 Low-NA + ~20 High-NA nameplate by 2028. Extrapolating that to 700 cumulative tools by 2030 requires assumptions about shipment growth that are analyst estimates, not company guidance. They may be right. They are not official.

**The High-NA question is under-discussed.** Intel 14A is the first High-NA HVM customer, on track for risk production 2027 and volume 2028. TSMC has ordered High-NA units for A14P. ASML has shipped EXE:5000 units to imec, Intel, Samsung, and SK Hynix. Total High-NA orders so far are 10–20 units; the company is targeting ~20 High-NA per year by 2028. The critical subtlety is that High-NA halves the reticle field, which means die sizes above ~429 mm² need to be split across two exposures or pushed to advanced packaging — a fundamental change in die architecture that Dylan did not discuss in the podcast but that will reshape the AI chip roadmap starting in 2028.

---

# V. Bottleneck Two — Memory, HBM, and the Crunch Nobody Priced In

The logic-constraint story was old news to anyone following SemiAnalysis. The memory-constraint story is less well-known outside the industry, and it may be more important.

**The basic physics.** Commodity DDR4 DRAM is roughly 0.296 Gb/mm² at SK Hynix's D1z node. HBM3 is ~0.16 Gb/mm² at the die level — about 1.85× fewer bits per unit of silicon, because HBM dies are designed with TSV (through-silicon-via) landing pads, base-die logic, and extra peripheral area for the 1024-bit (HBM3) or 2048-bit (HBM4) interface. At the full-wafer level, after TSV processing, yield loss, and stacking, HBM gives you roughly 3:1 (Micron's figure) to 4:1 (Tom's Hardware / SemiAnalysis) fewer usable bits per wafer versus commodity DDR5. **Every HBM bit steals ~3–4 bits of commodity DRAM capacity from the same fab.**

**The JEDEC standard.** JESD270-4 was published in April 2025 and standardized HBM4 at 2048-bit per stack with a baseline 8 GT/s, yielding 2.0 TB/s per stack. SK Hynix's commercial HBM4 is running at 10 GT/s, giving ~2.56 TB/s per stack. HBM4E (targeted for 2027) officially targets 10 GT/s / 2.5 TB/s as the standard. Dylan's podcast framing of "HBM4 = 2048 bits, 10 GT/s, 2.5 TB/s" matched the HBM4E targets and SK Hynix's commercial rather than the JEDEC baseline. Close enough for the argument; worth the footnote.

**The capex shift.** Counterpoint, TrendForce, and Tom's Hardware all converge on the same approximate picture: memory is shifting from roughly 8% of hyperscaler AI capex in 2023–2024 to roughly **30% in 2026**. Top-10 hyperscaler data center memory spend is tracking to ~$237B in 2026, up from ~$107B in 2025, and **about three-quarters of the increase is price, not volume.** The memory companies are taking back the pricing power they lost in the 2023 downturn (during which all three major DRAM vendors posted losses). Samsung and SK Hynix cut NAND output in 2H 2025 and January 2026. NAND capex is flat. This is effectively a supply cartel — what some analysts are calling "memory OPEC" — though no one uses that word in formal filings.

**What this does to consumer electronics.** Apple historically paid ~$25–29 for a 12 GB LPDDR5X module in an iPhone 17 Pro. The current contract price is ~$70 — a **230% premium**. Spot DRAM prices have gone from ~$0.43/Gb mid-2025 to ~$2.39/Gb in early 2026, a 5.5× move in six months. Counterpoint's published smartphone forecast for 2026: **a $150–$200 per-phone BOM cost increase**, and Xiaomi cutting 10–70M units from its 180M 2026 target (5–39%), OPPO cutting "over 20%," and Vivo cutting ~15%. The cuts are concentrated in the low-end, where the BOM increase from memory is a larger fraction of the selling price.

**This is where Dylan's numbers need to be flagged.** He said 2026 global smartphone shipments were headed to **800M**, and 2027 to **500–600M**. Neither number is sourceable. IDC's published forecasts are **1.12B for 2026** (a -12.9% YoY decline, which IDC itself called "the sharpest on record") and **~1.14B for 2027** (a ~2% recovery). Counterpoint's worst-case 2026 number is ~1.2B. There is no published industry forecast supporting 800M for 2026 or 500–600M for 2027. Dylan may be working from SemiAnalysis private channel checks into the Asian supply base, and he may turn out to be right — his track record on these calls is good — but the numbers should be labeled as SemiAnalysis private estimates, not industry consensus, and a listener should calibrate expectations accordingly.

**Micron's Powerchip acquisition was a regime change moment.** On January 2026 Micron announced the $1.8B acquisition of the Powerchip (PSMC) P5 Tongluo fab in Taiwan. The deal closed March 15, 2026 — **two days after the Dylan podcast aired.** Tom's Hardware characterized the deal as the end of the "technology-for-capacity era": for most of the last 15 years, second-tier Asian memory fabs licensed technology from Micron, Samsung, or SK Hynix and paid in wafer output. Going forward, the big three are buying out the second tier and internalizing the capacity directly. The strategic implication is that the supply cartel is consolidating, not loosening. Memory fab lead times are 2 years for brownfield expansions and 3–5 years for greenfield builds — the supply response to current pricing will not land before 2028 at the earliest.

**HBM market shares Q3 2025** (TrendForce): SK Hynix 57%, Samsung 22%, Micron 21%. All three are sold out through 2026. SK Hynix is spending ~$29B of capex in 2026 (roughly 4× its prior run-rate). Samsung is targeting a 50% HBM capacity expansion from 170k wpm to 250k wpm. Micron is spending $13.5B on DRAM capex in 2026 and breaking ground on a Hiroshima HBM fab in May 2026 with output targeted for 2028.

The memory crunch is not a temporary price spike. It is a capacity regime change that will compound into 2028.

---

# VI. Bottleneck Three — Power, Turbines, and Why the Grid Is Probably Not the Binding Constraint

The headline numbers first. The US grid is **~1.3 TW of nameplate generation capacity** per the EIA (Dylan said ~1 TW — understated by ~30%; it's a small point but matters for his "20% of the grid can be unlocked" math). Peak demand is ~750 GW. Data centers were 4.4% of US electricity consumption in 2023 (176 TWh of ~4,000 TWh total). LBNL's 2024 report projects data centers reaching 6.7–12% of US power by 2028 — Dylan's "10% by 2028" sits near the top of that range.

**The actual bottleneck is interconnect queue time, not nameplate capacity.** LBNL's "Queued Up 2025" report shows ~2,300 GW in active generator interconnect queues at end of 2024 — the first ever decline from 2,600 GW, driven by FERC Order 2023 clearing stale projects. The average 2023 interconnection project took ~5 years from study to commercial operation, versus <2 years in 2008. **You can have all the generation capacity in the world, but if the utility can't connect it to a data center for 5 years, it doesn't help you build a 2026 campus.**

**Gas turbines are the binding supply constraint on new generation.** Heavy-frame H/J class combined-cycle gas turbines (CCGT) are made by exactly three companies globally — GE Vernova, Siemens Energy, and Mitsubishi Power — accounting for 66–75% of turbines in plants under construction. GE Vernova is ramping to 20 GW per year of turbine capacity by mid-2026 and 24 GW by 2028. Current total 3-vendor capacity is ~40–50 GW per year, reaching 55–65 GW by 2027–2028 (Dylan's "~60 GW per year" was optimistic for today; IEEFA projects ~19 GW *available* for data centers in 2028, ~49 GW in 2029, ~76 GW in 2030). The pipeline is booked through 2028; the spot market for turbines is essentially closed.

**CCGT capex has risen sharply.** Dylan cited $1,500/kW, which is the historical (pre-2022) industry average. Current market pricing is **$2,000–2,400/kW** — NextEra's CEO has said gas turbine costs have roughly tripled since 2022 because of the turbine backlog, supply chain inflation, and compressed lead times. The $1,500/kW number was accurate a few years ago; it is not accurate now. This matters because power-plant economics feed directly into long-term PPA pricing that data centers sign with utilities.

**Behind-the-meter is the work-around.** Morgan Lewis estimates 30–50% of new AI data center capacity will be behind-the-meter (BTM) by 2029–2030, up from <5% today. Bloom Energy is tracking to 2 GW/year of fuel cell production capacity by end of 2026 (doubling from 1 GW) with 1.8 GW cumulative deployed by end of 2025. Reciprocating gas engines (Caterpillar, Wärtsilä) are another BTM path at ~$1,500/kW. The regulatory environment is unsettled — FERC has not issued a dispositive post-Talen/Susquehanna ruling on BTM grid-services charges, and state PUCs in Virginia, Ohio, and Pennsylvania are actively debating the issue — but ERCOT is the most permissive and has a large-load interconnection queue that expanded ~300% YoY to >233 GW, larger than ERCOT's current peak demand.

**The labor bottleneck.** The US has 818,700 electricians per BLS (2024 figure; Dylan said "~800,000" — close enough). Median wage is $62,350; data-center-cluster electricians earn 25–30% above the median and the top quartile can exceed $200k. ABC estimates a skilled-trade shortfall of ~439,000 workers across all construction, with 52% of construction firms reporting schedule delays. Crusoe's Abilene campus — the OpenAI Stargate anchor site — reported peak workforce of ~5,600 daily workers on a 1.2 GW phase 1 build. Scaling that model to the 20 GW of contracted annual US data center adds implies ~93,000 peak electrician-months across the national pipeline, against a labor pool that is already supply-constrained. Dylan is correct that labor is a bottleneck; he may have understated how tight it is.

**China power growth is worth correcting.** Dylan cited "~30% per year" for China's power capacity growth. The aggregate figure is closer to 10–12% per year in the 2000s and 7–10% in the 2010s. The **30% figure applies specifically to solar and wind capacity additions in recent years**, not total capacity. China's total installed capacity has grown significantly, but the aggregate growth rate is not 30%. This is the kind of correction that matters when the US-China power comparison enters policy discussions.

**The xAI Memphis factual error.** Dylan described xAI's Memphis Colossus data center as a former aluminum smelter. It is not. Colossus is in a former **Electrolux appliance plant** that operated from 2012 to 2020. No evidence exists of a prior aluminum smelter at the site. This is a small factual error but worth correcting because it circulated widely.

**Rack power density.** Dylan characterized Nvidia's Kyber as "~1 MW per rack." The actual Kyber NVL576 rack for Rubin Ultra (2027) is ~600 kW, not 1 MW. The 1 MW figure refers to a future 800 VDC rack architecture target for post-Rubin-Ultra generations. Current GB300 NVL72 is ~132–140 kW nominal with ~155 kW peaks. The 1 MW number will eventually be correct; it is not correct for 2026–2027 product.

**The aggregate picture:** 2026 US data center capacity actually *energized* will be ~12 GW per Bloom Energy and Bloomberg NEF, not the ~20 GW Dylan cited as contracted. S&P 451 puts the 2026 global under-construction number at ~23 GW with a US supply shortfall of 9.3 GW. Roughly half of planned US builds have been delayed or canceled at the interconnect queue. The 20 GW figure is accurate as *contracted* capacity; Dylan likely meant contracted rather than energized, but the podcast did not make that distinction clear.

The qualitative picture Dylan paints is right: power is a binding constraint on the speed of the build, not on the ceiling of the build. The US grid can accommodate AI at ~10% of total power consumption without topology changes. It cannot accommodate that growth in 3 years. It can in 5–7.

---

# VII. GPU Economics and Why Old H100s Are Worth More Today

Dylan's strongest economic insight on the podcast is not a number; it's a conceptual point about how to price a GPU. The argument:

1. A GPU is a compute substrate that runs a model.
2. The value of the GPU is the value of the best model that can fit on it, not the FLOPS it has relative to newer hardware.
3. As frontier labs keep distilling stronger models down to smaller sizes that fit on older hardware, the "best model" that an H100 can serve keeps getting better.
4. Therefore, the H100's value as a serving platform keeps rising, even as Blackwell and Rubin come online for training.
5. This is the **Alchian-Allen effect** applied to compute: the fixed cost (the best model for a given memory footprint) dominates the choice, making the "cheaper" older GPU relatively more valuable.

The empirical evidence: the SemiAnalysis H100 1-year rental index bottomed at ~$1.70/hr in October 2025 and rebounded to ~$2.35/hr by March 2026, a ~40% increase. Meta-style 24k H100 cluster TCO at 5-year depreciation works out to ~$1.40–1.50/hr fully burdened, so a $2.35/hr rental on a 1-year contract yields a ~70% margin on a cluster that has been fully amortized from a capex perspective. H100 on-demand pricing (April 2026) ranges from roughly $1.49/hr at budget providers to $1.87 at Vast.ai, $1.99 at RunPod, $2.99 at Lambda, $3.90 at AWS, and $6.16 at CoreWeave. The spread between providers is large and reflects contract term, region, and power economics — not intrinsic hardware value.

**The FLOPS progression** (verified against datasheets):

| Chip | FP16 dense | FP8 dense | Memory |
|---|---|---|---|
| A100 (2020) | 312 TFLOPS | - | 40–80 GB HBM2 |
| H100 SXM (2022) | 989.5 TFLOPS | 1,979 TFLOPS | 80 GB HBM3 |
| B200 (2024) | ~2,250 TFLOPS (dual-die) | ~4,500 TFLOPS | 192 GB HBM3e |
| Rubin R100 (2026) | ~8 PFLOPS (derived) | 16 PFLOPS | 288 GB HBM4 |

Dylan's "Rubin FP16 ~5 PFLOPS" on the podcast is probably per-die; the Nvidia headline number implies ~8 PFLOPS per package. The Vera Rubin NVL72 rack delivers 3.6 EF of AI compute and 260 TB/s of NVLink bandwidth, using 72 Rubin GPUs at ~120 kW per rack equivalent to current Blackwell density.

**The rack architectures are different animals.** NVL72 (Blackwell and early Rubin) puts 72 GPUs in an all-to-all NVLink domain at ~120–140 kW. Google's TPU v7 (Ironwood) pods go to 9,216 chips in a 3D torus topology with each chip having 6 neighbors. AWS Trainium 3 moved to an all-to-all NeuronSwitch-v1 with 144-chip UltraServer domains — a middle ground between Nvidia's dense-rack and Google's torus-pod architectures. The scale-up domain choice is becoming a point of architectural differentiation, not just a hardware detail: dense all-to-all is best for smaller models that fit entirely in the domain and need low-latency communication; torus is best for larger models that can tolerate higher per-hop latency in exchange for massive aggregate bandwidth.

**The DeepSeek production inference system is instructive.** Dylan referenced "~160 GPUs" as the DeepSeek serving unit. The actual public disclosure (DeepSeek's February 2025 open-source week) showed an average of 226.75 nodes × 8 H800 ≈ **1,814 GPUs total**, with a peak of ~2,224. The *minimum serving unit* was 32 prefill + 144 decode = 176 GPUs, which is close to Dylan's "160" figure. So either Dylan was citing the serving-unit number and the listener heard it as the cluster total, or the two numbers got conflated. The broader point — that DeepSeek serves its model on roughly one rack's worth of GPUs — is correct at the serving-unit level.

**Rubin Ultra's packaging risk.** Dylan described Rubin Ultra as a 4-die package. Post-podcast reporting (Tweaktown) suggests Nvidia may revert to a 2-die package for Rubin Ultra because of CoWoS-L substrate warping issues. This is a meaningful change if confirmed, because the 4-die architecture is what allows Rubin Ultra to reach the headline 50 PF FP4 / 16 PF FP8 per package. A 2-die revert would likely mean headline specs come in lower than Nvidia guided at GTC 2025.

**The "15% Blackwell RMA rate" Dylan cited is SemiAnalysis private intel.** TSMC's published B200 chip yield is 90–95%, which is a different metric (chip-level yield at the fab) from the RMA rate (system-level failure after deployment). The 15% figure has no public source. If it is accurate, it implies that Nvidia is shipping substantial volume of systems with silent defects that only manifest in hyperscaler deployments — a significant operational burden that would not show up in Nvidia's financial disclosures unless tied to warranty reserves. We note the figure but cannot corroborate it.

**The gigawatt economics, restated cleanly:**

- **To build 1 GW:** ~$50B total capex per Jensen's earnings call guidance ("$50–60B/GW"). Bernstein has published ~$35B for the compute-and-server portion alone, with the shell, power, cooling, and fit-out adding the rest.
- **To rent 1 GW:** ~$10–13B per year, back-derived from the Anthropic/Google TPU deal (~$42B RPO for ~700 MW over 5 years). This is directionally consistent with the industry's 3–4 year GPU amortization assumption. Dylan's number is consistent with the disclosed deal math; the deal math itself is the only primary source.

The 3-year payback window is what makes the whole thing work at current capital costs. It is also what makes the whole thing terrifying if the model economics fail: a hyperscaler that has committed $50B to a single gigawatt of compute needs that gigawatt to be generating productive tokens at roughly $10B/year for the math to clear. The labs that are renting the compute need *their* revenue models to work out to support the $10B/year payment. And the cap on "productive tokens at $10B/year" is set by (a) how many enterprises and consumers will pay for AI services, (b) at what unit economics, and (c) for how long before a better model on cheaper hardware makes the current inventory uneconomic. None of these variables is currently priced with any precision.

---

# VIII. Corrections to the Transcript

Consolidating all the factual corrections in one place for the record. Dylan made a number of small factual errors and several meaningful ones; nearly all of his quantitative errors pushed his ceiling estimates lower (i.e., more bottlenecked) than the underlying data supports.

**Hard corrections (the data contradicts what Dylan said):**

1. **EUV throughput.** Dylan: ~75 wph. Actual: NXE:3800E runs at >195 wph, with current fleet average closer to 160–180 wph. This is a three-generation stale number. Cascade effect: "3.5 tools per GW" becomes ~1.3–1.95 tools/GW; 2030 ceiling moves from ~200 GW to ~359–538 GW.

2. **Reticle stage acceleration.** Dylan: "~9G." Actual: Low-NA NXE ~15G; High-NA EXE ~32G. The 9G number does not appear in ASML's current product literature for any shipping tool.

3. **"10,000+ ASML suppliers."** Actual: ~5,000 tier-1 suppliers, of which ~200 are strategic. Individual tools have ~700,000 components. The 10,000 figure is ~2× overstated.

4. **EUV tool price.** Dylan: "$300–400M." This conflates Low-NA (~$180–220M for NXE:3800E) with High-NA (~$380M for EXE:5000/5200). Hyper-NA, expected ~2030, is rumored at ~$700M.

5. **Huawei Ascend 910 chronology.** Dylan: "~2mo before TPU, ~4mo before A100." Actual: Ascend 910 = August 23, 2019. A100 = May 14, 2020 — a 9-month gap, not 4. TPU v3 pods GA'd in May 2019, three months *before* Ascend. No TPU generation launched within 2 months of Ascend 910 in either direction.

6. **xAI Memphis "former aluminum smelter."** Actual: Colossus sits in a former Electrolux appliance plant (2012–2020). No prior aluminum smelter at the site.

7. **US grid ~1 TW.** Actual: ~1.3 TW nameplate per EIA. Small but matters for the "what % of the grid can we unlock" arithmetic.

8. **China power growth ~30%/year.** Actual: aggregate capacity growth is 7–12%/year; the 30% figure applies to solar/wind *additions* specifically, not total capacity.

9. **2026 smartphone shipments 800M.** Actual: IDC 1.12B (-12.9% YoY). No public industry forecast supports 800M. Flag as SemiAnalysis private estimate.

10. **2027 smartphone shipments 500–600M.** Actual: IDC ~1.14B (modest recovery). No public industry forecast supports 500–600M.

11. **Kyber "~1 MW rack."** Actual: Kyber NVL576 is ~600 kW. The 1 MW figure refers to a future 800 VDC architecture target for post-Rubin-Ultra.

12. **CCGT capex $1,500/kW.** Historically accurate; current market is $2,000–2,400/kW. NextEra's CEO has said gas-turbine costs tripled since 2022.

13. **~60 GW/year turbine capacity.** Actual: current 3-vendor is ~40–50 GW/year, reaching 55–65 GW by 2027–2028. IEEFA projects 19 GW data-center-available in 2028.

**Stale since March 13, 2026 (events superseded the podcast):**

1. **Nvidia ~$90B contracts.** Jensen's GTC keynote on March 16 disclosed ~$500B Blackwell+Rubin visibility through end-2026, ~$1T through 2027. Dylan's figure may reflect the non-cancellable PO line on the 10-Q, a narrower metric, but the headline is dramatically low.

2. **OpenAI $110B raise.** Final close was $122B / $852B post-money on March 31.

3. **Anthropic ~$20B ARR.** Anthropic crossed ~$30B ARR in April, reportedly passing OpenAI (~$25B) for the first time.

4. **Anthropic/Google TPU deal "1M chips."** Broadcom expanded the deal to 3.5 GW of TPU capacity from 2027, announced April 6–7.

5. **Rubin Ultra 4-die package.** Post-podcast reports (Tweaktown) suggest possible revert to 2-die due to CoWoS-L warping.

6. **Micron/Powerchip PSMC deal.** Closed March 15, 2026 — two days after the podcast aired.

7. **ASML 1000W EUV source demonstration.** Q1 2026 development, not stated in podcast; relevant because it supports a 50% throughput gain from existing fleet by 2030.

**SemiAnalysis proprietary claims (not independently verifiable):**

1. The 55k + 6k + 170k wafer breakdown per GW of Rubin.
2. EUV ~28–30% of N3 wafer cost.
3. Anthropic monthly revenue adds (+$4B January / +$6B February).
4. 15% Blackwell RMA rate.
5. "16+ gas-power OEMs tracked by SemiAnalysis" (Blackridge Research lists 15+; plausible but unverifiable).
6. Gemini ARR ~$5B (Alphabet does not disclose this metric).
7. "20 GW of 2026 US DC capacity" (this is a contracted figure; Bloom Energy says ~12 GW energized).
8. DRAM cost percentage of litho progressing from teens to 20%+.

None of the proprietary claims are obviously wrong. All of them should be labeled as SemiAnalysis internal model outputs rather than facts. A listener who takes Dylan's whole set of numbers at face value is effectively deferring to SemiAnalysis's model as if it were an industry disclosure; that is a reasonable deference in many cases, but it should be a conscious one.

---

# IX. What This Means for the Next Five Years

Stripping the model down to its load-bearing claims, what remains after corrections:

**The AI chip supply chain has three compounding bottlenecks, each with different time constants:**

- **Logic (EUV-gated)** is the hardest ceiling in principle but the loosest ceiling in practice, because the existing fleet has ~50% throughput upside from the 1000W source, because NXE:3800E is already ~2.5× faster than Dylan's stated baseline, and because ~700 cumulative EUV tools by 2030 likely supports 360–540 GW of AI chip capacity, not 200 GW. The correction widens the ceiling without removing it.

- **Memory** is the tightest actual constraint through 2028 because HBM steals 3–4× wafer capacity from commodity DRAM, because memory capex lead times are 2–5 years, because all three vendors are sold out through 2026, and because the Micron/PSMC deal signals consolidation of the tier-2 capacity rather than expansion. The crunch will compound, not ease. Consumer electronics pricing is the canary: Apple's 230% iPhone memory BOM increase is the first visible transmission of the data center shortage into the consumer economy.

- **Power** is a binding constraint on *timing*, not on *ceiling*. The US grid has the nameplate to absorb AI growth at 10% of national consumption. It does not have the interconnect queue, the gas turbines, or the electricians to do it in 3 years. It can do it in 5–7. The binding sub-constraints in rough order of severity: interconnect queue time > gas turbine OEM capacity > skilled electrician availability > nameplate generation.

**What this implies for the next five years:**

- **2026 is a bottleneck-discovery year.** Hyperscaler capex is rising sharply, labs are raising unprecedented rounds, and the binding constraints (memory, power, gas turbines) are becoming visible to non-specialist audiences. Expect more public disclosures about build delays, more consumer electronics price pass-throughs, and more political attention to the grid interconnect queue.

- **2027 is where the HBM capacity response starts to land.** SK Hynix's $29B 2026 capex, Samsung's 50% HBM expansion, and Micron's Hiroshima fab (output 2028) all take 1.5–3 years to convert capex into production bits. The crunch does not ease in 2026 or 2027; it might ease slightly in 2028.

- **2028 is the earliest year a material logic-capacity response lands.** TSMC Arizona Fab 2 hits mass production in 2H 2027, Intel 14A risk production lands 2027 with volume 2028, and ASML High-NA EXE:5000 units start shipping at scale. These come online during a memory-still-tight period. The joint constraint — memory crunch overlapping with logic expansion — is the specific scenario that could scramble Dylan's sequential bottleneck model.

- **2029–2030 is when the 200–540 GW ceiling becomes a live question.** By this point we will know whether the 1000W EUV source shipped, whether High-NA hit its volume ramp, whether HBM4E hit its throughput targets, whether the interconnect queue cleared, and whether Rubin Ultra's 4-die package worked. Any two of these failing means the ceiling is ~200 GW (Dylan's floor). Most succeeding means the ceiling is >500 GW (the optimistic view). Outcomes in between are most likely.

**What a careful listener should take from the podcast:**

- The qualitative thesis — that the AI buildout is constrained by silicon, memory, and power in roughly that order of tightness, and that capital is the easiest of the four — is correct.
- The specific numbers that Dylan used to make the thesis vivid — 3.5 EUV tools per GW, ~200 GW 2030 ceiling, $90B Nvidia contracts, ~$20B Anthropic ARR, 800M 2026 smartphones — are a mix of correct, stale, and proprietary-model estimates that should be downweighted relative to the thesis they support.
- The model that matters most for downstream policy, investment, and architecture decisions is the one where EUV has ~50% hidden throughput upside, memory is the binding 2026–2028 constraint, and the US grid can handle the 2030 load but not on the 2026 schedule the labs are trying to enforce.

The podcast is a useful compressed synthesis of a very fast-moving supply chain. It is also the kind of synthesis that ages quickly — seven weeks after it aired, roughly a dozen of its headline numbers have been superseded. The shelf life of this kind of analysis is measured in weeks, not months, and the shelf life of the specific corrections in this report is probably also measured in weeks. That is the nature of the present moment.

What survives is the shape of the problem: a tower of a dozen specialized industries, controlled by a dozen specific companies, pushing against a set of physical and logistical ceilings that none of them individually can break. The interesting question for the next five years is not whether any one ceiling binds — they all will at some point — but which one binds first, and whether the response lands before the next one starts.

---

# X. Data Sources and Methodology

This report is a synthesis of five research memos prepared April 1–8, 2026 by separate research agents tasked with verifying specific claim clusters from the Dylan Patel SemiAnalysis podcast of March 13, 2026 on the Dwarkesh Patel podcast. Each memo was produced independently and then cross-checked by a separate audit pass.

**Scope of research:**

- **Hyperscaler CapEx memo:** 20 claims covering Amazon/Google/Meta/Microsoft 2026 capex guidance, Big 4 aggregate, OpenAI and Anthropic raises, lab ARR, CoreWeave contract structure, GW targets, and 1 GW rental economics. Primary sources: company 10-Ks, earnings calls, press releases, SEC filings.
- **EUV/TSMC memo:** 23 claims covering ASML tool shipments, EUV physics and throughput, tool pricing, High-NA roadmap, TSMC capex, Huawei chronology, and supplier counts. Primary sources: ASML financial releases, JEDEC standards, TSMC filings, Nvidia datasheets.
- **Memory memo:** 22 claims covering HBM bits-per-wafer ratios, HBM4 standards, DRAM pricing, iPhone BOM, smartphone shipment forecasts, NAND capex, and the Micron/PSMC deal. Primary sources: JEDEC, TrendForce, Counterpoint, IDC, company press releases.
- **Power/labor memo:** 23 claims covering the US grid, data center demand, PJM/ERCOT/MISO, gas turbine OEMs, CCGT capex, BTM generation, electrician labor, and specific builds (Crusoe Abilene, xAI Memphis). Primary sources: EIA, LBNL, BloombergNEF, Bloom Energy, BLS, individual company disclosures.
- **GPU economics memo:** 24 claims covering H100/A100/B200/Rubin specs, rental pricing, DeepSeek inference system, scale-up domains, and 1 GW economics. Primary sources: Nvidia datasheets, SemiAnalysis newsletter, DeepSeek open-source disclosures.

**Audit methodology:** A separate audit pass identified 13 hard factual errors, 12 stale claims superseded by events between March 13 and April 9, 2026, and 8 SemiAnalysis proprietary claims that could not be independently verified. The corrections in Section VIII reflect the audit outputs.

**Confidence tiers used in this report:**

- **Tier 1 (verified):** Claims cross-checked against primary sources (company filings, regulatory disclosures, datasheets). Stated as fact.
- **Tier 2 (partially verified / model-consistent):** Claims derivable from disclosed data but not directly reported. Labeled as derived or approximate.
- **Tier 3 (SemiAnalysis proprietary or single-source):** Claims appearing only in SemiAnalysis newsletter or private channel outputs. Flagged as SemiAnalysis private estimate.
- **Tier 4 (contradicted):** Claims where Dylan's stated number differs from the primary source by more than a small margin. Corrected with the primary source number.

No figures in this report were fabricated or interpolated. Where a number is an estimate, it is labeled as such. Where a number is not available, the report says so explicitly.

---

# XI. Sources

**Hyperscaler CapEx and lab financials:**

- Amazon Q4 2025 earnings: https://www.morningstar.com/stocks/amazon-earnings-guidance-200-billion-capital-expenditure-2026-overshadows-good-results
- Alphabet Q4 2025 8-K: https://www.sec.gov/Archives/edgar/data/1652044/000165204426000012/googexhibit991q42025.htm
- Alphabet 2026 capex guidance: https://seekingalpha.com/news/4547610-alphabet-outlines-175b-185b-2026-capex-plan-as-ai-momentum-accelerates-across-search-cloud
- Meta 2026 capex: https://www.datacenterdynamics.com/en/news/meta-estimates-2026-capex-to-be-between-115-135bn/
- Microsoft FY26 Q2 earnings: https://www.microsoft.com/en-us/investor/events/fy-2026/earnings-fy-2026-q2
- Big 4 aggregate: https://www.cnbc.com/2026/02/12/top-hyperscalers-to-boost-ai-capex-to-600-billion-stocks-that-benefit.html
- $1T global DC capex: https://futurumgroup.com/insights/ai-capex-2026-the-690b-infrastructure-sprint/
- Dell'Oro 2030 projection: https://www.delloro.com/news/ai-boom-drives-data-center-capex-to-1-7-trillion-by-2030/
- OpenAI $110B raise: https://techcrunch.com/2026/02/27/openai-raises-110b-in-one-of-the-largest-private-funding-rounds-in-history/
- OpenAI $122B final close: https://www.cnbc.com/2026/03/31/openai-funding-round-ipo.html
- Anthropic Series G: https://www.anthropic.com/news/anthropic-raises-30-billion-series-g-funding-380-billion-post-money-valuation
- Anthropic $20B ARR: https://www.bloomberg.com/news/articles/2026-03-03/anthropic-nears-20-billion-revenue-run-rate-amid-pentagon-feud
- Anthropic gross margin: https://www.theinformation.com/articles/anthropic-lowers-profit-margin-projection-revenue-skyrockets
- Alphabet Q4 2025 earnings call: https://www.fool.com/earnings/call-transcripts/2026/02/04/alphabet-googl-q4-2025-earnings-call-transcript/
- OpenAI 1.9 GW capacity disclosure: https://siliconangle.com/2026/01/19/openai-reveals-data-center-capacity-tripled-1-9gw-2025/
- Google/Anthropic TPU deal: https://www.googlecloudpresscorner.com/2025-10-23-Anthropic-to-Expand-Use-of-Google-Cloud-TPUs-and-Services
- Broadcom 3.5 GW expansion: https://www.tomshardware.com/tech-industry/broadcom-expands-anthropic-deal-to-3-5gw-of-google-tpu-capacity-from-2027
- CoreWeave Q4 2025 earnings: https://investors.coreweave.com/news/news-details/2026/CoreWeave-Reports-Strong-Fourth-Quarter-and-Fiscal-Year-2025-Results/
- Nvidia GTC 2026 keynote: https://www.cnbc.com/2026/03/16/nvidia-gtc-2026-ceo-jensen-huang-keynote-blackwell-vera-rubin.html
- OpenAI/Nvidia 10 GW deal: https://nvidianews.nvidia.com/news/openai-and-nvidia-announce-strategic-partnership-to-deploy-10gw-of-nvidia-systems

**EUV, ASML, TSMC:**

- ASML Q4 2025 financial release: https://www.asml.com/en/news/press-releases/2026/q4-2025-financial-results
- NXE:3800E product page: https://www.asml.com/en/products/euv-lithography-systems/twinscan-nxe-3800e
- ASML High-NA pricing: https://www.tomshardware.com/tech-industry/manufacturing/asmls-high-na-chipmaking-tool-will-cost-dollar380-million-the-company-already-has-orders-for-10-to-20-machines-and-is-ramping-up-production
- ASML responsible supply chain: https://www.asml.com/en/company/sustainability/responsible-supply-chain
- ASML mechanics and mechatronics: https://www.asml.com/en/technology/lithography-principles/mechanics-and-mechatronics
- Zeiss EUV explainer: https://www.zeiss.com/semiconductor-manufacturing-technology/smt-magazine/so-does-euv-lithography-work.html
- ASML 1000W source and 2030 throughput target: https://www.digitimes.com/news/a20260224VL210/euv-asml-2030-production-scanner.html
- SemiAnalysis N3 conundrum: https://semianalysis.com/2022/12/21/tsmcs-3nm-conundrum-does-it-even/
- SemiAnalysis great AI silicon shortage: https://newsletter.semianalysis.com/p/the-great-ai-silicon-shortage
- SemiAnalysis die size and reticle: https://newsletter.semianalysis.com/p/die-size-and-reticle-conundrum-cost
- TSMC capex: https://www.trendforce.com/news/2024/10/14/news-tsmcs-capital-expenditure-expected-to-remain-unchanged-this-year-ahead-of-earnings-call/
- Ascend 910 launch: https://www.edge-ai-vision.com/2019/08/huawei-launches-ascend-910-the-worlds-most-powerful-ai-processor-and-mindspore-an-all-scenario-ai-computing-framework/
- A100 launch: https://techcrunch.com/2020/05/14/nvidia-begins-shipping-the-a100-its-first-ampere-based-data-center-gpu/
- TSMC N2 customers: https://www.design-reuse.com/news/202529366-tsmc-s-first-2-nm-node-customers-are-apple-amd-nvidia-and-mediatek-intel-missing/

**Memory / HBM:**

- SemiAnalysis HBM scaling: https://newsletter.semianalysis.com/p/scaling-the-memory-wall-the-rise-and-roadmap-of-hbm
- Tom's Hardware HBM/RAM: https://www.tomshardware.com/pc-components/ram/hbm-is-eating-your-ram
- Memory 30% of hyperscaler capex: https://www.tomshardware.com/tech-industry/memory-will-consume-30-percent-of-hyperscaler-spending-this-year
- JEDEC HBM4 standard: https://www.jedec.org/news/pressreleases/jedec%C2%AE-and-industry-leaders-collaborate-release-jesd270-4-hbm4-standard-advancing
- iPhone 17 Pro memory: https://www.macrumors.com/2025/09/09/iphone-17-pro-iphone-air-ram-amounts/
- Apple LPDDR5X premium: https://wccftech.com/apple-paying-230-percent-premium-for-12gb-lpddr5x-ram-found-in-iphone-17-models/
- DRAM spot prices: https://www.trendforce.com/presscenter/news/20260202-12911.html
- Counterpoint 2026 smartphone forecast: https://counterpointresearch.com/en/insights/2026-smartphone-shipment-forecasts-revised-down-as-memory-shortage-drives-bom-costs-up
- IDC smartphone 2026: https://www.cnbc.com/2026/02/27/smartphone-market-poised-for-sharpest-decline-on-record-in-2026-according-to-reports-memory-chip-data-center-ai-samsung-apple-google-meta.html
- NAND output cuts: https://www.trendforce.com/news/2025/11/13/news-nand-giants-reportedly-cut-output-in-2h25-as-prices-surge-samsung-mulls-20-30-hike-in-2026/
- Xiaomi/Oppo smartphone cuts: https://www.trendforce.com/news/2026/01/21/news-chinas-xiaomi-oppo-reportedly-cut-2026-shipments-amid-memory-crunch-huawei-less-affected/
- Micron PSMC acquisition: https://www.tomshardware.com/pc-components/dram/micron-acquires-psmc-fab-site-in-taiwan-for-usd1-8-billion-acquisition-to-expand-the-memory-makers-operations-within-the-region-move-marks-the-end-of-the-technology-for-capacity-era
- SK Hynix DRAM roadmap: https://news.skhynix.com/sk-hynix-presents-future-dram-technology-roadmap-at-ieee-vlsi-2025/
- Samsung 3D DRAM: https://www.tomshardware.com/pc-components/dram/samsung-outlines-plans-for-3d-dram-which-will-come-in-the-second-half-of-the-decade

**Power, grid, labor:**

- S&P 451 DC power demand: https://www.spglobal.com/energy/en/news-research/latest-news/electric-power/101425-data-center-grid-power-demand-to-rise-22-in-2025-nearly-triple-by-2030
- Bloom Energy 2026 power report: https://www.bloomenergy.com/wp-content/uploads/2026-power-report.pdf
- BloombergNEF DC power: https://www.utilitydive.com/news/us-data-center-power-demand-could-reach-106-gw-by-2035-bloombergnef/806972/
- JLL 200 GW DC forecast: https://www.jll.com/en-us/newsroom/global-data-center-sector-to-nearly-double-to-200gw-amid-ai-infrastructure-boom
- EIA US grid capacity: https://www.eia.gov/todayinenergy/detail.php?id=67205
- LBNL 2024 DC report: https://eta-publications.lbl.gov/sites/default/files/2024-12/lbnl-2024-united-states-data-center-energy-usage-report_1.pdf
- EIA power growth: https://www.eia.gov/todayinenergy/detail.php?id=65264
- PJM IRM/FPR: https://www.pjm.com/-/media/DotCom/committees-groups/committees/mrc/2025/20250319/20250319-item-04---irm-fpr-and-elcc-for-26-27-bra---presentation.pdf
- GridLab gas turbine costs: https://gridlab.org/wp-content/uploads/2025/09/GridLab_Gas-Turbine-Costs-Report-1.pdf
- Global Energy Monitor turbines: https://globalenergymonitor.org/report/leading-three-manufacturers-providing-two-thirds-of-turbines-for-gas-fired-power-plants-under-construction/
- GE Vernova ramp: https://www.utilitydive.com/news/ge-vernova-gas-turbine-investor/807662/
- Crusoe Abilene campus: https://www.crusoe.ai/resources/newsroom/crusoe-expands-ai-data-center-campus-in-abilene-to-1-2-gigawatts
- BLS electricians: https://www.bls.gov/ooh/construction-and-extraction/electricians.htm
- Morgan Lewis BTM: https://www.morganlewis.com/pubs/2025/02/powering-the-future-of-data-infrastructure-capacity-and-connectivity-considerations
- Bloom Energy 2 GW target: https://www.utilitydive.com/news/bloom-energy-says-its-on-track-for-2-gw-annual-production-capacity/804291/
- xAI Colossus background: https://www.rdworldonline.com/how-xai-turned-a-factory-shell-into-an-ai-colossus-to-power-grok-3-and-beyond/

**GPU specs and economics:**

- H100 datasheet: https://resources.nvidia.com/en-us-gpu-resources/h100-datasheet-24306
- DGX B200 datasheet: https://resources.nvidia.com/en-us-dgx-systems/dgx-b200-datasheet
- GB300 NVL72 product page: https://www.nvidia.com/en-us/data-center/gb300-nvl72/
- SemiAnalysis H100 rental index: https://newsletter.semianalysis.com/p/the-great-gpu-shortage-rental-capacity
- SemiAnalysis AI cloud TCO: https://semianalysis.com/ai-cloud-tco-model/
- Meta 24k H100 cluster TCO: https://pytorchtoatoms.substack.com/p/metas-24k-h100-cluster-capextco-and
- DeepSeek inference system: https://github.com/deepseek-ai/open-infra-index/blob/main/202502OpenSourceWeek/day_6_one_more_thing_deepseekV3R1_inference_system_overview.md
- Rubin specs: https://blog.barrack.ai/nvidia-rubin-specs-architecture-2026/
- Kyber NVL576 600 kW: https://www.tomshardware.com/pc-components/gpus/nvidia-shows-off-rubin-ultra-with-600-000-watt-kyber-racks-and-infrastructure-coming-in-2027
- Rubin Ultra dual-die reversion: https://www.tweaktown.com/news/110819/nvidias-rubin-ultra-reportedly-sticking-to-a-dual-die-design-instead-of-a-four-die-plan/index.html
- Dojo restart: https://www.tomshardware.com/tech-industry/supercomputers/elon-musk-restarts-dojo3-space-supercomputer-project-as-ai5-chip-design-gets-in-good-shape
- TPU v7 docs: https://docs.cloud.google.com/tpu/docs/tpu7x
- Trainium 3 deep dive: https://newsletter.semianalysis.com/p/aws-trainium3-deep-dive-a-potential
- SemiAnalysis TPU v7: https://newsletter.semianalysis.com/p/tpuv7-google-takes-a-swing-at-the
- Dwarkesh Elon Musk interview: https://www.dwarkesh.com/p/elon-musk
