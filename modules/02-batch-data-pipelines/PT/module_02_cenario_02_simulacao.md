Aqui está o arquivo do **Módulo 02** com a formatação aplicada, mantendo a hierarquia visual, o ritmo de leitura e a fidelidade absoluta ao texto original que você forneceu.

---

# 📘 Módulo 02 — Cenário de Aplicação: Licitação

## Modernização de Dados no Google Cloud (Data Lake e Data Warehouse)

---

## 🎯 Objetivo

Projetar uma arquitetura moderna, escalável e eficiente em custos no Google Cloud para substituir um sistema legado do **setor público**, permitindo que a empresa utilize seus dados tanto para **análises de negócio (Business Intelligence e Machine Learning)** quanto para **operações em tempo quase real**, seguindo os padrões recomendados pelo Google e cobrados na certificação **Google Professional Data Engineer**.

Este cenário foi escrito para que **mesmo alguém sem conhecimento prévio em dados** consiga entender o fluxo de dados:

* De onde os dados vêm;
* Onde eles são armazenados;
* Como são processados;
* Como são governados;
* Como são consumidos pelo negócio e por modelos de Machine Learning;
* Como o Log e as permissões são aplicados;
* A ordem do conteúdo abordado é o que foi visto em cada módulo;
* O Cenário não segue em ordem o que foi passado módulo por módulo, pois existe uma interseção onde cada conteúdo deve ser implementado.

---

## 📚 Conteúdos Abordados

### **(Módulo 1)**

* Arquitetura de Data Lake (camadas **Raw**, **Processed** e **Curated**)
* Ingestão de dados em **batch** e em **streaming**
* Tabelas externas (**External Tables**)
* Tabelas **Iceberg** e o padrão **Lakehouse (BigLake)**
* **Change Data Capture** com **Datastream**
* Pipelines de dados com **Dataflow**
* Comparação entre **AlloyDB** e **Cloud Spanner**
* Consultas federadas com **External Query**
* **Particionamento**, **pruning** e otimização de custos
* **BigQuery** como Data Warehouse serverless
* **BigQuery ML** para modelos analíticos
* **Vertex AI** para cenários avançados de Machine Learning
* Governança de dados com **Dataplex**
* **Data Loss Prevention (DLP)**
* **Cloud Key Management Service (KMS)**
* **Embeddings** e **Vector Search** no BigQuery

### **(Módulo 2)**

* Níveis de Controle (**Maturidade Arquitetural**)
* Fundamentos: **Bounded vs Unbounded Dataset**
* **Schema Management** (Evolution vs Enforcement)
* **Dataflow** (Apache Beam, SDK & Templates)
* **Dataproc Serverless** (Apache Spark)
* **Cloud Data Fusion** (Visual ETL)
* **Dead Letter Queue (DLQ)** & Resiliência
* Orquestração: **Cloud Composer** vs. **Cloud Workflows**
* **Cloud Logging** (Centralized Analysis)
* **Cloud Monitoring** (Proactive Alerting)
* **IAM para Logs** (Privacy & Security)
* **Troubleshooting** de Rede e Infraestrutura (IAM + VPC)

---

## 🏗️ Etapa 1 — Contexto Atual da Empresa

Nossa empresa ganhou uma licitação e fechou um grande contrato com o **setor público**. No contrato em si é solicitado um upgrade na área de tecnologia de dados, pois o mesmo possui uma grande massa de dados, mas não trabalha com a inteligência que deveria. O sistema possui diversas particularidades e cenários específicos que devem ser atendidos.

### 🔹 Necessidades Críticas do Contrato

Para cumprir as exigências da licitação, precisamos implementar:

1. **Processamento Batch de Gigantes:** Lidar com os bancos legados massivos do setor público.
2. **Rotina Diária:** Processamento batch simples de D-1.
3. **Real-Time Crítico:** Conexões em tempo real para processamentos que não podem esperar.
4. **Inovação Digital:** Criação de um novo aplicativo para o projeto de inovação do setor.
5. **Inteligência Artificial:** Modelos de IA desde previsões simples até validações avançadas em tempo real.
6. **Análise de Chat:** O setor quer entender as conversas via chat e exige transparência no sistema de **Log e Monitoramento**.

### 🔹 Problemas Identificados (Pain Points)

Apesar de o ambiente ser confiável para o operacional, ele apresenta gargalos graves:

1. Os dados não estão organizados para análises modernas.
2. Relatórios são lentos e difíceis de manter.
3. O sistema não escala bem para grandes volumes históricos.
4. Exigência de um banco moderno, escalável e com baixa latência para o novo aplicativo.
5. Necessidade de analisar interações de chat com clientes/cidadãos.
6. Urgência em adequação à segurança e governança de dados.
7. Criação de um setor de IA de ponta para suporte à tomada de decisão.
8. Migração de Logs do data center para nuvem.

---

## 🏗️ Etapa 2 — Fontes de Dados (Detalhamento Integral)

### 🔹 1. Banco de Dados Oracle (ODS)

* **Natureza do Dado:** Estruturado / Bounded (Batch).
* **O que é:** O banco de dados relacional (Oracle) que sustenta as operações administrativas e financeiras do órgão há mais de uma década.
* **Quando usar no cenário:** Sempre que precisarmos de dados históricos consolidados, cadastros oficiais de cidadãos e registros de transações financeiras passadas.
* **Como se encaixa na arquitetura:** É a fonte primária da camada "Legacy". Os dados são extraídos periodicamente para alimentar o Data Lake.
* **Exemplos práticos no cenário:** Exportar a tabela de CADASTRO_USUARIOS para validar quem tem direito a novos benefícios sociais.
* **Por que isso é relevante para a certificação:** Representa o cenário clássico de migração de banco relacional on-premises para a nuvem.

### 🔹 2. Data Center Hadoop On-Premises (HDFS)

* **Natureza do Dado:** Semi-estruturado / Bounded (Batch Massivo).
* **O que é:** Um cluster de servidores físicos (Data Center local) contendo Petabytes de arquivos brutos de logs de auditoria e acessos dos últimos 10 anos.
* **Quando usar no cenário:** Para análises de comportamento de longo prazo e auditorias de segurança retroativas que exigem o reprocessamento de volumes gigantescos.
* **Como se encaixa na arquitetura:** Os arquivos (Parquet/Avro) são movidos para o Cloud Storage. O processamento é feito em "lotes" (batches) esporádicos devido ao volume imenso.
* **Exemplos práticos no cenário:** Analisar 500TB de logs históricos para identificar padrões de uso dos serviços públicos desde 2015.
* **Por que isso é relevante para a certificação:** Introduz o conceito de processamento distribuído (Spark) e migração de grandes volumes de arquivos em formato HDFS.

### 🔹 3. Arquivos Office 365 (SaaS)

* **Natureza do Dado:** Estruturado / Bounded (Batch).
* **O que é:** Planilhas Excel e documentos orçamentários armazenados na nuvem da Microsoft utilizados pelas equipes administrativas.
* **Quando usar no cenário:** Para cruzamento de metas orçamentárias e dados de RH que não estão nos bancos de dados principais.
* **Como se encaixa na arquitetura:** Ingestão via conectores Cloud-to-Cloud para enriquecer as tabelas do Data Warehouse.
* **Exemplos práticos no cenário:** Importar a planilha de "Metas de Vacinação" por bairro para comparar com os dados reais vindos do banco de dados.
* **Por que isso é relevante para a certificação:** Demonstra a integração com fontes SaaS (Software as a Service) externas ao ecossistema Google.

### 🔹 4. APIs Externas de Enriquecimento

* **Natureza do Dado:** Estruturado / Request-Response (Micro-batch/Real-time).
* **O que é:** Endpoints de outros órgãos (ex: Receita Federal, IBGE) que fornecem dados sob demanda via chamadas HTTP.
* **Quando usar no cenário:** Para validação de dados em tempo real (verificar se um CPF é válido) ou para buscar o endereço de um cidadão através do CEP durante um cadastro.
* **Como se encaixa na arquitetura:** O pipeline de dados faz chamadas para a API durante o processamento para "enriquecer" o registro antes de salvá-lo.
* **Exemplos práticos no cenário:** Ao receber um novo registro de chat, o sistema consulta uma API de tradução ou de validação cadastral automaticamente.
* **Por que isso é relevante para a certificação:** Trata da integração de fluxos de dados com serviços externos e a importância da qualidade do dado.

### 🔹 5. Telemetria de Frota e Ambulâncias (IoT)

* **Natureza do Dado:** Semi-estruturado / Unbounded (Streaming).
* **O que é:** Um fluxo contínuo de pequenas mensagens JSON enviadas por GPS e sensores de telemetria instalados nos veículos oficiais.
* **Quando usar no cenário:** Para gestão de logística, monitoramento de rotas e despacho de ambulâncias (SAMU) para as ocorrências mais próximas.
* **Como se encaixa na arquitetura:** Ingestão contínua para um sistema de mensageria, alimentando painéis de controle que nunca "param".
* **Exemplos práticos no cenário:** Visualizar no mapa, em tempo real, a velocidade média das ambulâncias em cada região da cidade.
* **Por que isso é relevante para a certificação:** Define o conceito de Unbounded Dataset (dados infinitos) e a necessidade de ingestão de baixa latência.

### 🔹 6. Sensores de Monitoramento Ambiental

* **Natureza do Dado:** Semi-estruturado / Unbounded (Streaming com Janelas).
* **O que é:** Dispositivos IoT que enviam leituras constantes sobre a qualidade da água e do ar em pontos estratégicos da cidade.
* **Quando usar no cenário:** Para alertas de desastres naturais (enchentes) ou picos de poluição que exijam ação imediata do poder público.
* **Como se encaixa na arquitetura:** Exige processamento temporal, onde os dados são agrupados em "janelas" (ex: média de poluição dos últimos 10 minutos).
* **Exemplos práticos no cenário:** Disparar um alerta sonoro na região se o nível de um rio subir 1 metro em menos de 15 minutos.
* **Por que isso é relevante para a certificação:** Essencial para entender o uso de Windows (janelas de tempo) e agregação de séries temporais.

### 🔹 7. Logs de Segurança do Novo Aplicativo

* **Natureza do Dado:** Semi-estruturado / Unbounded (Streaming de Eventos).
* **O que é:** Registros digitais de cada ação (login, clique, erro) realizada pelos cidadãos no novo aplicativo móvel do setor público.
* **Quando usar no cenário:** Para detecção de fraudes em tempo real (ataques de força bruta) e monitoramento da saúde técnica do app.
* **Como se encaixa na arquitetura:** Captura contínua de eventos para análise imediata por ferramentas de segurança e monitoramento de performance.
* **Exemplos práticos no cenário:** Bloquear o acesso de um usuário que tentou errar a senha 10 vezes em menos de 1 minuto em contas diferentes.
* **Por que isso é relevante para a certificação:** Foca na resiliência e no processamento de eventos de alta frequência para segurança cibernética.

### 🔹 8. Documentação Digitalizada (PDFs/Imagens)

* **Natureza do Dado:** Não Estruturado / Bounded (Batch).
* **O que é:** Arquivos de processos licitatórios escaneados, fotos de obras e documentos de identificação.
* **Quando usar no cenário:** Para processos de modernização onde é necessário extrair informações de papéis físicos ou classificar imagens de fiscalização.
* **Como se encaixa na arquitetura:** Armazenados no Cloud Storage para serem processados por ferramentas de IA e Visão Computacional.
* **Exemplos práticos no cenário:** Um sistema que lê o número do processo e o valor em um PDF de 500 páginas automaticamente.
* **Por que isso é relevante para a certificação:** Introduz o tratamento de dados não estruturados e o uso de Object Storage como repositório bruto.

### 🔹 9. Informações de Interação via Chat (Texto)

* **Natureza do Dado:** Semi-estruturado / Unbounded (Streaming).
* **O que é:** Transcrições e logs de texto das conversas entre os cidadãos e os atendentes (ou bots) no portal de serviços.
* **Quando usar no cenário:** Para análise de sentimento (o cidadão está satisfeito?) e identificação das dúvidas mais frequentes para melhorar o atendimento.
* **Como se encaixa na arquitetura:** O texto flui em tempo real para um motor de processamento que aplica técnicas de Linguagem Natural (NLP).
* **Exemplos práticos no cenário:** Identificar que 80% dos usuários no chat estão reclamando do mesmo serviço, permitindo uma correção rápida pela prefeitura.
* **Por que isso é relevante para a certificação:** Crucial para cenários de IA Analítica e monitoramento de logs de comunicação.

---

## 🏗️ Etapa 3 — Estratégia de Ingestão e Processamento

### 🔹 Datastream (O Motor de Captura - CDC)

* **Natureza:** Streaming (Captura de Mudanças).
* **O que é:** Um serviço de **CDC (Change Data Capture)** serverless que captura alterações em tempo real de bancos de dados.
* **Quando usar no cenário:** Para o Banco de Dados Oracle (ODS), sempre que houver uma inserção ou alteração de um registro de transação.
* **Como se encaixa na arquitetura:** Ele "escuta" o log do Oracle e replica as mudanças instantaneamente para o Cloud Storage ou BigQuery sem impactar a performance do banco original.
* **Exemplos práticos no cenário:** Capturar o momento exato em que um novo cidadão é cadastrado no sistema legando e enviar para a nuvem.
* **Por que isso é relevante para a certificação:** É a solução padrão para sincronização de bancos relacionais com baixa latência.

### 🔹 Cloud Dataflow Streaming (Processamento Contínuo)

* **Natureza:** Unbounded (Streaming Complexo).
* **O que é:** Motor de processamento baseado em **Apache Beam** para lógica pesada e janelas de tempo.
* **Quando usar no cenário:** Para Telemetria de Frota (IoT) e Interação via Chat (Texto).
* **Como se encaixa na arquitetura:** Ele processa os dados "em voo". É necessário aqui por causa das regras complexas: fazer análise de sentimento no chat ou calcular a velocidade média da ambulância.
* **Exemplos práticos no cenário:** Realizar o **embedding** (vetorização) das frases do chat para entender a intenção do cidadão em tempo real.
* **Por que isso é relevante para a certificação:** Única ferramenta que lida com **Janelas (Windowing)** e **Exactly-once** em escala massiva.

### 🔹 Cloud Data Fusion Streaming (Pipeline Visual)

* **Natureza:** Unbounded (Streaming Simples).
* **O que é:** Ferramenta visual que facilita a criação de pipelines sem código.
* **Quando usar no cenário:** Para os Logs de Segurança do Novo Aplicativo e Sensores Ambientais.
* **Como se encaixa na arquitetura:** Usado quando o objetivo é apenas mover e limpar o dado rapidamente (regras simples), como validar se o campo "temperatura" do sensor é nulo.
* **Exemplos práticos no cenário:** Criar um dashboard visual que mostra tentativas de login inválidas no app sem precisar escrever linhas de código Spark.
* **Por que isso é relevante para a certificação:** Focado em agilidade de desenvolvimento e interface visual.

### 🔹 Cloud Dataflow Batch (Processamento em Lote)

* **Natureza:** Bounded (Batch Complexo).
* **O que é:** Execução de pipelines do Beam para dados parados (arquivos).
* **Quando usar no cenário:** Para a Documentação Digitalizada (PDFs/Imagens).
* **Como se encaixa na arquitetura:** Ideal para extração de metadados de arquivos não estruturados que exigem paralelismo extremo.
* **Exemplos práticos no cenário:** Rodar um **OCR** (reconhecimento de texto) em milhares de PDFs de licitações simultaneamente para extrair valores de contratos.
* **Por que isso é relevante para a certificação:** Útil quando se quer usar o mesmo código (SDK) tanto para streaming quanto para batch.

### 🔹 Cloud Dataproc Serverless (Apache Spark)

* **Natureza:** Bounded (Batch Massivo/Científico).
* **O que é:** Serviço gerenciado para rodar Spark/Hadoop de forma elástica.
* **Quando usar no cenário:** Para o Data Center Hadoop On-Premises (HDFS).
* **Como se encaixa na arquitetura:** Como os logs já estão em formato HDFS/Parquet, o Dataproc é a ferramenta nativa para "engolir" esse legado.
* **Exemplos práticos no cenário:** Rodar um job Spark SQL que consolida 10 anos de logs históricos para criar uma base de treinamento para IA.
* **Por que isso é relevante para a certificação:** A escolha de ouro para migração de ecossistemas Hadoop e grandes transformações de data lake.

### 🔹 Cloud Data Fusion Batch (ETL Visual)

* **Natureza:** Bounded (Batch Simples).
* **O que é:** Interface visual para ingestão e transformação de dados em lote.
* **Quando usar no cenário:** Para Arquivos Office 365, APIs Externas e a carga inicial do Oracle.
* **Como se encaixa na arquitetura:** Funciona como o integrador universal. Conecta no SaaS (Office), chama a API e limpa os dados antes de jogar no BigQuery.
* **Exemplos práticos no cenário:** Buscar diariamente a planilha de metas orçamentárias no Office 365 e cruzar com os CPFs validados na API da Receita Federal.
* **Por que isso é relevante para a certificação:** Resolve integrações heterogêneas (SaaS + API + SQL) em um único ambiente visual.

---

## 🏗️ Etapa 4 — Organização do Lakehouse por Camadas

Nesta fase, estruturamos como os dados serão armazenados no Google Cloud. O objetivo é equilibrar o baixo custo de um Data Lake com a performance e segurança de um Data Warehouse, utilizando o conceito de **Lakehouse**.

### 🔹 Camada Raw (Dados Brutos)

* **O que é:** O estágio inicial onde o dado reside exatamente como foi extraído da fonte original, sem qualquer modificação em seu conteúdo ou estrutura.
* **Quando usar no cenário:** Serve como o repositório fiel de "origem" para auditorias do governo, perícias técnicas e como base para reprocessamento caso as regras de negócio das camadas seguintes precisem ser alteradas.
* **Como se encaixa na arquitetura:**
* **Ingestão:** Os dados são carregados via Dataflow, Dataproc ou Data Fusion para buckets específicos no Cloud Storage (GCS).
* **External Tables (Tabelas Externas):** Para evitar custos de movimentação e armazenamento duplicado, expomos esses arquivos no BigQuery como Tabelas Externas. Isso permite que analistas visualizem o dado bruto via SQL sem precisar importá-lo.
* **Dead Letter Queue (DLQ):** É aqui que a resiliência começa. Se um dado (como um log de chat ou sensor) estiver malformado e não puder ser lido, ele é desviado para uma subpasta de erros (`/errors`) nesta camada para análise posterior.


* **Exemplos práticos no cenário:**
* Dumps originais das tabelas do banco Oracle (ODS).
* Arquivos binários de logs históricos migrados do Hadoop On-Premises.
* Arquivos JSON brutos vindos da telemetria das ambulâncias e sensores ambientais.


* **Por que isso é relevante para a certificação:** Foca na **Imutabilidade** do dado e no uso de **External Tables** para otimização de custos.

### 🔹 Camada Processed (Dados Técnicos / Refinados)

* **O que é:** O estágio onde os dados são limpos, padronizados (ex: datas no formato ISO 8601) e convertidos para formatos otimizados para leitura (como Parquet ou Avro).
* **Quando usar no cenário:** É a base principal para engenheiros de dados e cientistas de dados realizarem transformações pesadas e treinamentos de modelos iniciais.
* **Como se encaixa na arquitetura:**
* **External Tables vs. BigLake:** Para dados estáticos e volumosos, usamos Tabelas Externas para reduzir custos. Para dados que exigem segurança granular e transações, usamos o **BigLake**.
* **Tabelas Iceberg (BigLake):** Essenciais para garantir **Transações ACID** (atomicidade e consistência). Isso é crucial no setor público para garantir que, se uma atualização de dados falhar, o sistema não fique com dados parciais ou corrompidos.
* **Schema Management (Governança):**
* **Schema Enforcement:** O pipeline bloqueia ativamente dados que tentam "sujar" a camada Processed com tipos errados.
* **Schema Evolution:** Se a API de uma prefeitura parceira adicionar um novo campo de informação, o Iceberg permite que a tabela evolua e aceite a nova coluna sem quebrar os processos existentes.


* **Dataplex:** Atua como o "orquestrador de governança", monitorando a qualidade dos dados e catalogando o que existe nesta camada para facilitar a busca.


* **Exemplos práticos no cenário:**
* Base de cidadãos com CPFs já validados e endereços padronizados.
* Logs de telemetria convertidos de JSON para Parquet, ocupando 80% menos espaço e sendo lidos 10x mais rápido.


* **Por que isso é relevante para a certificação:** Introduz a **Segurança em nível de coluna/linha** em arquivos do Data Lake via BigLake e a resiliência através do gerenciamento de esquemas.

### 🔹 Camada Curated (Dados de Consumo / Negócio)

* **O que é:** A camada final de alto valor agregado, onde os dados estão modelados em estrelas (**Star Schema**) ou agregados para responder perguntas de negócio.
* **Quando usar no cenário:** Para alimentar dashboards executivos no Looker, relatórios de transparência pública, previsões via BigQuery ML e modelos avançados no Vertex AI.
* **Como se encaixa na arquitetura:**
* **Tabelas Nativas do BigQuery:** Usadas para os dados de maior importância e acesso frequente, onde a performance de milissegundos é prioritária sobre o custo de armazenamento.
* **Tabelas BigLake:** Usadas para grandes agregados históricos que precisam permanecer no GCS por economia, mas ainda exigem filtros de segurança e performance.
* **Otimizações de Performance (Obrigatórias):**
* **Particionamento (Partitioning):** Divide os dados por data ou categorias. Essencial para que o BigQuery leia apenas o necessário (**Pruning**).
* **Clusterização (Clustering):** Organiza os dados dentro das partições por colunas de busca frequente (ex: ID do Órgão), acelerando drasticamente as consultas.


* **Consumidores de IA:** O dado aqui está pronto tanto para o **BigQuery ML** (IA via SQL para analistas) quanto para o **Vertex AI** (IA profunda para cientistas).


* **Exemplos práticos no cenário:**
* Tabela de "Gastos por Secretaria" particionada por ano e mês para auditorias rápidas.
* Dataset de "Previsão de Demanda Hospitalar" pronto para o BigQuery ML gerar previsões para o próximo mês.
* **Segurança e Governança:** Citamos a aplicação de **Cloud KMS** (criptografia) e **DLP** (mascaramento de dados sensíveis), que serão detalhados no módulo de Segurança.


* **Por que isso é relevante para a certificação:** **Particionamento** e **Clusterização** são os temas mais recorrentes para garantir o controle de custos de processamento (Slot Time).

---

## ⚙️ Etapa 5 — Modernização de Bancos de Dados (OLTP)

### 🔹 AlloyDB (O PostgreSQL com esteroides)

* **O que é:** Banco de dados relacional totalmente gerenciado, 100% compatível com PostgreSQL, mas com uma camada de armazenamento inteligente que o torna até 4x mais rápido que o Postgres padrão.
* **Quando usar no cenário:** Como o banco de dados principal do novo aplicativo do governo. Ele é a escolha ideal para substituir o Oracle ODS, herdando as transações pesadas, mas com performance de nuvem.
* **Como se encaixa na arquitetura:** Recebe os dados migrados do Oracle (via DMS/Data Fusion) e serve como o motor transacional para o cidadão. Ele possui um acelerador colunar que permite rodar relatórios rápidos sem travar o uso do aplicativo.
* **Exemplos práticos no cenário:**
* Registro e atualização de cadastros de cidadãos em milissegundos.
* Processamento de pagamentos de taxas públicas e emissão de guias.
* Gerenciamento de perfis de acesso e permissões do novo portal.


* **Por que substitui o Oracle?** No setor público, o licenciamento Oracle é caríssimo. O AlloyDB oferece performance superior, alta disponibilidade automática e custo operacional menor, sem "lock-in" de código.

### 🔹 Cloud Spanner (Escala Nacional e Consistência)

* **O que é:** O único banco de dados do mundo que une a estrutura relacional (SQL) com a escala horizontal ilimitada, mantendo consistência forte (**External Consistency**).
* **Quando usar no cenário:** Para sistemas nacionais de alta criticidade onde não pode haver erro de saldo ou duplicidade, como o Sistema Nacional de Saúde ou o Cadastro Único de Benefícios.
* **Como se encaixa na arquitetura:** Atua como o "Cérebro Global" da federação. Enquanto cada estado pode ter seu AlloyDB local, o Spanner garante que o registro de um benefício pago no Acre seja visível instantaneamente no Rio Grande do Sul.
* **Exemplos práticos no cenário:**
* Controle de estoque nacional de vacinas e medicamentos de alto custo.
* Sistema de identificação civil unificado (RG Nacional).
* Processamento de transferências de recursos governamentais com 99,999% de disponibilidade.


* **Por que substitui o Oracle?** Para escalar o Oracle nacionalmente, você precisaria de arquiteturas complexas (RAC/Data Guard). O Spanner escala apenas adicionando nós, sem janelas de manutenção e com consistência garantida.

### 🔍 External Query (Consulta Federada)

* **O que é:** Recurso que permite ao BigQuery executar uma consulta SQL diretamente dentro do AlloyDB ou Cloud Spanner sem mover o dado fisicamente.
* **Quando usar no cenário:** Quando um auditor do governo precisa cruzar dados históricos de 20 anos (BigQuery) com uma transação que acabou de acontecer (há 1 segundo) no AlloyDB.
* **Como se encaixa na arquitetura:** Utiliza a função `EXTERNAL_QUERY`. O BigQuery envia a pergunta para o banco transacional, o banco processa e devolve apenas a resposta.
* **Exemplos práticos no cenário:**
* **Relatório de Auditoria:** Cruzar histórico de pagamentos de 2010 com o status atual da conta para detectar inconsistências.
* **Monitoramento de Fraude:** Verificar se um CPF solicitando benefício no Spanner possui registros de óbito no histórico do BigQuery.


* **Por que isso é relevante para a certificação:** Resolve o problema do dado que ainda não foi ingerido (o "gap" do ETL).

> **💡 Nota de Engenharia: Spanner vs AlloyDB na Prova**
> * Se a questão fala em **"Escala Global/Regional Massiva"** e **"Disponibilidade de 5 noves (99,999%)"**: Marque **Spanner**.
> * Se a questão fala em **"Compatibilidade com Postgres"**, **"Migração de Oracle/Postgres"** e **"Custo-benefício com alta performance"**: Marque **AlloyDB**.
> 
> 

---

## 🤖 Etapa 6 — Inteligência Artificial (Machine Learning)

Nesta etapa, transformamos os dados armazenados no Lakehouse em inteligência preditiva e automação para o setor público.

### 🔹 BigQuery ML (O Oráculo SQL)

* **O que é:** Funcionalidade que permite criar, treinar e executar modelos de ML utilizando apenas linguagem SQL padrão, sem mover os dados para fora do Data Warehouse.
* **Quando usar no cenário:** Para modelos de regressão linear, classificação logística ou séries temporais onde os dados já estão estruturados no BigQuery.
* **Como se encaixa na arquitetura:** Atua diretamente na camada Curated, utilizando o poder de processamento distribuído do BigQuery.
* **Exemplos práticos no cenário:**
* **Previsão de Arrecadação:** Antecipar a receita tributária do próximo trimestre.
* **Detecção de Fraude em Contratos:** Identificar padrões suspeitos em licitações.


* **Limitações e Critérios Técnicos:**
* **Linguagem:** Exclusivamente SQL.
* **Tipo de Dado:** Tabelas estruturadas e séries temporais.
* **Complexidade:** Não suporta customização profunda de redes neurais (CNN/RNN).
* **Deploy:** Altíssima facilidade (função SQL imediata).


* **Por que isso é relevante para a certificação:** Representa o conceito de **"Data Gravity"** (levar o modelo até o dado).

### 🔹 Vertex AI (Cenários Avançados e Customizados)

* **O que é:** Plataforma de IA unificada (MLOps) que permite treinar modelos customizados com frameworks como TensorFlow, PyTorch e Scikit-learn.
* **Quando usar no cenário:** Quando o problema exige Deep Learning, processamento de imagens, áudio, ou controle total sobre os algoritmos.
* **Como se encaixa na arquitetura:**
* **Camada Raw:** Utilizada para ler arquivos não estruturados (fotos e PDFs).
* **Camada Processed:** Engenharia de atributos (**Feature Engineering**) avançada em arquivos Parquet.
* **Camada Curated:** Consome os dados modelados para o treinamento final e validação.


* **Exemplos práticos no cenário:**
* **Visão Computacional:** Analisar imagens de drones para identificar focos de desmatamento.
* **Modelos Epidemiológicos:** Criar redes neurais complexas para prever a propagação de doenças.


* **Limitações e Critérios Técnicos:**
* **Linguagem:** Python ou R.
* **Tipo de Dado:** Imagens, vídeos, áudio e textos de alta complexidade.
* **Processamento:** Ocorre em instâncias dedicadas (CPU/GPU).
* **Deploy:** Exige estrutura de MLOps para versionamento.


* **Por que isso é relevante para a certificação:** É a solução para problemas de alta complexidade que vão além do dado tabular.

### 🔹 Embeddings e Vector Search (Ciência do Significado)

* **O que é:** **Embeddings** são representações numéricas (vetores) que capturam o significado semântico. O **Vector Search** permite encontrar itens semelhantes comparando esses vetores.
* **Quando usar no cenário:** Para criar sistemas de busca inteligente, busca por similaridade jurídica ou para o Chatbot de atendimento ao cidadão.
* **Como se encaixa na arquitetura:** O pipeline gera vetores das interações e os armazena no Vertex AI Vector Search para consultas de baixa latência.
* **Exemplos práticos no cenário:**
* **Busca Jurídica Inteligente:** Encontrar leis por significado semântico (ex: buscar "barulho" e encontrar "poluição sonora").
* **Triagem de Chatbot (RAG):** O bot consulta uma base de conhecimentos vetorial para responder dúvidas contextualizadas.


* **Por que isso é relevante para a certificação:** É a espinha dorsal das aplicações de IA Generativa moderna.

---

## ⚡ Etapa 7 — Orquestração de Pipelines (O Sistema Nervoso Central)

Nesta etapa, definimos o "cérebro" que coordenará a execução de todas as tarefas da arquitetura.

### 🔹 Cloud Composer (O Maestro do Relógio e Massa de Dados)

* **O que é:** Um serviço gerenciado baseado no **Apache Airflow**. Utiliza Python para definir fluxos de trabalho chamados **DAGs**.
* **Quando usar no cenário:** Indispensável para pipelines de dados pesados que exigem agendamento fixo (Batch) ou lógica condicional profunda (rollback).
* **Como se encaixa na arquitetura (Visão Total):**
* **Mecânica de Disparo:** Focado em Tempo (**Scheduling**). Checa o relógio continuamente (**Polling**).
* **Integração:** Dispara o Data Fusion, aciona o Dataflow, executa SQL no BigQuery e comanda o Vertex AI.


* **Decisão Técnica e Infraestrutura:**
* **Baseado em Clusters:** Roda sobre GKE. Exige alto conhecimento técnico para gerenciar recursos de nós e memória.
* **Modelo de Custo:** Fixo e Contínuo (paga pela infraestrutura ligada).


* **Exemplos práticos no cenário:**
* **Fluxo de Auditoria Diária:** Rodar toda madrugada às 03:00 a extração e validação de dados.
* **Retreino de Modelo:** Agendar o retreino mensal de IA no Vertex AI após a consolidação mensal.


* **Por que isso é relevante para a certificação:** Prova sua capacidade de montar sistemas complexos com extensibilidade via Python.

### 🔹 Cloud Workflows (O Maestro do Evento e Reação Ágil)

* **O que é:** Ferramenta de orquestração HTTP/API 100% Serverless (YAML ou JSON).
* **Quando usar no cenário:** Perfeito para fluxos ágeis, orientados a Gatilhos (**Triggers**) e microsserviços.
* **Como se encaixa na arquitetura:**
* **Mecânica de Disparo:** Focado em Eventos (**Event-Driven**). Reage instantaneamente a novos arquivos ou erros.
* **Integração:** Atua como a "cola" entre microsserviços, disparando Cloud Functions ou APIs de IA.


* **Decisão Técnica e Infraestrutura:**
* **Totalmente Serverless:** Sem servidores para gerenciar. O Google escala tudo de forma transparente.
* **Agilidade Técnica:** Exige conhecimento médio (YAML/APIs). Foca na lógica em vez da infraestrutura.
* **Modelo de Custo:** **Pay-per-use** (custo zero parado).


* **Exemplos práticos no cenário:**
* **Análise de Documentos:** Assim que um fiscal sobe uma foto de obra, o Workflow chama a IA e salva o resultado.
* **Automação de Custos:** Ligar ou desligar clusters de teste baseado em alertas de orçamento.


* **Por que isso é relevante para a certificação:** Representa o uso moderno de automação serverless com eficiência financeira.

---

## 🛡️ Etapa 8 — Governança e Segurança de Dados

Nesta fase, blindamos o ambiente de dados da licitação.

### 🔹 Governança de Dados com Dataplex

* **O que é:** Uma malha de dados (**Data Fabric**) que centraliza o gerenciamento de ativos distribuídos no Cloud Storage e BigQuery.
* **Quando usar no cenário:** Para organizar o Lakehouse em "Lakes" e "Zones" lógicas para as diferentes secretarias (Saúde, Educação).
* **Como se encaixa na arquitetura:** Monitora metadados e qualidade de forma contínua sem precisar de um "disparo".
* **Exemplos práticos no cenário:** Padronização automática de layouts e alerta imediato se uma tabela de "Gastos" vier com CPFs em branco.
* **Por que isso é relevante para a certificação:** Foca na **"Modern Data Governance"** e na linhagem do dado (**Lineage**).

### 🔹 Data Loss Prevention (DLP) — Proteção de Dados Sensíveis

* **O que é:** Serviço gerenciado para descobrir, classificar e mascarar (**anonimizar**) dados sensíveis (PII).
* **Quando usar no cenário:** Sempre que dados contendo CPFs ou históricos médicos passarem pelo pipeline (desde a Camada Raw).
* **Como se encaixa na arquitetura:** Atua como um scanner integrado ao Dataflow ou Workflows, aplicando **Mascaramento** ou **Tokenização**.
* **Exemplos práticos no cenário:** Anonimização automática de nomes em relatórios epidemiológicos, permitindo análise estatística segura.
* **Por que isso é relevante para a certificação:** Ferramenta essencial para conformidade com a **LGPD**.

### 🔹 Cloud Key Management Service (KMS) — Soberania Criptográfica

* **O que é:** Gerenciador de chaves de criptografia (**CMEK**) que permite ao Governo ter Soberania Total sobre seus dados.
* **Quando usar no cenário:** Requisitos de segurança máxima onde o Google não deve ter acesso às chaves que abrem os dados.
* **Como se encaixa na arquitetura:** Configuramos BigQuery, GCS e AlloyDB para usar chaves do KMS. Exige permissão explícita de IAM para o processamento.
* **Exemplos práticos no cenário:** Backups do AlloyDB criptografados com chave que o governo rotaciona a cada 90 dias.
* **Por que isso é relevante para a certificação:** Diferenciar criptografia padrão de **CMEK (Customer-Managed Encryption Keys)**.

---

## 🛰️ Etapa 9 — Observabilidade e Troubleshooting

Garantimos que a arquitetura seja transparente e resiliente.

### 🔹 Cloud Logging (Análise Centralizada e Estruturada)

* **O que é:** Repositório centralizado de logs de todos os serviços. Resolve a pergunta: "Por que o processo X falhou?".
* **Quando usar no cenário:** Para depurar falhas em pipelines ou auditar acessos a tabelas sensíveis.
* **Como se encaixa na arquitetura:** Implementamos **Logs Estruturados (JSON)** com metadados (severidade, ID da secretaria).
* **Exemplos práticos no cenário:** Identificar quem executou uma consulta caríssima ou qual registro do Oracle travou a ingestão.
* **Por que isso é relevante para a certificação:** Foca no ganho de eficiência operacional e troubleshooting cirúrgico.

### 🔹 Cloud Monitoring (Alertas Proativos e Saúde do Sistema)

* **O que é:** Plataforma de métricas e dashboards que monitora a saúde do sistema e antecipa falhas.
* **Quando usar no cenário:** Para criar **Alertas Proativos** e garantir o SLA (disponibilidade) da licitação.
* **Como se encaixa na arquitetura:** Define limites (**thresholds**). Se a CPU do Spanner passar de 80%, dispara um alerta (SMS/E-mail).
* **Exemplos práticos no cenário:** Dashboard em tempo real mostrando a latência de resposta do novo aplicativo do cidadão.
* **Por que isso é relevante para a certificação:** Essencial para garantir que os dados estejam prontos para o negócio no tempo certo.

### 🔹 IAM para Logs (Privacidade e Segurança de Auditoria)

* **O que é:** Controle granular de acesso aos logs, aplicando o **princípio do menor privilégio**.
* **Quando usar no cenário:** Para separar quem vê logs operacionais (erros de código) de quem vê logs de auditoria (acessos financeiros).
* **Como se encaixa na arquitetura:** Atribuímos permissões como `logging.accessor` ou `logging.privateLogViewer`.
* **Exemplos práticos no cenário:** Desenvolvedor vê erros de navegação; Auditor-Chefe vê quem acessou a tabela de salários.
* **Por que isso é relevante para a certificação:** Segurança e conformidade são pilares fundamentais.

### 🔹 Troubleshooting de Rede e Infraestrutura (IAM + VPC)

* **O que é:** Processo de diagnóstico de falhas de comunicação e segurança na rede.
* **Quando usar no cenário:** Quando um pipeline falha por "Conexão Negada", permitindo distinguir erro de rede (Firewall) de credencial (IAM).
* **Como se encaixa na arquitetura:** Utilizamos **VPC Service Controls** e ferramentas como o **Connectivity Tests**.
* **Exemplos práticos no cenário:** Identificar que um job do Dataproc não lê o bucket porque falta a permissão `storage.objectViewer`.
* **Por que isso é relevante para a certificação:** O engenheiro de dados deve saber navegar na infraestrutura básica para resolver bloqueios de fluxo.

---