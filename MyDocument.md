# MyDocument.md

## Project Overview
This project analyzes and processes the NYC Jobs dataset using PySpark inside a Docker-based Spark environment. The work includes data exploration, data cleaning, feature engineering, KPI extraction, and generating final output files in CSV and Parquet formats. The goal was to build a clean, reusable data processing pipeline and deliver accurate insights from the dataset.

## What I Did

### 1. Data Exploration
- Loaded the raw NYC Jobs dataset from `/dataset/nyc-jobs.csv`
- Computed key KPIs:
  - Total job postings
  - Top job categories
  - Salary statistics
  - Degree requirements
  - Posting year distribution
- Identified missing values and data quality issues
- Understood schema inconsistencies and fields requiring transformation

### 2. Data Processing
I built a reusable PySpark pipeline that performs:
- Column standardization
- Trimming and cleaning string fields
- Type conversions for numeric columns
- Salary range extraction
- Average salary computation
- Filtering invalid or incomplete rows
- Generating a final cleaned DataFrame with 2946 rows

This DataFrame was used for all KPI calculations and final outputs.

## Challenges Faced

### 1. Spark Write Issue in Docker Cluster
While writing the final DataFrame using Spark:
processed_df.write.csv(...)
processed_df.write.parquet(...)

Spark only produced:
_SUCCESS
._SUCCESS.crc
and no part files

### Root Cause
The Spark cluster was running in multi-container mode, where:
- The driver container (Jupyter) had access to the filesystem
- The executor containers did not share the same mounted volumes

Because executors could not see the output directory, they failed silently when writing part files. Only the driver wrote metadata (`_SUCCESS`), resulting in empty output folders.

This is a known limitation when Docker volumes are not shared across all Spark containers.

## Workaround Implemented

To ensure the final output files were generated correctly for submission, I used a driver-side fallback:

### 1. Convert Spark DataFrame to Pandas
```python
pdf = processed_df.toPandas()

 Output files
pdf.to_csv("processed_jobs.csv", index=False)
pdf.to_parquet("processed_jobs.parquet")


