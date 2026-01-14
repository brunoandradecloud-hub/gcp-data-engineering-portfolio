# 📘 Module 01 — Scenario 01 (v3)

## Data Modernization on Google Cloud (Data Lake, Data Warehouse, and Machine Learning)

---

## 🎯 Objective

Design a modern, scalable, and cost-efficient data architecture on Google Cloud to replace a legacy Oracle-based system, enabling the company to use its data for **business analytics (Business Intelligence and Machine Learning)** as well as **near real-time operations**, following Google-recommended best practices and the requirements of the **Google Professional Data Engineer** certification.

This scenario is written so that **even readers with no prior data knowledge** can understand:

- where data comes from,
- where it is stored,
- how it is processed,
- how it is governed,
- and how it is consumed by the business and Machine Learning models.

---

## 📚 Covered Topics

- Data Lake architecture (Raw, Processed, and Curated layers)
- Batch and streaming data ingestion
- External Tables
- Iceberg Tables and the Lakehouse pattern (BigLake)
- Change Data Capture with Datastream
- Data pipelines with Dataflow
- AlloyDB versus Cloud Spanner
- Federated queries using External Query
- Partitioning, pruning, and cost optimization
- BigQuery as a serverless data warehouse
- BigQuery ML for analytical models
- Vertex AI for advanced Machine Learning scenarios
- Data governance with Dataplex
- Data Loss Prevention (DLP)
- Cloud Key Management Service (KMS)

---

## 🔄 Data Ingestion Strategy

### 1️⃣ Historical Ingestion and Daily Processing (D-1)

Dataflow in batch mode is used to extract historical data from Oracle and external files (for example, CSV files).

The data is stored in Cloud Storage using the Parquet format, optimized for analytical reads and cost reduction.

This flow supports data that does not require real-time availability, such as:

- daily reports,
- historical analyses,
- accounting consolidations.

---

### 2️⃣ Near Real-Time Ingestion (Change Data Capture)

Datastream continuously captures inserts, updates, and deletes from Oracle.

Dataflow streaming pipelines process these changes in near real time.

This model allows critical information to be available quickly without impacting the legacy system.

---

## 🗄️ Data Lake Architecture

The Data Lake is stored in Cloud Storage and organized into three logical layers, also conceptually known as Bronze, Silver, and Gold, to facilitate governance, auditing, and scalability.

---

### 🔹 Raw Layer (Raw Data)

**What it is**  
Stores data exactly as received from the source, without any transformation.

**Characteristics**

- Append-only model  
- No updates or deletes  
- No planned concurrency  

**Technologies**

- Cloud Storage  
- BigQuery External Tables  

**Justification**

- Lowest possible storage cost  
- Full historical audit trail  
- No transactional control required  

---

### 🔹 Processed Layer (Processed Data)

**What it is**  
Data is cleaned, standardized, correctly typed, and subjected to initial technical rules.

**Technologies Used**

**External Tables**

- Used for simple and intermediate pipelines  
- Directly read Parquet files from Cloud Storage  
- Benefit from partitioning and pruning  

**Iceberg Tables (BigLake)**

Used when there is a need for:

- transactional control (ACID),
- data versioning (Time Travel),
- frequent reprocessing,
- multiple consumers.

Data remains stored in Cloud Storage with advanced metadata.

**Important**

- Partitioning is explicitly defined  
- No clustering is applied at this layer  

---

## 🧠 Curated Layer (Data for Consumption)

**What it is**  
Contains data ready for consumption by users, analytical systems, and Machine Learning models.

**Uses**

- Dashboards  
- Executive reports  
- Business analytics  
- Machine Learning  

---

### Data Types in the Curated Layer

**Native BigQuery Tables**

- Consolidated data  
- Data Warehouse modeling (facts and dimensions)  
- Partitioned and clustered for high performance  

**BigLake (Lakehouse Pattern)**

- Large historical volumes remain in Cloud Storage  
- BigQuery accesses this data through BigLake  
- Row-level and column-level security  
- Avoids data duplication between BI and Data Science teams  

---

## 🤖 Machine Learning in the Architecture

### BigQuery ML (BQML)

Used when data:

- is already consolidated in the Curated layer,
- has a stable structure,
- and the analytical problem is well defined.

Models are created and queried using SQL.

To update a model, it must be recreated.

The new training does not inherit learning from the previous model.

**Use cases**

- Churn prediction  
- Customer segmentation  
- Historical behavior analysis  

---

### Vertex AI (Advanced Machine Learning)

Used when:

- data changes frequently,
- continuous retraining is required,
- the model is part of a product or application.

Enables full Machine Learning pipelines (MLOps).

Consumes data from BigQuery and Cloud Storage.

**Use cases**

- Near real-time fraud detection  
- Predictive models integrated into the digital application  

---

## 🏛️ Data Governance with Dataplex

Dataplex is used to:

- catalog data,
- organize assets,
- apply governance policies.

It does not perform processing or Machine Learning.

It is not a prerequisite for BigQuery, BQML, or Vertex AI, but:

- makes the architecture more governable,
- facilitates audits and compliance.

---

## 🔐 Security and Sensitive Data Protection

### 🕵️ Data Loss Prevention (DLP)

Data Loss Prevention (DLP) is used to automatically identify and classify sensitive data stored in Cloud Storage and BigQuery.

**Application in the architecture**

- Applies to data in the:

  - Raw layer  
  - Processed layer  
  - Curated layer  

- Identifies:

  - personal data,  
  - financial data,  
  - regulated information.  

**Important notes**

- DLP does not encrypt data  
- DLP does not modify data  
- DLP generates classification metadata  

This metadata is used for access control, auditing, and governance policies.

---

### 🔑 Cloud Key Management Service (KMS)

Cloud KMS is responsible for managing encryption keys used to protect data at rest.

**Protects data in**

- Cloud Storage  
- BigQuery  
- BigLake  

**Responsibilities**

- Key management  
- Access control  
- Key rotation  
- Auditing  

Encryption ensures that only authorized users and services can access data, even in direct storage access scenarios.

---

## ⚙️ Operational System Modernization

### AlloyDB

Replaces Oracle for modern transactional workloads.

Fully managed and PostgreSQL-compatible.

Ideal for the new digital application.

---

### Cloud Spanner

Considered when there is a need for:

- global scale,
- strong distributed consistency.

Migration from AlloyDB would be required.

---

## 🔍 Federated Queries (External Query)

BigQuery can directly query AlloyDB or Cloud Spanner.

Usage is restricted to:

- lightweight enrichments,
- point-in-time queries,
- near real-time data.

Not recommended for heavy analytical workloads.

---

## ☁️ BigQuery as the Analytics Platform

- Fully serverless  
- Infrastructure managed by Google  
- Automatic scaling  
- Native high availability  

Engineers focus only on data and SQL.

---

## ✅ Final Architecture Summary

**Operational System**  
Oracle → AlloyDB → Cloud Spanner (global scale)

**Streaming**  
Datastream + Dataflow

**Data Lake**

- Cloud Storage (Raw and Processed)  
- Cloud Storage + BigLake (historical Curated data)

**Analytics**

- Native BigQuery tables  
- BigQuery + BigLake  
- BigQuery ML  
- Vertex AI  

**Governance and Security**

- Dataplex  
- Data Loss Prevention (DLP)  
- Cloud KMS  

**Consumption**

- Dashboards  
- Reports  
- Machine Learning  
- Digital application
