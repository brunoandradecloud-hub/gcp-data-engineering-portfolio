
# 📌 Module 01 — Bullet Points (Governance and Machine Learning)

## 🏛️ Data Governance — Dataplex
- Governance layer for Data Lake and Data Warehouse
- Does not process or query data
- Works across Cloud Storage, BigQuery, and BigLake
- Centralizes technical and business metadata
- Improves auditability and compliance
- Not required for BigQuery ML or Vertex AI

## 🔐 Data Loss Prevention (DLP)
- Identifies and classifies sensitive data
- Works on Cloud Storage and BigQuery
- Does not encrypt or modify data
- Produces classification metadata
- Supports access control and auditing

## 🔑 Cloud Key Management Service (KMS)
- Manages encryption keys
- Protects data at rest
- Integrated with Cloud Storage, BigQuery, and BigLake
- Supports rotation, access control, and auditing

## 🤖 BigQuery ML (BQML)
- Used with consolidated and stable data
- Created and queried using SQL
- Requires recreation to update models
- Does not inherit previous training

## 🤖 Vertex AI
- Used for advanced and production ML
- Supports pipelines, MLOps, and continuous retraining
- Integrates with BigQuery and Cloud Storage

## 🧠 Mental Summary
- Dataplex governs
- DLP identifies
- KMS encrypts
- BQML simplifies
- Vertex AI scales
