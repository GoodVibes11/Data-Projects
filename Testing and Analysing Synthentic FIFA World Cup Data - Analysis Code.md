import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.ticker as mticker

plt.rcParams['font.family'] = 'DejaVu Sans'

NAVY = "#1F3864"
GREEN = "#2E7D32"
ORANGE = "#C77800"
PURPLE = "#8E44AD"
RED = "#B03A2E"
TEAL = "#117A8B"
GREY = "#6B6B6B"
GOLD = "#B8860B"

OUT = "/home/claude/fifa_analysis/"

df = pd.read_csv('/mnt/user-data/uploads/fifa_world_cup_2026_player_performance.csv')
players = pd.read_csv(OUT + 'players_aggregated.csv')
outfield = pd.read_csv(OUT + 'outfield_clustered.csv')

# ============================================================
# CHART 1: Data integrity check - matches played per team (illustrating the impossible structure)
# ============================================================
matches_per_team = df.groupby('team')['match_id'].nunique().sort_values(ascending=False)
fig, ax = plt.subplots(figsize=(10, 6))
colors_bar = [RED if v > 20 else NAVY for v in matches_per_team.values]
ax.bar(range(len(matches_per_team)), matches_per_team.values, color=colors_bar, width=0.75)
ax.axhline(7, color=GREEN, linestyle="--", linewidth=1.6, label="Max possible in a real 48-team WC (7 matches)")
ax.set_xticks(range(len(matches_per_team)))
ax.set_xticklabels(matches_per_team.index, rotation=90, fontsize=7)
ax.set_ylabel("Match-records in dataset", fontsize=10.5)
ax.set_title("Data Integrity Check: Matches per Team, Actual vs. Physically Possible", fontsize=12.5, fontweight="bold", pad=14)
ax.legend(frameon=False, fontsize=9.5, loc="upper right")
ax.spines[["top", "right"]].set_visible(False)
ax.grid(axis="y", linestyle="--", alpha=0.3)
plt.tight_layout()
plt.savefig(OUT + "chart1_data_integrity.png", dpi=200, bbox_inches="tight")
plt.close()

# ============================================================
# CHART 2: Position benchmarks - key stats grouped bar (normalized)
# ============================================================
pos_stats = players.groupby('position')[['goals','assists','key_passes','tackles','interceptions','pass_accuracy']].mean()
pos_stats = pos_stats.loc[['Goalkeeper','Defender','Midfielder','Forward']]

fig, axes = plt.subplots(1, 3, figsize=(13, 4.8))
metrics_groups = [
    (['goals','assists'], "Attacking output (per match)", axes[0]),
    (['tackles','interceptions'], "Defensive actions (per match)", axes[1]),
    (['key_passes'], "Creativity (key passes/match)", axes[2]),
]
colors_metric = [NAVY, ORANGE]
for metrics, title, ax in metrics_groups:
    x = np.arange(len(pos_stats))
    width = 0.35
    for i, m in enumerate(metrics):
        ax.bar(x + i*width - (width*(len(metrics)-1)/2), pos_stats[m], width=width, label=m.replace('_',' ').title(), color=colors_metric[i % 2])
    ax.set_xticks(x)
    ax.set_xticklabels(pos_stats.index, fontsize=9, rotation=15)
    ax.set_title(title, fontsize=10.8, fontweight="bold")
    ax.legend(frameon=False, fontsize=8)
    ax.spines[["top", "right"]].set_visible(False)
    ax.grid(axis="y", linestyle="--", alpha=0.3)

fig.suptitle("Position Benchmarks: What Each Role Actually Does on the Pitch", fontsize=13.5, fontweight="bold", y=1.03)
plt.tight_layout()
plt.savefig(OUT + "chart2_position_benchmarks.png", dpi=200, bbox_inches="tight")
plt.close()

# ============================================================
# CHART 3: Finishing quality - Goals vs xG scatter (forwards & midfielders, min appearances)
# ============================================================
attackers = players[(players.position.isin(['Forward','Midfielder'])) & (players.total_goals_tournament >= 1)].copy()

fig, ax = plt.subplots(figsize=(9.5, 8))
sizes = attackers['total_goals_tournament'] * 12 + 20
xg_sum = df.groupby('player_id')['expected_goals_xg'].sum()
attackers['total_xg_check'] = attackers['player_id'].map(xg_sum)
ax.scatter(attackers['total_xg_check'], attackers['total_goals_tournament'], s=sizes, alpha=0.55, color=NAVY, edgecolor='white', linewidth=0.6)
lims = [0, max(attackers['total_xg_check'].max(), attackers['total_goals_tournament'].max()) + 1]
ax.plot(lims, lims, linestyle='--', color=GREY, linewidth=1.3, label="Goals = xG (expected finishing)")

# Annotate top over/under performers
top_over = attackers.assign(diff=attackers.total_goals_tournament - attackers.total_xg_check).sort_values('diff', ascending=False).head(4)
top_under = attackers.assign(diff=attackers.total_goals_tournament - attackers.total_xg_check).sort_values('diff').head(3)
for _, row in top_over.iterrows():
    ax.annotate(row['player_name'], (row['total_xg_check'], row['total_goals_tournament']), xytext=(6,4), textcoords='offset points', fontsize=8, fontweight='bold', color=GREEN)
for _, row in top_under.iterrows():
    ax.annotate(row['player_name'], (row['total_xg_check'], row['total_goals_tournament']), xytext=(6,-10), textcoords='offset points', fontsize=8, fontweight='bold', color=RED)

ax.set_xlabel("Total expected goals (xG), tournament", fontsize=10.5)
ax.set_ylabel("Total actual goals, tournament", fontsize=10.5)
ax.set_title("Finishing Quality: Actual Goals vs. Expected Goals (xG)", fontsize=13, fontweight="bold", pad=14)
ax.legend(frameon=False, fontsize=9.5, loc="upper left")
ax.spines[["top", "right"]].set_visible(False)
ax.grid(linestyle="--", alpha=0.3)
plt.tight_layout()
plt.savefig(OUT + "chart3_xg_vs_goals.png", dpi=200, bbox_inches="tight")
plt.close()

# ============================================================
# CHART 4: Age vs rating curve
# ============================================================
players['age_bucket'] = pd.cut(players['age'], bins=[16,20,23,26,29,32,40], labels=['17-20','21-23','24-26','27-29','30-32','33+'])
age_curve = players.groupby('age_bucket', observed=True).agg(mean_rating=('tournament_rating','mean'), n=('tournament_rating','count'))

fig, ax = plt.subplots(figsize=(9.5, 5.5))
ax2 = ax.twinx()
ax2.bar(age_curve.index.astype(str), age_curve['n'], color="#D9E2EC", width=0.6, zorder=1)
ax2.set_ylabel("Number of players", fontsize=10, color=GREY)
ax2.tick_params(axis='y', labelcolor=GREY)

ax.plot(age_curve.index.astype(str), age_curve['mean_rating'], marker='o', markersize=8, color=NAVY, linewidth=2.4, zorder=3)
for i, v in enumerate(age_curve['mean_rating']):
    ax.text(i, v + 0.03, f"{v:.2f}", ha='center', fontsize=9, fontweight='bold', color=NAVY)
ax.set_ylabel("Avg tournament rating", fontsize=10.5, color=NAVY)
ax.tick_params(axis='y', labelcolor=NAVY)
ax.set_ylim(7.2, 7.9)
ax.set_title("Does Age Predict Performance? A Nearly Flat Curve", fontsize=13, fontweight="bold", pad=14)
ax.set_zorder(ax2.get_zorder()+1)
ax.patch.set_visible(False)
ax.spines[["top"]].set_visible(False)
ax2.spines[["top"]].set_visible(False)
plt.tight_layout()
plt.savefig(OUT + "chart4_age_curve.png", dpi=200, bbox_inches="tight")
plt.close()

# ============================================================
# CHART 5: Market value vs tournament rating (value-for-money quadrant)
# ============================================================
fig, ax = plt.subplots(figsize=(10, 7.5))
med_value = players['market_value_eur'].median()
med_rating = players['tournament_rating'].median()

colors_pos = {'Forward': GREEN, 'Midfielder': NAVY, 'Defender': ORANGE, 'Goalkeeper': PURPLE}
for pos, color in colors_pos.items():
    sub = players[players.position == pos]
    ax.scatter(sub['market_value_eur']/1e6, sub['tournament_rating'], s=35, alpha=0.55, color=color, label=pos, edgecolor='white', linewidth=0.4)

ax.axvline(med_value/1e6, color=GREY, linestyle=":", linewidth=1.2)
ax.axhline(med_rating, color=GREY, linestyle=":", linewidth=1.2)
ax.set_xscale('log')
ax.set_xlabel("Market value (\u20ac millions, log scale)", fontsize=10.5)
ax.set_ylabel("Tournament rating", fontsize=10.5)
ax.set_title("Value for Money: Market Value vs. Tournament Rating (r = 0.56)", fontsize=13, fontweight="bold", pad=14)
ax.text(0.02, 0.97, "OVERPERFORMERS\n(high rating, low value)", transform=ax.transAxes, fontsize=9, color=GREEN, fontweight='bold', va='top', style='italic')
ax.text(0.98, 0.03, "UNDERPERFORMERS\n(low rating, high value)", transform=ax.transAxes, fontsize=9, color=RED, fontweight='bold', va='bottom', ha='right', style='italic')
ax.legend(frameon=False, fontsize=9.5, loc="lower right", bbox_to_anchor=(1,0.12))
ax.spines[["top", "right"]].set_visible(False)
ax.grid(linestyle="--", alpha=0.25)
plt.tight_layout()
plt.savefig(OUT + "chart5_value_for_money.png", dpi=200, bbox_inches="tight")
plt.close()

# ============================================================
# CHART 6: The "two ratings" problem - feature importance comparison
# ============================================================
feat_cols = ['goals','assists','shots','key_passes','pass_accuracy','successful_dribbles',
             'tackles','interceptions','recoveries','distance_covered_km','sprint_distance_km',
             'top_speed_kmh','possession_impact','pressure_resistance','consistency_score',
             'creativity_score','market_value_eur']

outfield_all = players[players.position != 'Goalkeeper']
corr_pr = outfield_all[feat_cols + ['player_rating']].corr()['player_rating'].drop('player_rating')
corr_tr = outfield_all[feat_cols + ['tournament_rating']].corr()['tournament_rating'].drop('tournament_rating')

comp = pd.DataFrame({'player_rating (per-match)': corr_pr, 'tournament_rating (season)': corr_tr})
comp['abs_max'] = comp.abs().max(axis=1)
comp = comp.sort_values('abs_max', ascending=True).drop(columns='abs_max')

fig, ax = plt.subplots(figsize=(9.5, 8))
y = np.arange(len(comp))
ax.barh(y + 0.19, comp['player_rating (per-match)'], height=0.36, color=RED, label='Correlation with player_rating (per-match)')
ax.barh(y - 0.19, comp['tournament_rating (season)'], height=0.36, color=NAVY, label='Correlation with tournament_rating (season)')
ax.set_yticks(y)
ax.set_yticklabels([c.replace('_',' ').title() for c in comp.index], fontsize=9.5)
ax.axvline(0, color='black', linewidth=0.8)
ax.set_xlabel("Correlation coefficient", fontsize=10.5)
ax.set_title("The Two-Ratings Problem: Same Player, Two Incompatible Scores", fontsize=12.8, fontweight="bold", pad=14)
ax.legend(frameon=False, fontsize=9.5, loc="lower right")
ax.spines[["top", "right"]].set_visible(False)
ax.grid(axis="x", linestyle="--", alpha=0.3)
plt.tight_layout()
plt.savefig(OUT + "chart6_two_ratings_problem.png", dpi=200, bbox_inches="tight")
plt.close()

# ============================================================
# CHART 7: Player archetype clusters (PCA scatter)
# ============================================================
cluster_names = {
    0: "Defensive Anchors",
    1: "Rotation / Low-Output (Def & Mid)",
    2: "Deep-Lying Playmakers",
    3: "Elite Finishers",
    4: "Support Forwards",
}
outfield['archetype'] = outfield['cluster'].map(cluster_names)
colors_cluster = {v: c for v, c in zip(cluster_names.values(), [ORANGE, GREY, NAVY, RED, GREEN])}

fig, ax = plt.subplots(figsize=(10, 7.5))
for name, color in colors_cluster.items():
    sub = outfield[outfield.archetype == name]
    ax.scatter(sub['pca1'], sub['pca2'], s=28, alpha=0.6, color=color, label=f"{name} (n={len(sub)})", edgecolor='white', linewidth=0.3)

ax.set_xlabel("Principal component 1 (44.6% of variance)", fontsize=10.5)
ax.set_ylabel("Principal component 2 (27.0% of variance)", fontsize=10.5)
ax.set_title("Player Archetypes: Unsupervised Clustering on Observed Match Actions", fontsize=13, fontweight="bold", pad=14)
ax.legend(frameon=False, fontsize=9, loc="best")
ax.spines[["top", "right"]].set_visible(False)
ax.grid(linestyle="--", alpha=0.25)
plt.tight_layout()
plt.savefig(OUT + "chart7_player_archetypes.png", dpi=200, bbox_inches="tight")
plt.close()

# ============================================================
# CHART 8: Physical demands by position (small multiples)
# ============================================================
phys = players.groupby('position')[['distance_covered_km','sprint_distance_km','top_speed_kmh']].mean()
phys = phys.loc[['Goalkeeper','Defender','Midfielder','Forward']]

fig, axes = plt.subplots(1, 3, figsize=(12.5, 4.5))
titles = ["Distance covered (km/match)", "Sprint distance (km/match)", "Top speed (km/h)"]
cols = ['distance_covered_km','sprint_distance_km','top_speed_kmh']
colors_phys = [NAVY, ORANGE, GREEN]
for ax, col, title, color in zip(axes, cols, titles, colors_phys):
    ax.bar(phys.index, phys[col], color=color, width=0.6, alpha=0.85)
    ax.set_title(title, fontsize=10.8, fontweight="bold")
    ax.tick_params(axis='x', rotation=20, labelsize=8.5)
    ax.spines[["top", "right"]].set_visible(False)
    ax.grid(axis="y", linestyle="--", alpha=0.3)
    for i, v in enumerate(phys[col]):
        ax.text(i, v + phys[col].max()*0.02, f"{v:.1f}", ha='center', fontsize=8.5)

fig.suptitle("Physical Demands by Position", fontsize=13.5, fontweight="bold", y=1.03)
plt.tight_layout()
plt.savefig(OUT + "chart8_physical_demands.png", dpi=200, bbox_inches="tight")
plt.close()

print("All 8 charts generated.")
