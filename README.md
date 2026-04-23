# Refugee Movement Analysis

**UC Berkeley MIDS — W205 Fundamentals of Data Engineering**  
Shanti Agung · Rahil Sharma · Sai Sriya Mudigonda

---

## Overview

This project analyzes global refugee and forced displacement patterns using a **multi-database architecture** built around UNHCR population data spanning 2010–2022. Rather than a single relational pipeline, each database was chosen for a specific analytical scenario where a relational model would be a poor fit.

---

## Data Source

- **TidyTuesday** — `refugees` R package accessing data from the [UNHCR](https://www.unhcr.org/)
- Population statistics: 2010–2022 (212 countries, 4,622 country-pair relationships in 2022)
- Tracks: refugees, asylum seekers, returned refugees, IDPs, stateless persons

---

## Database Architecture

### Neo4j — Graph Database

**Scenario:** Analyze refugee flow networks and identify the most influential countries in granting asylum or generating displacement.

A relational model is poorly suited for graph traversal and centrality analytics across thousands of directed, weighted relationships. Neo4j's native graph structure makes PageRank and shortest-path queries first-class operations.

**Graph 1 (Origin → Asylum):** Directed, weighted, mostly connected, cyclic  
- Year 2022: 212 nodes · 4,622 relationships

**Graph 2 (Asylum → Origin):** Reverse direction for "creating refugees" analysis

**PageRank — Most Influential Asylum-Granting Countries:**

| Period | #1 | #2 | #3 |
|--------|----|----|-----|
| Baseline (2010) | USA (0.286) | GFR (0.153) | CAN (0.134) |
| Syrian War (2015) | USA (2.272) | CAN (2.108) | GFR (1.531) |
| COVID (2020) | USA (2.166) | GFR (1.787) | CAN (1.719) |
| Ukraine (2022) | USA (1.971) | GFR (1.681) | CAN (1.589) |

**PageRank — Most Influential Refugee-Creating Countries:**

| Period | #1 | #2 | #3 |
|--------|----|----|-----|
| Baseline (2010) | IRQ (1.352) | SOM (1.286) | COD (1.272) |
| Syrian War (2015) | SYR (1.42) | AFG (1.331) | SOM (1.279) |
| COVID (2020) | SOM (0.181) | ETH (0.044) | ERT (0.041) |
| Ukraine (2022) | AFG (1.352) | SYR (1.304) | IRQ (1.221) |

> **Key finding:** The US, Germany, and Canada consistently ranked as the top three asylum-granting nations across all four geopolitical periods. Afghanistan, Syria, and Iraq/Somalia were the most persistent refugee-generating countries.

---

### MongoDB — Document Store

**Scenario:** Enrich each migration record with flexible contextual metadata — policy changes, wars, natural disasters — that varies by country and year.

A relational schema would require multiple sparse tables with large amounts of null data for attributes that only apply to certain countries. MongoDB's flexible schema handles heterogeneous metadata cleanly.

**Views implemented:**
- Country by Year (with context flags: `war`, `natural_disaster`, `policy_change`)
- Immigration Stats per Country (refugees + asylum_seekers per destination)

```json
{
  "2020": {
    "SYR": { "context": { "war": "true", "policy_change": "true" } },
    "GBR": { "context": { "war": "false", "natural_disaster": "false" } }
  }
}
```

---

### Redis — In-Memory Cache

**Scenario:** Store real-time migration event data, serve live dashboard statistics with millisecond latency, track geospatial last-known locations, and publish threshold alerts via Pub/Sub.

A relational database is too slow for volatile, constantly-changing counters and real-time event state. Redis is the appropriate tool for low-latency reads/writes where millisecond updates matter.

**Use cases:**
1. Real-time refugee border-crossing counters from migration sensors
2. High-speed caching to serve dashboards without re-querying the main store
3. Geospatial tracking of last known locations of refugee groups
4. Pub/Sub notifications when migration thresholds are exceeded

---

## Tools & Technologies

| Layer | Technology |
|-------|-----------|
| Graph database | Neo4j · Cypher |
| Document store | MongoDB |
| In-memory cache | Redis |
| Graph analytics | NetworkX · PageRank |
| Data wrangling | Python · pandas |
| Data source | UNHCR via `refugees` R package (TidyTuesday) |

---

## Repository Structure

```
.
├── code/
│   ├── population.csv           # UNHCR source data
│   ├── neo4j/                   # Graph modeling & Cypher queries
│   ├── mongodb/                 # Document schema & views
│   ├── redis/                   # Caching & Pub/Sub design
│   └── analysis/                # NetworkX PageRank, trend analysis
└── presentations/
    └── Presentation_DATASCI205.pdf
```

---

## Requirements

- Neo4j (community or AuraDB)
- MongoDB
- Redis
- Python: `neo4j`, `pymongo`, `redis`, `networkx`, `pandas`
