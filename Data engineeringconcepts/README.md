# 📘 Day 1: Data Engineering ETL & Orchestration Notes

## 🧠 Topic

Daily CSV → BigQuery ETL Pipeline with GCP

## 🚀 Overview

This document summarizes the discussion and steps for designing a robust ETL pipeline using GCP tools, suitable for interview preparation.

## ⚙️ ETL Steps

### 1️⃣ Extract

* Pull CSV from **GCS**.
* Use **Airflow sensor operator** (or alternate orchestration tool) to check file availability.

### 2️⃣ Transform / Validate

* Perform **basic data checks**:

  * Null checks
  * Duplicate checks
  * Delimiter/schema validation
* Optional enrichment (e.g., ingestion timestamp).

### 3️⃣ Error Handling

* Use **`max_bad_records`** to allow minor errors without failing the load.
* Major failures captured in a **dead-letter table / staging table**.

### 4️⃣ Load

* Insert validated data into the **main BigQuery table**.
* Supports append or overwrite as required.

### 5️⃣ Logging / Monitoring

* Track:

  * Source row count
  * Target row count
  * Count differences / discrepancies
  * Pipeline success/failure
* Helps with **audit and debugging**.

### 6️⃣ Post-Validation

* Verify **record tagging and completeness** in the target table.

## 🕓 Orchestration & Scheduling

* **Tool:** Airflow (Cloud Composer) or alternate platform
* **Scheduling:** CRON expressions (hourly, daily, monthly, yearly)
* **Retries & Retry Delay:**

  * Example: `retries=3`, `retry_delay=60 sec`
  * Ensures transient failures are handled and resources are freed between retries

## 💡 Interview Tips

* Explain the pipeline in **6 concise steps**.
* Highlight **error handling, logging, and orchestration**.
* Use **Airflow DAG / alternate tool** terminology to show practical experience.
* Keep explanation within **30–40 seconds**.

## 🧩 References / Best Practices

* Use **dead-letter table / staging table** for failed records.
* Always log source vs target counts for validation.
* Use `max_bad_records` only for minor/occasional errors.
* Include retry logic in orchestration to handle transient failures.

📘 Day 2: BigQuery Concepts
🧩 1. Partitioning

Partitioning helps in managing and querying large datasets efficiently by splitting data into segments.

Types:

Ingestion-time partitioning

Column-based partitioning (DATE, TIMESTAMP, or DATETIME)

Integer-range partitioning

Benefits: Faster queries, lower cost, and easier data management.

Tip: Always use partition filters to avoid full scans.

🧮 2. Clustering

Clustering organizes data within partitions (or tables) based on specific column values.

Up to 4 clustering columns allowed.

Ideal for high-cardinality fields like user_id, country, product_id.

Benefits:

Improves query performance

Reduces scan cost

Works best when combined with partitioning

🧰 3. Schema Evolution

Managing schema changes in BigQuery tables over time.

Allowed:

Add new nullable columns

Change mode from REQUIRED → NULLABLE

Not allowed:

Removing or renaming columns directly

Best Practice:

Use CREATE OR REPLACE TABLE for full updates

Keep schema files under version control (.json format)

⚙️ 4. Cost Optimization Tips

💡 BigQuery Best Practices:

Avoid SELECT * — always query required columns.

Use Table Preview for inspection.

Combine Partitioning + Clustering for best results.

Use Materialized Views for repeated heavy queries.

Monitor query slots and use reservations for predictable workloads.

🧩 5. Real-World Example

Daily Sales Data scenario:

Partition table by sale_date

Cluster by region, product_id

Keep last 90 days in main table

Archive older data into staging/dead-letter table for reference
