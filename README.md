# End-to-End-Data-Engineering-Pipeline-on-Databricks
M&amp;A (Merger &amp; Acquisition) Data Integration using Medallion Architecture

## Project Overview
This project simulates a real-world Merger & Acquisition (M&A) scenario where a child company’s data is incrementally integrated into a parent company’s data platform.

The solution is built using a modern data engineering architecture on Databricks, enabling:
- Scalable data ingestion from cloud storage 
- Incremental ETL processing
- Reliable data merging using Delta Lake
- Business-ready analytics through dashboards

## Architecture Overview
The pipeline follows a lakehouse architecture with AWS S3 as the data source and Databricks as the processing engine.
### End-to-End flow

AWS S3 (Landing - CSV Files)
        │
        ▼
Databricks Ingestion (Incremental Load)
        │
        ▼
Bronze Layer (Raw Delta Tables)
        │
        ▼
Silver Layer (Cleaned & Deduplicated Data)
        │
        ▼
Gold Layer (Business Aggregations - Child Company)
        │
        ▼
MERGE INTO Parent Gold Table (Delta Lake)
        │
        ▼
Serving Layer (Dashboards & Insights)

## Data Ingestion (AWS S3)
- Raw data is uploaded as CSV files into S3 landing folder
- Databricks ingests data using incremental batch processing
- After successful processing:
  - Files are moved from landing/ → processed/ (archival)

### Key Benefits
- Prevents duplicate processing
- Ensures data traceability
- Supports scalable ingestion patterns

## Medallion Architecture
### Bronze Layer (Raw Data)
- Ingested data stored in Delta format
- Schema applied with minimal transformation
- Acts as a source of truth

### Silver Layer (Cleaned Data)
- Data cleansing and standardization
- Deduplication logic applied
- Schema enforcement

### Gold Layer (Business Layer)
- Aggregated datasets for analytics
- Prepared for reporting and dashboards
- Represents child company business metrics

## 🔗 Data Integration (Core Logic)
The key business requirement is fulfilled here:

#### Merging child company data into parent company tables
gold_parent_delta = DeltaTable.forName(spark, f"{catalog}.{gold_schema}.fact_orders")
gold_parent_delta.alias("parent_gold").merge(df_monthly_recalc.alias("child_gold"), "parent_gold.date = child_gold.date AND parent_gold.product_code = child_gold.product_code AND parent_gold.customer_code = child_gold.customer_code").whenMatchedUpdateAll().whenNotMatchedInsertAll().execute()

### Highlights
- Incremental upserts using Delta Lake MERGE
- Ensures consistency and avoids duplication
- Enables unified reporting across organizations

### Workflow Automation
- Pipeline is scheduled using Databricks Jobs
- Runs automatically after business hours
- Handles:
    -Data ingestion
    -Transformation
    -Merge operations

### Serving Layer (Analytics)
- Built dashboards using Databricks SQL
- Provides insights on:
  - Sales trends
  - Revenue growth
  - Combined business performance

### Tech Stack
- Databricks
- Apache Spark (PySpark)
- Delta Lake
- AWS S3
- SQL
- Medallion Architecture
- Databricks Workflows (Jobs)

### Business Impact
- Unified view of parent + child company data
- Enables data-driven decision-making post-merger
- Scalable design for future acquisitions
- Production-ready ETL architecture

Architecture Diagram


### Learning Acknowledgment
This project was inspired by concepts learned from the
Codebasics YouTube Channel









