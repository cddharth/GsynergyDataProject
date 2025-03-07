# GsynergyData Project

This project demonstrates a **data pipeline** using **Databricks**, **Azure Blob Storage**, and **Delta Lake**. The pipeline ingests raw data, transforms it, and creates refined datasets for reporting and analytics. The project is divided into three layers: **Bronze**, **Silver**, and **Gold**.

---

## Table of Contents
1. [Problem Statement](#problem-statement)
2. [Tools and Resources Used](#tools-and-resources-used)
3. [Project Structure](#project-structure)
4. [Steps to Reproduce the Project](#steps-to-reproduce-the-project)
5. [Azure Blob Storage Setup](#azure-blob-storage-setup)
6. [Databricks Notebooks](#databricks-notebooks)
7. [Delta Lake Integration](#delta-lake-integration)
8. [Incremental Processing](#incremental-processing)
9. [Git Repository](#git-repository)
10. [Future Enhancements](#future-enhancements)

---

## Problem Statement
The goal of this project is to build a scalable and efficient data pipeline that:
1. Ingests raw data from various sources into the **Bronze Layer**.
2. Cleans, transforms, and standardizes the data in the **Silver Layer**.
3. Creates refined, business-ready datasets in the **Gold Layer** for reporting and analytics.

---

## Tools and Resources Used
- **Databricks**: For data processing and transformation.
- **Azure Blob Storage**: For storing raw and processed data in Bronze, Silver, and Gold layers.
- **Delta Lake**: For ACID transactions, schema enforcement, and time travel.
- **GitHub**: For version control and collaboration.
- **Azure Portal**: For managing Azure Blob Storage containers.
- **Python/PySpark**: For writing transformation logic.
- **dbdiagram.io**: For Creating ER Diagrams

---

## Project Structure
The project is organized into three layers:
1. **Bronze Layer**: Contains raw, unprocessed data.
2. **Silver Layer**: Contains cleaned and standardized data.
3. **Gold Layer**: Contains refined, business-ready data.

The Databricks workspace is organized as follows:
```
GsynergyDataTransformationLogic/
  ├── reading_notebook.py # Reads data from the Bronze layer.
  ├── transformation_notebook.py # Transforms data and writes to the Silver layer.
  └── creatingviews_notebook.py # Creates views and writes to the Gold layer.
```

---

## Steps to Reproduce the Project
Follow these steps to reproduce the project:

### 1. Create ER diagram using dbdiagram.io
- Used the diagram syntax to generate my ER diagram

### 2. Set Up Azure Blob Storage
- Create an Azure Storage Account.
- Create three containers: `bronze`, `silver`, and `gold`.
- Upload raw data files to the `bronze` container. (https://drive.google.com/drive/folders/1TJeOLemuvVHLUf3O3wMkYcjeaVuwBwk4?us
p=drive_link)

### 3. Set Up Databricks
- Create a Databricks workspace.
- Mount the Azure Blob Storage containers in Databricks:
  ```python
  configs = {
      "fs.azure.account.auth.type": "OAuth",
      "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
      "fs.azure.account.oauth2.client.id": "<client-id>",
      "fs.azure.account.oauth2.client.secret": "<client-secret>",
      "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<tenant-id>/oauth2/token"
  }

  # Mount bronze layer
  dbutils.fs.mount(
      source="abfss://bronze@<storage-account>.dfs.core.windows.net/",
      mount_point="/mnt/bronze",
      extra_configs=configs
  )

  # Mount silver layer
  dbutils.fs.mount(
      source="abfss://silver@<storage-account>.dfs.core.windows.net/",
      mount_point="/mnt/silver",
      extra_configs=configs
  )

  # Mount gold layer
  dbutils.fs.mount(
      source="abfss://gold@<storage-account>.dfs.core.windows.net/",
      mount_point="/mnt/gold",
      extra_configs=configs
  )


## Run the Databricks Notebooks
  The project include 3 notebooks
    1. DataReading - Deals with reading the files from bronze container which has raw data and basic data checks
    2. Transformation - Deals with Transformation logic and changes in datatype
    3. CreatingViewsforGold - Deeals with view creation for gold(serving layer) as per buisness logic

## Azure Blob Storage Setup
  - Bronze Layer: Contains raw data files (e.g., CSV, JSON).
      blob SAS Token : sp=r&st=2025-03-07T18:23:01Z&se=2025-03-22T02:23:01Z&spr=https&sv=2022-11-02&sr=c&sig=sVwvtGgxefVHnpWo9n6BrZXWiQC2%2FtupPR1U9cWVzZw%3D
      blob SAS URL : https://gsynergyrgstorage.blob.core.windows.net/bronze?sp=r&st=2025-03-07T18:23:01Z&se=2025-03-22T02:23:01Z&spr=https&sv=2022-11-02&sr=c&sig=sVwvtGgxefVHnpWo9n6BrZXWiQC2%2FtupPR1U9cWVzZw%3D
  
  - Silver Layer: Contains cleaned and standardized data in Delta format.
      blob SAS Token : sp=r&st=2025-03-07T18:25:57Z&se=2025-03-22T02:25:57Z&spr=https&sv=2022-11-02&sr=c&sig=xjLiMcOuXcgfZWhBUdM4Vw0Q%2BpaCZaUczywBVhhCvlo%3D
      blob SAS URL : https://gsynergyrgstorage.blob.core.windows.net/silver?sp=r&st=2025-03-07T18:25:57Z&se=2025-03-22T02:25:57Z&spr=https&sv=2022-11-02&sr=c&sig=xjLiMcOuXcgfZWhBUdM4Vw0Q%2BpaCZaUczywBVhhCvlo%3D
  
  - Gold Layer: Contains refined datasets in Delta format for reporting and analytics.
      blob SAS Token : sp=r&st=2025-03-07T18:27:28Z&se=2025-03-22T02:27:28Z&spr=https&sv=2022-11-02&sr=c&sig=Huon4vcMU5zB77U6RxMWlb319ylexKX4yQ4lyuqYPGA%3D
      blob SAS URL : https://gsynergyrgstorage.blob.core.windows.net/gold?sp=r&st=2025-03-07T18:27:28Z&se=2025-03-22T02:27:28Z&spr=https&sv=2022-11-02&sr=c&sig=Huon4vcMU5zB77U6RxMWlb319ylexKX4yQ4lyuqYPGA%3D

## Delta Lake Integration - 
   Delta Lake is used for:
    - ACID transactions: Ensures data consistency.
    - Schema enforcement: Prevents data corruption.
    - Time travel: Allows querying historical data.

## Incremental Processing
   The pipeline supports incremental processing using Delta Lake's MERGE operation.

## Git Repository
  The project is version-controlled using Git. The repository structure is as follows:
  ```
  GsynergyDataProject/
  ├── GsynergyData/
  │   ├── reading_notebook.py
  │   ├── transformation_notebook.py
  │   └── creatingviews_notebook.py
  ├── BlobStorage/
  │   ├── bronze/
  │   │   └── sample_data.csv
  │   ├── silver/
  │   │   └── sample_data.csv
  │   └── gold/
  │       └── sample_data.csv
  ├── README.md
  └── .gitignore
```

## Future Enhancements
    1. Automation:
      Use Apache Airflow or Databricks Jobs to automate the pipeline.
    2. Monitoring:
      Implement monitoring and alerting for pipeline failures.
    3. Scalability:
      Optimize the pipeline for large-scale datasets.

## Conclusion
  This project demonstrates how to build a scalable and efficient data pipeline using Databricks, Azure Blob Storage, and Delta Lake. The pipeline ingests raw data, transforms it, and        creates refined datasets for reporting and analytics.



Author
Siddhartha Sarkar

License
This project is licensed under the MIT License. See the LICENSE file for details.
