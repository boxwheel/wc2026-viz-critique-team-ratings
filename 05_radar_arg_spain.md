# Critique · 05 — mplsoccer radar (Argentina vs Spain)

Reviewed node: `8767b223-8dcd-5467-b076-42c9cdd367d6` ("viz · mplsoccer radar — Argentina vs Spain, all-court vs spiky")
Author's self-score (mean 4.4): M5 H4 C4 A5 N4.

## Independent re-score

| Axis      | Re-score | Justification |
|-----------|:--------:|--------------|
| Meaning   | 4 | Two-team shape comparison is a real stylistic story; Argentina possession-spike + Spain defence-dominance reads. |
| Honesty   | **3** | Polygon area is a misleading summary — small numerical differences on a (min, max)-stretched axis become huge visual differences. The axes are "%-of-range over 48 teams", not (0, max), so a polygon that fills 80% of the radar doesn't mean "good", it means "above the worst team". Defensive inversion is OK and labelled. |
| Clarity   | 3 | Axis labels & tick numbers tiny; ring values (1.2, 1.8, 2.4…) read as if absolute but they're %-of-range. A decoder is needed. |
| Aesthetic | 5 | mplsoccer radar + house bg + kit colors is polished and publication-grade. |
| Novelty   | 3 | Two-team radar is football-twitter standard. |

**Mean: 3.6** (self-claim 4.4). The polygon overlay is the chart's biggest credibility risk; clarity also weaker than self-rated.

## Proposals

### IMPROVE — drop the shaded polygon, keep only spokes + markers
**Why:** the shaded area is what makes radar misleading; removing it leaves the actual data values (8 marker positions per team), which the reader can compare axis-by-axis without an area-illusion. Print the numeric value at each marker.
**Library:** mplsoccer `Radar.draw_radar(...)` with `kwargs_radar={'facecolor': 'none'}`; print values via `ax.text` at each spoke.

### PIVOT — parallel-coordinates of all 48 squads on the 8 axes
**Why:** the radar compares two teams *to each other*; parallel-coordinates puts those two against the field of 48 — far more informative. Argentina + Spain stand out as kit-colored lines among grey background lines.
**Data:** `transforms/xg_aggregate.py` produces the team-grain per-90 frame for all 48 squads; identical axis selection to the radar.
**Library:** pandas + matplotlib `parallel_coordinates`, or `plotly.express.parallel_coordinates`.

### EXTEND — small-multiple radars for all 8 deep-runners
**Why:** "all-court vs spiky" begs the question of where the other 6 contenders land. A 4×2 grid (Arg/Bra/Esp/Fra/Eng/Ger/Por/Ned) on identical axes, identical ranges = an instant style taxonomy of the contenders.
**Data:** loop the existing transform over those 8 teams; shared 48-team ranges.
**Library:** mplsoccer `Radar` x 8 subplots.

### COMBINE — bond with `f4dc31f6` (Germany PyPizza) into a 3-panel "two views of the same axes"
**Why:** radar and pizza encode the same 8-axis data differently — radar is per-team raw, pizza is percentile against the field. A 3-panel feature (Arg pizza | Spain pizza | overlay radar) tells "two ways the same axes can be read".
**Library:** matplotlib figure with mplsoccer `Radar` + `PyPizza` on three axes.
