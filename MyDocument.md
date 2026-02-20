# MyDocument.md

## Project Overview
This project analyzes and processes the NYC Jobs dataset using PySpark.  
It includes data exploration, data cleaning, feature engineering, visualizations, and test cases.  
The goal is to build a clean, reusable data processing pipeline and extract insights from the dataset.

## What I Did

### 1. Data Exploration
- Loaded the raw dataset
- Computed KPIs:
  - Total job postings
  - Top job categories
  - Salary statistics
  - Degree requirements  : Here both in KPI 1 & 5 i temporarily disabled the degree_mapping because while executing data processing its showing error due to mismatch col name ie Mininmum Rual Requirements 
  - Posting year distribution
- Identified missing values and data quality issues

### 2. Data Processing
I built a reusable PySpark pipeline that performs:

- **Column name cleaning**  
  Converted all column names to lowercase and removed special characters.

- **Salary casting**  
  Converted salary ranges to integers and computed `avg_salary`.

- **Text cleaning**  
  Removed newline characters and trimmed text fields.

- **Degree level mapping**  
  Converted degree requirements into numeric levels (1–4).

- **Posting year extraction**  
  Converted posting_date to timestamp and extracted posting_year.

- **Dropping unused columns**  
  Removed irrelevant fields such as recruitment_contact, additional_information, etc.

- **Saving processed data**  
  Saved output in:
  - CSV format → `/output/processed_jobs`
  - Parquet format → `/output/processed_jobs_parquet`

### 3. Feature Engineering
Created the following features:
- `avg_salary`
- `degree_level`
- `posting_year`

### 4. Visualizations
Generated charts using Pandas, Matplotlib, and Seaborn:
- Top 10 job categories
- Salary distribution by category
- Salary trend by posting year
- Most frequent skills

### 5. Test Cases
Implemented 5 tests:
- clean_column_names
- cast_salary_columns
- map_degree_level
- extract_posting_year
- process_data (end-to-end)

All tests passed successfully.

---

## Challenges Faced
- Column names contained special characters (/, -, #)
- Multiple versions of functions caused conflicts in the notebook
- Posting dates had inconsistent formats
- Some text fields contained newline characters
- Some job postings had missing salary or degree information

---

## Learnings
- How to build a clean PySpark data processing pipeline
- Importance of consistent column naming
- How to debug PySpark errors (AnalysisException, NameError)
- How to write unit tests for Spark DataFrames
- How to visualize Spark data using Pandas

---

## Assumptions
- Salary ranges are valid numeric values
- Degree keywords appear in minimum qualifications
- Posting dates follow a consistent timestamp format after cleaning

---

## Deployment / Trigger Approach
If this pipeline were deployed:
- I would package the functions into a Python module
- Trigger the pipeline using:
  - Airflow DAG
  - Cron job
  - Azure Data Factory pipeline
- Input → raw CSV  
- Output → cleaned Parquet dataset

---

## Conclusion
This project demonstrates end‑to‑end data processing using PySpark, including exploration, cleaning, feature engineering, visualization, and testing.  
The final dataset is clean, structured, and ready for downstream analytics or machine learning.
