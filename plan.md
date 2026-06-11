# eco-or-feed

> **Which VCT teams beat the odds — and which teams choke when they shouldn't?**

An analysis of economic upsets in professional Valorant, using round-level economy and positioning data from VCT international events.

---

## Motivation

In Valorant, **economy is destiny** — or so the conventional wisdom goes. A full-buy team facing an eco should win almost every time. But pro play tells a different story. Some teams consistently punch above their economic weight, while others routinely blow rounds they should never lose.

This project quantifies that gap: which teams are the best eco fighters, which teams are the most reliable favorites, and what does the positioning data reveal about *how* those upsets happen?

---

## Research Questions

1. **How often do economic upsets actually happen at the VCT level?** Is economy as deterministic as people think?
2. **Which teams have the highest eco win rate?** Who consistently beats the odds?
3. **Which teams have the worst anti-eco win rate?** Who chokes their economic advantages?
4. **Does map or role composition affect upset rates?** Are some maps more "anti-econ" than others?
5. **What does positioning look like in upset rounds?** Do eco-winning teams play differently spatially?

---

## Methodology

### Data Source
- [`valorantr`](https://tonyelhabr.github.io/valorantr/) R package wrapping the rib.gg API
- Pre-saved VCT match data: ~5,600 matches across international events (2022–2023)
- Key tables: `economies` (per-round loadout values), `locations` (x/y + view direction), `events`, `series`

### Round Classification
Rounds are classified by **loadout value differential** between teams:

| Label | Condition |
|---|---|
| **Full buy** | Both teams spent above threshold |
| **Eco** | Team spent significantly less than opponent |
| **Force buy** | Team spent moderately less than opponent |
| **Anti-eco** | Team spent significantly more than opponent |

Thresholds derived empirically from the distribution of loadout values across all rounds.

### Core Metrics
- **Eco Win Rate (EWR)**: % of rounds a team wins when their loadout value is significantly below the opponent's
- **Anti-Eco Win Rate (AER)**: % of rounds a team wins when their loadout value is significantly above the opponent's
- **Upset Delta**: EWR − (1 − AER), a single score capturing a team's overall economic resilience

### Deliverables

**Phase 1 — Economy Analysis (R + Python)**
- [ ] Pull and clean economy data via `valorantr`
- [ ] Classify rounds by economic state
- [ ] Compute EWR and AER per team across VCT events
- [ ] 2×2 quadrant chart: EWR vs AER with team labels/logos
- [ ] Upset rate by map
- [ ] Upset rate by agent role composition

**Phase 2 — Positional Analysis (Python)**
- [ ] Pull location data for upset rounds vs. expected-outcome rounds
- [ ] Compare team spatial spread at key round timestamps (e.g., 30s, 60s mark)
- [ ] Heatmaps of player positions in eco wins vs eco losses
- [ ] Rotation speed analysis: how quickly do teams respond when the eco team commits?

---

## Repo Structure

```
eco-or-feed/
├── data/
│   ├── raw/          # CSVs exported from R
│   └── processed/    # Cleaned data for analysis
├── R/
│   └── pull_data.R   # valorantr data extraction
├── notebooks/
│   ├── 01_economy_classification.ipynb
│   ├── 02_team_eco_metrics.ipynb
│   └── 03_positional_analysis.ipynb
├── outputs/
│   └── figures/      # Charts and visualizations
└── README.md
```

---

## Tech Stack

| Tool | Purpose |
|---|---|
| R + `valorantr` | Data extraction from rib.gg API |
| Python + pandas | Data cleaning and analysis |
| matplotlib / seaborn | Visualizations |
| geopandas / shapely | Spatial analysis (Phase 2) |

---

## Scope

- **Events**: VCT Champions 2022, VCT Masters 2022–2023 (maps with sufficient sample size)
- **Map filter**: Only maps appearing in 2+ events to allow cross-tournament comparison
- **Minimum rounds**: Teams with fewer than 50 classified rounds excluded from leaderboards

---

## Key Visuals (Planned)

- **Team quadrant chart** — EWR vs AER, four archetypes: elite / scrappy / reliable / inconsistent
- **Upset rate by map** — which maps are most "anti-econ"?
- **Positional heatmaps** — where do teams go in eco wins vs losses?
- **Eco leaderboard** — ranked table of VCT teams by upset delta

---

## Authors

Ryan — [UWAGGS](https://uwaggs.ca) | University of Waterloo

---

## Acknowledgements

Data sourced via [`valorantr`](https://github.com/tonyelhabr/valorantr) by Tony ElHabr, wrapping the [rib.gg](https://rib.gg) API.
