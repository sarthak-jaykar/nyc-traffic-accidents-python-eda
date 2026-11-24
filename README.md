# NYC Traffic Accidents 2020: When and Where Collisions Hurt People

When and where collisions cause injuries and deaths in New York City, who is hurt, and which contributing factors are recorded. Built in Python on 74,881 collisions from 1 January to 29 August 2020.

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?logo=numpy&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?logo=scipy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C)
![Domain](https://img.shields.io/badge/Domain-Road%20Safety%20%26%20BA-success)

---

## Project Overview

A city road safety programme needs to direct enforcement, engineering and campaign effort to where collisions cause the most harm. The 2020 data covers a year in which traffic changed sharply in spring, so the team also needs to know whether 2020 can serve as a baseline.

The analysis answers these business questions:

1. When do collisions and injuries happen?
2. How do boroughs compare?
3. Which contributing factors are most common?
4. Who is injured and killed?
5. Which streets and corridors have the most collisions?
6. Did the pattern change between early and spring 2020?

Scope covers collisions, casualties, factors, road users, locations and timing in 2020. Traffic volumes, speed data, population-based rates and causes of the spring change are out of scope.

## Dataset

**Dataset:** NYC motor vehicle collision extract covering 1 January to 29 August 2020, with 74,881 collision records and 29 source fields.

**Dataset note:** Secondary dataset obtained from publicly available online educational material and used for learning and portfolio analysis. The original CSV file is included in `data/`. The analysis is limited to the supplied date range.

## Key Metrics (1 January to 29 August 2020)

| KPI | Result |
| --- | --- |
| Collisions | **74,881** |
| People injured / killed | **27,448** / **144** |
| Collisions per day, January / April | **461** / **137** |
| Share of collisions with injuries, January to March / June to August | **about 22%** / **about 35%** |
| Collisions with borough recorded | **65.6%** (25,741 not recorded) |

## Key Findings

These findings describe the data. They show where and when injuries sit, not why, and they do not establish causal effects.

1. **Collisions fell sharply in spring, but the share with injuries rose.** The April to June average (197 per day) is 58% lower than January and February (466 per day), a statistically clear change. The injury rate rose from about 22% to about 35% over the same months.
2. **Pedestrians account for a large share of deaths.** Pedestrians are 14.9% of people injured but 38.9% of people killed (56 of 144). Motorists are 73.1% of injuries and 54.9% of deaths.
3. **Driver inattention is the largest recorded factor, but a quarter of collisions have no factor.** It accounts for 34.7% of collisions with a recorded factor. "Unspecified" covers 26% of all collisions.
4. **Late evening and evening collisions are more often injurious.** The injury rate is 34.0% from 20:00 to 23:59 and 29.7% from 16:00 to 19:59, against 24.4% at midday. Midday has the most collisions (25,533).
5. **Expressways dominate the streets with the most collisions.** Belt Parkway has 1,241 collisions and 689 injuries. Street names are missing for 26% of collisions, so these counts are a minimum.
6. **Borough injury rates differ, but a third of collisions have no borough.** Rates range from 24.5% in Manhattan to 30.4% in Staten Island, the smallest group. The "Not recorded" group is 34.4% of collisions.

## Recommended Actions

These are proposed management targets, not proven results.

| Priority | Area | Action | Proposed target |
| --- | --- | --- | --- |
| **P1** | Pedestrian safety | Prioritise pedestrian review on the corridors and time bands in findings 4 and 5. | Pedestrian deaths and serious injuries, monthly |
| **P1** | Late-evening enforcement | Test targeted enforcement in late-evening and evening periods on the highest-injury corridors. | Injury rate by time band, monthly |
| **P2** | Engineering | Review speed and merging design on the top three corridors, and fill missing street names before finalising the ranking. | Collisions and injuries per corridor, quarterly |
| **P2** | Data quality | Ask the data owner to improve factor and borough recording. | Share of collisions with a recorded factor and borough, monthly |
| **P3** | Baseline | Check speed and traffic-volume data for the same months, and do not treat 2020 as a baseline until a second year is available. | Injury rate per collision against a multi-year baseline |

## Tools and Methodology

- **Python:** pandas and NumPy for cleaning and aggregation; SciPy for the Mann-Whitney U test; Matplotlib and Seaborn for charts.
- **Pipeline:** five steps, run in order by `script/run_all.py`.

**Data model:** one row per collision, keyed on `COLLISION_ID`. Derived fields include month, weekday, time band, injury and fatality flags, and pedestrian and cyclist totals.

**Definitions**
- Collisions per day: collisions in a month divided by days in that month (August has 29).
- Injury rate: collisions with at least one person injured, divided by collisions.
- Share of casualties: a road user group's count divided by the total.

**Cleaning rules**
- Missing values are flagged and reported, never filled. Borough is kept as "Not recorded".
- Coordinates of zero (63 rows) are treated as missing.
- "Unspecified" is kept as its own category.
- Extreme injury counts are kept and listed, not removed.

**Validation**
- Totals reconcile: 74,881 collisions, 144 killed, 27,448 injured. Monthly and borough counts each sum to 74,881.
- A clean re-run reproduces all 10 result tables and all 6 charts exactly.

## Repository Structure

```text
nyc-traffic-accidents-python/
├── README.md
├── data/                              Original nyc_accidents_2020.csv dataset
├── documentation/
│   ├── 01_business_brief.md           Context, stakeholders, decisions, questions
│   ├── 02_data_and_quality.md         Source, fields, quality decisions, reconciliation
│   ├── 03_findings_and_recommendations.md  Metric definitions, findings, actions
│   ├── how_to_run.txt                 Setup and run guide (VS Code, Windows)
├── results/                           Analysis output tables
├── screenshots/                       Charts
├── script/
│   ├── common.py                      Paths and shared helpers
│   ├── 01_ingest.py                   Profile the raw file (prints to console)
│   ├── 02_clean.py                    Clean and flag issues (prints checks to console)
│   ├── 03_features.py                 Time bands, injury and fatality flags
│   ├── 04_analysis.py                 Answers to Q1 to Q6
│   ├── 05_charts.py                   Charts
│   └── run_all.py                     Runs steps 1 to 5
└── requirements.txt
```

## Analysis

| Business question | Code | Output |
| --- | --- | --- |
| Q1. When do collisions happen? | `script/04_analysis.py` | `results/q1_monthly.csv`, `q1_time_bands.csv`, `q1_weekday.csv` |
| Q2. How do boroughs compare? | `script/04_analysis.py` | `results/q2_borough.csv` |
| Q3. Which factors are most common? | `script/04_analysis.py` | `results/q3_factors_top.csv`, `q3_factors_injury_rate.csv` |
| Q4. Who is injured and killed? | `script/04_analysis.py` | `results/q4_road_users.csv` |
| Q5. Which streets have most collisions? | `script/04_analysis.py` | `results/q5_top_streets.csv` |
| Q6. Did the pattern change in spring? | `script/04_analysis.py` | `results/q6_period_comparison.csv` |

## Documentation

- [Business Brief](documentation/01_business_brief.md): stakeholders, decisions and questions
- [Data and Quality](documentation/02_data_and_quality.md): source, fields, quality decisions and reconciliation
- [Findings and Recommendations](documentation/03_findings_and_recommendations.md): metric definitions, findings and actions
- [How to Run](documentation/how_to_run.txt): setup and run guide

## How to Reproduce

```bash
pip install -r requirements.txt
```

1. The source dataset is already included at `data/nyc_accidents_2020.csv`; no download or rename is needed.
2. Run the full pipeline:

```bash
python script/run_all.py
```

Results are written to `results/` and charts to `screenshots/`. Full steps are in [documentation/how_to_run.txt](documentation/how_to_run.txt).

## Limitations

- One year of data with no baseline, so the spring change cannot be explained from this data.
- Borough is not recorded for 34% of collisions, and street names for 26%.
- Daily counts are not fully independent, so the Q6 test is indicative.
- Findings describe association. No causes are claimed.

## Author

[GitHub](https://github.com/sarthak-jaykar) · [LinkedIn](https://www.linkedin.com/in/sarthak-jaykar/)
