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
