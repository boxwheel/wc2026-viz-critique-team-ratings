# Critique · 09 — Elo vs FIFA rank scatter

Reviewed node: `1abdb79d-5fec-5265-8cc2-895791af4784` ("viz · Elo vs FIFA rank scatter — where the two rating systems disagree")
Author's self-score (mean 4.4): M5 H5 C4 A4 N4.

## Independent re-score

| Axis      | Re-score | Justification |
|-----------|:--------:|--------------|
| Meaning   | 4 | Residual story is real (Argentina/Brazil over-Elo, AFC/CAF mid-pack under-Elo); but the "disagrees most around ranks 20–60" claim isn't visually obvious from the rendered chart — residuals look roughly even across rank. |
| Honesty   | 4 | Log axis labelled; `polyfit(log(rank), elo, 1)` disclosed; line clearly identified as a trend, not truth. The "20–60 disagreement" claim slightly overstates what the chart shows. |
| Clarity   | **3** | "Curaçao" and "New Zealand" labels collide at the bottom-right (rank ~90+); the bottom-right cluster of dots is partly unreadable; trend-line entry in the legend is small. |
| Aesthetic | 4 | Well composed; could use kit colors for the top-6 labelled teams. |
| Novelty   | 4 | Residual-driven editorial framing is fresh. |

**Mean: 3.8** (vs self 4.4). Solid chart with two fixable polish gaps.

## Proposals

### IMPROVE — fix label collisions with `adjustText`
**Why:** Curaçao and New Zealand labels overlap; same for some mid-rank labels. Apply `adjustText.adjust_text(...)` to push labels off each other with leader lines. Same data, label readability fix.
**Library:** `pip install adjustText`; call with `arrowprops=dict(arrowstyle='-', color='grey')`.

### IMPROVE — add a residual side-panel
**Why:** the "who's Elo-rich vs FIFA-rich" reading currently requires the viewer to mentally subtract the trend. A right-side strip of `residual = elo - predicted_elo` (one bar per team, ordered by residual) makes the disagreement instantly legible.
**Library:** `seaborn.JointGrid` or matplotlib `make_axes_locatable` to attach a side axes.

### PIVOT — dumbbell of "FIFA rank" vs "Elo-implied rank"
**Why:** convert the two systems to comparable rank space (1–48). For each team, draw a horizontal segment from its FIFA rank to its Elo-implied rank; the LENGTH = disagreement. Sort teams by |delta| descending. Same dataset; very direct.
**Data:** `teams.csv`; `elo_rank = elo.rank(ascending=False)`.
**Library:** matplotlib `hlines` + two `scatter` calls.

### COMBINE — bond with `61855135` (Elo trajectory) into a "pre-vs-mid tournament" panel
**Why:** this scatter is a pre-tournament snapshot; the trajectory tells us who's being corrected. Add a third axis (color or marker) = "Elo change so far this tournament" — reveals "Argentina was Elo-rich AND keeping pace" vs "Germany was Elo-rich AND being corrected hard".
**Library:** matplotlib scatter with `c=elo_change_so_far` colormap; or two side-by-side scatters.
