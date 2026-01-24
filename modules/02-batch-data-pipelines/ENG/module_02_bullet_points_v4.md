# 📑 Study Guide — Module 02

## Batch, Data Quality, Orchestration, and Observability (PDE Exam Level)

---

## 🧪 Data Quality in Batch Pipelines (Dataproc Serverless / Spark)

**What it is** Implementation of explicit data quality rules during batch processing, separating valid and invalid data deterministically.

**When to use** When raw data arrives with inconsistencies (nulls, invalid formats, out-of-range values) and **must not contaminate** the analytical layer.

**Where it fits in the architecture** Between Raw → Processed / Curated.

**Practical example** Validating null IDs and invalid emails before writing customer data to BigQuery.

**Architectural decision (Exam)** Spark Serverless is ideal when:

* The processing is batch.


* The logic is already in PySpark.


* There is no need for continuous streaming.



**Exam point / common trap** ❌ Thinking that Data Quality is BigQuery's responsibility.

✅ Quality must be applied **before** the data enters the warehouse.

---

## ☁️ Dataproc Serverless for Apache Spark

**What it is** Execution of Spark jobs without managing clusters (no master/worker visible, no manual resizing).

**When to use** Batch jobs, heavy ETL, complex validations, and legacy Spark workloads.

**Where it fits in the architecture** Processing layer (Compute).

**Practical example** Running a PySpark job that validates data and writes to BigQuery and a DLQ.

**Exam point / common trap** ❌ Confusing it with Dataflow.

✅ Dataproc Serverless runs Spark; Dataflow runs Apache Beam.

---

## 🚨 Dead Letter Queue (DLQ)

**What it is** A separate destination for records that failed quality rules.

**When to use** Whenever data failures **should not break** the main pipeline.

**Where it fits in the architecture** Alongside the main pipeline (error branch).

**Practical example** Saving invalid records into a GCS bucket for later auditing.

**Exam point / common trap** ❌ DLQ is only for streaming.

✅ DLQ is an architectural pattern, not an exclusive feature.

---

## 🎼 Orchestration with Cloud Composer (Apache Airflow)

**What it is** Pipeline orchestrator based on Directed Acyclic Graphs (DAGs).

**When to use** When there are dependencies between jobs, conditional validations, and controlled retries.

**Where it fits in the architecture** Control and coordination layer (The Maestro).

**Practical example** Wait for a file → run Dataflow/Dataproc → validate load in BigQuery.

**Recommended DAG structure (Exam)** 1. **Sensor:** waits for data/event.

2. **Operator:** executes the actual processing job.

3. **Validation:** confirms the final result.

**Exam point / common trap** ❌ Using `BashOperator` for everything.

✅ Using Provider-specific Operators.

---

## 🔧 Provider-specific Operators (Best Practice)

**What it is** Native Airflow operators that integrate directly with Google Cloud services.

**When to use** Whenever a specific operator is available for the service.

**Practical example** - `DataflowStartFlexTemplateOperator`

* `DataprocSubmitJobOperator`
* 
`BigQueryInsertJobOperator` 



**Why it is better** - Automatic authentication via Service Account.

* Observability and status tracking.


* Native error handling.



**Exam point / common trap** ❌ `gcloud` via `BashOperator` is equivalent.

✅ `BashOperator` is considered an anti-pattern for GCP service integration.

---

## 🔁 Cloud Workflows

**What it is** Serverless orchestration based on YAML/JSON for API calls.

**When to use** Simple, event-driven flows with low cost and immediate execution.

**Where it fits in the architecture** Lightweight orchestration layer.

**Practical example** Call a Cloud Function → wait → trigger a Dataflow job.

**Exam point / common trap** ❌ It replaces Cloud Composer.

✅ They are complementary tools.

---

## 🧱 Cloud Data Fusion (Visual ETL)

**What it is** A fully managed, visual ETL tool based on graphical DAGs. It allows building pipelines node by node in the **Pipeline Studio**.

**When to use** Fast integration, teams with SQL/Traditional ETL background, and the need for enterprise connectors (SAP, Oracle).

**Where it fits in the architecture** Ingestion and transformation layer.

**Key Component: Wrangler** - A plugin used to interactively transform, prepare, and clean data.

* Uses a "data-first" approach for real-time feedback on transformations.


* Transformations are called **directives**, which collectively form a **recipe**.



**Internal Mechanics** - Generates execution code for the pipeline.

* Executes on an ephemeral **Dataproc** (Spark) cluster or **Dataflow**.


* 
**Provisioning Time:** It takes about 15 - 20 minutes to provision the necessary resources and instance.



**Exam point / common trap** ❌ Data Fusion processes data internally.

✅ It only orchestrates Spark/Beam code.

✅ **Technical Note from Lab:** In a CSV parse operation, the first row is consumed to set column headers, reducing the processed record count by 1 (e.g., 893 source records → 892 processed records).

---

## 📊 Monitoring and Observability

### Centralized Log Analysis

**What it is** Centralized analysis of logs within Cloud Logging.

**Typical use** Auditing and Root Cause Analysis (RCA).

---

### Proactive Alerting

**What it is** Alerts based on metrics within Cloud Monitoring.

**Typical use** Detecting failures proactively before they impact the business.

---

### Structured Logging

**What it is** Logs emitted in a structured JSON format.

**Benefit** Enables advanced filtering and the creation of automated log-based metrics.

---

### IAM for Logs

**Critical exam point** - **Logs Viewer:** Basic access to operational logs.

* **Private Logs Viewer:** Required to view Data Access audit logs containing sensitive information (PII).

---

## ⚔️ Strategic Comparison (PDE Mindset)

### Dataflow

* Native Streaming 


* Based on Apache Beam 


* Total Serverless (horizontal and vertical auto-scaling)

### Dataproc

* Spark/Hadoop ecosystem 


* Fine-grained cluster configuration control
* Ideal for heavy Batch and "Lift & Shift" migrations 



### Data Fusion

* Visual Interface (No-code/Low-code) 


* High agility for integration 


* Less coding, focus on Enterprise connectors 



---

## 🧠 Architecture Insight

**Code Complexity ≠ Operational Complexity** Well-defined pipelines (whether via code or visual tools) must be:

* 
**Predictable:** Idempotency and error handling.


* 
**Observable:** Structured logs and clear metrics.


* 
**Easy to maintain:** Utilizing DLQs and automated validations.



---

Would you like me to create the **Architect Challenge** simulation based on these points to test your application of these concepts in a real exam scenario?