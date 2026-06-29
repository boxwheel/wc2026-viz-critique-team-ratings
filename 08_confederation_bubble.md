# Critique · 08 — Confederation bubble (strength × money × depth)

Reviewed node: `260ef4c9-7790-5666-8eff-477c2714f17c` ("viz · Confederation bubble — strength, money, and depth on one plane")
Author's self-score (mean 4.4): M5 H4 C5 A4 N4.

## Independent re-score

| Axis      | Re-score | Justification |
|-----------|:--------:|--------------|
| Meaning   | 4 | Six dots is sparse, but the cluster pattern (UEFA upper-right with depth, CONMEBOL upper-right without depth, AFC below the strength-money diagonal) is informative. |
| Honesty   | **3** | Aggregating to medians over n=1 (OFC) is meaningless — the OFC "median" is just New Zealand's value, but the chart treats it visually identically to the n=16 UEFA median. The aggregation also hides every within-group outlier (which `ca4630bd` separately shows). Caveat is in the body, not on the chart. |
| Clarity   | 5 | Six labelled bubbles read in <5 seconds; n=N stamps integrated cleanly. |
| Aesthetic | 4 | Generous whitespace, log-y labelled, palette consistent. |
| Novelty   | 4 | Confederation-level scatter is uncommon framing. |

**Mean: 4.0** (vs self 4.4). Strong chart, single honest-aggregation concern.

## Proposals

### IMPROVE — overlay ghost-dots of every squad behind each bubble
**Why:** the median bubble hides 47 of 48 teams. Plot all 48 squads as small ghost-dots at (their_Elo, their_value), colored by confederation; the median bubble sits on top. The reader gets both the headline and the dispersion. Add IQR cross-hairs through each bubble.
**Data:** `teams.csv` + aggregated `squads_and_players.csv`.
**Library:** matplotlib scatter (alpha 0.35) + bubble overlay.

### PIVOT — every-team scatter with confederation ellipses
**Why:** drop median altogether — plot all 48 teams as dots, then draw 1σ ellipses per confederation. Same plane, no aggregation distortion; the OFC ellipse degenerates to a point, which is honest signalling.
**Library:** matplotlib `scatter` + `matplotlib.patches.Ellipse` from per-group covariance.

### EXTEND — bang-per-buck bar across confederations
**Why:** the chart begs "who buys the most Elo per €?". Compute `median_elo / median_value_M€` per confederation; barplot in descending order — almost certainly AFC and CONMEBOL on top.
**Data:** existing aggregates.
**Library:** matplotlib barh, log-friendly.

### COMBINE — bond with `ca4630bd` (Confederation box+swarm) for a 2-panel
**Why:** box+swarm = strength dispersion; bubble = strength × money × depth. Stacked: top = box+swarm, bottom = bubble. Same six labels, two complementary encodings — handles the "median hides spread" concern via cross-panel reference.
**Library:** matplotlib `GridSpec(2, 1)`.
