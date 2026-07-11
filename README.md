# The Silicon Decade: A Data-Driven Read on the Global Semiconductor Industry, 2010–2026

**A data analysis by Emeka Richard Ogochukwu**

## Why this analysis

Five datasets, one industry, one question: what actually happened in semiconductors between 2010 and 2026, and what does the data say about what happens next? This analysis joins AI accelerator shipment data, 17 years of company financials across 40 companies, monthly chip pricing, a timeline of export-control actions, and fab-level wafer capacity to build a single, evidence-based narrative — not five separate charts.

The approach mirrors how I'd scope a market or competitive-intelligence brief: start with the headline trend, stress-test it against adjacent data, and flag where the data itself has limits.

## 1. The AI accelerator boom, by the numbers

NVIDIA's data-center GPU revenue (as reconstructed from shipment volumes and ASPs across its V100 → B300 product line) grew from **$573M in 2020 to a peak of $87.8bn in 2025** — a 153x increase in five years. The wave is generational, not linear: each new chip (H100, then H200/B200, then GB200) launches into a market that immediately cannibalizes the prior generation rather than adding to it in parallel.

![NVIDIA's AI accelerator revenue by chip generation, 2020-2026](chart1_nvidia_revenue_wave.png)

**A note on the 2026 dip:** the chart shows revenue falling to $57.4bn in 2026. I want to be upfront that I don't read this as an actual demand collapse — the underlying data only has chip launches and shipment records through mid-2026 (consistent with today's date), so 2026 is a partial year, not a completed one. Taking a partial-year figure at face value and calling it "the AI bubble bursting" would be exactly the kind of analytical error I'd want to catch before it reached a client or a trading desk.

**Performance-per-dollar has improved roughly 13x in nine years.** NVIDIA's compute-per-$1,000 of ASP rose from 6.7 TFLOPS (V100, 2017) to 87.5 TFLOPS (B300, 2025). This is the real story underneath the revenue numbers: NVIDIA isn't just selling more chips, it's selling exponentially more compute per dollar, which is what has kept hyperscaler capex growing even as unit prices stayed roughly flat.

**Market concentration is real, but eroding slightly.** NVIDIA held 95% of tracked AI accelerator revenue in 2020. By 2026 that's down to 81% — still dominant, but AMD (5%), Google's TPU program (5%), and a longer tail of Huawei, Cerebras, AWS Trainium/Inferentia, and Groq now collectively represent a fifth of the market.

![AI accelerator vendor market share, 2020 vs 2026](chart2_market_share.png)

## 2. The changing of the guard: NVIDIA overtakes Intel

Using each company's *total* reported revenue (not just AI-chip revenue), NVIDIA passed Intel for the first time in company history in **2023** — $62.1bn vs. $53.9bn — and the gap has since become a chasm: by 2026 NVIDIA's $250.7bn in revenue is more than 4x Intel's $59.8bn. Intel's revenue has been essentially flat for a decade (a 1.9% CAGR from 2010–2026, against TSMC's 14.3% and NVIDIA's ~30%+ over the AI era), while its operating margin sits at 12.5% in 2026 — the weakest of any major company in this dataset.

![NVIDIA vs Intel revenue crossover, 2010-2026](chart3_nvidia_vs_intel.png)

This isn't just a chip story, it's a strategic-positioning story: Intel bet on being an integrated device manufacturer (design + fab) at exactly the moment the industry rewarded specialization — TSMC in pure-play manufacturing, NVIDIA in pure-play design. Both of Intel's more focused competitors grew revenue roughly 7–15x faster.

## 3. Sanctioned, not stopped: China's semiconductor sector under export controls

This is the most consequential finding in the dataset. Overlaying the 34-entry export control timeline onto Chinese chip company revenue shows a clear, three-act structure:

1. **2017–2020: unrestricted growth.** HiSilicon (Huawei's chip design arm) grew from $6.6bn to $10.7bn in revenue, becoming a top-5 global fabless design house.
2. **2020–2022: the collapse.** The May 2020 Foreign Direct Product Rule — which barred TSMC from manufacturing any chip designed by a US-sanctioned entity — cut off HiSilicon's only path to leading-edge fabrication. Its revenue fell 82% in two years, from $10.7bn (2020) to $1.97bn (2022).
3. **2023–2026: the recovery.** HiSilicon didn't just stabilize — it **fully recovered and then exceeded its pre-sanctions peak**, reaching $11.5bn in 2026 (+8% above 2020), powered by SMIC's success in reaching 7nm-class production domestically (via multi-patterning DUV lithography, since ASML's EUV tools remain export-restricted) and Huawei's Mate 60 relaunch.

![China's chip sector under sanctions](chart4_china_selfsufficiency.png)

The combined revenue of the five tracked Chinese chip companies (HiSilicon, SMIC, YMTC, CXMT, Hua Hong) grew from **$18.3bn in 2020 to $41.4bn in 2026** — up 126% *despite* being the direct target of 26 separate US export-control actions over that period (average severity 7.4/10 under the Biden administration alone).

**The strategic read:** export controls delayed China's advanced-chip capability by roughly three years and inflicted a genuine, sharp shock on individual firms — but they did not prevent Chinese firms from reaching the industry's next tier, and arguably accelerated a coordinated, state-backed push toward full-stack self-sufficiency (design, foundry, and memory all show industry-wide growth). Any market-entry or supply-chain-risk analysis that assumes controls will permanently freeze Chinese capability is not supported by this data.

## 4. Memory: the cycle that never stopped

DRAM and NAND remain the semiconductor industry's most classically cyclical products. DDR4 8Gb DRAM ran from **$1.38/chip (trough, Aug 2023) to $8.43/chip (peak, Jul 2018)** — a 6.1x swing — and the annual average price has moved by more than 20% year-on-year in 9 of the last 12 years.

![DRAM/NAND commodity cycles and HBM3 pricing](chart5_memory_cycles.png)

The more interesting story is **HBM3** (the high-bandwidth memory that sits inside every AI accelerator): its price has risen from **$188/stack (Jan 2022) to $841/stack (Apr 2026), up 346%**, moving in the opposite direction from commodity DRAM over the same window. This is the clearest evidence in the dataset that AI demand has created a structural, not cyclical, repricing of advanced memory — and it explains why Samsung Memory and SK Hynix both show meaningfully improved profitability through 2024–2025 despite commodity DRAM/NAND being range-bound.

**AI accelerators depreciate in price fast, and faster with each generation.** H100 street pricing fell 43% over its first 38 months on the market (from ~$34,200 to ~$19,300); B200, launched 19 months later, has already fallen 28% in its first 19 months — a steeper initial decline than H100 saw over the same window. For any buyer, this argues strongly against long-lead-time bulk purchasing at launch pricing; for any seller, it argues for aggressive, front-loaded revenue recognition before the next generation cannibalizes the current one.

![AI GPU price decay since launch](chart6_gpu_price_decay.png)

## 5. The Taiwan concentration risk — real, but being actively diversified

At the leading edge (≤5nm logic), Taiwan's share of global capacity has fallen from **64.7% in 2021 to 49.9% in 2026** — not because Taiwan is producing less (TSMC's leading-edge capacity has grown 5.7x over that window), but because Korea (Samsung) and, since 2025, the United States (TSMC Arizona) have been adding capacity faster off a smaller base.

![Leading-edge fab capacity by geography](chart7_fab_capacity_geo.png)

This matters for anyone doing supply-chain risk assessment: the often-repeated "90% of advanced chips come from one earthquake-and-conflict-exposed island" framing is becoming dated. It was closer to true in 2021; by 2026 the concentration is meaningfully — if not yet fully — diversified, with real US capacity (108,922 wafers/month) coming online for the first time in the dataset's history.

## 6. Who actually spends on what: a capital intensity map

Plotting R&D spend and capex, both as a share of revenue, against company segment produces a clean structural picture: **foundries and memory IDMs are capex-heavy** (TSMC and Samsung Foundry spend ~45% of revenue on capex; the memory majors spend ~35%), while **fabless design houses are R&D-heavy and capex-light** (NVIDIA spends 25% of revenue on R&D but only 2% on capex — it owns no fabs).

![Capital intensity map, 2026](chart8_capital_intensity.png)

Intel is the outlier that stands out immediately on this chart: it carries foundry-level capex intensity (25% of revenue) *and* a middling R&D intensity (20%), without earning foundry-level margins (its 12.5% operating margin is roughly a third of TSMC's 37.6%) or fabless-level margins (NVIDIA's 61.9%). Structurally, Intel is paying the capital cost of a foundry and the R&D cost of a chip designer, while capturing the returns of neither — a direct, data-grounded explanation for why the company's 2024 decision to formally split IDM (design) and Foundry (manufacturing) operations makes strategic sense.

## Synthesis: five things this data says about where the industry is headed

1. **The AI compute buildout is real and still accelerating, but topline "revenue" numbers for 2026 understate demand** because most datasets (this one included) simply haven't caught up to a fast-moving present — a caveat any analyst working with real-time industry data needs to build into their models by default.
2. **Export controls are a timing tool, not a containment tool.** They inflict genuine multi-year pain on individual firms but have not stopped China's semiconductor sector from reaching a larger scale than before the controls began.
3. **Specialization has beaten integration.** TSMC (pure foundry) and NVIDIA (pure design) have both dramatically outgrown Intel (integrated), and Intel's own capital structure explains why.
4. **AI has structurally repriced advanced memory** (HBM) even while commodity memory (DRAM/NAND) continues its old boom-bust pattern — these are now two different markets wearing the same "memory chip" label.
5. **Geographic concentration risk in leading-edge manufacturing is real but decreasing**, not static — a risk register built in 2021 needs updating with 2026 capacity data, not last cited five years ago.

## Methodology & limitations

- All figures are drawn directly from the five supplied datasets (`ai_chip_market.csv`, `chip_companies_financials.csv`, `chip_prices.csv`, `export_controls.csv`, `fab_capacity.csv`); no external data was introduced.
- Revenue figures in `ai_chip_market.csv` are shipment-volume-based estimates (units × ASP), which will differ from companies' own reported segment revenue (visible when comparing NVIDIA's $57.4bn in modeled 2026 AI-chip revenue against $250.7bn in total reported 2026 company revenue in the financials table — the gap reflects NVIDIA's networking, gaming, and automotive segments, plus the partial-year effect noted above).
- The "foundry war" comparison in this report deliberately does not chart Intel's *total* revenue against TSMC's *foundry-only* revenue as a like-for-like foundry comparison, since Intel's financials in this dataset represent its whole IDM business (chip design and sales included), not a standalone foundry unit — conflating the two would overstate Intel's foundry competitiveness.
- Fab capacity figures cover the companies and process nodes included in the dataset; they are a representative, not exhaustive, sample of global capacity.

## Sources

- All data: user-supplied Kaggle dataset, "Global Semiconductor Industry 2010–2026" (`ai_chip_market.csv`, `chip_companies_financials.csv`, `chip_prices.csv`, `export_controls.csv`, `fab_capacity.csv`)

---
*Full analysis code, all eight charts, and the underlying computations are available in this repository.*
