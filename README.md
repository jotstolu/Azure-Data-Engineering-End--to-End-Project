# Azure-Data-Engineering-Project
This is an End to End Project which involved Data Ingestion, Data Storage, Data Processing / ETL Pipelines, Data Transformation & Cleaning, Data Orchestration & Automation and Data Delivery. This project makes use of the popular Netflix dataset, which contains the main source file (Netflix_titles) and four other look up tables - Netflix directors, Netflix category, Netflix cast, and Netflix countries. 
A Medallion architecture was used to structure and organize data effectively:

Bronze Layer: Raw data ingested directly from the source and Github repository API, stored in Delta format.
Silver Layer: Cleaned and normalized data, removing duplicates and handling missing values, ensuring it’s ready for analytics.
Gold Layer: Aggregated and enriched data tailored to specific business needs, such as adding in country codes.

![project_architecture](https://private-user-images.githubusercontent.com/121890747/453922655-d477b71f-1c48-49cc-813c-f3f8d273f512.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDk2NDczNjUsIm5iZiI6MTc0OTY0NzA2NSwicGF0aCI6Ii8xMjE4OTA3NDcvNDUzOTIyNjU1LWQ0NzdiNzFmLTFjNDgtNDljYy04MTNjLWYzZjhkMjczZjUxMi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwNjExJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDYxMVQxMzA0MjVaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT05ZjQ0MGQ4NzBlODg5OGNmM2Y3YzVjZjQ3NzE1MGRlYTdiM2QzNjRlNjliYmQ4NWJmZDZlMjAwYzdiZGI2NjkzJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.o_AOy24VHabxYMCIH92hLKyjefmbsJxFLNmOOg0tbFg)


