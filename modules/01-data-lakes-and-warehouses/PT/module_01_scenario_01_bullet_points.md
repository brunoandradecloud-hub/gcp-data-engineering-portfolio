# Bullet Points — Scenario 01
## Module 1: Build Data Lakes and Data Warehouses on Google Cloud

> Quick-review document for consolidation, memorization, and generation of certification-style questions.

---

## General Context

- Legacy Oracle system acting as an ODS
- 10+ years of historical data
- Concurrent needs for BI, Analytics, and a Global Application
- Limitations in scalability, cost, and analytical flexibility in the legacy environment

---

## Architectural Principles (This appears on the exam)

- Clear separation between OLTP and OLAP
- Operational systems should not serve analytical workloads
- Data Lake as a decoupling layer from legacy systems
- Processing strategy driven by use case (batch vs streaming)

---

## Data Ingestion

- Batch (D-1) processing used to optimize cost
- CDC with Datastream for continuous change capture
- Streaming does not replace batch — they are complementary
- CDC pipelines must not impact the source database

---

## Data Lake — Cloud Storage

- Raw: immutable, auditable, reprocessable source data
- Processed: cleaned, typed, columnar-formatted data
- Layer separation improves governance and evolution
- Low-cost, highly scalable storage

---

## Curated Layer

- Data ready for analytical consumption
- Not a raw ingestion zone
- May contain multiple technologies

### BigLake (Lakehouse Pattern)

- Designed for large, mostly immutable datasets
- Reduces storage costs
- Enables SQL access without physical ingestion into BigQuery
- Column-level security and centralized governance
- Same data used by BI and Data Science teams

### BigQuery Native Tables

- Used when analytical performance is critical
- Power dashboards and executive reports
- Fully modeled and consolidated datasets

---

## Operational Modernization

- Cloud Spanner as the global transactional database
- Strong consistency with horizontal scalability
- Cloud Bigtable for high-frequency metrics and price data
- Ultra-low latency and massive throughput

---

## Key Exam Decisions

- BigLake ≠ BigQuery native tables
- Lakehouse does not fully replace a Data Warehouse
- Immutable data favors Data Lake / BigLake
- Mutable data favors transactional databases or native tables
- Cost, latency, and governance drive service selection

---

## Common Pitfalls (Exam Traps)

- Using BigQuery for OLTP workloads
- Using streaming for all ingestion scenarios
- Ignoring storage cost considerations
- Mixing ingestion and consumption layers
- Failing to justify architectural trade-offs

---

## Review Questions

- Why must OLTP and OLAP be separated?
- When is BigLake preferable to BigQuery native tables?
- When is batch processing preferable to streaming?
- What is the role of the processed layer?
- How do you optimize cost while maintaining governance?

---

## Final Takeaway

- Architecture comes before tools
- The Data Lake is an enabler, not an end goal
- The exam tests decisions, not implementations
- Thinking like an architect is essential
