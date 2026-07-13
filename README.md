# Beyond the Box Score: A Player Performance Analysis of the FIFA World Cup 2026 Dataset

**A data analysis by Emeka Richard Ogochukwu**

## An important note before anything else

Before building a single chart, I checked whether this dataset's structure was actually consistent with a real FIFA World Cup. It isn't, and I think that finding is itself worth reporting rather than quietly working around.

A 48-team World Cup (the real format used in 2026) plays exactly **104 matches**: 72 in the group stage, then 16-8-4-2-1-1 through the knockout rounds. No team can play more than 7 matches. This dataset contains **1,050 match-records**, and individual teams — Qatar, Jamaica, Morocco among them — appear in as many as **66 matches**, including dozens of "Group Stage" fixtures scattered across late July, well after the real group stage would have concluded, and multiple "Final" appearances by the same team. Player names are also procedurally generated (e.g. "Rodri Fati," "Ansu Le Normand" — first and last names recombined from a pool of real players).

**This is a synthetic, randomly generated dataset** — well-suited for practicing sports analytics and ML techniques, which is exactly how I've treated it, but not a record of real matches or real players. Every finding below describes patterns in this simulated dataset, not real-world World Cup outcomes. I want to be explicit about that rather than let a chart with real team names imply otherwise.

That distinction doesn't make the dataset useless — quite the opposite. Structured, realistic-looking sports data is exactly what's used to practice the full analytics pipeline, and (as Section 5 shows) working out *why* a model performs suspiciously well is itself a useful, transferable skill.

<img width="1978" height="1178" alt="image" src="https://github.com/user-attachments/assets/6081ac9a-8614-435c-ade0-822cc915a1f7" />


## 1. Position benchmarks: the numbers tell the right story, even in simulation

Despite the structural issue above, the per-position statistical patterns are sensible and internally consistent — a good sign that the underlying player-attribute generation, at least, followed realistic rules. Forwards average the most goals and shots per match; midfielders lead in key passes; defenders lead comfortably in tackles and interceptions; goalkeepers are correctly near-zero on outfield attacking/defensive actions.

<img width="2580" height="996" alt="image" src="https://github.com/user-attachments/assets/3c2f2aad-4698-4465-9bf8-b2652aae4e1d" />


## 2. Finishing quality: goals vs. expected goals (xG)

Plotting actual tournament goals against total expected goals (xG) per player is a standard football-analytics technique for separating clinical finishers from those generating (or wasting) high-quality chances.

<img width="1878" height="1576" alt="image" src="https://github.com/user-attachments/assets/c36acdbe-2d51-4f68-8a24-b5465cb9e601" />


Two things stand out. First, several players — led by Declan Rice, Saka and Pedro Cubarsi in this simulated data — score far more than their xG would predict, the signature of an efficient finisher (or, in reality, a hot streak / small-sample variance). Second, and more analytically important: **almost the entire player population sits above the "goals = xG" line.** In real football data, players cluster fairly evenly around that diagonal, since xG models are calibrated against actual outcomes league-wide. Here, overperformance is the overwhelming norm, not the exception — a second, independent signal (alongside the schedule-structure issue above) that `expected_goals_xg` was generated without being statistically tied to the `goals` field.

## 3. Age and performance: a flatter curve than conventional wisdom suggests

Grouping players into age bands shows tournament rating peaking modestly in the 17–20 bracket (7.73) and declining gently to 7.46 for players 33 and older — but the curve is remarkably flat through a player's twenties and early thirties (7.58–7.65), with a total spread of only 0.27 rating points across the entire adult career span.

<img width="1878" height="1078" alt="image" src="https://github.com/user-attachments/assets/05d7bc23-b50a-424e-bcc9-25075bf42fda" />


For a recruitment or scouting context, the practical read is that **age alone is a weak signal** (correlation with rating: r = –0.04 to –0.15 depending on which rating field is used) — far weaker than most conventional scouting heuristics would assume. Whatever is actually driving performance in this dataset, it isn't age.

## 4. Market value vs. performance: a real but imperfect relationship

Market value correlates with tournament rating at **r = 0.56** (r² = 0.31) — a meaningful relationship, but one that leaves roughly 69% of the variance in performance unexplained by transfer-market price alone.

<img width="1978" height="1469" alt="image" src="https://github.com/user-attachments/assets/2c85cd1d-5e87-4059-99dc-c53e7c26c55a" />


The practically useful part of this chart is the top-left quadrant: players rated highly despite modest market valuations represent the "value" picks a recruitment-analytics team would flag for further scouting, while the bottom-right quadrant (high value, underwhelming rating) is exactly the pattern a data-driven recruitment department would want an early warning on before a big transfer decision.

## 5. The headline finding: two "ratings," two completely different stories

This is the most important result in the analysis, and the one I'd lead with in an interview.

I built two models — a standardized linear regression and a random forest — to find out what actually drives `player_rating` (the per-match score). Both models returned an R² above 0.94 on held-out test data, which is an extraordinarily strong fit for any real-world performance metric, and immediately made me suspicious rather than pleased. Digging into *why*: **`top_speed_kmh` alone explains the overwhelming majority of the variance** (correlation r = 0.88; 76% of the random forest's total feature importance). No real football rating — one meant to reflect all-around match performance — should be driven almost entirely by one physical sprint metric, regardless of position or role.

So I ran the identical exercise against `tournament_rating` (the season-long rating field) — and got a completely different answer. Here, `top_speed_kmh` barely matters (r = 0.058), while `consistency_score`, `pressure_resistance`, and `possession_impact` dominate (r = 0.70, 0.70, and 0.63 respectively). The two rating fields, which you'd expect to broadly agree on who the good players are, **correlate with each other at only r = 0.41.**

<img width="1877" height="1576" alt="image" src="https://github.com/user-attachments/assets/672b8477-63da-47d6-b437-134fa8181f45" />


**The takeaway I'd want a hiring manager to notice:** a suspiciously perfect model isn't a result to celebrate, it's a prompt to investigate. In this case, the investigation revealed that `player_rating` and `tournament_rating` in this dataset appear to have been generated from two disjoint, non-overlapping formulas rather than emerging organically from match events — reinforcing that this is simulated data and, more generally, demonstrating the habit of checking a model's "why" before trusting its "what."

## 6. Player archetypes: unsupervised clustering on observed actions

Rather than cluster on the two suspect rating fields, I ran K-means clustering (k=5, chosen via silhouette score) purely on observable match actions — goals, tackles, passing, physical output — deliberately excluding any pre-computed "rating." The five resulting archetypes are cleanly separated in PCA space and map to recognizable football roles:

<img width="1978" height="1477" alt="image" src="https://github.com/user-attachments/assets/aa5918e8-db7b-4f43-8e1e-4f9963ff87b0" />


- **Defensive Anchors** (n=273): high tackles/interceptions, minimal attacking output — classic centre-backs.
- **Deep-Lying Playmakers** (n=270): high key-passes and recoveries with modest defensive numbers — the double-pivot/regista profile.
- **Elite Finishers** (n=113): the highest goals-per-match and shot volume in the dataset by a clear margin.
- **Support Forwards** (n=175): meaningful attacking output but below the Elite Finishers tier — useful for separating undervalued/breakout strikers from proven ones.
- **Rotation / Low-Output (Def & Mid)** (n=273): below-average output across the board regardless of nominal position — likely squad depth players rather than a genuine tactical role.

This is a legitimate use of unsupervised learning to recover tactical structure from raw action data, independent of whatever the dataset's built-in rating fields are doing.

## 7. Physical demands by position

Distance covered and sprint distance both peak with midfielders (4.5 km and 0.52 km per match respectively) — consistent with their box-to-box role — while goalkeepers are correctly the outlier on every physical metric.

<img width="2479" height="934" alt="image" src="https://github.com/user-attachments/assets/2a3a7a1a-a00c-4abc-82c9-17dee4a40c0e" />


One data-quality note: top speed is nearly identical across defenders, midfielders, and forwards (19.8–19.9 km/h). In real tracking data, wide forwards and full-backs typically clock meaningfully higher top speeds than central defenders — this near-uniformity is a further (minor) signal of the dataset's synthetic generation, consistent with the findings in Sections 2 and 5.

## Conclusion: what this project demonstrates

Setting aside the specific (simulated) numbers, the analytical process here is the deliverable:

1. **Validate structure before trusting content** — checking the schedule against the real tournament format caught the synthetic nature of the data before it could produce a misleading headline.
2. **Distrust suspiciously clean models** — a 0.95 R² was a prompt to dig deeper, not a result to report at face value.
3. **Build unsupervised structure independent of pre-built labels** — clustering on raw actions rather than a suspect "rating" field produced more trustworthy, interpretable archetypes.
4. **Use the right technique for the right question** — correlation for simple relationships, regression for driver analysis, clustering for typology, all applied to the same underlying table.

## Methodology & tools

- Player-match records aggregated to player-tournament level (1,248 unique players across 48 teams) for cross-sectional analysis; raw match-level records used for the data-integrity check.
- **scikit-learn**: StandardScaler, LinearRegression, RandomForestRegressor (300 trees, max depth 6, 80/20 train-test split), KMeans (k selected via silhouette score across k=3–6), PCA for 2D visualization.
- **pandas / matplotlib** for data wrangling and all visualizations.
- Goalkeepers were excluded from the driver-analysis and clustering (Sections 5–6) since their statistical profile is categorically different from outfield players and would dominate any shared model.

## Sources

- Data: user-supplied Kaggle dataset, "FIFA World Cup 2026 Player Performance Dataset" (`fifa_world_cup_2026_player_performance.csv`)

---
*Full analysis code and all eight charts are available in this repository.*
