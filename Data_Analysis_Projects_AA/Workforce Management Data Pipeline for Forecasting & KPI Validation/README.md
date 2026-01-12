# Workforce Management Data Pipeline for Forecasting & KPI Validation

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

