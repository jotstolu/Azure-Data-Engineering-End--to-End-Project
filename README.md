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

![img](https://github.com/jotstolu/Azure-Data-Engineering-End--to-End-Project/blob/main/images/Project%20Archictecture.png?raw=true)




### Phase 1: Bronze (Raw Ingestion)
![adf_data_pipeline](https://github.com/jotstolu/Azure-Data-Engineering-End--to-End-Project/blob/main/images/adf_datapiipeline.png?raw=true)


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

![img2](https://private-user-images.githubusercontent.com/121890747/453961432-b53605ce-25fc-49b1-8275-8ca2972cd7c6.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDk5MDE3NzYsIm5iZiI6MTc0OTkwMTQ3NiwicGF0aCI6Ii8xMjE4OTA3NDcvNDUzOTYxNDMyLWI1MzYwNWNlLTI1ZmMtNDliMS04Mjc1LThjYTI5NzJjZDdjNi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwNjE0JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDYxNFQxMTQ0MzZaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0zYzQxM2IzNTZlZGYwZTRkMTNhMzM2ODNmZTcwYThlY2UwMDc2Zjg5ODNiMzdlYjBhMTEwZmI1MzFlZWU1N2JjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.oSmzgBzVxihkFwmw-KssO4YQCdlAx-NbAmrIxbDJZIY)


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

![img_3](https://private-user-images.githubusercontent.com/121890747/453973262-1517a8c5-dce5-4000-ba5a-6ec1aa6f519b.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDk5MDE4NjEsIm5iZiI6MTc0OTkwMTU2MSwicGF0aCI6Ii8xMjE4OTA3NDcvNDUzOTczMjYyLTE1MTdhOGM1LWRjZTUtNDAwMC1iYTVhLTZlYzFhYTZmNTE5Yi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwNjE0JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDYxNFQxMTQ2MDFaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT05Yjg2OGFiZjM4MmM2MTYwN2VhYzQ1MTBmM2E3NWQ1YmQzZThhZmNiNDA3ZmNmNDcyZDgwOWQ1MTM3YjMwYWJlJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.Vob8sGBVieQQ2XPVyRj7Z0avs7lIl64J5b6Y8qz9lSo)


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

