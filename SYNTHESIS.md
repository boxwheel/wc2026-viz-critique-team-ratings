# SYNTHESIS — Top-5 highest-leverage build proposals for Wave-2 (Team-Ratings)

A queue across the 10 reviewed Team-Ratings leaves. Selected for **leverage** (one move that lifts multiple nodes or opens new territory), not for ease.

Family-mean re-score: **3.78** (self-claim ≈ 4.36) — the wave's biggest gaps are in **honesty** (5 of 10 nodes have a title or aggregation that overstates the chart) and **clarity** (label / legend placement). Charts themselves are largely sound; their editorial frames over-claim.

---

## #1 · Title-and-honesty quick-fix bundle (covers feedbacks 01 / 02 / 03)
**Why first:** cheapest move, biggest honesty payoff. Three text edits eliminate the wave's three biggest credibility leaks.
- `d0a17b1b` — "300× gap" → **"76× gap"** (€1.523B / €20M ≈ 76, not 300).
- `ca4630bd` — "UEFA owns both the median and the spread" → **"UEFA owns the spread, CONMEBOL the top median"** (the chart itself orders CONMEBOL > UEFA by median).
- `b5bf38e2` — replace the central piled-up median lines with right-side inline stats per ridge (`n · median · % over-30`).

One PR; ~30 lines of code changed across three scripts.

## #2 · "Team profile" 4×2 small-multiple gallery (combines feedbacks 05 + 06)
**Why:** single-team pizza (`f4dc31f6`) and two-team radar (`8767b223`) both use the same 8-axis xG_aggregate frame. Looping them into a 4×2 grid over the 8 deep-runners (Arg / Bra / Esp / Fra / Eng / Ger / Por / Ned) turns two single-team charts into **a 16-figure gallery** powered by one transform. Highest reuse-ratio in the queue.
- Data: existing `transforms/xg_aggregate.py` frame; percentile-rank for the pizza row.
- Output: two figures — `team_pizza_grid_4x2.png` and `team_radar_grid_4x2.png` — on identical axes/ranges.
- Library: mplsoccer `PyPizza` + `Radar`, `subplots(2, 4)`.

## #3 · Bang-per-buck Elo residual (extends feedbacks 01 + 08 + 09)
**Why:** introduces a single new derived metric — `elo_residual = elo − predicted_elo(log10(squad_value_eur))` over 48 teams — that can be visualised three different ways the existing gallery doesn't cover, and it surfaces a story the lollipop and bubble currently *hint at* but never *name*: who buys the most Elo per €?
- Primary view: horizontal bar of 48 teams sorted by residual, two-color (positive green / negative red), confederation tick on the y-axis.
- Bonus views (free if the residual exists): overlay as **color** on `1abdb79d` (Elo-vs-FIFA scatter) and as **per-team ghost-dot color** on the redesigned confederation bubble.
- Data: join `teams.csv` × aggregated `squads_and_players.csv`; fit `np.polyfit(log10(value_eur), elo, 1)`.
- Library: matplotlib barh + `cmap` colored scatter.

## #4 · Elo trajectory — facet to 4×2 + K-sensitivity 3-panel (feedback 07)
**Why:** `61855135` is the weakest scored node in the assigned subset (3.4). Two cheap moves take it to gallery-grade: (a) replace the single overplotted spaghetti with a 4×2 small-multiple (one team per panel, shared y, no label collisions), and (b) add a 3-panel K-sensitivity (K=40 / 60 / 80) showing whether "Germany shed Elo" survives parameter perturbation. Same `transforms/elo_timeline.py` parameterised. Calendar-date x-axis (`matches_detailed.csv:date`) on top of the facet.
- Library: matplotlib `subplots(2, 4)` + `subplots(1, 3, sharey=True)`.

## #5 · Possession vs xG — outcome-split LOWESS + residual top/bottom-10 (feedback 10)
**Why:** `8a79b86d`'s single gold trend collapses an interaction. Splitting it into three LOWESS curves (W / D / L) reveals "wins climb steeply with possession; **losses cap around 1.2 xG no matter the possession**" — a much sharper editorial than "modest positive slope". Add a follow-on residual chart (`xG − LOWESS(possession)` ranked) that names "most efficient with the ball" vs "wasted possession" — the names the current scatter wants to surface but doesn't.
- Data: existing team-match frame; `statsmodels.nonparametric.lowess`.
- Library: matplotlib + statsmodels LOWESS x 3; barh for the residual ranking.

---

## Re-score gap heat-map (per-axis re-score minus self-score)

| node | M | H | C | A | N | mean Δ |
|------|:-:|:-:|:-:|:-:|:-:|:-:|
| d0a17b1b lollipop | 0 | **−2** | −1 | 0 | 0 | −0.6 |
| ca4630bd conf-box | −1 | **−2** | 0 | 0 | 0 | −0.6 |
| b5bf38e2 age-ridge | **−2** | **−2** | −1 | −1 | 0 | −1.2 |
| fcadfab6 value-ridge | 0 | 0 | +1 | +1 | 0 | +0.4 |
| 8767b223 radar | −1 | −1 | −1 | 0 | −1 | −0.8 |
| f4dc31f6 pizza | −1 | −1 | 0 | 0 | 0 | −0.4 |
| 61855135 elo-traj | 0 | −1 | −1 | −1 | 0 | −0.6 |
| 260ef4c9 conf-bubble | −1 | −1 | 0 | 0 | 0 | −0.4 |
| 1abdb79d elo-vs-rank | −1 | −1 | −1 | 0 | 0 | −0.6 |
| 8a79b86d poss-vs-xG | 0 | −1 | −1 | 0 | 0 | −0.4 |

The **Honesty** column shows the wave's structural failure: 7 of 10 nodes get re-scored lower on honesty, mostly because the body text accurately states the limitation but the **title/headline** still over-claims.

---

## Cross-cutting COMBINE proposals (spanning the full 29-node graph)

For follow-on waves that want to weave across clusters:

- **`61855135` Elo trajectory** × **`7a22b8a3` upset map** × **`4e7860c0` Elo-vs-points scatter** → "Where did Elo predict wrong, and is it correcting?" feedback-loop chart. Pre-tournament Elo as anchor, Elo-vs-points as outcome, trajectory as the correction in progress.
- **`fcadfab6` Player-value ridgeline** × **`5b1371e5` Confederation→team treemap of goals** → "Does money buy goals?" Sankey-style flow from squad value to actual goals scored, per confederation. Tests whether the "three economies" story shows up on the scoreboard.
- **`260ef4c9` Confederation bubble** × **`d0a17b1b` Squad-value lollipop** × **`ca4630bd` Confederation box+swarm** → 3-panel "money × strength × depth" overview (also proposal #4 in feedback 01 — included here as cross-cluster anchor).
