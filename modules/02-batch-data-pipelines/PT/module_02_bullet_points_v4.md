# 📑 Guia de Estudos — Módulo 02

## Batch, Data Quality, Orquestração e Observabilidade (nível prova PDE)

---

## 🧪 Data Quality em Pipelines Batch (Dataproc Serverless / Spark)

**O que é** Implementação de regras explícitas de qualidade de dados durante o processamento batch, separando dados válidos e inválidos de forma determinística.

**Quando usar** Quando dados chegam da camada Raw com inconsistências (nulls, formatos inválidos, valores fora do domínio) e **não podem contaminar** a camada analítica.

**Onde se encaixa na arquitetura** Entre Raw → Processed / Curated.

**Exemplo prático** Validar IDs nulos e e-mails inválidos antes de gravar dados de clientes no BigQuery.

**Decisão arquitetural (prova)** Spark Serverless é ideal quando:

* O processamento é batch
* A lógica já está em PySpark
* Não há necessidade de streaming contínuo

**Ponto de prova / armadilha comum** ❌ Achar que Data Quality é responsabilidade do BigQuery.

✅ A qualidade deve ser aplicada **antes** do dado entrar no warehouse.

---

## ☁️ Dataproc Serverless for Apache Spark

**O que é** Execução de jobs Spark sem gerenciar clusters (no master/worker, sem resize manual).

**Quando usar** Jobs batch, ETL pesado, validações complexas, workloads legados em Spark.

**Onde se encaixa na arquitetura** Camada de processamento.

**Exemplo prático** Rodar um job PySpark que valida dados e escreve no BigQuery e DLQ.

**Ponto de prova / armadilha comum** ❌ Confundir com Dataflow.

✅ Dataproc Serverless executa Spark; Dataflow executa Apache Beam.

---

## 🚨 Dead Letter Queue (DLQ)

**O que é** Destino separado para registros que falharam em regras de qualidade.

**Quando usar** Sempre que falhas de dados **não devem quebrar** o pipeline.

**Onde se encaixa na arquitetura** Ao lado do pipeline principal (ramificação de erro).

**Exemplo prático** Gravar registros inválidos em um bucket GCS para auditoria.

**Ponto de prova / armadilha comum** ❌ DLQ é só para streaming.

✅ DLQ é um padrão de arquitetura, não uma feature exclusiva.

---

## 🎼 Orquestração com Cloud Composer (Apache Airflow)

**O que é** Orquestrador de pipelines baseado em DAGs (Directed Acyclic Graphs).

**Quando usar** Quando há dependências entre jobs, validações condicionais e retries controlados.

**Onde se encaixa na arquitetura** Camada de controle e coordenação.

**Exemplo prático** Esperar arquivo → rodar Dataflow → validar carga no BigQuery.

**Estrutura recomendada de DAG (prova)** 1. Sensor (espera dado)

2. Operator (processa)

3. Validation (confirma resultado)

**Ponto de prova / armadilha comum** ❌ Usar BashOperator para tudo.

✅ Usar Provider-specific Operators.

---

## 🔧 Provider-specific Operators (Best Practice)

**O que é** Operadores nativos do Airflow que integram diretamente com serviços GCP.

**Quando usar** Sempre que houver um operador específico disponível.

**Exemplo prático** - DataflowStartFlexTemplateOperator

* DataprocSubmitJobOperator
* BigQueryInsertJobOperator

**Por que é melhor** - Autenticação automática

* Observabilidade
* Tratamento de erro nativo

**Ponto de prova / armadilha comum** ❌ gcloud via BashOperator é equivalente.

✅ BashOperator é anti-pattern.

---

## 🔁 Cloud Workflows

**O que é** Orquestração serverless baseada em YAML/JSON para chamadas de API.

**Quando usar** Fluxos simples, event-driven, baixo custo.

**Onde se encaixa na arquitetura** Orquestração leve.

**Exemplo prático** Chamar Cloud Function → esperar → chamar Dataflow.

**Ponto de prova / armadilha comum** ❌ Substitui o Composer.

✅ São complementares.

---

## 🧱 Cloud Data Fusion (Visual ETL)

**O que é** Ferramenta visual de ETL baseada em DAGs gráficos. O Pipeline Studio permite compor pipelines visualmente através de nós como Source, Sink e Transform.

**Quando usar** Integração rápida, times SQL/ETL tradicional, conectores corporativos. Ideal para processar fontes comuns como arquivos CSV ou bancos de dados.

**Onde se encaixa na arquitetura** Camada de ingestão e transformação.

**Componente Chave: Wrangler** - Plugin para preparação e limpeza interativa de dados (*data-first approach*).

* Utiliza **diretrizes** de transformação (ex: `drop :body`) que juntas formam uma **receita**.


* Permite visualizar transformações em tempo real para feedback imediato.



**Funcionamento interno** - Gera jobs Spark

* Executa em Dataproc efêmero ou Dataflow
* O provisionamento da instância e dos recursos necessários pode levar entre **15 a 20 minutos**.



**Ponto de prova / armadilha comum** ❌ Data Fusion processa dados internamente.

✅ Ele apenas orquestra código Spark.

✅ **Nota técnica do Lab:** Na operação de parsing de CSV, a primeira linha é consumida para definir os cabeçalhos das colunas, o que reduz o contador de registros processados (ex: 893 na fonte → 892 no destino).

---

## 📊 Monitoramento e Observabilidade

### Centralized Log Analysis

**O que é** Análise centralizada de logs no Cloud Logging.

**Uso típico** Auditoria e root cause analysis (RCA).

---

### Proactive Alerting

**O que é** Alertas baseados em métricas no Cloud Monitoring.

**Uso típico** Detectar falhas de forma proativa antes que impactem o negócio.

---

### Structured Logging

**O que é** Logs emitidos em formato JSON estruturado.

**Benefício** Permite a criação de filtros avançados e métricas automáticas baseadas em campos específicos.

---

### IAM para Logs

**Ponto crítico de prova** - **Logs Viewer:** Acesso básico a logs operacionais.

* **Private Logs Viewer:** Necessário para visualizar logs de Auditoria de Acesso a Dados contendo informações sensíveis (PII).

---

## ⚔️ Comparativo Estratégico (PDE Mindset)

### Dataflow

* Streaming nativo
* Baseado em Apache Beam
* Serverless total (auto-scaling horizontal e vertical)

### Dataproc

* Ecossistema Spark/Hadoop
* Maior controle de configuração do cluster
* Ideal para Batch pesado e migrações "Lift & Shift"

### Data Fusion

* Interface Visual (No-code/Low-code)
* Alta agilidade para integração
* Menos código, foco em conectores corporativos

---

## 🧠 Insight de Arquitetura

**Complexidade no código ≠ Complexidade operacional** Pipelines bem definidos (seja por código ou visualmente) devem ser:

* **Previsíveis:** Idempotência e tratamento de erro.
* **Observáveis:** Logs estruturados e métricas claras.
* **Fáceis de manter:** Uso de DLQs e validações automatizadas.

---

Agora que finalizamos este guia completo do Módulo 02, gostaria que eu criasse um **Desafio de Simulado** para testar sua aplicação prática desses conceitos em um cenário real?