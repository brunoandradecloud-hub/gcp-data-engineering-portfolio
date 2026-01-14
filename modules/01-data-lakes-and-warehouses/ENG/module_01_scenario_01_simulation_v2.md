# 📘 Module 01 — Scenario 01
## Data Modernization on Google Cloud (Data Lake and Data Warehouse)

---

## 🎯 Objective

Design a modern, scalable, and cost-efficient architecture on Google Cloud to replace a legacy Oracle-based system, enabling the company to use its data for **business analytics (Business Intelligence and Machine Learning)** as well as **near real-time operations**, following Google-recommended best practices and the requirements of the **Google Professional Data Engineer** certification.

This scenario is written so that **even non-technical readers** can understand:

- where data comes from,
- where it is stored,
- how it is processed,
- and how it is consumed by the business.

---

## 📚 Covered Topics

- Data Lake architecture (Raw, Processed, and Curated layers)
- Batch and streaming data ingestion
- External Tables
- Iceberg Tables and the Lakehouse pattern (BigLake)
- Change Data Capture with Datastream
- Data pipelines using Dataflow
- AlloyDB versus Cloud Spanner
- Federated queries using External Query
- Partitioning, pruning, and cost optimization
- BigQuery as a serverless Data Warehouse

---

## 🏗️ Scenario Description

### Current Business Context

A **digital currency exchange company** has stored more than **10 years** of data in an Oracle database.
This database functions as an **Operational Data Store (ODS)**, designed to support daily operations such as recording transactions, customers, and marketing campaigns.

Despite being reliable, the environment has major limitations:

1. Data is not organized for modern analytics.
2. Reports are slow and difficult to maintain.
3. The system does not scale well for large historical volumes.
4. The company plans to **launch a new digital application**, requiring a more modern, scalable, and low-latency database.

As a result, the company decides to **fully modernize its data architecture on Google Cloud**.

---

## 🔄 Data Ingestion Strategy

### 1️⃣ Historical and Daily Processing (D-1)

- **Dataflow in batch mode** is used to extract historical data from Oracle and external files (such as CSV files).
- Data is stored in **Cloud Storage** using the **Parquet** format, which is compact and efficient for analytical workloads.

This process is used for data that **does not require real-time availability**, such as daily reports and historical analysis.

---

### 2️⃣ Near Real-Time Ingestion (Change Data Capture)

- **Datastream** is configured to capture all changes occurring in the Oracle database (inserts, updates, and deletes).
- These changes are processed using **Dataflow in streaming mode**, allowing critical information to be available quickly.

This model supports decisions that require **near real-time data**, without overloading the legacy system.

---

## 🗄️ Data Lake Architecture

The Data Lake is stored in **Cloud Storage** and organized into three layers to facilitate governance, auditing, and processing.

---

### 🔹 Raw Layer

**What it is**  
The Raw layer stores data **exactly as it is received from the source**, without any modifications.

**Characteristics**

- Append-only data
- No updates or deletes
- No planned concurrency

**Technologies**

- Cloud Storage
- BigQuery External Tables

**Why this makes sense**

- Lowest storage cost
- Full audit history
- No transactional control required

---

### 🔹 Processed Layer

**What it is**  
Data is cleaned, properly typed, and transformed using basic technical and business rules.

**Technologies Used**

1. **External Tables**
   - Used for intermediate data and simple pipelines
   - Read directly from Cloud Storage in Parquet format
   - Optimized using partitioning and pruning

2. **Iceberg Tables (BigLake)**
   - Used when data requires:
     - ACID transactional control
     - Version history (Time Travel)
     - Frequent reprocessing
     - Multiple consumers
   - Data remains in Cloud Storage, with an intelligent metadata layer

**Important Notes**

- Partitioning is explicitly defined
- No clustering is applied at this layer

---

## 🧠 Curated Layer

**What it is**  
The Curated layer contains data ready for consumption by people and systems.

It is used for:

- Dashboards
- Executive reports
- Business analytics
- Machine Learning

---

### Data Types in the Curated Layer

1. **Native BigQuery Tables**
   - Consolidated data
   - Data Warehouse modeling (facts and dimensions)
   - Partitioned and clustered for high performance

2. **BigLake (Lakehouse Pattern)**
   - Large historical datasets remain in **Cloud Storage**
   - BigQuery accesses these datasets through BigLake
   - Column- and row-level security
   - Avoids data duplication between BI and Data Science teams

---

## ⚙️ Operational System Modernization

### AlloyDB (Modern Transactional Database)

To support the new digital application, Oracle is replaced by **AlloyDB**, which is:

- Fully managed
- PostgreSQL-compatible
- Significantly faster and more scalable than the legacy Oracle system
- Ideal for modern transactional applications

**Example use cases**

- Customer registration
- Current balance
- Transaction status

---

### Cloud Spanner (Global Expansion)

If the application expands to multiple countries and requires global consistency:

- **Cloud Spanner** is considered
- It provides globally distributed writes with strong consistency

📌 In this case, migration from AlloyDB to Cloud Spanner would be required.

---

## 🔍 Federated Queries (External Query)

- BigQuery can query AlloyDB or Cloud Spanner directly using **External Query**
- Used only for:
  - Lightweight enrichment
  - Point-in-time queries
  - Near real-time information

Not suitable for heavy analytical queries or dashboards.

---

## ☁️ BigQuery as the Analytics Platform

BigQuery is used as the primary analytics engine because it is:

- Fully managed by Google
- Serverless (no infrastructure management)
- Automatically scalable
- Highly available

Engineers only need to:

- load data
- write SQL queries

---

## ✅ Final Architecture Summary

- **Operational System**: Oracle → AlloyDB (→ Cloud Spanner for global scale)
- **Streaming**: Datastream + Dataflow
- **Data Lake**:
  - Cloud Storage (Raw and Processed layers)
  - Cloud Storage + BigLake (historical Curated data)
- **Analytics**:
  - Native BigQuery tables
  - BigQuery + BigLake (querying data in Cloud Storage)
- **Consumption**:
  - Dashboards
  - Reports
  - Machine Learning
  - Digital application

---

### 🎯 Conclusion

This architecture:

- resolves legacy system limitations,
- prepares the company for a modern digital application,
- reduces costs,
- increases scalability,
- and strictly follows the patterns required for the **Google Professional Data Engineer** certification.
