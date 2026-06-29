# Critique · 01 — Squad market-value lollipop (300× gap)

Reviewed node: `d0a17b1b-6332-525b-a816-27e07a9b7ad7` ("viz · Squad market value lollipop — a 300× gap from France to Qatar")
Author's self-score (mean 4.2): M5 H5 C5 A4 N3 → corrected: M4 H5 C5 A4 N3.

## Independent re-score

| Axis      | Re-score | Justification |
|-----------|:--------:|--------------|
| Meaning   | 4 | Money×confederation cross-cut is a real story; "France leads at €1.5B, Qatar trails at €20M" is concrete. |
| Honesty   | **3** | The TITLE is mathematically wrong: €1,523M / €20M ≈ **76×**, not 300×. The "€5M sides" framing is also inflated — Qatar is €20M, no team is anywhere near €5M. The chart is honest; its headline is not. |
| Clarity   | 4 | 48 names in one tall column reads, with inline €M labels; bottom 6 rows visually crowd the confederation legend box. |
| Aesthetic | 4 | Legend awkwardly placed inside plot bounds, partly over Qatar/Iraq row labels. Otherwise clean house style. |
| Novelty   | 3 | A classic lollipop; confederation color + inline value labels are the only twists. |

**Mean: 3.6** (self-claim 4.2). Disagree on Meaning (over-confident headline) and Honesty (the headline ratio is wrong). The chart deserves a corrected title.

## Proposals

### IMPROVE — fix the headline ratio and the legend placement
**Why:** the current title overstates the gap by ~4×; replace "300× squad-value gap" with "**76× squad-value gap** (France €1.52B vs Qatar €20M)" and remove "€5M sides" entirely. Move the confederation legend **above** the x-axis (full-width, horizontal) so it stops colliding with the Qatar/Iraq/Jordan rows. Add a thin dotted vertical at the median (€275M-ish).
**Data:** unchanged (`squads_and_players.csv` summed by `team_id`).
**Library:** matplotlib; one-line text edit + `legend(bbox_to_anchor=(0.5, -0.05), loc='upper center', ncols=6)`.

### PIVOT — small-multiple lollipops by confederation (6 panels)
**Why:** the single tall column hides within-confederation dispersion. Six side-by-side panels (UEFA, CONMEBOL, CAF, CONCACAF, AFC, OFC) sharing an x-axis show "how rich is the rich tail of YOUR continent" — a richer story than the single global ranking.
**Data:** same aggregation, then `groupby('confederation')`.
**Library:** matplotlib `subplots(2, 3, sharex=True)`; one lollipop per panel sorted internally.

### EXTEND — bang-per-buck horizontal bar (Elo per €M)
**Why:** the gap chart begs the follow-up "who is over-/under-performing their wage bill?" Plot `elo_residual = elo - predicted_elo(log10(value))` per team; positive bars = Elo-over-buying-power (Morocco / Senegal / Mexico / Japan candidates), negative = the opposite.
**Data:** join `teams.csv` × aggregated `squads_and_players.csv`; fit `np.polyfit(log10(value_eur), elo, 1)` over 48 teams; residual per team.
**Library:** matplotlib horizontal bar, two-color (positive green / negative red).

### COMBINE — money×strength×depth dashboard with `260ef4c9` + `ca4630bd`
**Why:** the lollipop is **money**, `260ef4c9` (confederation bubble) is **money×strength×n**, `ca4630bd` (box+swarm) is **strength spread per confederation**. Stitch into a single 3-panel "Team Ratings overview" figure (top: lollipop; middle: box+swarm; bottom: bubble) — one editorial unit, three encodings.
**Library:** matplotlib `figure(constrained_layout)` with 3 GridSpec rows.
