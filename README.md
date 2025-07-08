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

![img2](https://github.com/jotstolu/Azure-Data-Engineering-End--to-End-Project/blob/main/images/silver_notebook_databricks_job.png?raw=true)


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

![DLT_Pipeline](https://github.com/jotstolu/Azure-Data-Engineering-End--to-End-Project/blob/main/images/dlt_pipeline.png?raw=true)


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

