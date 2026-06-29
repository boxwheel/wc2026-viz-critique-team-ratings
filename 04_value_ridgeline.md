# Critique · 04 — Player-value ridgeline (three economies)

Reviewed node: `fcadfab6-80af-54db-98c6-80a64b1d5286` ("viz · Player-value ridgeline — the three economies of WC-2026")
Author's self-score (mean 4.4): M5 H5 C4 A4 N4.

## Independent re-score

| Axis      | Re-score | Justification |
|-----------|:--------:|--------------|
| Meaning   | 5 | "The whole curve shifts" insight is well-supported by the three medians (€31M / €6M / €550k); two orders of magnitude between top and bottom medians is a real story. |
| Honesty   | 5 | Log scale clearly labelled with €1k…€100M ticks, zeros excluded, each median annotated with its raw € value. |
| Clarity   | 5 | Three colors, three ridges, three medians — one of the cleanest plots in the wave. |
| Aesthetic | 5 | The €1k…€100M raw-€ tick labels on a log axis read naturally; subtle, balanced palette. |
| Novelty   | 4 | Banding by team-aggregate value is unusual at player-grain; the framing is fresh. |

**Mean: 4.8** (slightly above the self-claim of 4.4). This is the strongest node in the assigned subset — confident gallery-ready.

## Proposals

### IMPROVE — document the "middle" band choice on-chart
**Why:** "Middle (ranks 21-28)" is a specific slice (24 ranks between top-8 and bottom-8 are dropped); a viewer fairly wonders "why those?". One-line caption: "Middle = teams ranked 21–28 by total squad market value (≈median ±4)."
**Effort:** subtitle text edit.

### PIVOT — quartiles or octiles instead of three tiers
**Why:** three tiers leave 24 teams (the gap between 9–20 and 29–40) **unrepresented**. Four quartiles (12 teams each) or eight octiles (6 each) cover the field. A six-octile ridgeline shows the gradient is smooth — there are not "three economies", there's a continuum.
**Data:** unchanged; replace `top8/middle/bottom8` with `pd.qcut`.
**Library:** joypy with 4 or 8 rows.

### EXTEND — per-squad Gini coefficient of player values
**Why:** the ridgeline shows squads SLIDE as a unit; the natural follow-up is "is the slide uniform, or do top squads carry just 1-2 stars while bottom squads are uniform-poor?" Compute Gini of player values per squad (48 numbers). Plot as horizontal bar, sorted by Gini, colored by tier — likely reveals top-tier squads are *more* unequal (concentrated stars) while bottom-tier are uniform-cheap.
**Data:** `squads_and_players.csv` grouped by `team_id`; Gini = `sum((2*i - n - 1) * v_sorted[i]) / (n * sum(v_sorted))`.
**Library:** matplotlib barh.

### COMBINE — pair with `d0a17b1b` (Squad-value lollipop) on a 2-panel
**Why:** lollipop is squad-aggregate; this is player-grain underneath. Top panel = lollipop (the headline gap); bottom panel = this ridgeline (the texture inside the gap). One editorial unit, two zoom levels.
**Library:** matplotlib `GridSpec` 2-row figure.
