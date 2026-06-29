# Critique · 07 — Elo trajectories (eight deep-runners)

Reviewed node: `61855135-632a-5766-9223-1d6a34094526` ("viz · Elo trajectories — the eight deep-runners over their first matches")
Author's self-score (mean 4.0): M4 H5 C4 A4 N3.

## Independent re-score

| Axis      | Re-score | Justification |
|-----------|:--------:|--------------|
| Meaning   | 4 | "Top seeds can lose Elo" (Germany dropping ~50pts) is a genuine, counter-intuitive finding. |
| Honesty   | 4 | K=60 + GD-multiplier formula disclosed (good); but x-axis "Matches played" **conflates time across teams** — Argentina's match 3 isn't the same calendar moment as Mexico's match 3. The horizontal axis encodes a per-team match-index, not a shared time axis, which makes the multi-line layout misleading. |
| Clarity   | **3** | The 8 highlighted lines crowd the 1950–2150 band; end-of-line labels stack (Spain/Brazil/Netherlands within 50 Elo); England-red and Spain-red kit colors collide, making them ambiguous. |
| Aesthetic | 3 | Labels feel pushed off; lots of empty whitespace above 2200 and below 1500 (no team lives there in the highlights). |
| Novelty   | 3 | Multi-line spaghetti is standard form. |

**Mean: 3.4** (self-claim 4.0). The chart works; layout + axis choice undercut what's already a strong idea.

## Proposals

### IMPROVE — use calendar date on x, or facet to small-multiples
**Why:** "match index" is per-team; a calendar x-axis lets cross-team comparison make actual sense. Alternative: 4×2 small-multiples (one team per panel) sharing y; eliminates label collisions entirely.
**Data:** `matches_detailed.csv:date` for x-axis, joined per team_id.
**Library:** matplotlib date-axis with `mdates.DateFormatter`, or `subplots(2, 4)`.

### PIVOT — slope-graph of Elo before/after each match
**Why:** smoothed trajectories hide the per-match shock. A slope-graph (segments from pre-match Elo to post-match Elo per team) shows exactly which match Germany shed Elo in, and at what GD. One row per team-match.
**Library:** matplotlib `vlines`/`hlines` or seaborn `pointplot` paired by match.

### EXTEND — K-sensitivity panel (K=40, 60, 80)
**Why:** is Germany's Elo drop a real signal or an artefact of K=60? Recompute the 8 highlighted trajectories at K=40 and K=80; a 3-panel small-multiple instantly shows robust signal vs K-noise.
**Data:** `transforms/elo_timeline.py` parameterised on K.
**Library:** matplotlib `subplots(1, 3, sharey=True)`.

### COMBINE — fold into `1abdb79d` (Elo vs FIFA rank scatter)
**Why:** the residual from `1abdb79d` (pre-tournament Elo vs FIFA rank) tells us who Elo *thinks* is over/underrated; the trajectory tells us who is being *corrected* by results. Plot `pre-tournament Elo residual` (x) vs `Elo change so far` (y), one dot per team — a single scatter says "Argentina's overrated and proving it" vs "Germany's overrated and being corrected".
**Library:** matplotlib scatter on derived team-grain frame.
