# Workforce Management Data Pipeline for Forecasting & KPI Validation

## Disclaimer

This project reflects the type of data automation, data preparation, and KPI validation workflows that I work on in my professional role as an Associate Solutions Engineer – Data Analyst at Aligned Automation Ltd.

The implementation in this repository is a **recreated and generalized version** built strictly for learning, demonstration, and portfolio purposes.

- No proprietary data
- No internal systems
- No client information
- No confidential business logic

All datasets used in this project are either:
- Synthetically generated, or
- Sourced from publicly available online resources

This project demonstrates **technical approach, system design, and workflow patterns only**, while fully maintaining organizational data privacy and confidentiality.

---

## Project Overview

This project represents an **enterprise-style Workforce Management (WFM) data pipeline** designed to replicate real-world analytics workflows used for:

- Forecasting preparation
- KPI and SLA validation
- Operational reporting
- Automation readiness

The goal of this project is **not to build a forecasting model** at this stage.

Instead, the goal is to build a **forecasting-ready, SQL-based master dataset** by transforming raw, messy, multi-source WFM operational data into a clean, standardized, and automation-ready form.

---

## Why This Project Exists

In real enterprise WFM environments, the following challenges are very common:

- Data arrives from **multiple sources** (Excel files, tools, systems)
- Queue names are **inconsistent and messy**
- Demand is affected by **country-level holidays**
- Manual Excel-based workflows are:
  - Slow
  - Error-prone
  - Not scalable
- Forecasting and KPI validation require a **trusted historical dataset**

### Problem Statement

> How can raw, messy Workforce Management operational data be converted into a single, standardized, SQL-based dataset that supports forecasting, KPI validation, and automation — without relying on manual Excel processes?

---

## What This Project Builds

This project does **not** build a forecasting model yet.

This project builds a **forecasting-ready, enterprise-grade data pipeline**.

### Final Goal

Produce a **single master WFM dataset in SQL Server** that can later be used for:

- Demand forecasting
- SLA and KPI validation
- Power BI dashboards
- Automated weekly data refresh

---

## Technology Stack and Reasoning

- **Python** – Core data processing logic
- **PySpark** – Large dataset generation and scalable processing
- **SQL Server** – System of record and historical storage
- **Excel** – Business realism and common enterprise input format
- **Web Scraping** – Public holiday reference data
- **Power BI** – Reporting and visualization (future stage)

### Why PySpark?

PySpark was intentionally chosen because:
- The dataset is large (lakhs to millions of rows)
- Memory-safe and scalable processing is required
- It reflects real enterprise data engineering practices
- It provides strong justification for distributed processing in interviews

---

## Data Sources Used

### 1. WFM Operational Reference Data

Reference datasets were reviewed to understand real-world behaviour, including:
- Call center analytics data
- Incoming, answered, and abandoned calls
- Service-level style metrics
- 311 / service request style datasets

Purpose:
- Understand real operational patterns
- Design a realistic base dataset structure

---

### 2. Holiday Data (Web Scraping)

- Global public holiday data
- Multiple countries
- Multi-year coverage
- Scraped once and stored as reference data

Holiday data is later merged using:

Date + Country

yaml
Copy code

Holiday data helps explain:
- Demand drops or spikes
- Forecast bias
- KPI fairness during holidays

---

### 3. Business Queue and KPI Logic

Enterprise WFM systems rely on:
- Canonical queue structures
- Business unit groupings
- Tier-based SLA logic

This logic is applied later using:
- Fuzzy matching
- Rule-based mappings
- Business documentation inputs

---

## Core Transformations (High-Level)

### Queue Standardization (Later Stage)

Raw queue names such as:

India Core Email
India Email Core
India Core Email DSP

sql
Copy code

Are standardized into:

Matched_Queue = Core Email

yaml
Copy code

This ensures:
- Consistent forecasting
- Correct KPI aggregation
- Clean reporting

---

### Business Filters and Tiering

Business rules assign:

- **All_Units_Filter** – High-level business grouping
- **Queue_Units_Filter** – Channel-level grouping
- **ASU_Tier_Filter** – Tier1 / Tier2 / Tier3 classification

These rules typically come from **business KPI or SLA documentation**.

---

### Holiday Enrichment (Later Stage)

Holiday indicators are added later using:

Date + Country

yaml
Copy code

Resulting columns include:
- Is_Holiday
- Holiday_Name
- Holiday_Type

---

## Final Base Dataset (Locked)

This is the **only dataset created at the initial stage** of the project.

### Base Table: `wfm_forecasting_base`

| #  | Column Name            | Meaning                      |
| -- | ---------------------- | ---------------------------- |
| 1  | Date                   | Activity / demand date       |
| 2  | Weekday                | Day of week                  |
| 3  | Week_Number             | ISO week number              |
| 4  | Month                  | Month name or number         |
| 5  | Country                | Country / region             |
| 6  | Region                 | AMER / EMEA / APJ            |
| 7  | Queue_Name              | Raw queue label              |
| 8  | Channel                | Voice / Chat / Email         |
| 9  | All_Units_Filter        | Business grouping            |
| 10 | Queue_Units_Filter      | Channel grouping             |
| 11 | ASU_Tier_Filter         | Tier1 / Tier2 / Tier3        |
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

## Pipeline Structure

01_data_synthesis
02_queue_standardization
03_holiday_enrichment
04_sql_ingestion
05_kpi_validation
powerbi

csharp
Copy code

Current focus is strictly on:

01_data_synthesis

yaml
Copy code

---

## Step 1 – Data Synthesis (Current Stage)

Objective:
- Generate a synthetic but realistic WFM dataset
- Large volume (lakhs / millions of rows)
- Enterprise-like behaviour:
  - Weekday vs weekend demand
  - Channel-wise variation
  - Country-level differences
  - Queue complexity

Output file:

finaldataset.csv

yaml
Copy code

---

## PySpark Setup Reality (Windows)

During development, the following setup issues were encountered:
- Java not installed
- JAVA_HOME not configured
- HADOOP_HOME / winutils requirement on Windows
- Version compatibility challenges with PySpark 4.x

Final understanding:
- PySpark works reliably only with proper setup
- Correct version alignment between Python, Java, and Spark is mandatory
- Hadoop/winutils configuration is critical on Windows systems

These setup steps are fully documented separately to avoid confusion.

---

## SQL Server Role

SQL Server acts as the **system of record**:

- Stores historical data (append-only)
- Supports stored procedures for KPI validation
- Serves as the Power BI data source
- Enables automation and auditing

---

## Automation Vision (Future Scope)

Once the pipeline stabilizes:

- New WFM data arrives weekly
- Same pipeline logic executes
- Data is cleaned, enriched, and appended
- SQL tables update automatically
- Power BI datasets refresh
- Success or failure notifications are triggered

Initial orchestration can be local, with future migration to cloud tools.

