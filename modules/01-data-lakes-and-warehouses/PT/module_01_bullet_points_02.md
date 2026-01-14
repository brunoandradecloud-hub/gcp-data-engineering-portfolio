# 📌 Módulo 01 — Cenário 01
## Bullet Points — Modernização de Dados no Google Cloud

## 🎯 Objetivo do Cenário
- Substituir um sistema legado Oracle usado como Operational Data Store
- Criar uma arquitetura moderna no Google Cloud
- Atender análises de negócio e operações em tempo quase real
- Seguir boas práticas da certificação Google Professional Data Engineer

## 🏗️ Contexto do Sistema Legado
- Banco Oracle com mais de 10 anos de dados
- Usado como Operational Data Store (ODS)
- Foco em processamento operacional
- Limitações:
  - Baixa performance analítica
  - Relatórios lentos
  - Escalabilidade limitada
  - Arquitetura inadequada para novo aplicativo digital

## 🔄 Estratégia de Ingestão de Dados
### Batch (Histórico e D-1)
- Dataflow em modo batch
- Extração de dados históricos e arquivos CSV
- Armazenamento no Cloud Storage
- Formato Parquet
- Dados sem necessidade de tempo real

### Streaming (Tempo Quase Real)
- Datastream com Change Data Capture
- Captura de inserts, updates e deletes
- Dataflow em modo streaming
- Baixa latência sem impactar o Oracle

## 🗄️ Arquitetura de Data Lake
- Armazenado no Cloud Storage
- Camadas:
  - Raw
  - Processed
  - Curated

## 🔹 Camada Raw
- Dados brutos
- Append-only
- Sem updates, deletes ou concorrência
- Tecnologias:
  - Cloud Storage
  - External Tables
- Benefícios:
  - Baixo custo
  - Auditoria completa
  - Sem necessidade de ACID

## 🔹 Camada Processed
- Dados limpos e tipados
- Regras técnicas e de negócio básicas

### External Tables
- Pipelines simples
- Leitura direta do Cloud Storage
- Formato Parquet
- Uso de particionamento e pruning

### Iceberg Tables (BigLake)
- Necessidade de ACID
- Time Travel
- Reprocessamentos frequentes
- Múltiplos consumidores
- Dados no Cloud Storage com metadados
- Particionamento explícito
- Sem clusterização

## 🧠 Camada Curated
- Dados prontos para consumo
- Usada para:
  - Dashboards
  - Relatórios
  - Análises
  - Machine Learning

### Tipos de Dados
- BigQuery Nativo:
  - Fatos e dimensões
  - Alta performance
  - Particionamento e clusterização
- BigLake:
  - Dados históricos no Cloud Storage
  - Segurança por coluna e linha
  - Evita duplicação de dados

## ⚙️ Modernização Operacional
### AlloyDB
- Substitui o Oracle
- Banco gerenciado
- Compatível com PostgreSQL
- Alta performance transacional

### Cloud Spanner
- Necessário para escala global
- Consistência forte distribuída
- Migração futura se necessário

## 🔍 Consultas Federadas
- BigQuery consulta AlloyDB ou Spanner
- Uso apenas para:
  - Enriquecimentos leves
  - Consultas pontuais
  - Dados quase em tempo real
- Não usar para relatórios pesados

## ☁️ BigQuery
- Totalmente serverless
- Infraestrutura gerenciada pelo Google
- Escalabilidade automática
- Alta disponibilidade
- Foco apenas em dados e SQL

## ✅ Resumo Arquitetural
- Operacional:
  - Oracle → AlloyDB → Cloud Spanner
- Streaming:
  - Datastream + Dataflow
- Data Lake:
  - Cloud Storage (Raw e Processed)
  - Cloud Storage + BigLake (Curated)
- Analytics:
  - BigQuery nativo
  - BigQuery + BigLake
- Consumo:
  - Dashboards
  - Relatórios
  - Machine Learning
  - Aplicativo digital
