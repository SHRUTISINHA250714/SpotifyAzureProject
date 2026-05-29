# Spotify End-to-End Data Engineering Pipeline on Azure

## Project Overview

This project demonstrates an end-to-end Data Engineering pipeline built on Microsoft Azure using the Medallion Architecture (Bronze, Silver, Gold). The pipeline ingests Spotify streaming data from Azure SQL Database, performs incremental data loading, applies transformations using Azure Databricks and Delta Lake, orchestrates processing through Lakeflow (Delta Live Tables), and delivers business insights through interactive dashboards.

The solution is designed to be scalable, reliable, and analytics-ready following modern Lakehouse architecture principles.

---

## Architecture Overview

```text
Azure SQL Database
        │
        ▼
Azure Data Factory
(Incremental CDC Load)
        │
        ▼
Azure Data Lake Storage Gen2
(Bronze Layer)
        │
        ▼
Azure Databricks
(Silver Layer)
        │
        ▼
Azure Databricks
(Gold Layer)
        │
        ▼
Lakeflow / Delta Live Tables
        │
        ▼
Databricks SQL & Dashboards
```
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/55c17b93-f025-4b44-9ea2-37fb12179c63" />

---

## Technology Stack

| Category | Technology |
|-----------|------------|
| Data Ingestion | Azure Data Factory (ADF) |
| Source Database | Azure SQL Database |
| Storage | Azure Data Lake Storage Gen2 |
| Processing Engine | Azure Databricks |
| Storage Format | Delta Lake |
| Pipeline Orchestration | Lakeflow / Delta Live Tables (DLT) |
| Data Governance | Unity Catalog |
| Analytics | Databricks SQL |
| Visualization | Databricks Dashboards |

---

## Data Ingestion Layer

Data is extracted from Azure SQL Database using Azure Data Factory.

### Features

- Incremental data loading using CDC principles
- Watermark-based extraction strategy
- Lookup activity to retrieve last processed timestamp
- Copy activity for loading new and updated records
- Automated pipeline execution

### Benefits

- Reduced processing time
- No duplicate record ingestion
- Improved scalability
- Efficient resource utilization

---

## Bronze Layer

The Bronze layer stores raw ingested data exactly as received from the source system.
<img width="1555" height="605" alt="WhatsApp Image 2026-05-26 at 8 10 55 PM" src="https://github.com/user-attachments/assets/01361ca3-76c4-4a46-a2ad-24ee055a9f04" />


### Characteristics

- Raw data storage
- Minimal transformations
- Historical preservation
- Source-of-truth layer

### Purpose

- Data recovery
- Auditing
- Historical tracking
- Replay capability

---

## Silver Layer

The Silver layer performs data cleansing, validation, and standardization.

### Transformations Applied

- Null handling
- Data validation
- Deduplication
- Data type corrections
- Standardization
- Business rule enforcement

### Purpose

- Improve data quality
- Prepare data for analytics
- Create reusable datasets

---

## Gold Layer

The Gold layer contains business-ready analytical tables optimized for reporting and dashboarding.

### Features

- Aggregated datasets
- Dimensional modeling
- Optimized query performance
- Business-focused schema

### Benefits

- Faster analytics
- Simplified reporting
- Enhanced user experience

---

## Dimensional Modeling

A Star Schema was implemented to support analytical workloads.

### Fact Table

#### factstream

Stores streaming events and listening activity.

| Column |
|----------|
| stream_id |
| user_id |
| track_id |
| date_key |
| listen_duration |
| device_type |
| stream_timestamp |

<img width="1600" height="883" alt="WhatsApp Image 2026-05-29 at 2 53 38 AM" src="https://github.com/user-attachments/assets/d0a4fc1b-3bcf-4158-8d32-1ea39e5b71c3" />


---

### Dimension Tables

#### dimuser

Stores user-related information.

| Column |
|----------|
| user_id |
| user_name |
| country |
| subscription_type |
| start_date |
| end_date |

#### dimtrack

Stores track metadata.

| Column |
|----------|
| track_id |
| track_name |
| artist_id |
| album_name |
| duration_sec |
| release_date |
| durationFlag |

#### dimdate

Stores date-related attributes.

| Column |
|----------|
| date_key |
| date |
| day |
| month |
| year |
| weekday |

---

## Incremental Loading Strategy

The project uses Change Data Capture (CDC)-style processing through watermark columns.

### Workflow

1. Retrieve the last processed timestamp.
2. Extract only newly inserted or updated records.
3. Load incremental data into the Bronze layer.
4. Apply transformations in the Silver layer.
5. Populate Gold layer analytical tables.
6. Refresh dashboards and reports.

### Benefits

- Faster execution
- Lower compute costs
- Improved scalability
- Reduced storage overhead

---

## Lakeflow (Delta Live Tables)

Lakeflow (DLT) is used to automate data pipeline orchestration and management.

### Features Implemented

- Automated dependency resolution
- Incremental processing
- Materialized Views
- Streaming Tables
- Pipeline Monitoring
- Data Lineage

<img width="1600" height="765" alt="WhatsApp Image 2026-05-28 at 10 58 56 PM" src="https://github.com/user-attachments/assets/d4de177a-2470-43fa-b290-c3eace8b2f93" />

### Advantages

- Reduced operational complexity
- Reliable data processing
- Simplified maintenance
- Better observability

---

## Slowly Changing Dimensions (SCD)

Historical tracking is implemented using Slowly Changing Dimension concepts.

### Columns Used

| Column | Description |
|----------|-------------|
| __START_AT | Record validity start timestamp |
| __END_AT | Record validity end timestamp |

<img width="1572" height="740" alt="WhatsApp Image 2026-05-29 at 12 22 19 AM" src="https://github.com/user-attachments/assets/68fb5ee2-7b84-4b2d-be58-1c51704748ef" />

### Benefits

- Historical analysis
- Change tracking
- Time-based reporting
- Auditability

---

## Business KPIs

### 1. Total Listening Time by Subscription Type

Measures listening behavior across:

- Free Users
- Premium Users
- Family Plans

### Business Value

Identifies subscription engagement patterns and customer behavior.

---

### 2. Top 10 Most Streamed Tracks

Ranks tracks based on total stream counts.

### Business Value

Highlights the most popular content on the platform.

---

### 3. User Activity by Country

Analyzes streaming activity geographically.

### Business Value

Identifies high-engagement regions and user distribution.

---

### 4. Streaming Trend Over Time

Tracks streaming volume across different time periods.

### Business Value

Measures platform growth and usage trends.

---

### 5. Device Usage Analysis

Analyzes listening behavior across different device types.

### Business Value

Provides insights into platform and device preferences.

---

### 6. Long vs Short Track Performance

Compares streaming behavior based on track duration categories.

### Business Value

Helps understand content consumption patterns.

---

## Dashboard Insights
<img width="867" height="1052" alt="image" src="https://github.com/user-attachments/assets/15385d6d-7596-491f-ab1c-d9029da80674" />


The Databricks Dashboard provides insights into:

- Subscription-based listening behavior
- Most streamed tracks
- Country-wise engagement
- Streaming growth trends
- Device usage distribution
- Track duration performance

---

## Project Outcomes

- Automated end-to-end Azure data pipeline
- Implemented incremental data loading strategy
- Built scalable Lakehouse architecture
- Designed analytics-ready Star Schema
- Implemented Slowly Changing Dimensions
- Developed business KPI dashboards
- Enabled data-driven decision-making

## Overall Overview
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/0a13594d-22de-4c30-95ae-215c6eb6cd0f" />


