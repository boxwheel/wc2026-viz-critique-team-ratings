# Critique · 06 — PyPizza percentile profile (Germany)

Reviewed node: `f4dc31f6-02d9-5654-924b-821a6c23eae9` ("viz · PyPizza percentile profile — Germany's 99th-pct attack, 45th-pct goals-against")
Author's self-score (mean 4.4): M5 H4 C5 A5 N3.

## Independent re-score

| Axis      | Re-score | Justification |
|-----------|:--------:|--------------|
| Meaning   | 4 | The "xG-against good (84) but goals-against mediocre (45)" gap is a sharp, specific finding — worth the chart. |
| Honesty   | **3** | Percentile over 48 teams with only **3 matches per team** is a *noisy* baseline — one over-performing keeper in one match shifts Germany's GA percentile by ~20 points. The chart presents the 45 vs 84 gap as if it's stable structural truth; sample-size caveat is in the body text but not on the chart. |
| Clarity   | 5 | PyPizza is famously legible; in-slice percentile values mean zero axis-reading. |
| Aesthetic | 5 | Attack-blue / defence-green split makes the asymmetry land instantly. |
| Novelty   | 3 | PyPizza is a known idiom; editorial pairing with the radar is the novelty. |

**Mean: 4.0** (self-claim 4.4). The chart is excellent; the only ding is honesty around small-n percentiles, which a single caption fixes.

## Proposals

### IMPROVE — add a small-n caveat on the figure itself
**Why:** "n=3 matches; percentile bands ±10pts due to small sample" prevents naive over-interpretation. Without it, readers will treat 45 vs 84 as immutable.
**Effort:** one footer line.

### PIVOT — small-multiple pizzas for all 8 deep-runners (4×2 grid)
**Why:** "Germany's chink" is one team's chart; the same axes for 8 contenders is gallery-grade and lets readers spot the chink of every favourite at once. Same transform, same axes, same percentile.
**Data:** `transforms/xg_aggregate.py` + percentile rank over 48; iterate over the 8 deep-runner team_ids.
**Library:** mplsoccer `PyPizza`, looped in a `subplots(2, 4)`.

### EXTEND — percentile-trajectory over matches for Germany
**Why:** if "45th goals-against percentile" is volatile (the "bad luck" hypothesis), it should swing match-by-match. Plot Germany's percentile on each of the 8 axes per match (1 line per axis, 3 points each), as small-multiples. A flat 45 across all 3 matches = real defect; a 90→25→20 line = the keeper had one bad outing.
**Data:** `match_team_stats.csv` per team per match; percentile-rank per match across the 48-team field.
**Library:** matplotlib small-multiple line.

### COMBINE — feature with `8767b223` (Argentina-vs-Spain radar)
**Why:** pair this pizza with the radar into a single "Team Profile" mini-template that any future viz reusable: pizza = percentile-vs-field, radar = head-to-head shape, applied to multiple teams.
**Library:** matplotlib figure with PyPizza + Radar subplots.
