# Critique · 02 — Confederation strength box+swarm

Reviewed node: `ca4630bd-9af5-5e1d-90bb-4c2982bc05f6` ("viz · Confederation strength — UEFA owns both the median and the spread")
Author's self-score (mean 4.4): M5 H5 C4 A4 N4.

## Independent re-score

| Axis      | Re-score | Justification |
|-----------|:--------:|--------------|
| Meaning   | 4 | Three numbers per group (median, IQR, n) + raw swarm is genuinely useful; "UEFA spread is widest" is well-supported by the chart. |
| Honesty   | **3** | The TITLE "UEFA owns both the median and the spread" is **wrong on median** — the chart is ordered CONMEBOL→UEFA→CONCACAF→CAF→AFC→OFC by median descending, with CONMEBOL at **1975 vs UEFA at 1900**. The chart contradicts its own headline. The encoding itself is honest; the editorial label is not. |
| Clarity   | 4 | n=N and med=M footer per column + top-team annotation is well done; the OFC column visibly reads as "tiny" which is a clarity win. |
| Aesthetic | 4 | House palette holds together; box widths slightly chunky. |
| Novelty   | 4 | Box+swarm + per-column stamp + named-outlier label is uncommon in WC media. |

**Mean: 3.8** (self-claim 4.4). The headline is the only real flaw; otherwise it's a strong chart.

## Proposals

### IMPROVE — fix the title to match the chart
**Why:** retitle as "**UEFA owns the spread, CONMEBOL the top median**" (or just "Confederations stack by median, UEFA stretches widest"). The current title is the chart's biggest credibility leak.
**Effort:** one-line text edit.

### PIVOT — violin + sina-plot instead of box
**Why:** the box hides distribution shape. CONCACAF shows a barbell (USA/Mexico high, Curaçao/Haiti low) that a box flattens to "wide IQR"; a violin makes the bimodality visible. Pair with an internal swarm so the n is still readable.
**Data:** unchanged (`teams.csv`).
**Library:** `seaborn.violinplot(inner=None)` + `swarmplot` overlay; same ordering by median.

### COMBINE — splice with `260ef4c9` (Confederation bubble)
**Why:** box+swarm gives spread; bubble gives `(median Elo, median value, n)`. A two-panel: top = box+swarm of Elo, bottom = bubble plane — covers "spread of strength" AND "where each confederation sits on money×strength" in one editorial unit.
**Library:** matplotlib `GridSpec(2, 1, height_ratios=[1.0, 1.2])`.

### EXTEND — confederation × actual-tournament W/D/L stacked bars
**Why:** the box+swarm shows **pre-tournament** strength. Add a companion showing **outcome** — for each confederation, the count of wins/draws/losses its teams produced this tournament. Reveals whether CONMEBOL's high median actually translated to results.
**Data:** `matches_detailed.csv` filtered to `status=='Completed'`; join each side's `confederation` from `teams.csv`; tally W/D/L per confederation.
**Library:** seaborn `barplot` with `hue='outcome'`, stacked; same x-order as the box plot for visual symmetry.
