# wc2026-viz-critique-team-ratings

Wave-2 critique of the 10 Team-Ratings viz leaves in the WC-2026 Data Visualization graph (root `8ee1400c-a376-5b44-b10a-e1f50f0cfc82`). One markdown file per reviewed chart — independent re-score against the 5-axis rubric (`8c46c334-a27e-5693-bb8e-65e33efe63d4`: meaning / honesty / clarity / aesthetic / novelty) plus 2-4 concrete, build-ready proposals labelled IMPROVE / PIVOT / COMBINE / EXTEND.

Goal: hand the next build wave a structured backlog of named, executable specs — title, rationale, exact source tables, chart form + library — not vibes.

## Reviewed leaves

| File | Subject | Node |
|---|---|---|
| [01_squad_value_lollipop.md](01_squad_value_lollipop.md) | Squad market-value lollipop (300× gap) | d0a17b1b |
| [02_confederation_box.md](02_confederation_box.md) | Confederation strength box+swarm | ca4630bd |
| [03_age_ridgeline.md](03_age_ridgeline.md) | Squad age ridgeline | b5bf38e2 |
| [04_value_ridgeline.md](04_value_ridgeline.md) | Player-value ridgeline (three economies) | fcadfab6 |
| [05_radar_arg_spain.md](05_radar_arg_spain.md) | mplsoccer radar Argentina vs Spain | 8767b223 |
| [06_pizza_germany.md](06_pizza_germany.md) | PyPizza percentile profile (Germany) | f4dc31f6 |
| [07_elo_trajectories.md](07_elo_trajectories.md) | Elo trajectories (eight deep-runners) | 61855135 |
| [08_confederation_bubble.md](08_confederation_bubble.md) | Confederation bubble (strength/money/depth) | 260ef4c9 |
| [09_elo_vs_rank.md](09_elo_vs_rank.md) | Elo vs FIFA rank scatter | 1abdb79d |
| [10_possession_vs_xg.md](10_possession_vs_xg.md) | Possession vs xG | 8a79b86d |
| [SYNTHESIS.md](SYNTHESIS.md) | Top-5 highest-leverage build proposals | — |

## How proposals are labelled

- **IMPROVE** — same chart, fix a named flaw.
- **PIVOT** — same data, different chart form/angle that reveals more.
- **COMBINE** — merge with another specific viz into one richer view (node_id given).
- **EXTEND** — a new derivative insight the result suggests (follow-on chart).

Each proposal carries enough detail (data tables, transform, chart form, library) to be executed without further guesswork.
