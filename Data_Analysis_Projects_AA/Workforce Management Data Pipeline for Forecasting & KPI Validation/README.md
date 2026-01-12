# Workforce Management Data Pipeline for Forecasting & KPI Validation

## Disclaimer
This project reflects the type of data automation and KPI validation workflows I work on in my current role as an Associate Solutions Engineer-Data Analyst at Aligned Automation Ltd.

The implementation in this repository is a recreated, generalized version built solely for demonstration and learning purposes. No proprietary data, internal systems, client information, or confidential business logic from Aligned Automation Ltd. has been used.

All datasets used in this project are either synthetically generated or sourced from publicly available online resources. The project focuses only on showcasing the technical approach, data processing logic, and system design patterns used in real-world scenarios while fully maintaining organizational data privacy and confidentiality.

---

## Project Overview

This project represents an **enterprise-style Workforce Management (WFM) data pipeline**, designed to replicate real-world analytics workflows used for forecasting preparation and KPI validation.

The objective of this project is **not to build a forecasting model**, but to build a **forecasting-ready, SQL-based master dataset** by transforming raw, messy, multi-source WFM operational data into a clean, standardized, and automation-ready form.

All data used in this project is **public, synthetic, or externally sourced**.  
No real organizational data has been used.  
The project replicates **logic and workflow patterns only**, ensuring data privacy and confidentiality.

---

## Why This Project Exists

In real enterprise WFM environments:

- Data comes from **multiple sources**
- Queue names are **inconsistent and messy**
- Demand is affected by **holidays and regional factors**
- Manual Excel-based workflows are **slow, error-prone, and non-scalable**
- Forecasting and KPI validation require a **trusted historical dataset**

### Problem Statement

> How can raw, messy Workforce Management data be converted into a single, standardized, SQL-based dataset that supports forecasting, KPI validation, and automation — without relying on manual Excel processes?

---

## What This Project Builds

This project does **not** build a forecasting model yet.

This project builds a **forecasting-ready, enterprise-grade data pipeline**.

### Final Goal

Produce a **single master WFM dataset in SQL Server** that can later be used for:

- Future forecasting
- SLA and KPI validation
- Power BI dashboards
- Automated weekly data refresh

---

## Data Sources

### 1. WFM Operational Data (Large Dataset)

Core operational data containing:

- Date and week information
- Country and region
- Raw queue names
- Channel information (Voice / Chat / Email)
- Demand and contact volume metrics

This dataset is intentionally **large**, justifying the use of **PySpark** for scalable processing.

---

### 2. Holiday Data (Web Scraping)

- Global public holiday data (multiple countries, multi-year)
- Scraped once and stored as reference data
- Later merged using `Date + Country`

Holiday data helps explain:

- Demand drops or spikes
- Forecast bias
- KPI fairness during holidays

---

### 3. Business Queue & KPI Logic

Enterprise WFM systems require:

- Canonical queue structures
- Business unit groupings
- Tier/SLA classifications

This logic is applied later using **fuzzy matching and rule-based mappings**.

---

## Core Transformations (High Level)

### Queue Standardization (Later Stage)

Raw queue names such as:
India Core Email 
India Email Core
India Core Email


Are standardized into:
Matched_Queue = Core Email


This ensures consistency for forecasting and KPI analysis.

---

### Business Filters & Tiering

Business rules assign:

- **All_Units_Filter** – high-level business grouping  
- **Queue_Units_Filter** – channel-level grouping  
- **ASU_Tier_Filter** – Tier1 / Tier2 / Tier3 for SLA and KPI logic  

These rules typically originate from **business KPI or SLA documentation**.

---

### Holiday Enrichment (Later Stage)

Holiday indicators are added later using:
Date + Country


Resulting columns include holiday flags and descriptions.

---

## Final Base Dataset (Locked)

This is the **only dataset created at the initial stage** of the project.

### Base Table: `wfm_forecasting_base`

| #  | Column Name            | Meaning                      |
| -- | ---------------------- | ---------------------------- |
| 1  | Date                   | Activity / demand date       |
| 2  | Weekday                | Day of week                  |
| 3  | Week_Number            | ISO week number              |
| 4  | Month                  | Month name or number         |
| 5  | Country                | Country / region             |
| 6  | Region                 | AMER / EMEA / APJ (optional) |
| 7  | Queue_Name             | Raw queue label              |
| 8  | Channel                | Voice / Chat / Email         |
| 9  | All_Units_Filter       | Business grouping            |
| 10 | Queue_Units_Filter     | Channel grouping             |
| 11 | ASU_Tier_Filter        | Tier1 / Tier2 / Tier3        |
| 12 | Demand                 | Primary workload metric      |
| 13 | Contact_Count          | Secondary volume metric      |

---

## Not Included at Base Stage

The following columns are **not part of the base dataset** and are added later via pipeline logic:

- Matched_Queue
- Is_Holiday
- Holiday_Name
- Holiday_Type

---

## SQL Server Role

SQL Server acts as the **system of record**:

- Stores historical data (append-only)
- Supports stored procedures for KPI validation
- Acts as the data source for Power BI
- Enables automation and auditing

---

## Automation Vision (Future Scope)

Once the pipeline stabilizes:

- New WFM data arrives weekly
- The same pipeline logic runs
- Data is cleaned, enriched, and appended
- SQL tables are updated
- Power BI datasets refresh automatically
- Success or failure notifications are triggered

Initial orchestration can be done locally (scheduler), with future migration to cloud-based orchestration tools.

---

## Current Status

- Project scope finalized
- Base dataset schema locked
- Holiday enrichment strategy defined
- Fuzzy queue mapping placement confirmed
- SQL and automation vision established

---

## One-Line Summary

> I designed an enterprise-style Workforce Management data pipeline that consolidates large operational datasets, standardizes messy queue structures, enriches data with global holidays, and produces a single SQL-based, automation-ready dataset designed for forecasting and KPI analytics.

---

## Next Step

The next step is **data synthesis**:

1. Reference datasets will be reviewed  
2. A large, realistic synthetic WFM dataset will be generated  
3. Development will continue in VS Code with pipeline implementation  

---

