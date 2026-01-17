# 📑 Study Guide — Module 01

## 🗄️ Data Lake Architecture (Raw, Processed, Curated)

**What it is**
Layered storage model in Cloud Storage.

**When to use**
To organize data from its raw origin to its final consumption point.

**Where it fits in the architecture**
The foundation of the data pyramid.

**Practical example**
Separating raw Oracle logs from tables ready for Looker.

**Exam point / Common pitfall**
❌ Thinking Curated can only be BigQuery.
✅ Curated can be BigLake on GCS.

---

## 📁 External Tables

**What it is**
BigQuery tables that read data directly from GCS without importing it.

**When to use**
For data in the Raw layer or logs that are rarely queried.

**Where it fits in the architecture**
Raw layer of the Data Lake.

**Practical example**
Querying a historical CSV from 2015 without paying for ingestion.

**Exam point / Common pitfall**
❌ High performance.
✅ It is slower than a native table.

---

## ❄️ BigLake & Iceberg Tables (Lakehouse)

**What it is**
A layer that grants Data Warehouse powers (granular security) to files in GCS.

**When to use**
When you want to combine Data Lake economy with BigQuery security.

**Where it fits in the architecture**
Processed/Curated layer.

**Practical example**
Granting access to a specific column in Parquet files only to the HR department.

**Exam point / Common pitfall**
❌ Replaces transactional databases.
✅ It is for analyzing immutable data.

---

## 🔁 Datastream (CDC)

**What it is**
Real-time Change Data Capture (CDC) service.

**When to use**
To synchronize operational databases (Oracle/MySQL) with the cloud.

**Where it fits in the architecture**
Between the operational database and pipelines.

**Practical example**
Reflecting an Oracle sale instantly in Cloud Storage.

**Exam point / Common pitfall**
❌ It transforms data.
✅ It only moves logs.

---

## 🌊 Dataflow (Pipelines)

**What it is**
Data processing (ETL) based on Apache Beam.

**When to use**
When data needs cleaning, calculation, or masking "in flight."

**Where it fits in the architecture**
Between Ingestion and Storage.

**Practical example**
Converting Dollars to Reais before saving into BigQuery.

**Exam point / Common pitfall**
❌ It is the cheapest option.
✅ It is powerful, but cost scales with volume.

---

## ⚡ Optimization (Partitioning & Clustering)

**What it is**
Physical data organization techniques in BigQuery.

**When to use**
To reduce query costs and increase speed.

**Where it fits in the architecture**
Tables in the Curated layer (Native BigQuery).

**Practical example**
Partitioning by SALE_DATE and clustering by CUSTOMER_ID.

**Exam point / Common pitfall**
❌ Clustering columns with few distinct values.
✅ Use for high-cardinality columns.

---

## ⚙️ AlloyDB vs Cloud Spanner

**What it is**
High-performance operational databases (OLTP).

**When to use**
AlloyDB for migrating Oracle (regional); Spanner for global scale.

**Where it fits in the architecture**
Data source or application database.

**Practical example**
Global bank balance in Spanner.

**Exam point / Common pitfall**
❌ Using them as a Data Warehouse.
✅ These are databases for fast transactions.

---

## 🔗 External Query (Federated Query)

**What it is**
Querying AlloyDB/Spanner from within BigQuery via SQL.

**When to use**
To enrich reports with "live" data from the operational database.

**Where it fits in the architecture**
Analytical layer.

**Practical example**
Cross-referencing 10 years of history with today's live balance.

**Exam point / Common pitfall**
❌ Using it for heavy BI.
✅ It impacts the performance of the operational database.

---

## 🤖 BigQuery ML (BQML)

**What it is**
Creating Machine Learning models using only SQL.

**When to use**
For fast predictions (Churn, Regression) on data already in BigQuery.

**Where it fits in the architecture**
Consumption/AI layer.

**Practical example**
CREATE MODEL to predict next month's sales.

**Exam point / Common pitfall**
❌ It learns incrementally as data comes in.
✅ It must be recreated to update.

---

## 🧠 Embeddings & Vector Search

**What it is**
Converting text into numbers to search by "meaning."

**When to use**
Analyzing chats, complaints, and semantic similarity.

**Where it fits in the architecture**
BigQuery (Generative/Analytical AI).

**Practical example**
Finding similar complaints even if they use different words.

**Exam point / Common pitfall**
❌ Using it for numerical codes.
✅ Use it for free-form text.

---

## 🤖 Vertex AI

**What it is**
Complete platform for ML and MLOps.

**When to use**
Production environments, continuous retraining, ML APIs.

**Where it fits in the architecture**
Outside BigQuery, integrated with the application.

**Practical example**
Real-time fraud detection.

**Exam point / Common pitfall**
❌ Vertex AI replaces BigQuery ML.
✅ They are complementary.

---

## 🏛️ Dataplex

**What it is**
Data governance layer.

**When to use**
For cataloging, policies, and domains.

**Where it fits in the architecture**
Across the entire architecture.

**Practical example**
Governing an entire Data Lake.

**Exam point / Common pitfall**
❌ Dataplex processes data.
✅ Dataplex only governs.

---

## 🔐 DLP (Data Loss Prevention)

**What it is**
Classification of sensitive data.

**When to use**
For compliance and security.

**Where it fits in the architecture**
On top of stored data.

**Practical example**
Identifying PII (Personally Identifiable Information) in tables.

**Exam point / Common pitfall**
❌ DLP encrypts data.
✅ DLP only classifies.

---

## 🔑 KMS (Key Management Service)

**What it is**
Encryption key management.

**When to use**
For control and auditing of keys.

**Where it fits in the architecture**
Protection of data at rest.

**Practical example**
CMEK (Customer-Managed Encryption Keys).

**Exam point / Common pitfall**
❌ Google manages everything.
✅ The customer has control over the key.