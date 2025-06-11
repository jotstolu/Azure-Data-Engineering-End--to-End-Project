# Azure-Data-Engineering-Project using Netflix dataset
An end-to-end data engineering pipeline on Microsoft Azure leveraging the publicly available Netflix dataset. This project covers:
- Data Ingestion (Bronze)
- Data Processing & Cleaning (Silver)
- Data Quality & Delivery (Gold)
- Automation & Orchestration

**Medallion Layers**:

| Layer  | Purpose                                                    |
| ------ | ---------------------------------------------------------- |
| Bronze | Ingest raw data (Autoloader & ADF) into Delta format      |
| Silver | Clean, dedupe, enrich; enforce schemas with PySpark       |
| Gold   | Apply Delta Live Tables for quality checks & aggregations |

---

# PROJECT ARCHITECTURE
![project_architecture](https://private-user-images.githubusercontent.com/121890747/453922655-d477b71f-1c48-49cc-813c-f3f8d273f512.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDk2NDczNjUsIm5iZiI6MTc0OTY0NzA2NSwicGF0aCI6Ii8xMjE4OTA3NDcvNDUzOTIyNjU1LWQ0NzdiNzFmLTFjNDgtNDljYy04MTNjLWYzZjhkMjczZjUxMi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwNjExJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDYxMVQxMzA0MjVaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT05ZjQ0MGQ4NzBlODg5OGNmM2Y3YzVjZjQ3NzE1MGRlYTdiM2QzNjRlNjliYmQ4NWJmZDZlMjAwYzdiZGI2NjkzJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.o_AOy24VHabxYMCIH92hLKyjefmbsJxFLNmOOg0tbFg)


### Phase 1: Bronze (Raw Ingestion)
![adf_data_pipeline](https://private-user-images.githubusercontent.com/121890747/453949832-92fd0b31-0ee6-4218-9580-efebdbfe61cd.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDk2NTA3MDYsIm5iZiI6MTc0OTY1MDQwNiwicGF0aCI6Ii8xMjE4OTA3NDcvNDUzOTQ5ODMyLTkyZmQwYjMxLTBlZTYtNDIxOC05NTgwLWVmZWJkYmZlNjFjZC5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwNjExJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDYxMVQxNDAwMDZaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT00YTgxYWUwYjk1YTc5NTgzZTYwZGQ0OWExMjhhZmNkMTY3NzczZWE5NzE5ZWVlMWM2ZTNmOWM3ZGE1OGU0ZDNiJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.guq5ZKv3XdIzAsVLl_E8E7tAtAJ83T8QMlpzkNzXZvY)


- **Sources**  
  - `Netflix_titles.csv` in ADLS Gen2 (`rawdata/Netflix_titles.csv`)  
  - Lookup tables (directors, cast, categories, countries) from `github`

- **Orchestration**  
  - Azure Data Factory pipelines using **Copy Data**, **ForEach**, **validation** and **If Condition** activities  
  - Parameterized datasets & pipelines for reusability  

- **Autoload**  
  - Incremental ingest of new CSV files into `bronze.netflix_titles_delta`using **Databricks Autoloader**

- **Storage**  
  - All raw ingestions stored as Delta tables in the `bronze/` container  

### Phase 2: Silver (Cleansing & Enrichment)
![silver_pipeline](https://private-user-images.githubusercontent.com/121890747/453961432-b53605ce-25fc-49b1-8275-8ca2972cd7c6.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDk2NTcwMTIsIm5iZiI6MTc0OTY1NjcxMiwicGF0aCI6Ii8xMjE4OTA3NDcvNDUzOTYxNDMyLWI1MzYwNWNlLTI1ZmMtNDliMS04Mjc1LThjYTI5NzJjZDdjNi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwNjExJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDYxMVQxNTQ1MTJaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT02MzUyMWU3Mjg4YmRhOTRiYzIwMTkxZmM4ZDNjYTk2OGU2NGE0MjBlMGY2MzUzMzgwNGEzNGM1MjRiYmViNGViJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.rNylI7gDIDwaRHRjvRvXEoJDrHmUcmOGZ3Lv8sjvT60)
**Compute**  
  - Azure Databricks PySpark notebooks  

- **Transformations**  
  - Split multi-valued columns (e.g., rating)  
  - Remove duplicates, filter invalid records  
  - filling of null values  
  - Cast of data types for analytics readiness  

- **Orchestration**  
  - Databricks Workflows chaining parameterized notebooks  

- **Output**  
  - Cleaned Delta tables in the `silver/` container  

### Phase 3: Gold (Quality & Aggregation)
![dlt_pipeline](https://private-user-images.githubusercontent.com/121890747/453973262-1517a8c5-dce5-4000-ba5a-6ec1aa6f519b.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDk2NTY4MjQsIm5iZiI6MTc0OTY1NjUyNCwicGF0aCI6Ii8xMjE4OTA3NDcvNDUzOTczMjYyLTE1MTdhOGM1LWRjZTUtNDAwMC1iYTVhLTZlYzFhYTZmNTE5Yi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwNjExJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDYxMVQxNTQyMDRaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1iODMxZGJmNDgzYzJhMDczYzE0MGRkODlmMWYxMWQxMmRkZTJiNWRkOGYxZmYzZWQwYzQ3OGVmZjJjN2FiOWQ1JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.A5qRYsv90QcTJk2jEUQ3fH-FBY9eUYrrHykDIoTP_n4)
- **Framework**  
  - Delta Live Tables (DLT) for declarative pipelines  

- **Data Quality**  
  - Define **Expectations** (e.g., `NOT NULL`, `UNIQUE` etc.)  
  - Configure actions:  `drop`


---

## Technology Stack

| Component                 | Purpose                                   |
| ------------------------- | ----------------------------------------- |
| Azure Data Factory (ADF)  | Data orchestration & ingestion            |
| Azure Data Lake Storage   | Scalable storage for Delta tables         |
| Azure Databricks          | Spark-based ETL & Delta Live Tables       |
| Delta Lake                | ACID-compliant, performant data format    |
| Python / PySpark          | Data transformation logic                 |

