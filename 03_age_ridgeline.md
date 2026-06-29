# Critique · 03 — Squad age ridgeline

Reviewed node: `b5bf38e2-13e6-5856-b017-6c5a7cd6f863` ("viz · Squad age ridgeline — same age curve, different tails")
Author's self-score (mean 4.4): M5 H5 C4 A5 N3.

## Independent re-score

| Axis      | Re-score | Justification |
|-----------|:--------:|--------------|
| Meaning   | 3 | The editorial frame ("CAF runs young, UEFA runs long") IS visible in the tails — but **all six medians pile up around 27y** in the rendered chart, so the visible body story is "they're all the same"; the tails are the real signal and they're undersold. |
| Honesty   | **3** | Six dotted medians ALL stack visually around 27.2y. The viewer reads "no difference" — yet the body text claims CONMEBOL 28.5y / OFC 27.2y. The annotation collapses real (small) differences into visual coincidence. |
| Clarity   | 3 | Six ridges with overlapping median lines and a single shared x is reasonable, but the medians are unreadable; group labels on left are fine. |
| Aesthetic | 4 | Joypy stack reads as one composition; palette holds. |
| Novelty   | 3 | Joypy ridgelines are standard. |

**Mean: 3.2** (self-claim 4.4 — significant gap). The headline frames a "tail story" but the chart's most prominent annotation is the centre, which all six groups share.

## Proposals

### IMPROVE — replace the central median lines with right-side inline stats per ridge
**Why:** the medians are all within 1.5y of each other, so a vertical-line annotation buries the signal. Replace with a small inline-stat block at the right end of each ridge: `n=276 · median=27.2 · over-30 %=18`. The "over-30 %" is the **tail statistic** that the headline actually depends on.
**Data:** unchanged (`squads_and_players.csv` + `teams.csv`).
**Library:** matplotlib `text(x_right, ridge_y, ...)`; joypy retained.

### PIVOT — squad-grain "fraction over 30" bar chart
**Why:** the editorial story is about tails, so make a tail-statistic the y-axis directly. For each of 48 squads, plot `share_over_30 = count(age>=30) / squad_size` as a horizontal bar, colored by confederation. Reveals "veteran-heavy" squads (likely Croatia, Mexico, Brazil) vs "kid" squads (Ghana, USA?).
**Data:** `squads_and_players.csv` aggregated per `team_id`.
**Library:** matplotlib barh.

### COMBINE — pair with `fcadfab6` (Player-value ridgeline) on a 2-panel "age × money"
**Why:** age and money are correlated stories: top-tier squads have older late-career bulges. A two-row ridgeline (top = age, bottom = value) with the SAME band split (top-8/middle/bottom-8 squad value) lets the reader see "do richer squads also run older?" in one composition.
**Data:** same `squads_and_players.csv`; same banding logic as `fcadfab6`.
**Library:** joypy on two stacked axes.

### EXTEND — age-by-position ridgeline
**Why:** the `position` field in `squads_and_players.csv` (GK/CB/FB/CM/W/ST) almost certainly produces clearer age-distribution differences than confederation does. GKs skew oldest, wingers skew youngest — that's a sharper, less debatable claim.
**Data:** group `squads_and_players` by `position`; same KDE.
**Library:** joypy with 5–6 position rows.
