# 🌆 Brisbane City Intelligence Engine

> 8 AI agents simultaneously analyse real Brisbane suburb data — scoring investment priorities, flagging service gaps, and surfacing insights across transit, green space, affordability, flood risk, and development pressure. Visualised on a live 3D map.

[![Built with Claude](https://img.shields.io/badge/Powered%20by-Claude%20Haiku-orange)](https://anthropic.com)
[![deck.gl](https://img.shields.io/badge/Maps-MapLibre%20%2B%20deck.gl-blue)](https://deck.gl)
[![ECharts GL](https://img.shields.io/badge/3D%20Charts-ECharts%20GL-green)](https://echarts.apache.org)

---

## What Is This?

A live agentic AI demo built for Brisbane City Council urban intelligence scenarios. Eight specialist agents analyse 20 real Brisbane suburbs across six data dimensions — scoring each suburb's investment priority and flagging specific infrastructure gaps. Everything updates live on a 3D Kepler.gl-inspired map as agents work.

---

## The 8 Agents

| Agent | Emoji | Specialty | What They Identify |
|-------|-------|-----------|-------------------|
| **Atlas** | 🏘 | Property Analyst | Affordability stress, price growth corridors, displacement risk |
| **Transit** | 🚌 | Transport Analyst | Translink coverage gaps, transport deserts, route frequency failures |
| **Verde** | 🌳 | Green Space Analyst | Park coverage below WHO standard, urban heat island risk, canopy gaps |
| **Dev** | 🏗 | Development Analyst | Intensification pressure, infrastructure capacity constraints |
| **Pulse** | 📊 | Demographics | Population growth corridors, community service demand |
| **Link** | 🚶 | Connectivity Analyst | Walkability gaps, missing footpaths, absent bike infrastructure |
| **River** | 🌊 | Environment Analyst | BCC flood risk zones, climate resilience gaps |
| **Sage** | 🧠 | Strategic Analyst | Cross-dimensional compound disadvantage, portfolio investment priorities |

---

## 20 Real Brisbane Suburbs

Data sourced from **BCC Open Data Portal, CoreLogic 2024, ABS Census 2021, Translink GTFS 2024, BCC Flood Awareness Maps, Walk Score methodology**.

| Zone | Suburbs |
|------|---------|
| **Inner City** | New Farm, Paddington, West End, South Brisbane, Fortitude Valley, Newstead, Teneriffe, Kangaroo Point |
| **Near City** | Hamilton, Ascot, Woolloongabba |
| **Middle Ring** | Toowong, St Lucia, Indooroopilly, Camp Hill, Coorparoo |
| **Outer** | Chermside, Nundah, Mt Gravatt, Kenmore |

### Data Dimensions (per suburb)

| Dimension | Source | Range |
|-----------|--------|-------|
| Transit Score | Translink GTFS 2024 | 0–100 |
| Green Space Score | BCC Open Data Portal | 0–100 |
| Median House Price | CoreLogic 2024 | $730k–$2.1M |
| Price Growth (12m) | CoreLogic 2024 | % |
| Development Applications | BCC Open Data | count |
| Population Density | ABS Census 2021 | persons/km² |
| Walkability Index | Walk Score methodology | 0–100 |
| Flood Risk | BCC Q100 Flood Maps | % |
| Infrastructure Quality | BCC Open Data | 0–100 |

---

## The Visualisations

### Left Panel — MapLibre GL + deck.gl 3D Map

A real-world dark CARTO map of Brisbane at 50° pitch with four switchable deck.gl layers:

| Layer | What It Shows | Default |
|-------|--------------|---------|
| **3D Column Bars** | Extruded hexagonal prisms rising from each suburb. Height = selected metric. Colour = priority score (grey → green → yellow → orange → red) | ✓ On |
| **Flood Risk Heatmap** | Gaussian hot spots from BCC Q100 flood event data. Red = high climate vulnerability (West End, South Brisbane) | ✓ On |
| **Transit Coverage Gap** | Contour iso-lines where Translink is weakest. Red = severe gap (Kenmore transit score: 42/100) | Off |
| **Property Price Bubbles** | Circles sized by median price, coloured by 12-month growth rate. Orange = hot market | Off |

### Right Panel — ECharts GL 3D Scatter

All 20 suburbs as floating 3D bubbles, auto-rotating:
- **X axis** = Transit Score (35–95)
- **Y axis** = Green Space Score (30–90)
- **Z axis** = Median Price ($700k–$2.2M)
- **Bubble size** = Development activity
- **Colour** = Investment priority score (grey = unscored, green → red as agents score)

Click any bubble for a tooltip with all 6 data dimensions.

---

## Layer Controls (Floating Panel)

The Kepler.gl-inspired control panel sits in the bottom-left of the map:

| Control | Options |
|---------|---------|
| **Layer toggles** | 3D Columns / Flood Heatmap / Transit Contours / Property Bubbles |
| **Column height** | Priority Score / Flood Risk / Transit Gap / Dev Activity / Median Price |
| **Zone filter** | All / Inner / Near / Middle / Outer |
| **View** | Reset view / 2D↔3D toggle |

The **2D/3D toggle** smoothly transitions the map pitch between 50° (3D perspective) and 0° (flat plan view).

---

## Priority Scoring

Each agent scores suburbs 0–100:

| Score | Priority | Meaning |
|-------|----------|---------|
| 75–100 | 🔴 Urgent | Immediate council investment recommended |
| 55–74 | 🟠 High | Significant action needed, develop plan |
| 35–54 | 🟡 Moderate | Monitor, include in next planning cycle |
| 0–34 | 🟢 Well-served | No urgent action required |

As agents score suburbs, the map column for that suburb transitions from grey to the priority colour, and the ECharts bubble updates simultaneously.

---

## Agent Tools

Each agent has access to four tools:

| Tool | Cost | What It Does |
|------|------|-------------|
| `scan_suburbs` | $5 | Scan portfolio for suburbs matching agent's specialty dimension |
| `analyse_suburb` | $8 | Deep analysis of a specific suburb — returns all 9 data dimensions |
| `score_suburb` | $12 | Assign priority score 0–100 with opportunity description and recommendation |
| `identify_gap` | $8 | Flag a specific infrastructure or service gap with severity and recommended action |

Each agent has a **$500 budget**. The camera occasionally flies to suburbs being actively analysed.

---

## What the Metrics Mean

| Metric | Meaning |
|--------|---------|
| **X/20 Scored** | Number of suburbs that received a priority score from any agent |
| **High Priority** | Suburbs scoring ≥55 — significant or urgent investment needed |
| **Gaps Found** | Specific infrastructure/service gaps identified across all agents |
| **API Calls** | Total Claude Haiku calls across all 8 agents |

---

## Live Map Interactions

- **Drag** — pan the map
- **Scroll** — zoom in/out
- **Right-click drag** — tilt and rotate
- **Hover** any area — suburb tooltip with all data dimensions + priority score
- **2D/3D toggle** — smooth camera pitch transition
- **Zone filter** — show only Inner / Near / Middle / Outer suburbs
- **Column height selector** — change what drives bar height in real time

---

## Rate Limiting & Architecture

```
8 agents sharing a global rate limiter (2.2s gap = ~27 calls/min)

Atlas  Transit  Verde  Dev  Pulse  Link  River  Sage
  \       |       |     |     |     |      |     /
   ╔══════════════════════════════════════════════╗
   ║  20 Suburbs × 9 Data Dimensions             ║
   ║  BCC Open Data · CoreLogic · ABS · Translink ║
   ╚══════════════════════════════════════════════╝
          ↓                    ↓
   MapLibre GL map      ECharts GL 3D scatter
   + deck.gl layers     (updates on score)
```

- Agents stagger starts by 1.2s each to avoid burst traffic
- On 429: waits 5s then retries
- On 400: resets message context and continues
- Message trimming never splits tool_use/tool_result pairs

---

## Data Sources & Accuracy

All suburb data reflects real-world conditions as of 2023/2024. Specific figures:

| Source | What's Sourced |
|--------|---------------|
| **CoreLogic 2024** | Median prices and 12-month price growth by suburb |
| **ABS Census 2021** | Population density (persons/km²), household composition |
| **Translink GTFS 2024** | Bus and train route frequency, stop density, CBD travel time |
| **BCC Open Data Portal** | Development application counts, park locations, green space coverage |
| **BCC Flood Awareness Maps** | Q100 flood event risk percentage by suburb |
| **Walk Score methodology** | Walkability index derived from amenity proximity and footpath coverage |

Notable real data points: Kenmore transit score 42/100 (worst in dataset), West End flood risk 22% (highest), Woolloongabba price growth 13.8% (fastest), Ascot median $2.1M (most expensive).

---

## Running Locally

```bash
# Serve over HTTPS/HTTP (required for MapLibre tile loading)
npx serve .
# or
python3 -m http.server 8080

open http://localhost:8080/brisbane.html
```

Enter your Anthropic API key in the intro screen.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| AI | Claude Haiku (`claude-haiku-4-5-20251001`) |
| Base Map | MapLibre GL JS v3.6.2 + CARTO Dark Matter tiles |
| 3D Layers | deck.gl v8.9 — ColumnLayer, HeatmapLayer, ContourLayer, ScatterplotLayer |
| 3D Scatter | Apache ECharts v5.4 + ECharts GL v2 (scatter3D) |
| Build | None — single HTML file, CDN only |

---

## The Full Demo Suite

| File | Demo |
|------|------|
| `brisbane.html` | **Brisbane City Intelligence** — this file |
| `req_factory.html` | **Agentic Requirements Factory** — 6 agents, 8 artefact tabs + PDF export |
| `cs_engine.html` | **CS Escalation Engine** — 8 agents, $5.7M ARR portfolio |
| `rev_heb.html` | **Revenue Engine** — 10 agents, HEB visualisation |
| `kerala_v2.html` | **Kerala Kitchen** — multi-agent directed market graph |

---

## About

Built by **Saagar Devadiga** — Data Engineer & AI Developer, Brisbane, Australia.
MSc Data Science · Queensland University of Technology

> *"Real data. Real Brisbane suburbs. Real AI analysis. This is what urban intelligence tooling looks like when you combine agentic AI with proper geospatial visualisation."*

---
*Powered by [Anthropic Claude](https://anthropic.com) · Maps by [MapLibre GL](https://maplibre.org) · Layers by [deck.gl](https://deck.gl) · Charts by [Apache ECharts](https://echarts.apache.org)*
