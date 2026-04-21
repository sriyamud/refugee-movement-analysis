# Refugee Movement Analysis

**Team Members:** Sai Sriya Mudigonda, Rahil Sharma, Shanti Agung

**Data Source:** [UNHCR Population Statistics (2010-2022) via TidyTuesday](https://github.com/rfordatascience/tidytuesday/blob/main/data/2023/2023-08-22/readme.md)


## Project Overview

This project investigates global refugee movements in year 2010, 2015, 2020, and 2022. Using UNHCR data, we constructed directional graphs to explore migration patterns, identify key countries, and uncover structural relationships.

## Folder Structure

- **Notebook 1: Data Preparation**
  - Create and load the refugee staging table.

- **Notebook 2: Query Building**
  - Develop SQL queries on the refugee staging table to later build graphs.

- **Notebook 3: COO to COA Graph**
  - Directed graph: From Country of Origin to Country of Asylum
  - Graph algorithms: PageRank, Personalized PageRank, Harmonic Centrality, Louvain Modularity.

- **Notebook 4: COA to COO Graph**
  - Directed graph: From Country of Asylum to Country of Origin
  - Graph algorithms: PageRank, Personalized PageRank, Harmonic Centrality, Louvain Modularity.


