# DS2002 Capstone Project: Game Day Pulse — UVA Home Weekend Analytics

**University of Virginia | DS2002 Data Science Systems | Fall 2026**
**Capstone Group Project — 4 Weeks**

---

## Background

Six times each fall, Charlottesville transforms. On UVA home football Saturdays, tens of thousands of fans pour toward Scott Stadium, and a small economy of food trucks, Corner restaurants, carts, and merch tents scrambles to feed and outfit them. Some vendors sell out by kickoff. Others sit on inventory they can't move. And when the weather turns, everything changes at once.

The (fictional) **Cville Game Day Alliance** — a coalition of ~20 vendors around the stadium — has been collecting point-of-sale data all season, but it lives in messy, disconnected files. Nobody has cleaned it, nobody has connected it to weather, and the Alliance board is asking hard questions before next season: **Where is demand surging, and when? Which items sell out? Should vendors stock more rain gear? Which zones are underserved?**

Your team has been hired as the Alliance's data consultants. You have raw data and real questions. Your job is to build a pipeline that turns the mess into a defensible recommendation.

**This project builds directly on your Walmart midterm skills** — demand surges, SKU consolidation, weather APIs, geography, and signal-vs-artifact investigation — applied to a brand-new story.

---

## Team Requirements

- **Group size:** 3–4 students
- **Duration:** 4 weeks
- **Language:** Python (all work in Python)
- **Environment:** Kaggle Notebook or Google Colab
- **Required libraries:** `pandas`, `matplotlib` and/or `seaborn`, `sqlite3`, `requests`

---

## Supplied Data Files

All files are in the `data/` directory. Every file contains **intentional data quality issues** you must find and fix.

| File | Format | Description | Approx. Size |
|------|--------|-------------|--------------|
| `gameday_orders.csv` | CSV | Order-level point-of-sale records across vendors, all Fall 2026 home weekends + baseline Saturdays | ~19,000 rows |
| `vendor_locations.csv` | CSV | Vendor metadata: type, zone, distance to stadium, lat/lon | 20 rows |
| `menu_catalog.csv` | CSV | Master item/SKU reference (food, drink, merch, rain gear) | ~35 rows |
| `zone_capacity.csv` | CSV | Stadium-area zones and foot-traffic capacity | 3 rows |
| `inventory_and_sales.db` | SQLite | `daily_sales_summary` + `inventory_levels` tables | ~90 rows |

### Known Data Issues (Discover and Fix These)

The data is messy on purpose. Expect:

- Duplicate order records
- Missing values (empty strings, `NULL`, `N/A`, `NaN`, `None`, whitespace)
- Inconsistent timestamp formats (`ISO T`, space-separated, US `MM/DD/YYYY`, no seconds)
- The same item under **multiple SKUs and name spellings** (your Pop-Tarts problem — see Foam Finger, Rain Poncho, UVA T-Shirt, Cheeseburger)
- Inconsistent category labels (`Food`/`food`/`FOOD`/`Concessions`; `Merch`/`Merchandise`/`Apparel`; `RainGear`/`Rain Gear`/`Weather Gear`)
- Vendor ID format inconsistencies (`V-01`, `V_01`, `V01`, `v-01`, `V 01`)
- Prices stored as strings with `$` signs
- Negative quantities (data-entry errors)
- Missing latitude/longitude for some vendors

---

## Weather API Requirement

You **must** pull real weather data from a **free API** and integrate it. This is not optional — Question 3 depends on it, and weather context strengthens your other answers. The home-weekend Saturdays in this dataset are:

`2026-09-05, 2026-09-19, 2026-10-10, 2026-10-24, 2026-11-14, 2026-11-21`
(baseline non-game Saturdays: `2026-09-12, 2026-10-03, 2026-10-31, 2026-11-07`)

### Suggested Free APIs (Pick One)

1. **Open-Meteo** — <https://open-meteo.com/> — free, no key, excellent historical hourly weather (temperature, precipitation, wind). *Recommended.* Charlottesville ≈ `lat 38.03, lon -78.51`.
2. **OpenWeatherMap** — <https://openweathermap.org/api> — free tier, widely documented.
3. **Visual Crossing** — <https://www.visualcrossing.com/weather-api> — free tier, clean CSV/JSON.

### What You Must Demonstrate

- Python code that calls the API (`requests`)
- Parsing the response into a pandas DataFrame
- Joining weather data with your orders/summary data
- Handling any timezone/date alignment (weather may be UTC; orders are local ET)

---

## Analytical Questions

Answer all 5. Each answer needs **code, a visualization, and a written explanation** in Markdown.

### Question 1: Kickoff Demand Surge

> How does order volume on game Saturdays compare to baseline (non-game) Saturdays? Within a game day, how does demand build in the hours before kickoff (≈3:30 PM)? Quantify the surge and visualize the hourly trend.

**Requires:** cleaning timestamps, separating game vs. baseline days, hourly grouping around kickoff, line/bar chart, percentage calculations.

### Question 2: The Item Consolidation Problem

> After standardizing all SKU/name variants into canonical items, what are the true top-selling items? Show the fragmented view vs. the consolidated view for at least Foam Finger and Rain Poncho. What decisions would differ between the two views?

**Requires:** building a SKU→canonical mapping table, consolidating fragments, side-by-side before/after visualization, written business impact.

### Question 3: Weather-Driven Demand

> Using weather data from your chosen API, how do rain and temperature correlate with sales by category? Does rain gear spike on rainy games? Is there a detectable lag between weather and purchasing?

**Requires:** API integration, date/timezone alignment, correlation analysis (scatter/heatmap/time-series overlay), lag discussion, join with category sales.

### Question 4: Zone & Distance Patterns

> Do vendors closer to the stadium (Zone A) capture more of the surge than those on the Corner (B) or outer lots (C)? Identify the top and bottom vendors by revenue and investigate whether distance-to-stadium or vendor type explains the difference. Flag any outliers.

**Requires:** joining `vendor_locations` and `zone_capacity` with orders, grouping by zone/distance, comparison chart, outlier identification, interpretation.

### Question 5: Signal vs. Artifact — The Rain Poncho Investigation

> Rain ponchos show a huge surge on rainy game days. Using the SQLite `inventory_levels` table, investigate whether the poncho numbers are a genuine demand signal or distorted by SKU fragmentation and restock/stock-out effects. Present evidence and make a recommendation: should the Alliance pre-stock more ponchos next season?

**Requires:** querying SQLite with `sqlite3` + `pandas`, cross-referencing orders vs. inventory, SKU/category audit, evidence-based argument, supporting visualization.

---

## Deliverables

### 1. Python Notebook

A single Kaggle/Colab notebook (`.ipynb`) with:

- **Data Loading:** all CSVs via pandas, SQLite via `sqlite3`, weather via `requests`
- **Cleaning Pipeline:** clear section showing dedup, SKU consolidation, timestamp/type fixes, missing-value handling, vendor-ID normalization
- **Analysis:** all 5 questions with code, visuals, and Markdown explanations
- **Visualizations:** **minimum 6 plots** (`matplotlib`/`seaborn`)
- **SQLite step:** at least one real query against the provided database

### 2. Project README

Use `templates/PROJECT_README_TEMPLATE.md`. Include team, data sources, how to run, findings, recommendation.

### 3. Presentation

8 minutes per team. Tell the story: the mess, your pipeline, your recommendation.

### 4. Reflection Write-Up

In the notebook (at the end) or a separate Markdown file. Each member contributes. Answer all 5:

1. **Data Quality Impact** — Describe one issue you found. How did your cleaning decision change the outcome? What if you'd skipped it?
2. **ETL Trade-offs** — Pick one decision (SKU mapping, timestamp handling, weather join). What alternative existed, and how might results differ?
3. **Pipeline Trust** — If this pipeline had to run automatically every game day, what would break first?
4. **Business vs. Data** — For Q5 (ponchos), how did you weigh statistical evidence against business risk?
5. **Team Collaboration** — How did you divide work? What would another week change?

---

## 4-Week Timeline (suggested cadence)

### Week 1 — Ingestion, Exploration, Cleaning
- Read this brief; load all files; explore with `.shape`, `.dtypes`, `.info()`, `.describe()`, `.isnull().sum()`
- Catalog every data quality issue
- Start the cleaning pipeline; test your weather API calls
- **Milestone:** clean DataFrames + weather data retrieved

### Week 2 — ETL, Joins, SQLite
- Finish cleaning: dedup, SKU consolidation, timestamp/type fixes, vendor-ID normalization
- Join weather + vendor + zone data; load cleaned tables into SQLite
- **Milestone:** integrated dataset ready for analysis

### Week 3 — Analysis & Visualization
- Answer Questions 1–5; query SQLite for Q5
- Build all visualizations (≥6); write Markdown explanations
- **Milestone:** all questions answered with support

### Week 4 — Polish, Present, Submit
- Clean up the notebook; write reflections; prepare the 8-minute talk
- Restart kernel + Run All; peer review
- **Milestone:** final notebook + README submitted, presentation delivered

---

## Grading Rubric

| Component | Weight | What We're Looking For |
|-----------|--------|------------------------|
| **Data Pipeline & Cleaning** | 25% | Thorough pipeline: dedup, SKU consolidation, timestamp/type fixes, vendor-ID normalization; clear code; SQLite load |
| **API Integration** | 10% | Working API calls, parsing, date/timezone alignment, meaningful join |
| **Analytical Questions (1–5)** | 25% | Correct methodology, sound reasoning, decision-focused answers |
| **Visualizations** | 15% | ≥6 plots, right chart types, labeled, visual storytelling |
| **Presentation** | 10% | Clear 8-minute talk; defensible recommendation |
| **Reflection Write-Up** | 15% | Specific, thoughtful; pipeline-risk and teamwork insight |

---

## Important Notes

- **Do not fabricate data.** All weather data must come from a real API call shown in your notebook.
- **Show your work.** Messy input → cleaning steps → clean result, all visible.
- **Comment code** where logic is non-obvious; don't narrate every line.
- **Cite your weather API** (name + base URL).
- The supplied data is synthetic but designed to mirror the patterns of a real game-day vendor economy.

---

## Repository Structure

```
08-capstone-gameday/
├── DS2002_Capstone_Project_Brief.md      <- you are here
├── 2026-11-16 — Capstone Starter — Template.ipynb
├── data/
│   ├── gameday_orders.csv                <- ~19,000 messy order records
│   ├── vendor_locations.csv              <- vendor metadata + geography
│   ├── menu_catalog.csv                  <- item/SKU reference (fragmented)
│   ├── zone_capacity.csv                 <- stadium zone capacities
│   └── inventory_and_sales.db            <- SQLite: daily_sales_summary + inventory_levels
└── [your_team_notebook].ipynb            <- your deliverable
```

Good luck. Go Hoos — and stock the ponchos.
