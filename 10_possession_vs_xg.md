# Critique · 10 — Possession vs xG (ball-share isn't a verdict)

Reviewed node: `8a79b86d-66cc-5bf5-8127-9055afe2d01c` ("viz: Possession isn't a verdict — WC-2026 group-stage xG vs ball-share")
Author's self-score (mean 4.2): M4 H5 C4 A4 N4.

## Independent re-score

| Axis      | Re-score | Justification |
|-----------|:--------:|--------------|
| Meaning   | 4 | Answers a common football question ("does possession track xG?") with a clear "yes, modestly" — slope from 1.0 → 2.0 xG across the full possession range is concrete. |
| Honesty   | 4 | Raw scatter shown, binned trend labelled. But the 5%-bin gold line is **spiky at the extremes** (one bin in the 20%-25% poss range may have only 3–5 dots; the trend spike doesn't reflect real shape). |
| Clarity   | **3** | The legend box sits over the upper-left scatter cloud and physically covers ~10 dots in that region. Outlier labels (Australia, South Africa, Türkiye, South Korea) are readable but Türkiye appears twice with overlapping callouts. |
| Aesthetic | 4 | Three-color outcome scheme + dot-size=goals is intuitive; gold trend reads well. |
| Novelty   | 4 | possession × xG × outcome multi-encoding at WC scope is fresh. |

**Mean: 3.8** (self-claim 4.2). Pleasant chart; legend placement is the single biggest annoyance.

## Proposals

### IMPROVE — move the legend out of the scatter cloud + use LOWESS, not 5%-bins
**Why:** the legend at upper-left covers data dots; move to outside (below the x-axis, full width) or to the empty bottom-right. Replace the 5%-binned-mean line with `statsmodels.nonparametric.lowess(frac=0.3)` so the trend is smooth and the spike artefacts disappear.
**Library:** matplotlib `legend(bbox_to_anchor=(1.02, 0.5))` + statsmodels lowess.

### IMPROVE — split the trend by outcome (3 lines)
**Why:** the single gold trend collapses a meaningful interaction. Plot one LOWESS line per outcome (W/D/L) — readers will see "wins climb steeply with possession; losses stay capped around 1.2 xG no matter the possession". Stronger story than a single line.
**Data:** group `(possession_pct, xg)` by outcome.
**Library:** matplotlib + statsmodels LOWESS x 3.

### PIVOT — jointplot with marginal histograms
**Why:** marginals on x and y, color-stacked by outcome, reveal that possession is bimodal across team-matches (pressers vs counter-attackers) and xG is right-skewed — neither is visible in a raw scatter.
**Library:** `seaborn.jointplot(kind='scatter', marginal_kws=dict(stack=True))`.

### EXTEND — possession-residual ranking
**Why:** the chart says "possession ≠ verdict"; the natural follow-up is "who outperformed their possession?" Compute `xG_residual = xG - LOWESS(possession)`; rank 144 team-matches; top-10 = "most efficient with their ball-share", bottom-10 = "wasted possession". Horizontal bar of named team-matches.
**Data:** existing team-match frame + LOWESS residual.
**Library:** matplotlib barh, two-color.
