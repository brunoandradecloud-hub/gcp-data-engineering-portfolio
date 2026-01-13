# Bullet Points — Cenário 01
## Módulo 1: Criar Data Lakes e Data Warehouses no Google Cloud

> Documento de revisão rápida para consolidação, memorização e geração de perguntas no estilo da prova.

---

## Contexto Geral

- Sistema legado baseado em Oracle atuando como ODS
- 10+ anos de dados históricos
- Necessidade simultânea de BI, Analytics e Aplicação Global
- Limitações de escala, custo e flexibilidade no ambiente legado

---

## Princípios Arquiteturais (Isso cai na prova)

- Separação clara entre OLTP e OLAP
- Sistemas operacionais não devem atender cargas analíticas
- Data Lake como camada de desacoplamento do legado
- Processamento orientado a casos de uso (batch vs streaming)

---

## Ingestão de Dados

- Batch (D-1) utilizado para otimizar custo
- CDC com Datastream para mudanças contínuas
- Streaming não substitui batch — são complementares
- CDC não deve impactar o banco de origem

---

## Data Lake — Cloud Storage

- Raw: dados brutos, auditáveis e reprocessáveis
- Processed: dados limpos, tipados e em formato colunar
- Separação de camadas facilita governança e evolução
- Storage barato e escalável

---

## Camada Curated

- Dados prontos para consumo analítico
- Não é local de ingestão bruta
- Pode conter múltiplas tecnologias

### BigLake (Lakehouse)

- Usado para grandes volumes imutáveis
- Reduz custo de storage
- Permite acesso SQL sem ingestão física no BigQuery
- Segurança em nível de coluna
- Mesmo dado para BI e Ciência de Dados

### BigQuery Tabelas Nativas

- Usadas quando performance é crítica
- Dashboards e relatórios executivos
- Dados consolidados e modelados

---

## Modernização Operacional

- Cloud Spanner como banco transacional global
- Consistência forte e escalabilidade horizontal
- Cloud Bigtable para métricas e cotações de alta frequência
- Baixa latência e alto throughput

---

## Decisões-Chave de Prova

- BigLake ≠ BigQuery nativo
- Lakehouse não substitui DW em todos os casos
- Dados imutáveis favorecem Data Lake / BigLake
- Dados mutáveis favorecem bancos transacionais ou tabelas nativas
- Custo, latência e governança guiam a decisão

---

## Erros Comuns (Armadilhas de Prova)

- Usar BigQuery para OLTP
- Usar streaming para tudo
- Ignorar custo de storage
- Misturar ingestão e consumo
- Não justificar trade-offs arquiteturais

---

## Perguntas para Revisão

- Por que separar OLTP de OLAP?
- Quando BigLake é melhor que BigQuery nativo?
- Quando batch é preferível a streaming?
- Qual o papel da camada processed?
- Como otimizar custo mantendo governança?

---

## Takeaway Final

- Arquitetura vem antes da ferramenta
- O Data Lake é um habilitador, não um fim
- A prova testa decisão, não implementação
- Pensar como arquiteto é essencial

