# 📑 Guia de Estudos — Módulo 01

## 🗄️ Arquitetura de Data Lake (Raw, Processed, Curated)

**O que é**
Armazenamento em camadas no Cloud Storage.

**Quando usar**
Para organizar dados desde a origem bruta até o consumo final.

**Onde se encaixa na arquitetura**
Base da pirâmide de dados.

**Exemplo prático**
Separar logs brutos de tabelas prontas para o Looker.

**Ponto de prova / armadilha comum**
❌ Achar que Curated só pode ser BigQuery.
✅ Curated pode ser BigLake no GCS.

---

## 📁 Tabelas Externas (External Tables)

**O que é**
Tabelas do BigQuery que leem dados diretamente do GCS sem importá-los.

**Quando usar**
Dados da camada Raw ou logs que são consultados raramente.

**Onde se encaixa na arquitetura**
Camada Raw do Data Lake.

**Exemplo prático**
Consultar um CSV histórico de 2015 sem pagar pela ingestão.

**Ponto de prova / armadilha comum**
❌ Performance alta.
✅ É mais lenta que a tabela nativa.

---

## ❄️ BigLake & Tabelas Iceberg (Lakehouse)

**O que é**
Camada que dá poderes de Data Warehouse (segurança granular) a arquivos no GCS.

**Quando usar**
Quando você quer unir a economia do Data Lake com a segurança do BigQuery.

**Onde se encaixa na arquitetura**
Camada Processed/Curated.

**Exemplo prático**
Dar acesso a uma coluna de arquivos Parquet apenas para o RH.

**Ponto de prova / armadilha comum**
❌ Substitui bancos transacionais.
✅ É para análise de dados imutáveis.

---

## 🔁 Datastream (CDC)

**O que é**
Captura de alterações em tempo real (Change Data Capture).

**Quando usar**
Sincronizar bancos operacionais (Oracle/MySQL) com a nuvem.

**Onde se encaixa na arquitetura**
Entre banco operacional e pipelines.

**Exemplo prático**
Refletir uma venda no Oracle instantaneamente no Cloud Storage.

**Ponto de prova / armadilha comum**
❌ Ele transforma dados.
✅ Ele apenas move os logs.

---

## 🌊 Dataflow (Pipelines)

**O que é**
Processamento de dados (ETL) baseado em Apache Beam.

**Quando usar**
Quando o dado precisa de limpeza, cálculo ou máscara "em movimento".

**Onde se encaixa na arquitetura**
Entre a Ingestão e o Armazenamento.

**Exemplo prático**
Converter dólar para real antes de salvar no BigQuery.

**Ponto de prova / armadilha comum**
❌ É a opção mais barata.
✅ É potente, mas o custo escala com o volume.

---

## ⚡ Otimização (Partitioning & Clustering)

**O que é**
Técnicas de organização física de dados no BigQuery.

**Quando usar**
Reduzir custos de consulta e aumentar velocidade.

**Onde se encaixa na arquitetura**
Tabelas na camada Curated (BigQuery Nativo).

**Exemplo prático**
Particionar por DATA_VENDA e clusterizar por ID_CLIENTE.

**Ponto de prova / armadilha comum**
❌ Clusterizar colunas com poucos valores.
✅ Use em colunas de alta cardinalidade.

---

## ⚙️ AlloyDB vs Cloud Spanner

**O que é**
Bancos operacionais (OLTP) de alta performance.

**Quando usar**
AlloyDB para migrar Oracle (regional); Spanner para escala global.

**Onde se encaixa na arquitetura**
Fonte dos dados ou banco do aplicativo.

**Exemplo prático**
Saldo bancário global no Spanner.

**Ponto de prova / armadilha comum**
❌ Usar como Data Warehouse.
✅ São bancos para transações rápidas.

---

## 🔗 External Query (Federated Query)

**O que é**
Consultar o AlloyDB/Spanner de dentro do BigQuery via SQL.

**Quando usar**
Enriquecer relatórios com dados "ao vivo" do banco operacional.

**Onde se encaixa na arquitetura**
Camada analítica.

**Exemplo prático**
Cruzar histórico de 10 anos com o saldo de hoje.

**Ponto de prova / armadilha comum**
❌ Usar para BI pesado.
✅ Impacta a performance do banco operacional.

---

## 🤖 BigQuery ML (BQML)

**O que é**
Criação de modelos de Machine Learning usando apenas SQL.

**Quando usar**
Previsões rápidas (Churn, Regressão) em dados já no BigQuery.

**Onde se encaixa na arquitetura**
Camada de consumo/IA.

**Exemplo prático**
CREATE MODEL para prever vendas do próximo mês.

**Ponto de prova / armadilha comum**
❌ Aprende sozinho conforme entram dados.
✅ Precisa ser recriado para atualizar.

---

## 🧠 Embeddings & Vector Search

**O que é**
Converter texto em números para buscar por "significado".

**Quando usar**
Analisar chats, reclamações e similaridade semântica.

**Onde se encaixa na arquitetura**
BigQuery (IA Generativa/Analítica).

**Exemplo prático**
Achar reclamações parecidas mesmo com palavras diferentes.

**Ponto de prova / armadilha comum**
❌ Usar para códigos numéricos.
✅ Use para texto livre.

---

## 🤖 Vertex AI

**O que é**
Plataforma completa de ML e MLOps.

**Quando usar**
Produção, retreinamento contínuo, APIs de ML.

**Onde se encaixa na arquitetura**
Fora do BigQuery, integrado à aplicação.

**Exemplo prático**
Detecção de fraude em tempo real.

**Ponto de prova / armadilha comum**
❌ Vertex AI substitui BigQuery ML.
✅ Eles são complementares.

---

## 🏛️ Dataplex

**O que é**
Camada de governança de dados.

**Quando usar**
Para catálogo, políticas e domínios.

**Onde se encaixa na arquitetura**
Sobre toda a arquitetura.

**Exemplo prático**
Governar Data Lake inteiro.

**Ponto de prova / armadilha comum**
❌ Dataplex processa dados.
✅ Dataplex só governa.

---

## 🔐 DLP

**O que é**
Classificação de dados sensíveis.

**Quando usar**
Para compliance e segurança.

**Onde se encaixa na arquitetura**
Sobre dados armazenados.

**Exemplo prático**
Identificar PII em tabelas.

**Ponto de prova / armadilha comum**
❌ DLP criptografa dados.
✅ DLP só classifica.

---

## 🔑 KMS

**O que é**
Gerenciamento de chaves de criptografia.

**Quando usar**
Para controle e auditoria de chaves.

**Onde se encaixa na arquitetura**
Proteção de dados em repouso.

**Exemplo prático**
CMEK (Customer-Managed Encryption Keys).

**Ponto de prova / armadilha comum**
❌ Google gerencia tudo.
✅ Cliente tem o controle da chave.