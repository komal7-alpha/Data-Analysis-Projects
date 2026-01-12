
# Data Synthesis – Workforce Management Base Dataset

## What is Parquet? (Background)

Parquet is a columnar storage file format widely used in big data ecosystems such as Apache Spark and Hadoop.  
It is designed for efficient storage and fast analytical queries on very large datasets.

However, during early development, exploration, and debugging phases, CSV files are preferred because they are:
- Human-readable
- Easy to inspect in Excel or text editors
- Simple to version-control
- Tool-agnostic

In this project, **CSV is intentionally used at this stage** to simplify development and validation.  
Parquet may be introduced later once the pipeline is production-stable.

---

## Purpose of This Folder

This folder represents **Step 1 of the overall Workforce Management (WFM) data pipeline**.

The objective of this step is to generate a **large, realistic, synthetic WFM base dataset** that:
- Reflects real enterprise operational behavior
- Preserves data privacy and confidentiality
- Is large enough to justify PySpark usage
- Acts as the foundation for all downstream processing

At this stage:
- No fuzzy logic is applied
- No holiday data is merged
- No SQL Server loading is performed

Only the **base dataset** is created here.

---

## Output of This Step

The script generates a single CSV dataset:

```

finaldataset.csv

````

This dataset will later be enriched with:
- Fuzzy queue standardization
- Holiday indicators
- Business KPI logic
- SQL Server ingestion

---

## Base Dataset Schema (Locked)

The dataset generated in this step strictly follows the schema below:

| #  | Column Name            | Description                          |
| -- | ---------------------- | ------------------------------------ |
| 1  | Date                   | Activity / demand date               |
| 2  | Weekday                | Day of week                          |
| 3  | Week_Number             | ISO week number                      |
| 4  | Month                  | Month name                           |
| 5  | Country                | Country / region                     |
| 6  | Region                 | AMER / EMEA / APJ                    |
| 7  | Queue_Name              | Raw queue label                      |
| 8  | Channel                | Voice / Chat / Email                 |
| 9  | All_Units_Filter        | High-level business grouping         |
| 10 | Queue_Units_Filter      | Channel-level grouping               |
| 11 | ASU_Tier_Filter         | Tier1 / Tier2 / Tier3                |
| 12 | Demand                 | Primary workload metric              |
| 13 | Contact_Count          | Secondary operational metric         |

No additional columns are created at this stage.

---

## Script Explanation: `data_synthesis_pyspark.py`

This script is written using PySpark to ensure scalability for large datasets.

---

### 1. Spark Session Initialization

A Spark session is created to enable distributed data processing.  
This allows the script to scale to millions of records without memory issues.

```python
spark = SparkSession.builder.appName("WFM_Data_Synthesis").getOrCreate()
````

---

### 2. Date Range Generation

A continuous daily date range is generated from 2021 to 2024.
This forms the **time backbone** of the dataset and enables time-series analysis.

Each date represents one operational day.

---

### 3. Country and Region Mapping

A fixed mapping of countries to regions (AMER, EMEA, APJ) is defined.
This reflects how enterprises organize workforce data geographically.

This mapping is later used for grouping, reporting, and regional analysis.

---

### 4. Queue Definitions (Raw / Messy)

Raw queue names are intentionally created in an inconsistent format.
This simulates real enterprise systems where naming standards are not strictly enforced.

Each queue is associated with:

* Channel (Voice / Chat / Email)
* Business grouping filters
* Tier classification

These attributes are critical for later KPI and SLA logic.

---

### 5. Cartesian Expansion (Scale Creation)

A Cartesian join is performed across:

* Dates
* Countries
* Queues

This creates a **large operational dataset**, simulating daily activity across all queues and regions.

This step is the primary reason PySpark is used.

---

### 6. Time Attribute Derivation

Additional time-related columns are derived from the Date column:

* Weekday
* ISO week number
* Month name

These attributes are required for:

* Seasonality detection
* Weekly planning
* Trend analysis

---

### 7. Demand Generation Logic

Synthetic demand values are generated using randomized logic based on channel type:

* Voice queues have higher demand
* Email queues have moderate demand
* Chat queues have comparatively lower demand

Weekend demand is reduced to reflect real-world behavior.

The final Demand column represents the **primary forecasting metric**.

---

### 8. Contact Count Calculation

Contact_Count is derived as a realistic proportion of Demand.
This represents actual handled workload and is useful for validation and quality checks.

---

### 9. Final Column Selection

Only the locked base schema columns are selected and ordered.
No derived or enrichment columns are added in this step.

This ensures schema stability for downstream pipelines.

---

### 10. CSV Output Generation

The final dataset is written as a CSV file named `finaldataset.csv`.
CSV is chosen intentionally for ease of inspection and iterative development.

---

## What This Step Does NOT Do

* No queue standardization
* No fuzzy matching
* No holiday enrichment
* No KPI calculation
* No forecasting
* No SQL Server integration

These steps are handled in later pipeline stages.

---

## Next Step

The output of this folder will be used in the next stage of the pipeline, where:

* Fuzzy logic will standardize queue names
* Holiday data will be merged
* The enriched dataset will be prepared for SQL Server ingestion

```
