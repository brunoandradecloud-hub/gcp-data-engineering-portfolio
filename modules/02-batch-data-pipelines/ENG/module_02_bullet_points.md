# 📑 Study Guide — Module 02 (Apache Spark, Dataflow, and Orchestration)

## ⚖️ Bounded vs Unbounded Dataset

**What it is** Classification of data based on whether or not it has a defined end.

**When to use** * **Bounded:** Historical data, batch processing.  
* **Unbounded:** Continuous events, streaming.

**Where it fits in the architecture** The foundation for deciding between batch or streaming.

**Practical example** Historical logs → bounded.  
Real-time payment events → unbounded.

**Exam point / Common trap** ❌ Thinking streaming is always better.  
✅ Streaming only makes sense for non-finalized data.

---

## 🌊 Dataflow

**What it is** Serverless data processing service based on Apache Beam.

**When to use** ETL in batch or streaming with low operational management.

**Where it fits in the architecture** Between ingestion and analytical storage.

**Practical example** Transforming Datastream CDC before writing to BigQuery.

**Exam point / Common trap** ❌ Requires cluster creation.  
✅ Fully serverless.



---

## ⚡ Apache Spark on GCP

**What it is** Distributed framework for large-scale processing.

**When to use** Complex jobs, advanced SQL, or legacy Spark workloads.

**Where it fits in the architecture** Processing layer.

**Practical example** Reprocessing years of financial history.

**Exam point / Common trap** ❌ Managed only by Dataflow.  
✅ Offered mainly via **Dataproc**.

---

## 🛠️ Dataproc

**What it is** Managed service for running Apache Spark, Flink, and Hadoop clusters.

**When to use** When you need control over the infrastructure or have legacy Spark/Hadoop code.

**Where it fits in the architecture** Processing layer (Cluster-based).

**Practical example** Migrating an on-premises Hadoop cluster to the cloud.

**Exam point / Common trap** ❌ Always keep the cluster on.  
✅ Prefer serverless or ephemeral clusters when possible.



---

## 🧠 Performance (Spark vs PySpark vs Spark SQL)

**What it is** Different interfaces for the same Spark engine.

**When to use** * **Spark SQL:** When you master SQL.  
* **PySpark:** When the logic is complex.

**Where it fits in the architecture** Job execution.

**Practical example** Large SQL transformations → Spark SQL.

**Exam point / Common trap** ❌ Spark SQL is slower.  
✅ The execution plan is exactly the same.

---

## 🧩 I/O Optimization & Batch Size

**What it is** Techniques to reduce processing time and cost.

**When to use** Large volumes of data.

**Where it fits in the architecture** Processing pipelines.

**Practical example** Adjusting partition sizes in Spark.

**Exam point / Common trap** ❌ More partitions are always better.  
✅ Excessive partitions generate overhead.

---

## 🕸️ Cloud Composer (Airflow)

**What it is** Workflow orchestrator based on Apache Airflow.

**When to use** When there are dependencies between different jobs.

**Where it fits in the architecture** Orchestration layer.

**Practical example** Running a sequence: Dataflow → BigQuery → ML model.

**Exam point / Common trap** ❌ Composer executes ETL.  
✅ It only orchestrates (triggers and monitors).



---

## 🔐 IAM and Service Accounts

**What it is** Access control for resources.

**When to use** Always, following the "Least Privilege" principle.

**Where it fits in the architecture** Security layer (cross-cutting).

**Practical example** Giving the Composer service account permission to trigger Dataflow.

**Exam point / Common trap** ❌ Use the default Compute Engine key.  
✅ Create specific Service Accounts for each service.