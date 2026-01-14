# 📌 Module 01 — Scenario 01
## Bullet Points — Data Modernization on Google Cloud

## 🎯 Scenario Goal
- Replace a legacy Oracle Operational Data Store
- Build a modern architecture on Google Cloud
- Support analytics and near real-time operations
- Align with Google Professional Data Engineer best practices

## 🏗️ Legacy System Context
- Oracle database with over 10 years of data
- Used as an Operational Data Store
- Optimized for transactions, not analytics
- Limitations:
  - Poor analytical performance
  - Slow and complex reporting
  - Limited scalability
  - Not suitable for a modern digital application

## 🔄 Data Ingestion Strategy
### Batch (Historical and D-1)
- Dataflow in batch mode
- Historical extraction and CSV ingestion
- Stored in Cloud Storage
- Parquet format
- No real-time requirement

### Streaming (Near Real-Time)
- Datastream with Change Data Capture
- Captures inserts, updates, and deletes
- Dataflow streaming pipelines
- Low latency without impacting Oracle

## 🗄️ Data Lake Architecture
- Stored in Cloud Storage
- Logical layers:
  - Raw
  - Processed
  - Curated

## 🔹 Raw Layer
- Raw, unmodified data
- Append-only
- No updates, deletes, or concurrency
- Technologies:
  - Cloud Storage
  - External Tables
- Benefits:
  - Lowest cost
  - Full audit history
  - No ACID requirements

## 🔹 Processed Layer
- Cleaned and typed data
- Basic business and technical rules

### External Tables
- Simple pipelines
- Direct read from Cloud Storage
- Parquet format
- Partitioning and pruning

### Iceberg Tables (BigLake)
- ACID compliance
- Time Travel support
- Frequent reprocessing
- Multiple consumers
- Metadata-managed tables
- Explicit partitioning
- No clustering

## 🧠 Curated Layer
- Data ready for consumption
- Used by:
  - Dashboards
  - Reports
  - Analytics
  - Machine Learning

### Data Types
- Native BigQuery Tables:
  - Facts and dimensions
  - High performance
  - Partitioned and clustered
- BigLake:
  - Historical data in Cloud Storage
  - Column- and row-level security
  - Avoids data duplication

## ⚙️ Operational Modernization
### AlloyDB
- Replaces Oracle
- Fully managed
- PostgreSQL-compatible
- High transactional performance

### Cloud Spanner
- Required for global scale
- Strong consistency
- Future migration path

## 🔍 Federated Queries
- BigQuery queries AlloyDB or Spanner
- Used only for:
  - Lightweight enrichment
  - Point-in-time access
  - Near real-time data
- Not for heavy analytics

## ☁️ BigQuery
- Fully serverless
- Infrastructure managed by Google
- Automatic scaling
- High availability
- Engineers focus on SQL and data

## ✅ Architecture Summary
- Operational:
  - Oracle → AlloyDB → Cloud Spanner
- Streaming:
  - Datastream + Dataflow
- Data Lake:
  - Cloud Storage (Raw and Processed)
  - Cloud Storage + BigLake (Curated)
- Analytics:
  - Native BigQuery
  - BigQuery + BigLake
- Consumption:
  - Dashboards
  - Reports
  - Machine Learning
  - Digital application
