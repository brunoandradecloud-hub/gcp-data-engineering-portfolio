# Scenario Simulation 01 — Build Data Lakes and Data Warehouses on Google Cloud

## Objective

This scenario consolidates the core concepts of **Module 1 – Build Data Lakes and Data Warehouses on Google Cloud**.

The goal is to simulate a realistic certification-style problem that requires architectural decision-making rather than tool-specific implementation.

By the end of this scenario, the reader should be able to:
- Distinguish between operational and analytical systems
- Apply Data Lake layering (raw / processed / curated)
- Decide when to use BigQuery native tables vs BigLake
- Separate batch and streaming workloads
- Justify architectural choices using Google Cloud best practices

---

## Covered Topics

- Legacy systems and ODS concepts
- Batch vs streaming ingestion
- Data Lake architecture on Cloud Storage
- Curated analytics on BigQuery
- BigLake as a Lakehouse pattern
- Cost optimization and governance
- Separation of OLTP and OLAP workloads

---

## Business Scenario — Digital Currency Exchange

A digital currency exchange company operates in the financial sector and has accumulated more than **10 years of transactional data** related to currency exchange operations and customer activity.

### Current State

- All historical and operational data is stored in an **Oracle database** acting as a legacy ODS.
- The system is reliable but tightly coupled, expensive to scale, and unsuitable for modern analytics.
- Reporting is limited, slow, and lacks historical flexibility.
- The company plans to launch a **global digital application**, requiring high availability and low-latency transactions.

---

## Requirements

### Analytical Requirements (BI and Analytics)

- Access to the full 10-year historical dataset
- Support for daily (D-1) analytical workloads
- Ability to run complex SQL queries for dashboards and executive reporting
- Cost-efficient storage for large historical datasets
- Data governance and column-level security

### Operational Requirements (Application)

- Strong consistency for financial transactions
- Global scalability for a worldwide user base
- Low-latency access to currency prices and usage metrics
- Clear separation from analytical workloads

---

## Proposed Architecture

### 1. Data Ingestion

**Historical and Batch Data**
- Historical data is extracted from Oracle and external CSV sources using managed batch pipelines.
- Batch workloads follow a D-1 processing model to optimize cost.

**Change Data Capture (CDC)**
- Datastream captures ongoing changes from the Oracle database.
- CDC events are forwarded to the analytical platform without impacting the source system.

---

### 2. Data Lake — Cloud Storage

The Data Lake is implemented on **Cloud Storage** and organized into logical layers:

**Raw Layer**
- Stores data exactly as received from source systems
- Preserves original formats for audit and reprocessing

**Processed Layer**
- Applies cleaning, normalization, and schema enforcement
- Converts data into columnar formats (Parquet) for analytical efficiency

---

### 3. Curated Analytics Layer

The curated layer represents data ready for consumption and decision-making.

**BigLake (Lakehouse Pattern)**
- Used for very large, mostly immutable historical datasets (~85 TB)
- Provides SQL access, centralized governance, and column-level security
- Enables BI and Data Science teams to consume the same data without duplication

**BigQuery Native Tables**
- Store fully consolidated and frequently accessed analytical datasets
- Power dashboards, executive reports, and interactive BI workloads
- Optimized for high-performance analytical queries

---

### 4. Operational Modernization

**Cloud Spanner (OLTP System)**
- Acts as the transactional database for the new global application
- Ensures strong consistency and horizontal scalability
- Decouples application workloads from analytical systems

**Cloud Bigtable (Low-Latency Access)**
- Stores high-frequency currency price updates and application usage metrics
- Optimized for massive throughput and millisecond-level access
- Supports real-time application features such as charts and activity history

---

## Architectural Rationale

- The legacy ODS is preserved operationally while analytics are offloaded to the Data Lake and BigQuery.
- Batch and streaming workloads are clearly separated based on latency requirements.
- BigLake reduces storage cost and data duplication while maintaining governance.
- BigQuery native tables ensure performance where it matters most.
- OLTP and OLAP workloads are strictly isolated to avoid contention and scalability issues.

---

## Certification Alignment

This scenario reflects common patterns tested in the **Google Professional Data Engineer** exam:
- Choosing the right storage and analytics services
- Designing cost-efficient and scalable architectures
- Applying modern Data Lake and Lakehouse principles
- Justifying architectural trade-offs based on workload characteristics

The solution prioritizes architectural correctness over implementation detail, mirroring the expectations of the certification exam.

