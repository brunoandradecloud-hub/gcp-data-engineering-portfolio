# 📘 Módulo 01 — Cenário 01 (v3)

## Modernização de Dados no Google Cloud (Data Lake, Data Warehouse e Machine Learning)

---

## 🎯 Objetivo

Projetar uma arquitetura moderna, escalável e eficiente em custos no Google Cloud para substituir um sistema legado baseado em Oracle, permitindo que a empresa utilize seus dados tanto para **análises de negócio (Business Intelligence e Machine Learning)** quanto para **operações em tempo quase real**, seguindo os padrões recomendados pelo Google e cobrados na certificação **Google Professional Data Engineer**.

Este cenário foi escrito para que **mesmo alguém sem conhecimento prévio em dados** consiga entender:

- de onde os dados vêm,
- onde eles são armazenados,
- como são processados,
- como são governados,
- e como são consumidos pelo negócio e por modelos de Machine Learning.

---

## 📚 Conteúdos Abordados

- Arquitetura de Data Lake (camadas Raw, Processed e Curated)
- Ingestão de dados em batch e em streaming
- Tabelas externas (External Tables)
- Tabelas Iceberg e o padrão Lakehouse (BigLake)
- Change Data Capture com Datastream
- Pipelines de dados com Dataflow
- Comparação entre AlloyDB e Cloud Spanner
- Consultas federadas com External Query
- Particionamento, pruning e otimização de custos
- BigQuery como Data Warehouse serverless
- BigQuery ML para modelos analíticos
- Vertex AI para cenários avançados de Machine Learning
- Governança de dados com Dataplex
- Data Loss Prevention (DLP)
- Cloud Key Management Service (KMS)

---

## 🔄 Estratégia de Ingestão de Dados

### 1️⃣ Ingestão Histórica e Processamento Diário (D-1)

O Dataflow em modo batch é utilizado para extrair dados históricos do Oracle e de arquivos externos (por exemplo, arquivos CSV).

Os dados são armazenados no Cloud Storage em formato Parquet, otimizado para leitura analítica e redução de custos.

Esse fluxo atende dados que não exigem disponibilidade em tempo real, como:

* relatórios diários,
* análises históricas,
* consolidações contábeis.

---

### 2️⃣ Ingestão em Tempo Quase Real (Change Data Capture)

O Datastream captura continuamente inserções, atualizações e exclusões no Oracle.

O Dataflow em modo streaming processa essas mudanças quase em tempo quase real.

Esse modelo permite que informações críticas estejam disponíveis rapidamente, sem impactar o sistema legado.

---

## 🗄️ Arquitetura do Data Lake

O Data Lake é armazenado no Cloud Storage e organizado em três camadas lógicas, também conhecidas conceitualmente como Bronze, Silver e Gold, para facilitar governança, auditoria e escalabilidade.

---

### 🔹 Camada Raw (Dados Brutos)

**O que é**
Armazena os dados exatamente como chegam da fonte, sem qualquer transformação.

**Características**

* Modelo append-only
* Sem atualizações ou exclusões
* Sem concorrência planejada

**Tecnologias**

* Cloud Storage
* Tabelas Externas (External Tables) no BigQuery

**Justificativa**

* Menor custo possível de armazenamento
* Histórico completo para auditoria
* Não exige controle transacional

---

### 🔹 Camada Processed (Dados Tratados)

**O que é**
Os dados passam por limpeza, padronização, tipagem correta e regras técnicas iniciais.

**Tecnologias Utilizadas**

**Tabelas Externas**

* Usadas em pipelines simples e intermediários
* Leitura direta de arquivos Parquet no Cloud Storage
* Beneficiam-se de particionamento e pruning

**Tabelas Iceberg (BigLake)**

Utilizadas quando há necessidade de:

* controle transacional (ACID),
* versionamento de dados (Time Travel),
* reprocessamentos frequentes,
* múltiplos consumidores.

Os dados continuam no Cloud Storage, com metadados avançados.

**Importante**

* Particionamento é explícito
* Não há clusterização nessa camada

---

## 🧠 Camada Curated (Dados para Consumo)

**O que é**
Contém dados prontos para consumo por usuários, sistemas analíticos e modelos de Machine Learning.

**Usos**

* Dashboards
* Relatórios executivos
* Análises de negócio
* Machine Learning

---

### Tipos de Dados na Camada Curated

**Tabelas Nativas do BigQuery**

* Dados consolidados
* Modelagem de Data Warehouse (fatos e dimensões)
* Particionadas e clusterizadas para alta performance

**BigLake (Padrão Lakehouse)**

* Grandes volumes históricos permanecem no Cloud Storage
* BigQuery acessa esses dados via BigLake
* Segurança granular por linha e coluna
* Evita duplicação entre equipes de BI e Ciência de Dados

---

## 🤖 Machine Learning na Arquitetura

### BigQuery ML (BQML)

Utilizado quando os dados:

* já estão consolidados na camada Curated,
* possuem estrutura estável,
* e o problema analítico é bem definido.

Os modelos são criados e consultados via SQL.

Para atualizar o modelo, ele deve ser recriado.

O novo treinamento não herda aprendizado do modelo anterior.

**Exemplos de uso**

* Previsão de churn
* Segmentação de clientes
* Análise de comportamento histórico

---

### Vertex AI (Machine Learning Avançado)

Utilizado quando:

* os dados variam constantemente,
* há necessidade de retreinamento contínuo,
* o modelo faz parte de um produto ou aplicação.

Permite pipelines completos de Machine Learning (MLOps).

Consome dados do BigQuery e do Cloud Storage.

**Exemplos de uso**

* Detecção de fraude em tempo quase real
* Modelos preditivos integrados ao aplicativo digital

---

## 🏛️ Governança de Dados com Dataplex

O Dataplex é utilizado para:

* catalogar dados,
* organizar ativos,
* aplicar políticas de governança.

Ele não executa processamento nem Machine Learning.

Não é pré-requisito para BigQuery, BQML ou Vertex AI, mas:

* torna a arquitetura mais governável,
* facilita auditorias e conformidade.

---

## 🔐 Segurança e Proteção de Dados Sensíveis

### 🕵️ Data Loss Prevention (DLP)

O Data Loss Prevention (DLP) é utilizado para identificar e classificar automaticamente dados sensíveis armazenados no Cloud Storage e no BigQuery.

**Aplicação na arquitetura**

* Atua sobre dados nas camadas Raw, Processed e Curated
* Identifica dados pessoais, financeiros e regulados
* Gera metadados de classificação

---

### 🔑 Cloud Key Management Service (KMS)

O Cloud KMS gerencia as chaves de criptografia utilizadas para proteger dados em repouso.

* Protege dados no Cloud Storage, BigQuery e BigLake
* Permite rotação, auditoria e controle de acesso

---

## ⚙️ Modernização do Sistema Operacional

### AlloyDB

Substitui o Oracle para cargas transacionais modernas.

Totalmente gerenciado e compatível com PostgreSQL.

Ideal para o novo aplicativo digital.

---

### Cloud Spanner

Considerado quando há necessidade de escala global e consistência forte distribuída.

Migração a partir do AlloyDB seria necessária.

---

## 🔍 Consultas Federadas (External Query)

O BigQuery pode consultar AlloyDB ou Cloud Spanner diretamente.

Uso restrito a enriquecimentos leves, consultas pontuais e dados quase em tempo real.

---

## ☁️ BigQuery como Plataforma Analítica

* Totalmente serverless
* Escala automática
* Alta disponibilidade

---

## ✅ Resumo Final da Arquitetura

**Sistema Operacional**
Oracle → AlloyDB → Cloud Spanner

**Streaming**
Datastream + Dataflow

**Data Lake**
Cloud Storage (Raw e Processed)
Cloud Storage + BigLake (Curated)

**Analítico**
BigQuery
BigQuery ML
Vertex AI

**Governança e Segurança**
Dataplex
DLP
Cloud KMS

**Consumo**
Dashboards
Relatórios
Machine Learning
Aplicativo digital
