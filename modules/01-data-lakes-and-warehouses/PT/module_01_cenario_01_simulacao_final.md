# 📘 Módulo 01 — Cenário 01 - FINAL

## Modernização de Dados no Google Cloud (Data Lake e Data Warehouse)

---

## 🎯 Objetivo

Projetar uma arquitetura moderna, escalável e eficiente em custos no Google Cloud para substituir um sistema legado baseado em Oracle, permitindo que a empresa utilize seus dados tanto para **análises de negócio (Business Intelligence e Machine Learning)** quanto para **operações em tempo quase real**, seguindo os padrões recomendados pelo Google e cobrados na certificação **Google Professional Data Engineer**.

Este cenário foi escrito para que **mesmo alguém sem conhecimento prévio em dados** consiga entender:

* De onde os dados vêm;
* Onde eles são armazenados;
* Como são processados;
* Como são governados;
* Como são consumidos pelo negócio e por modelos de Machine Learning.

---

## 📚 Conteúdos Abordados

* Arquitetura de Data Lake (camadas Raw, Processed e Curated)
* Ingestão de dados em batch e em streaming
* Tabelas externas (External Tables)
* Tabelas Iceberg e o padrão Lakehouse (BigLake)
* Change Data Capture com Datastream
* Pipelines de dados com Dataflow
* Comparação entre AlloyDB e Cloud Spanner
* Consultas federadas com External Query
* Particionamento, pruning e otimização de custos
* BigQuery como Data Warehouse serverless
* BigQuery ML para modelos analíticos
* Vertex AI para cenários avançados de Machine Learning
* Governança de dados com Dataplex
* Data Loss Prevention (DLP)
* Cloud Key Management Service (KMS)
* Embeddings e Vector Search no BigQuery

---

## 🏗️ Etapa 1 — Contexto Atual da Empresa

Uma **casa de câmbio digital** armazena há mais de **10 anos** seus dados em um banco de dados Oracle. Esse banco funciona como um **Operational Data Store (ODS)**, ou seja, ele foi criado para suportar operações do dia a dia, como registrar transações, clientes e campanhas de marketing.

Apesar de confiável, o ambiente apresenta problemas importantes:

1. Os dados não estão organizados para análises modernas.
2. Relatórios são lentos e difíceis de manter.
3. O sistema não escala bem para grandes volumes históricos.
4. A empresa deseja **criar um novo aplicativo digital**, o que exige um banco mais moderno, escalável e com baixa latência.
5. Empresa quer analisar as informações de contato com o cliente via chat.
6. Empresa precisa se adequar a segurança de dados.
7. Empresa quer criar um setor de IA de ponta para ajudar nas tomadas de decisões.

Por isso, a empresa decide **modernizar completamente sua arquitetura de dados no Google Cloud**.

---

## 🔄 Etapa 2 — Ingestão de Dados

### 🔹 Datastream (O Motor de Captura - CDC)

* **O que é:** Serviço sem servidor que realiza a captura de alterações (Change Data Capture) diretamente dos logs do banco de dados.
* **Quando usar no cenário:** Para capturar, em tempo real, cada venda de moeda ou alteração de cadastro que ocorre no Oracle.
* **Como se encaixa na arquitetura:** Conecta-se ao Oracle, extrai o CDC e entrega os arquivos formatados (Avro ou JSON) em um Bucket no Cloud Storage.
* **Exemplos práticos no cenário:**
* Sincronizar uma nova compra de Euro feita por um cliente.
* Capturar a alteração de um nível de fidelidade de usuário.
* Detectar a deleção de um registro para manter a conformidade.


* **Por que isso é relevante para a certificação:** É a ferramenta padrão para migrações com tempo de inatividade zero (Zero Downtime Migration).

### 🔹 Dataflow Streaming (Processamento Contínuo)

* **O que é:** Ferramenta de processamento de fluxo contínuo baseada em Apache Beam.
* **Quando usar no cenário:** Quando o dado que o Datastream capturou precisa ser limpo ou transformado antes de chegar ao Lakehouse.
* **Como se encaixa na arquitetura:** Recebe o fluxo vindo do Datastream (ou Pub/Sub), aplica regras de negócio e escreve na camada Raw do Cloud Storage.
* **Exemplos práticos no cenário:**
* Mascarar CPFs de clientes assim que saem do Oracle.
* Converter moedas estrangeiras para a base Real em tempo real.
* Filtrar transações com valores que fogem do padrão de segurança.


* **Por que isso é relevante para a certificação:** Foco em janelas de tempo (Windowing) e tratamento de dados em tempo real.

### 🔹 Dataflow Batch (Processamento em Lote)

* **O que é:** Processamento de grandes volumes de dados estáticos ou históricos.
* **Quando usar no cenário:** Para a carga inicial dos 10 anos de histórico que estão parados no Oracle ou para processar arquivos de parceiros externos recebidos uma vez ao dia.
* **Como se encaixa na arquitetura:** Lê grandes volumes do Oracle (via JDBC) ou arquivos do Cloud Storage e os processa de forma massiva para a camada Raw.
* **Exemplos práticos no cenário:**
* Migrar os 85 Terabytes de transações históricas para a nuvem.
* Processar o fechamento de câmbio consolidado do mês anterior.
* Carregar listas de sanções financeiras internacionais de arquivos CSV diários.


* **Por que isso é relevante para a certificação:** Ensina a gerenciar custos de processamento e paralelismo em larga escala.

---

## 🗄️ Etapa 3 — Arquitetura de Data Lakehouse

### 🔹 Camada Raw (Dados Brutos)

* **O que é:** Onde o dado reside exatamente como foi extraído da fonte.
* **Quando usar no cenário:** Como repositório fiel para auditoria e reprocessamento.
* **Como se encaixa na arquitetura:** O dado é carregado pelo Dataflow para o Cloud Storage. Por boa prática, expõe-se esses arquivos no BigQuery via External Tables, evitando custos de armazenamento duplicado.
* **Exemplos práticos no cenário:**
* Dump original de transações de 2016.
* Logs brutos de sistema.
* Arquivos JSON de interações de chat.


* **Por que isso é relevante para a certificação:** Uso de External Tables para economia de custo (Storage vs Native).

### 🔹 Camada Processed (Dados Técnicos)

* **O que é:** Dados limpos e padronizados, escritos em formato Parquet.
* **Quando usar no cenário:** Como a base de trabalho para engenheiros e cientistas de dados.
* **Como se encaixa na arquitetura:**
* **Tabelas Iceberg (BigLake):** Permite transações ACID (garantia de escrita correta) e segurança granular por coluna para diferentes usuários.
* **External Tables:** Para leitura rápida de arquivos históricos que não sofrem alterações.


* **Exemplos práticos no cenário:**
* Base de clientes com endereços validados.
* Transações com campos de data e hora padronizados (ISO 8601).
* Histórico de cotações limpo de "ruídos" técnicos.


* **Por que isso é relevante para a certificação:** O BigLake permite Segurança em nível de coluna em arquivos do Data Lake.

### 🔹 Camada Curated (Dados de Consumo)

* **O que é:** Camada final com dados modelados para o negócio.
* **Quando usar no cenário:** Para alimentar dashboards do Looker e modelos de Inteligência Artificial.
* **Como se encaixa na arquitetura:** Utiliza tabelas BigLake ou nativas. É obrigatório o uso de Particionamento (por data) e Clusterização (por Moeda ou ID do Cliente) para performance.
* **Exemplos práticos no cenário:**
* Tabela consolidada de lucro mensal por país.
* Segmentação de clientes pronta para o marketing.
* Dados agregados para treinamento de modelos de Churn.


* **Por que isso é relevante para a certificação:** Particionamento e Clusterização são fundamentais para passar na prova (Otimização de Custos).

---

## ⚙️ Etapa 4 — Modernização de Bancos de Dados (OLTP)

### 🔹 AlloyDB

* **O que é:** Banco de dados PostgreSQL totalmente gerenciado com processamento analítico acelerado.
* **Quando usar no cenário:** Como banco principal do novo aplicativo para gerenciar transações e perfis.
* **Como se encaixa na arquitetura:** Substitui o Oracle com a vantagem de ter um motor colunar que permite fazer BI rápido no banco operacional. Permite External Query (Consulta Federada) para que o BigQuery consulte saldos em tempo real sem mover os dados.
* **Exemplos práticos:**
* Registro de novos usuários no app.
* Consulta instantânea do saldo disponível.
* Execução de ordens de compra de moedas.


* **Por que substitui o Oracle?** Possui recuperação de desastres automática em segundos e custo de licenciamento muito inferior.

### 🔹 Cloud Spanner

* **O que é:** Banco relacional com escala horizontal e consistência forte em nível mundial.
* **Quando usar no cenário:** Para garantir que o saldo de um cliente que viaja entre Londres e Tóquio seja idêntico em ambos os lugares (evitando gasto duplo).
* **Como se encaixa na arquitetura:** Atua como o "Ledger" (livro-razão) global. Também suporta External Query, permitindo que o BigQuery cruze dados históricos com saldos mundiais ao vivo.
* **Exemplos práticos:**
* Transferências internacionais entre sedes globais.
* Liquidação financeira em múltiplas moedas.
* Manutenção de disponibilidade de 99,999% sem janelas de manutenção.


* **Por que substitui o Oracle?** O Oracle não consegue manter consistência global forte com a mesma facilidade de escala do Spanner.

### 🔍 External Query (Consulta Federada)

* **O que é:** Recurso que permite ao BigQuery executar SQL diretamente dentro do AlloyDB ou Cloud Spanner.
* **Quando usar no cenário:** Quando o analista de BI precisa de um dado que acabou de ser gerado e ainda não passou pelo pipeline de ingestão.
* **Como se encaixa na arquitetura:** Cria-se uma conexão entre BigQuery e o banco OLTP; o dado não sai da origem, apenas o resultado da consulta viaja.
* **Exemplos práticos no cenário:**
* Cruzar o histórico de 10 anos (BigQuery) com o saldo atual (AlloyDB) para um relatório de fidelidade.
* Verificar o status de uma remessa global no Spanner sem esperar o processo de ETL.
* Enriquecer dados analíticos com informações em tempo real.


* **Por que isso é relevante para a certificação:** É a solução perfeita para o desafio de "Zero ETL" e dados de última hora.

---

## 🤖 Etapa 5 — Inteligência Artificial (Machine Learning)

### 🔹 BigQuery ML (O Oráculo SQL)

* **O que é:** Uma extensão do BigQuery que permite criar modelos de Machine Learning usando SQL. As funções fundamentais para o ciclo de vida são:
* **ML.EVALUATE:** Serve para testar o quão "esperto" seu modelo está. Ele compara a previsão do modelo com dados reais que ele nunca viu antes, gerando métricas como Acurácia, Precisão e R².
* **ML.PREDICT:** É a execução. Você passa novos dados e ele devolve a probabilidade (ex: "Este cliente tem 85% de chance de cancelar a conta").


* **Quando usar no cenário:** Quando os dados estão em tabelas e você precisa de uma resposta preditiva rápida para decisões de negócio.
* **Como se encaixa na arquitetura:** Roda processamento distribuído dentro do próprio BigQuery, eliminando a necessidade de exportar terabytes de dados para um servidor externo.
* **Exemplos práticos no cenário:**
* Usar ML.EVALUATE para checar se o modelo de previsão de alta do Dólar está errando por muito ou pouco.
* Rodar ML.PREDICT toda manhã para listar clientes com alto risco de churn.
* Usar ML.PREDICT para sugerir um limite de compra de moeda estrangeira para um novo usuário.


* **Por que isso é relevante para a certificação:** A prova foca na eficiência de "levar o modelo ao dado" em vez de levar o dado ao modelo.

### 🔹 Embeddings e Vector Search (A Ciência do Significado)

* **O que é:**
* **Embeddings:** Converter uma frase complexa de chat em uma lista de números (um vetor). Palavras com significados parecidos ficam "perto" matematicamente.
* **Vector Search:** Motor que vasculha milhões desses vetores em milissegundos. Ele busca por distância matemática (intenção), não por palavras iguais.


* **Quando usar no cenário:** Para organizar e extrair valor de milhares de conversas de chat e documentos de compliance que não cabem em uma tabela comum.
* **Como se encaixa na arquitetura:** O modelo de Embedding do Vertex AI processa o texto; os vetores são guardados no BigQuery ou no Vertex Vector Search; e a função VECTOR_SEARCH realiza a consulta rápida.
* **Exemplos práticos no cenário:**
* **Busca por Intenção:** Um cliente digita "não vejo meu saldo" e o sistema identifica que é o mesmo problema de "transferência pendente".
* **Detecção de Fraude Semântica:** Identificar padrões de conversas de golpistas que mudam as palavras, mas mantêm a estratégia.
* **Agrupamento Automático:** O sistema lê 50 mil chats e identifica que 30% reclamam especificamente sobre a taxa do Euro.


* **Por que isso é relevante para a certificação:** É a base para RAG (Retrieval-Augmented Generation) e busca semântica moderna.

---

## 🔐 Etapa 6 — Segurança e Governança

### 🔹 Dataplex (A Torre de Controle)

* **O que é:** Um serviço de governança inteligente que "enxerga" todo o seu Data Lake.
* **Qualidade de Dados (Auto Data Quality):** Cria regras (ex: "email não nulo") e varre os dados automaticamente, gerando alertas.
* **Descoberta e Catalogação:** Identifica dados sensíveis (PII) e cria um catálogo pesquisável com linhagem (origem).


* **Quando usar no cenário:** Para evitar que o Data Lake vire um pântano e para garantir que apenas dados limpos alimentem os dashboards.
* **Como se encaixa na arquitetura:** Ele "abraça" as camadas Raw, Processed e Curated, monitorando a integridade do dado em cada passagem.
* **Exemplos práticos no cenário:**
* Bloquear uma carga do Oracle se o campo "valor_transacao" vier com letras.
* Identificar automaticamente tabelas com CPFs para aplicar regras de privacidade.
* Ver o caminho completo (linhagem) de um dado desde o Oracle até o Looker.


* **Por que isso é relevante para a certificação:** Foco em Governança Centralizada em ambientes multicloud e multiregião.

### 🔹 Cloud Data Loss Prevention - DLP (O Escaneador de Segredos)

* **O que é:** Ferramenta especializada em ler conteúdos e identificar informações sensíveis por meio de padrões (Regex) e IA.
* **Quando usar no cenário:** Para garantir que nenhum analista humano veja dados que deveriam ser secretos (como passaportes ou cartões).
* **Como se encaixa na arquitetura:** Atua como um filtro na camada Processed, "anonimizando" os dados antes deles serem liberados para consumo.
* **Exemplos práticos no cenário:**
* **Redação:** Apagar nomes de clientes em transcrições de áudio.
* **Mascaramento:** Substituir o meio de um CPF por asteriscos (123.***.***-99).
* **Tokenização:** Substituir o número de um cartão por um código aleatório que pode ser revertido.


* **Por que isso é relevante para a certificação:** Fundamental para questões sobre Compliance (LGPD/GDPR).

### 🔹 Cloud Key Management Service (KMS)

* **O que é:** Cofre centralizado para chaves de criptografia gerenciadas pelo cliente (CMEK).
* **Quando usar no cenário:** Para transações de alto valor onde a lei exige que o Google não tenha acesso às chaves que abrem os dados.
* **Como se encaixa na arquitetura:** Funciona como uma "chave mestra". Se a permissão no KMS for revogada, o BigQuery não consegue ler as tabelas.
* **Exemplos práticos no cenário:**
* Criar chaves separadas para os dados de câmbio de cada país.
* Rotacionar a chave de criptografia a cada 90 dias automaticamente.
* Revogar o acesso instantâneo aos dados em caso de suspeita de invasão.


* **Por que isso é relevante para a certificação:** Foco em CMEK vs Google-Managed Keys.

---