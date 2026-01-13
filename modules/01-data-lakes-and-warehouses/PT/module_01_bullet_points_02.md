
# Módulo 01 — Bullet Points 02

- Arquitetura moderna separa claramente cargas analíticas e operacionais
- Data Lake no Cloud Storage reduz custo e aumenta flexibilidade
- Camada Raw: dados brutos, append-only, External Tables
- Camada Processed: transformação, Parquet, Iceberg quando há necessidade de ACID e histórico
- Camada Curated: consumo final, BigQuery nativo e BigLake
- BigLake permite padrão Lakehouse sem duplicação de dados
- Iceberg fornece ACID, Time Travel e controle de metadados
- External Tables são ideais quando não há concorrência ou mutabilidade
- BigQuery é serverless e totalmente gerenciado
- AlloyDB atende aplicações modernas regionais
- Cloud Spanner é necessário para escala global e consistência forte
