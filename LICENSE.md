**Project Summary**

This project showcases how to design and implement a modern cloud-based data engineering pipeline to ingest, store, process, and manage NYC Taxi datasets using Azure services and Databricks.

It follows the Bronze-Silver-Gold Lakehouse Architecture for optimal data management and analytics readiness.


It contains information about trip times, locations, passenger counts, fares, tips, and more.

---

## Architecture Overview

```plaintext
                  ┌────────────────────────────┐
                  │  Public NYC Taxi Dataset   │
                  └────────────┬───────────────┘
                               │
                     (Ingestion via ADF - HTTP)
                               │
                               ▼
                  ┌────────────────────────────┐
                  │   Bronze Layer (Raw Data)   │
                  │ Azure Data Lake (Parquet)   │
                  └────────────┬───────────────┘
                               │
                  (Processing via Databricks - PySpark)
                               │
                               ▼
                  ┌────────────────────────────┐
                  │ Silver Layer (Cleaned Data)│
                  │ Selected columns, enriched │
                  └────────────┬───────────────┘
                               │
                 (Written as Delta Tables - Optimized)
                               │
                               ▼
                  ┌────────────────────────────┐
                  │   Gold Layer (Analytics)   │
                  │    Delta Tables + SQL      │
                  └────────────────────────────┘
```




## Implementation

**Step 1: Resource Setup**

   ● Created a Resource Group and Storage Account with hierarchical namespace enabled (for Data Lake Gen2).
   ● Created a Databricks Workspace and configured a single-node cluster with Photon runtime.

**Step 2: Data Ingestion (Bronze Layer)**

   ● Built an Azure Data Factory pipeline:

         ● Uses HTTP connector to fetch monthly data files (January to December)
         ● Stores raw Parquet files into the Bronze layer in the Data Lake

**Step 3: Data Transformation (Silver Layer)**

  ● Used Databricks Notebooks with PySpark to:

         ● Load raw data from Bronze     
         ● Clean, rename, and enrich columns
         ● Extract fields like trip_date, trip_month, and trip_year
         ● Save transformed data into the Silver layer in Parquet format

**Step 4: Data Modeling (Gold Layer)**

  ● Loaded data from Silver into DataFrames
  
  ● Saved data into Delta Tables
  
  ● Registered Delta Tables into a SQL-accessible Gold layer
  
  ● Created structured tables: trip_type, trip_zone, trip2023

  
##  Tools & Technologies Used

| Service | Purpose |
|--------|---------|
| **Azure Data Factory** | Data ingestion pipeline (HTTP to Data Lake) |
| **Azure Data Lake Storage Gen2** | Central data repository (Bronze/Silver/Gold layers) |
| **Azure Databricks** | Data transformation and Delta table creation |
| **PySpark** | Data processing and cleaning |
| **Delta Lake** | Optimized storage format for fast querying |

