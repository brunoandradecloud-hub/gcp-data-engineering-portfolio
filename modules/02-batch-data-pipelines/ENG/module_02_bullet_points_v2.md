# 📑 Study Guide — Module 02

## ⚡ Serverless for Apache Spark (Dataproc Serverless)

- Concept of **Serverless for Apache Spark** and how it eliminates the need for cluster management.
- Difference between **Traditional Dataproc (managed cluster)** and **Dataproc Serverless (batch jobs)**.
- Execution of **Spark batch jobs** using ready-made templates.
- Use of **Cloud Storage** as staging and data source.
- Reading **Avro** files in Spark.
- Native integration between **Spark and BigQuery** using the `spark-bigquery` connector.
- Automatic creation of **BigQuery datasets and tables** via Spark jobs.
- Use of **environment variables** to configure projects, regions, and dependencies.
- Importance of **temporary buckets** for loading data into BigQuery.
- Execution monitoring via logs and job status.
- Validation of loaded data using **SQL queries in BigQuery**.



## ⚡ Dataflow — Batch Pipeline (Job Builder UI)

- Introduction to **Dataflow as a serverless data processing service**.
- Difference between **Dataflow (Beam)** and **Spark (Dataproc Serverless)**.
- Creating **no-code batch pipelines** using the **Job Builder UI**.
- Reading CSV data directly from **Cloud Storage**.
- Using ready-made transformations, such as **Filter (Python)**.
- Applying simple business rules (e.g., filtering records with `status = Approved`).
- Writing results to **BigQuery**, with automatic creation of datasets and tables.
- Correct configuration of **staging bucket** for Dataflow execution.
- Importance of **job region** and alignment with project resources.
- Using the **Validate** button to prevent errors before job creation.
- Visual monitoring of the pipeline (**Graph View and Logs**).
- Job lifecycle management (execute, cancel, delete).
- Understanding common errors related to **Region and Project Constraints**.



## 🧠 Key Learnings of the Day

- Serverless drastically reduces operational complexity.
- Spark and Dataflow solve similar problems but with different approaches.
- BigQuery is the central destination for analytics in GCP.
- Region configuration is critical in managed services.
- UI-first (Dataflow Job Builder) accelerates onboarding and learning.