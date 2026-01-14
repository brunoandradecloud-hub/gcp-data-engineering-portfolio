# 📘 Módulo 01 — Cenário 01
## Modernização de Dados no Google Cloud (Data Lake e Data Warehouse)

---

## 🎯 Objetivo

Projetar uma arquitetura moderna, escalável e eficiente em custos no Google Cloud para substituir um sistema legado baseado em Oracle, permitindo que a empresa utilize seus dados tanto para **análises de negócio (Business Intelligence e Machine Learning)** quanto para **operações em tempo quase real**, seguindo os padrões recomendados pelo Google e cobrados na certificação **Google Professional Data Engineer**.

Este cenário foi escrito para que **mesmo alguém sem conhecimento prévio em dados** consiga entender:

- de onde os dados vêm,
- onde eles são armazenados,
- como são processados,
- e como são consumidos pelo negócio.

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

---

## 🏗️ Descrição do Cenário

### Contexto Atual da Empresa

Uma **casa de câmbio digital** armazena há mais de **10 anos** seus dados em um banco de dados Oracle.
Esse banco funciona como um **Operational Data Store (ODS)**, ou seja, ele foi criado para suportar operações do dia a dia, como registrar transações, clientes e campanhas de marketing.

Apesar de confiável, o ambiente apresenta problemas importantes:

1. Os dados não estão organizados para análises modernas.
2. Relatórios são lentos e difíceis de manter.
3. O sistema não escala bem para grandes volumes históricos.
4. A empresa deseja **criar um novo aplicativo digital**, o que exige um banco mais moderno, escalável e com baixa latência.

Por isso, a empresa decide **modernizar completamente sua arquitetura de dados no Google Cloud**.

---

## 🔄 Estratégia de Ingestão de Dados

### 1️⃣ Ingestão Histórica e Processamento Diário (D-1)

- Utiliza-se o **Dataflow em modo batch** para extrair dados históricos do Oracle e arquivos externos (como arquivos CSV).
- Os dados são gravados no **Cloud Storage** em formato **Parquet**, que é compacto e eficiente para leitura analítica.

Esse processo é usado para dados que **não precisam estar disponíveis em tempo real**, como relatórios diários e análises históricas.

---

### 2️⃣ Ingestão em Tempo Quase Real (Change Data Capture)

- O **Datastream** é configurado para capturar todas as mudanças que acontecem no banco Oracle (inserções, atualizações e exclusões).
- Essas mudanças são processadas pelo **Dataflow em modo streaming**, permitindo que informações críticas estejam disponíveis rapidamente.

Esse modelo atende decisões que exigem **dados atualizados quase em tempo quase real**, sem sobrecarregar o sistema legado.

---

## 🗄️ Arquitetura do Data Lake

O Data Lake é armazenado no **Cloud Storage** e organizado em três camadas para facilitar governança, auditoria e processamento.

---

### 🔹 Camada Raw (Dados Brutos)

**O que é**  
A camada Raw armazena os dados **exatamente como eles chegam da fonte**, sem qualquer modificação.

**Características**

- Dados apenas adicionados (append-only)
- Nenhuma atualização ou exclusão
- Nenhuma concorrência planejada

**Tecnologias**

- Cloud Storage
- Tabelas Externas (External Tables) no BigQuery

**Por que isso faz sentido**

- É a forma mais barata de armazenar dados
- Mantém o histórico completo para auditoria
- Não exige controle transacional

---

### 🔹 Camada Processed (Dados Tratados)

**O que é**  
Aqui os dados são limpos, tipados corretamente e passam por regras técnicas e de negócio básicas.

**Tecnologias Utilizadas**

1. **Tabelas Externas**
   - Usadas para dados intermediários e pipelines simples
   - Lidas diretamente do Cloud Storage em formato Parquet
   - Otimizadas por particionamento e pruning

2. **Tabelas Iceberg (BigLake)**
   - Usadas quando os dados exigem:
     - Controle transacional (ACID)
     - Histórico de versões (Time Travel)
     - Reprocessamentos frequentes
     - Múltiplos consumidores
   - Os dados continuam armazenados no Cloud Storage, mas com uma camada de metadados inteligente

**Importante**

- O particionamento é definido explicitamente
- Não existe clusterização nessa camada

---

## 🧠 Camada Curated (Dados para Consumo)

**O que é**  
A camada Curated contém os dados prontos para consumo por pessoas e sistemas.

Ela é usada para:

- Dashboards
- Relatórios executivos
- Análises de negócio
- Machine Learning

---

### Tipos de Dados na Camada Curated

1. **Tabelas Nativas do BigQuery**
   - Dados consolidados
   - Modelagem de Data Warehouse (fatos e dimensões)
   - Particionadas e clusterizadas para alta performance

2. **BigLake (Padrão Lakehouse)**
   - Grandes volumes históricos permanecem no **Cloud Storage**
   - O BigQuery acessa esses dados via BigLake
   - Segurança granular por coluna e linha
   - Evita duplicação de dados entre equipes de Business Intelligence e Ciência de Dados

---

## ⚙️ Modernização do Sistema Operacional

### AlloyDB (Banco Transacional Moderno)

Para suportar o novo aplicativo digital, o Oracle é substituído por **AlloyDB**, que é:

- Um banco relacional totalmente gerenciado
- Compatível com PostgreSQL
- Muito mais rápido e escalável que o Oracle legado
- Ideal para aplicações transacionais modernas

**Exemplos de uso**

- Cadastro de clientes
- Saldo atual
- Status de transações

---

### Cloud Spanner (Expansão Global)

Caso o aplicativo cresça para múltiplos países e precise de consistência global:

- O **Cloud Spanner** passa a ser considerado
- Ele oferece escrita distribuída e consistência forte global

📌 Nesse caso, a migração de AlloyDB para Cloud Spanner seria necessária.

---

## 🔍 Consultas Federadas (External Query)

- O BigQuery pode executar consultas diretas em AlloyDB ou Cloud Spanner usando **External Query**
- Esse recurso é usado apenas para:
  - Enriquecimentos leves
  - Consultas pontuais
  - Informações em tempo quase real

Não é utilizado para relatórios pesados ou dashboards.

---

## ☁️ BigQuery como Plataforma Analítica

O BigQuery é utilizado como o principal mecanismo analítico porque:

- É totalmente gerenciado pelo Google
- Não exige administração de servidores
- Escala automaticamente
- Alta disponibilidade nativa

Os engenheiros precisam apenas:

- colocar os dados
- escrever consultas SQL

---

## ✅ Resumo Final da Arquitetura

- **Sistema Operacional**: Oracle → AlloyDB (→ Cloud Spanner para escala global)
- **Streaming**: Datastream + Dataflow
- **Data Lake**:
  - Cloud Storage (camadas Raw e Processed)
  - Cloud Storage + BigLake (dados históricos na Curated)
- **Analítico**:
  - BigQuery (tabelas nativas)
  - BigQuery + BigLake (acesso a dados no Cloud Storage)
- **Consumo**:
  - Dashboards
  - Relatórios
  - Machine Learning
  - Novo aplicativo digital

---

### 🎯 Conclusão

Essa arquitetura:

- resolve limitações do sistema legado,
- prepara a empresa para um aplicativo moderno,
- reduz custos,
- aumenta escalabilidade,
- e segue exatamente os padrões cobrados na certificação **Google Professional Data Engineer**.
