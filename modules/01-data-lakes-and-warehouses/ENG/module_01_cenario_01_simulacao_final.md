Aqui está a versão em inglês do seu arquivo, mantendo o padrão de formatação Markdown e a estrutura de tópicos solicitada:

---

# 📘 Module 01 — Scenario 01 - FINAL

## Data Modernization on Google Cloud (Data Lake and Data Warehouse)

---

## 🎯 Objective

Design a modern, scalable, and cost-efficient architecture on Google Cloud to replace a legacy Oracle-based system. This architecture allows the company to use its data for both **Business Intelligence (BI) and Machine Learning (ML)** analysis, as well as **near real-time operations**, following the standards required for the **Google Professional Data Engineer** certification.

This scenario is written so that even those without prior data knowledge can understand:

* Where the data comes from;
* Where it is stored;
* How it is processed;
* How it is governed;
* How it is consumed by the business and ML models.

---

## 📚 Topics Covered

* Data Lake Architecture (Raw, Processed, and Curated layers)
* Batch and streaming data ingestion
* External Tables
* Iceberg Tables and the Lakehouse pattern (BigLake)
* Change Data Capture (CDC) with Datastream
* Data pipelines with Dataflow
* AlloyDB vs. Cloud Spanner
* Federated Queries with External Query
* Partitioning, pruning, and cost optimization
* BigQuery as a serverless Data Warehouse
* BigQuery ML for analytical models
* Vertex AI for advanced Machine Learning
* Data governance with Dataplex
* Data Loss Prevention (DLP)
* Cloud Key Management Service (KMS)
* Embeddings and Vector Search in BigQuery

---

## 🏗️ Step 1 — Current Business Context

A **digital exchange office** has stored its data in an Oracle database for over **10 years**. This database acts as an **Operational Data Store (ODS)**, supporting daily operations like recording transactions and customer profiles.

Despite being reliable, the environment has major issues:

1. Data is not organized for modern analysis.
2. Reports are slow and difficult to maintain.
3. The system does not scale well for large historical volumes.
4. The company wants to **create a new digital app**, requiring a more modern, scalable, low-latency database.
5. The company needs to analyze customer chat interaction data.
6. Compliance with data security standards is required.
7. The company wants to create a high-end AI sector to assist in decision-making.

---

## 🔄 Step 2 — Data Ingestion

### 🔹 Datastream (The Capture Engine - CDC)

* **What it is:** A serverless service that performs Change Data Capture (CDC) directly from database logs.
* **When to use in this scenario:** To capture every currency sale or registration change in Oracle in real-time.
* **How it fits the architecture:** Connects to Oracle, extracts CDC, and delivers formatted files (Avro or JSON) to a Cloud Storage Bucket.
* **Practical examples:**
* Syncing a new Euro purchase made by a customer.
* Capturing a change in a user's loyalty level.
* Detecting record deletions for compliance.


* **Certification relevance:** Standard tool for **Zero Downtime Migration**.

### 🔹 Dataflow Streaming (Continuous Processing)

* **What it is:** A continuous stream processing tool based on Apache Beam.
* **When to use in this scenario:** When data captured by Datastream needs cleaning or transformation before reaching the Lakehouse.
* **How it fits the architecture:** Receives the stream from Datastream (or Pub/Sub), applies business rules, and writes to the Raw layer of Cloud Storage.
* **Practical examples:**
* Masking customer IDs as they leave Oracle.
* Converting foreign currencies to the base currency in real-time.
* Filtering transactions that deviate from security patterns.


* **Certification relevance:** Focuses on **Windowing** and real-time data handling.

### 🔹 Dataflow Batch (Batch Processing)

* **What it is:** Processing of large volumes of static or historical data.
* **When to use in this scenario:** For the initial load of 10 years of historical data or daily processing of partner files.
* **How it fits the architecture:** Reads large volumes from Oracle (via JDBC) or Cloud Storage and processes them in bulk for the Raw layer.
* **Practical examples:**
* Migrating 85 Terabytes of historical transactions to the cloud.
* Processing consolidated monthly currency exchange closings.
* Loading daily CSV files of international financial sanctions.


* **Certification relevance:** Teaches how to manage large-scale **parallelism and processing costs**.

---

## 🗄️ Step 3 — Data Lakehouse Architecture

### 🔹 Raw Layer (Bronze)

* **What it is:** Where data resides exactly as it was extracted from the source.
* **When to use in this scenario:** As a faithful repository for auditing and reprocessing.
* **How it fits the architecture:** Data is loaded by Dataflow into Cloud Storage. Using **External Tables** in BigQuery avoids duplicate storage costs.
* **Practical examples:**
* Original transaction dumps from 2016.
* Raw system logs.
* JSON files of chat interactions.


* **Certification relevance:** Use of External Tables for cost savings (**Storage vs Native**).

### 🔹 Processed Layer (Silver)

* **What it is:** Cleaned, standardized data written in Parquet format.
* **When to use in this scenario:** As the foundation for data engineers and scientists.
* **How it fits the architecture:**
* **Iceberg Tables (BigLake):** Enables ACID transactions and granular column-level security.
* **External Tables:** For fast reading of historical files that do not change.


* **Practical examples:**
* Customer database with validated addresses.
* Transactions with standardized date and time fields (ISO 8601).
* Cleaned historical price quotes.


* **Certification relevance:** BigLake enables **Column-level Security** on Data Lake files.

### 🔹 Curated Layer (Gold)

* **What it is:** The final layer with data modeled for business consumption.
* **When to use in this scenario:** To feed Looker dashboards and AI models.
* **How it fits the architecture:** Uses BigLake or native tables. **Partitioning** and **Clustering** are mandatory for performance.
* **Practical examples:**
* Consolidated monthly profit table by country.
* Marketing-ready customer segmentation.
* Aggregated data for Churn model training.


* **Certification relevance:** Partitioning and Clustering are key for **Cost Optimization**.

---

## ⚙️ Step 4 — Database Modernization (OLTP)

### 🔹 AlloyDB

* **What it is:** A fully managed PostgreSQL database with accelerated analytical processing.
* **When to use in this scenario:** As the main database for the new app to manage transactions and profiles.
* **How it fits the architecture:** Replaces Oracle with a columnar engine for fast BI. Allows **External Query** for real-time balance checks.
* **Why replace Oracle?** Features automatic disaster recovery in seconds and much lower licensing costs.

### 🔹 Cloud Spanner

* **What it is:** A relational database with horizontal scale and strong global consistency.
* **When to use in this scenario:** To ensure a customer's balance is identical worldwide, preventing double-spending.
* **How it fits the architecture:** Acts as the global **Ledger**. Supports External Query to cross-reference historical data with live global balances.
* **Certification relevance:** Oracle cannot maintain **strong global consistency** with Spanner's ease of scale.

---

## 🤖 Step 5 — Artificial Intelligence (Machine Learning)

### 🔹 BigQuery ML (The SQL Oracle)

* **What it is:** An extension of BigQuery for creating ML models using SQL.
* **ML.EVALUATE:** Tests the model's accuracy against real data.
* **ML.PREDICT:** Executes the model to return probabilities (e.g., churn risk).


* **Certification relevance:** Focuses on efficiency by **"bringing the model to the data"**.

### 🔹 Embeddings and Vector Search

* **What it is:**
* **Embeddings:** Converting complex chat text into numerical vectors where similar meanings are mathematically "close".
* **Vector Search:** An engine that scans millions of vectors to find customer "intent" rather than exact keywords.


* **Certification relevance:** Foundation for **RAG (Retrieval-Augmented Generation)** and semantic search.

---

## 🔐 Step 6 — Security and Governance

### 🔹 Dataplex (The Control Tower)

* **What it is:** An intelligent governance service for the entire Data Lake.
* **Auto Data Quality:** Scans data based on rules (e.g., "email cannot be null").
* **Discovery & Cataloging:** Identifies sensitive data (PII) and creates a searchable catalog with **Lineage**.


* **Certification relevance:** Focuses on **Centralized Governance** in multicloud environments.

### 🔹 Cloud Data Loss Prevention (DLP)

* **What it is:** A tool that identifies sensitive information using patterns (Regex) and AI.
* **When to use in this scenario:** To prevent human analysts from seeing secret data (like passport numbers).
* **Practical examples:** Redaction, Masking, and Tokenization.
* **Certification relevance:** Essential for **Compliance (GDPR/LGPD)**.

### 🔹 Cloud Key Management Service (KMS)

* **What it is:** A vault for **Customer-Managed Encryption Keys (CMEK)**.
* **How it fits the architecture:** Acts as a "master key". If KMS permission is revoked, BigQuery cannot read the tables.
* **Certification relevance:** Focuses on **CMEK vs. Google-Managed Keys**.