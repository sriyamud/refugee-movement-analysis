# Refugee Movement Analysis

**UC Berkeley MIDS — W205 Fundamentals of Data Engineering**

---

## Project Overview

This project analyzes global refugee and forced displacement movement patterns using UNHCR population data. The analysis was built on a PostgreSQL data engineering pipeline that stages, cleans, and queries structured population data spanning multiple years (2010–2022).

The data tracks displaced populations by country of origin and country of asylum, including refugees, asylum seekers, internally displaced persons (IDPs), stateless persons, and other persons of concern.

---

## Data

Source: UNHCR Population Statistics (`population.csv`)

Key fields: country of origin, country of asylum, refugee counts, asylum seekers, returned refugees, IDPs, stateless persons — across years 2010–2022.

---

## Methods

- PostgreSQL data pipeline (staging, cleaning, transformation)
- Multi-year comparative queries (2010, 2015, 2020, 2022)
- Analysis of displacement trends by origin and destination country

---

## Repository Structure

```
.
├── code/
│   ├── population.csv           # UNHCR source data
│   ├── proj_3ref_1.ipynb        # Data ingestion & staging
│   ├── proj_3ref_2.ipynb        # Data cleaning & transformation
│   ├── proj_3ref_3_*.ipynb      # Trend analysis by year
│   └── proj_3ref_4_*.ipynb      # Cross-country movement analysis
└── slides/
```

---

## Requirements

- PostgreSQL
- Python: `psycopg2`, `pandas`, `numpy`
