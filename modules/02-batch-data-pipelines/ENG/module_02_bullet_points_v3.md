# 📑 Study Guide — Module 02 (Deduplication, Data Quality, and Maturity)

## 🏗️ Data Control Levels (Architectural Maturity)

**What it is** The hierarchy of technical decision-making on where and how to handle data quality, separating temporary workarounds from professional engineering solutions.

**When to use** * **Level 1 (UI/Templates):** Fast "Lift & Shift" ingestion for simple data without the need for cleaning logic.  
* **Level 2 (SQL/MERGE):** Reactive correction after loading; useful for minor adjustments, but scales poorly and generates reprocessing costs.  
* **Level 3 (Beam SDK):** Proactive control; mandatory for Exactly-once processing, in-flight deduplication, and end-to-end governance.

**Where it fits in the architecture** In the processing layer that transforms and moves data from Raw to Processed/Curated.

**Practical Example** Preventing a duplicate record from ever reaching BigQuery via Beam code (Level 3) instead of paying for a scheduled SQL cleanup script (Level 2).

**Exam Point / Common Pitfall** ❌ The exam may offer "Dataflow Templates" as a solution for complex validation logic.  
✅ The Professional PDE standard requires the Apache Beam SDK to ensure "Exactly-once" and prevent "garbage-in" to the storage.

---

## 🧹 Proactive Deduplication (In-flight Dedup)

**What it is** The strategy of ensuring data uniqueness within the processing pipeline before final persistence.

**When to use** Whenever it is necessary to avoid "storage bloat" and reduce operational costs of DML (MERGE) in the Data Warehouse.

**Where it fits in the architecture** Inside the Dataflow logic using native transformations like `beam.Distinct()` or `beam.CombinePerKey`.

**Practical Example** Removing identical event logs triggered by network failures before they occupy space in analytical tables.

**Exam Point / Common Pitfall** ❌ "I will run a DISTINCT after loading into BigQuery."  
✅ Deduplicating in the Beam SDK is the correct answer for scale and saving BigQuery slots.

---

## 🧠 GroupByKey vs. CombinePerKey

**What it is** Fundamental operations for data aggregation and reduction within the Dataflow engine.

**When to use** * **GroupByKey:** When the data volume per key is small and the processing requires all elements to be grouped together.  
* **CombinePerKey:** For massive deduplication and aggregations; performs partial reduction at the source workers.

**Where it fits in the architecture** The core processing of the data pipeline.

**Practical Example** Counting billions of hits per URL: `CombinePerKey` reduces the count locally before sending the total, avoiding network bottlenecks.

**Exam Point / Common Pitfall** ❌ `GroupByKey` is the recommended solution for large-scale deduplication.  
✅ `GroupByKey` generates high shuffle and causes Out of Memory (OOM) errors; `CombinePerKey` is the standard for performance and scale.

---

## 🧬 Schema as a Contract

**What it is** The philosophy that the pipeline code (Schema as Code) is the final judge that enforces the data structure before consumption.

**When to use** To ensure that the Curated layer never receives malformed data that could break Dashboards or ML models.

**Where it fits in the architecture** In the Dataflow validation logic between the Raw and Processed layers.

**Practical Example** A pipeline that accepts extra fields in Raw (flexible) but intentionally fails or diverts the data if the "CUSTOMER_ID" field is not numeric in Curated (strict).

**Exam Point / Common Pitfall** ✅ Raw tolerates change (Schema-on-read); Curated requires a contract (Schema-on-write). Beam is the one that executes this contract.

---

## 🪟 Facade View Pattern

**What it is** Creating an abstraction layer via BigQuery Views to decouple physical tables from consumers.

**When to use** To allow structural changes in the database without interrupting dashboards or APIs.

**Where it fits in the architecture** Presentation/Consumption Layer.

**Practical Example** The View `v_sales_current` points to `table_sales_2024`. When the year changes, you update the View to point to `table_sales_2025`, and Looker continues to work without edits.

**Exam Point / Common Pitfall** ❌ Using Views to increase query processing speed.  
✅ Use them to "avoid breaking dashboards", "abstract complexity", and provide a "stable interface".

---

## ⚠️ Dead Letter Queue (Real DLQ)

**What it is** A technical diversion mechanism to capture records that failed during processing for later auditing.

**When to use** Scenarios where data cannot simply be discarded, requiring analysis of the failure reason.

**Where it fits in the architecture** Implemented via Side Outputs (`TaggedOutput`) in the Beam code, saving the error + original data to GCS or BQ.

**Practical Example** If a sales record fails currency conversion, it is sent to an "errors" bucket with the exception stack trace attached.

**Exam