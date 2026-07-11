# The Renewable-Share Paradox: What Nigeria's Energy Data Means for Sustainability Strategy

**A data analysis by Emeka Richard Ogochukwu**

## Why this analysis

Nigeria is routinely described in energy statistics as having one of the highest "renewable energy" shares in the world — over 80% of final energy consumption, according to World Bank data. On its face, that sounds like a sustainability success story. It isn't. This analysis uses four publicly available World Bank indicators to show why that headline number is misleading, and what it actually implies for any company — particularly international consumer goods companies — building a sustainability or market-entry strategy around Nigeria.

This builds on questions I explored in both of my theses: how international consumer goods companies frame sustainability strategy in Nigeria (BBA, Karelia UAS), and how entrepreneurship and sustainable practices adoption interact in Nigeria's economic transition (MSc, University of Jyväskylä). This piece takes a narrower, data-first slice of that same question.

## The data

I compiled three World Bank World Development Indicators for Nigeria and three peer markets (Kenya, Ghana, South Africa), sourced via the World Bank's public data via TradingEconomics and CEIC:

| Country | Renewable share of final energy (2021) | Access to electricity (2023) | Access to clean cooking fuels (latest available) |
|---|---|---|---|
| Nigeria | 80.3% | 61.2% | 15.0% (2020) |
| Kenya | 67.7% | 76.2% | 30.0% (2022) |
| Ghana | 39.0% | 89.5% | 21.7% (2016)* |
| South Africa | 9.7% | 87.7% | 89.4% (2022) |

\*Ghana's clean-cooking figure is the most recent comparable data point available; it is several years older than the other three and should be read with that caveat.

*(Full dataset: `energy_data.csv` in this repository)*

## The finding

The chart below shows all three indicators side by side. Nigeria has the **highest** renewable energy share of the four countries — and the **lowest** access to clean cooking fuels.

<img width="1780" height="1072" alt="image" src="https://github.com/user-attachments/assets/25258b66-cbea-41a9-9f18-832e1e9bcffc" />


That's not a contradiction — it's a definitional artifact. The World Bank's "renewable energy consumption" indicator counts **traditional biomass** (firewood, charcoal, crop waste burned for cooking) as renewable. In Nigeria, biomass dominates household energy use precisely *because* modern, clean alternatives — LPG, electricity, biogas — haven't reached most of the population. South Africa shows the mirror image: a fossil-fuel-heavy, coal-based national grid gives it the *lowest* "renewable" score of the four, but the *highest* access to clean cooking fuels and electricity, because its energy system is modern and grid-delivered rather than biomass-based.

<img width="1480" height="1178" alt="image" src="https://github.com/user-attachments/assets/992719d7-0038-4e14-a9de-bd40aca22384" />


Kenya and Ghana sit in between, and in a genuinely interesting way: Kenya's higher renewable share is increasingly driven by real investment in geothermal, wind, and hydropower generation (not just biomass), while Ghana's lower renewable share reflects a more gas-dependent grid with comparatively better electrification.

## Why this matters for strategy, not just statistics

For a market research or corporate strategy analyst advising a consumer goods company on Nigeria, the practical implications are direct:

- **Don't cite Nigeria's "80% renewable" figure as a sustainability tailwind.** It signals energy poverty, not clean infrastructure. Using it uncritically in a market-entry pitch or ESG narrative would misread the market to a well-informed stakeholder.
- **Cold-chain and manufacturing dependability tracks grid access, not the renewable-share number.** At 61% electrification, large parts of Nigeria still can't reliably support refrigeration, retail point-of-sale systems, or continuous manufacturing — a real constraint on distribution strategy for perishable or temperature-sensitive goods.
- **The clean cooking gap (15%) is itself a market signal.** Companies selling into the household energy, appliance, or FMCG-adjacent space have a large, underserved population to design for — this is closer to a market-sizing insight than a sustainability metric.

## Limitations

This is a compact, four-country snapshot built from three publicly available indicators, not a comprehensive market study. The clean cooking figures are not all from the same year, which limits precise year-over-year comparison (flagged above). A fuller version of this analysis would bring in urban/rural splits, income-quintile breakdowns (which the World Bank's SDG7 tracking reports do provide), and time-series trends rather than single-year snapshots.

## Sources

- World Bank, World Development Indicators — Renewable energy consumption (% of total final energy consumption), Access to electricity (% of population), Access to clean fuels and technologies for cooking (% of population), accessed via [TradingEconomics](https://tradingeconomics.com) and [CEIC](https://www.ceicdata.com)
- International Energy Agency (IEA) — underlying methodology for the renewable energy consumption series

---
*Data, charts, and full source file available in this repository. Built as part of an independent portfolio project.*
