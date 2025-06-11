# Azure-Data-Engineering-Project
This is an End to End Project which involved Data Ingestion, Data Storage, Data Processing / ETL Pipelines, Data Transformation & Cleaning, Data Orchestration & Automation and Data Delivery. This project makes use of the popular Netflix dataset, which contains the main source file (Netflix_titles) and four other look up tables - Netflix directors, Netflix category, Netflix cast, and Netflix countries. 
A Medallion architecture was used to structure and organize data effectively:

Bronze Layer: Raw data ingested directly from the source and Github repository API, stored in Delta format.
Silver Layer: Cleaned and normalized data, removing duplicates and handling missing values, ensuring it’s ready for analytics.
Gold Layer: Aggregated and enriched data tailored to specific business needs, such as adding in country codes.

https://github.com/jotstolu/Azure-Data-Engineering-Project-/edit/main/Project%20Archictecture.png

