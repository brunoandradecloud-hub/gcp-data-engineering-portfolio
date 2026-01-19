# 📑 Guia de Estudos — Módulo 02 (Apache Spark, Dataflow e Orquestração)

## ⚖️ Bounded vs Unbounded Dataset

**O que é**  
Classificação dos dados conforme tenham ou não um fim definido.

**Quando usar**  
Bounded: dados históricos, batch.  
Unbounded: eventos contínuos, streaming.

**Onde se encaixa na arquitetura**  
Base para decidir entre batch ou streaming.

**Exemplo prático**  
Logs históricos → bounded.  
Eventos de pagamento em tempo real → unbounded.

**Ponto de prova / armadilha comum**  
❌ Achar que streaming é sempre melhor.  
✅ Streaming só faz sentido para dados não finalizados.

---

## 🌊 Dataflow

**O que é**  
Serviço serverless de processamento de dados baseado em Apache Beam.

**Quando usar**  
ETL em batch ou streaming com baixo gerenciamento operacional.

**Onde se encaixa na arquitetura**  
Entre ingestão e armazenamento analítico.

**Exemplo prático**  
Transformar CDC do Datastream antes de gravar no BigQuery.

**Ponto de prova / armadilha comum**  
❌ Exige criação de cluster.  
✅ Totalmente serverless.

---

## ⚡ Apache Spark no GCP

**O que é**  
Framework distribuído para processamento em larga escala.

**Quando usar**  
Jobs complexos, SQL avançado ou workloads legados em Spark.

**Onde se encaixa na arquitetura**  
Camada de processamento.

**Exemplo prático**  
Reprocessar anos de histórico financeiro.

**Ponto de prova / armadilha comum**  
❌ Spark é sempre melhor que Dataflow.  
✅ Spark exige gestão de cluster.

---

## 🧱 Dataproc

**O que é**  
Serviço gerenciado de clusters Spark/Hadoop.

**Quando usar**  
Quando você precisa de controle de cluster ou compatibilidade com Spark.

**Onde se encaixa na arquitetura**  
Processamento batch.

**Exemplo prático**  
Executar Spark SQL em dados do Cloud Storage.

**Ponto de prova / armadilha comum**  
❌ Dataproc é serverless.  
✅ Cluster é responsabilidade do cliente.

---

## 🧩 Spark vs Dataflow (Decisão de Prova)

**O que é**  
Comparação entre dois motores de processamento.

**Quando usar**  
Spark → cluster controlado, SQL complexo.  
Dataflow → serverless, streaming nativo.

**Onde se encaixa na arquitetura**  
Decisão de processamento.

**Exemplo prático**  
Equipe domina SQL → Dataflow SQL ou BigQuery.

**Ponto de prova / armadilha comum**  
❌ Escolher Spark sem necessidade de cluster.  
✅ Preferir serverless quando possível.

---

## 🧠 Performance (Spark vs PySpark vs Spark SQL)

**O que é**  
Interfaces diferentes do mesmo motor Spark.

**Quando usar**  
Spark SQL quando domina SQL.  
PySpark quando lógica é complexa.

**Onde se encaixa na arquitetura**  
Execução do job.

**Exemplo prático**  
Transformações SQL grandes → Spark SQL.

**Ponto de prova / armadilha comum**  
❌ Spark SQL é mais lento.  
✅ O plano de execução é o mesmo.

---

## 🧩 I/O Optimization & Batch Size

**O que é**  
Técnicas para reduzir custo e tempo de processamento.

**Quando usar**  
Grandes volumes de dados.

**Onde se encaixa na arquitetura**  
Pipelines de processamento.

**Exemplo prático**  
Ajustar tamanho de partições no Spark.

**Ponto de prova / armadilha comum**  
❌ Mais partições sempre é melhor.  
✅ Excesso gera overhead.

---

## 🕸️ Cloud Composer (Airflow)

**O que é**  
Orquestrador de workflows.

**Quando usar**  
Dependência entre jobs.

**Onde se encaixa na arquitetura**  
Camada de orquestração.

**Exemplo prático**  
Rodar Dataflow → BigQuery → ML.

**Ponto de prova / armadilha comum**  
❌ Composer executa ETL.  
✅ Ele só orquestra.

---

## 🔐 IAM e Contas de Serviço

**O que é**  
Controle de acesso a recursos.

**Quando usar**  
Sempre.

**Onde se encaixa na arquitetura**  
Camada de segurança.

**Exemplo prático**  
Dar acesso do Composer ao BigQuery.

**Ponto de prova / armadilha comum**  
❌ Editor resolve tudo.  
✅ Use princípio do menor privilégio.
