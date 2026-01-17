# 📝 Simulado de Certificação: Módulo 01 (50 Questões)
**Tema:** Modernização de Dados, Data Lakes e Governança no Google Cloud



---

**1.** Uma casa de câmbio digital precisa migrar 10 anos de dados históricos de um banco Oracle para o Google Cloud. O objetivo é realizar auditorias esporádicas com o menor custo de armazenamento possível, sem necessidade de alta performance de consulta. Qual estratégia deve ser adotada?
A) Ingerir os dados diretamente em tabelas nativas do BigQuery.
B) Armazenar os dados no Cloud Storage e utilizar External Tables no BigQuery.
C) Migrar os dados para o Cloud Spanner para garantir consistência global.
D) Utilizar o Datastream para replicar os dados históricos para o AlloyDB.

**2.** Você está a desenhar um pipeline para capturar transações de câmbio em tempo real de um banco MySQL. A solução deve garantir que alterações (inserts e updates) sejam refletidas no Data Lake sem sobrecarregar o banco de origem. Qual combinação de ferramentas é a recomendada?
A) Cloud Scheduler + BigQuery Load Jobs.
B) Datastream + Dataflow.
C) Pub/Sub + Cloud Functions.
D) BigQuery Data Transfer Service.

**3.** Uma empresa deseja unificar a governança de seus dados que estão espalhados em buckets do Cloud Storage (formato Parquet) e datasets do BigQuery. Eles precisam catalogar esses dados e aplicar políticas de segurança centralizadas. Qual serviço deve ser utilizado?
A) Cloud KMS.
B) Data Loss Prevention (DLP).
C) Dataplex.
D) Vertex AI.

**4.** Analistas de dados precisam cruzar o histórico de transações de 5 anos (armazenado no BigQuery) com o saldo atual do cliente que reside em um banco operacional AlloyDB. A consulta é pontual e o volume de dados no AlloyDB é pequeno. Qual a forma mais eficiente de realizar essa tarefa sem mover os dados do banco operacional?
A) Exportar o AlloyDB para CSV e carregar no BigQuery.
B) Usar External Query (Federated Query) via BigQuery.
C) Configurar um pipeline de streaming no Dataflow.
D) Usar o Datastream para mover o saldo para o Cloud Storage.

**5.** Uma equipe de Ciência de Dados quer prever a probabilidade de um cliente realizar uma nova operação de câmbio utilizando SQL, pois possuem prazos curtos e a equipe domina essa linguagem. Os dados já estão limpos na camada Curated do BigQuery. O que você recomendaria?
A) Exportar os dados para o Vertex AI para treinamento customizado em Python.
B) Utilizar o BigQuery ML (BQML) para criar e treinar o modelo.
C) Usar o Cloud SQL com extensões de ML.
D) Criar um script no Dataflow para realizar regressão logística.

**6.** Durante uma revisão de custos, você percebe que uma tabela de logs de 50 TB no BigQuery está a gerar cobranças altas em consultas que filtram apenas por "Data". Como otimizar essa tabela para reduzir o volume de dados lidos (pruning) e o custo?
A) Clusterizar a tabela pela coluna de Data.
B) Particionar a tabela pela coluna de Data.
C) Converter a tabela em uma External Table no Cloud Storage.
D) Criar uma View para limitar as colunas lidas.

**7.** Uma instituição financeira exige que as chaves de criptografia que protegem os dados em repouso no BigQuery sejam gerenciadas e rotacionadas por sua própria equipe de segurança, e não pelo Google. Qual recurso deve ser configurado?
A) Chaves de criptografia padrão do Google.
B) Customer-Managed Encryption Keys (CMEK) via Cloud KMS.
C) Data Loss Prevention (DLP) com criptografia determinística.
D) Identidade e Gerenciamento de Acesso (IAM) com papéis granulares.

**8.** Uma empresa possui Petabytes de dados históricos imutáveis no Cloud Storage em formato Parquet. Eles querem que os analistas de BI consultem esses dados via SQL no BigQuery, mantendo a performance e aplicando segurança de nível de coluna, mas sem pagar pelo armazenamento duplicado no BigQuery. Qual a solução ideal?
A) BigQuery Native Tables.
B) BigQuery External Tables.
C) BigLake.
D) Cloud Spanner.

**9.** Você precisa processar mensagens de um sistema de câmbio que chegam via streaming. O pipeline deve realizar cálculos de conversão de moeda e lidar com dados que chegam atrasados (late data) usando janelas de tempo. Qual ferramenta é projetada para essa complexidade?
A) Cloud Storage.
B) BigQuery Load Jobs.
C) Dataflow.
D) Datastream.

**10.** Um engenheiro de dados configurou o Datastream para capturar mudanças de um banco Oracle. No entanto, o analista reclama que as tabelas no BigQuery não estão a ser atualizadas automaticamente. O que está a faltar na arquitetura para que a sincronização seja contínua?
A) Uma chave primária no BigQuery.
B) Um pipeline do Dataflow para ler do GCS e aplicar as mudanças no BigQuery.
C) Habilitar o modo "Auto-sync" no Datastream.
D) Converter os dados para formato Avro manualmente.

**11.** Uma empresa de câmbio precisa de um banco de dados transacional para um novo aplicativo que operará em 3 continentes. O requisito principal é a consistência forte global e alta disponibilidade (99,999%). Qual banco você escolheria?
A) AlloyDB.
B) Cloud SQL.
C) Cloud Spanner.
D) BigQuery.

**12.** No processo de limpeza de dados na camada Processed, você precisa garantir que números de cartões de crédito e nomes de clientes não sejam visíveis para os analistas, mas sem eliminar a informação da fonte bruta. Qual serviço automatiza essa identificação e classificação de PII (Personally Identifiable Information)?
A) Cloud KMS.
B) Dataplex.
C) Data Loss Prevention (DLP).
D) BigQuery ML.

**13.** Você tem uma tabela de reclamações de clientes no BigQuery e deseja encontrar mensagens que sejam semanticamente parecidas com "problema no levantamento", mesmo que as palavras exatas não sejam as mesmas. Qual técnica deve ser aplicada?
A) Operador LIKE no SQL padrão.
B) Criação de índices de busca de texto completo.
C) Gerar Embeddings e usar VECTOR_SEARCH no BigQuery.
D) Criar uma Regressão Logística no BQML.

**14.** Uma empresa deseja implementar um padrão "Lakehouse". Eles querem as vantagens de governança de um Data Warehouse, mas a flexibilidade de armazenar dados brutos em formatos abertos (como Iceberg ou Parquet) no Cloud Storage. Qual tecnologia do Google Cloud habilita isso?
A) Cloud Storage nativo.
B) BigQuery ML.
C) BigLake.
D) Datastream.

**15.** Ao treinar um modelo de churn no BigQuery ML, você percebe que os dados de entrada mudaram significativamente no último mês. Como você garante que o modelo reflita esses novos dados?
A) O BigQuery ML faz o aprendizado incremental automaticamente.
B) É necessário executar novamente o comando CREATE OR REPLACE MODEL.
C) O Vertex AI atualizará o modelo no BigQuery via API.
D) Basta inserir novos dados na tabela de treinamento que o modelo se atualiza sozinho.

**16.** Qual é a principal característica da camada **Raw** em uma arquitetura de Data Lake no Cloud Storage?
A) Dados transformados e prontos para dashboards.
B) Dados brutos, imutáveis e armazenados exatamente como vieram da fonte (Append-only).
C) Tabelas com particionamento e clusterização obrigatórios.
D) Dados anonimizados por ferramentas de DLP.

**17.** Uma empresa quer reduzir custos de consulta no BigQuery. Eles têm uma tabela de transações imensa que é filtrada frequentemente por `ID_Loja` e `Data`. Qual a melhor estratégia de otimização?
A) Particionar por `ID_Loja` e clusterizar por `Data`.
B) Particionar por `Data` e clusterizar por `ID_Loja`.
C) Usar apenas tabelas externas.
D) Criar uma tabela separada para cada loja.

**18.** O time de marketing quer enviar notificações push para utilizadores da app de câmbio assim que uma cotação atingir um valor X. A latência deve ser de poucos segundos. Qual o fluxo de dados correto?
A) Oracle -> Datastream -> Cloud Storage -> BigQuery -> App.
B) Oracle -> Dataflow Batch -> BigQuery -> App.
C) Oracle -> Datastream -> Dataflow Streaming -> Pub/Sub -> App.
D) Oracle -> Export CSV -> BigQuery ML -> App.

**19.** Você precisa comparar a acurácia de dois modelos de classificação criados no BigQuery ML antes de colocá-los em produção. Qual comando SQL você utilizaria?
A) ML.PREDICT.
B) ML.EVALUATE.
C) ML.TRAINING_INFO.
D) SELECT * FROM MODEL.

**20.** Em uma auditoria de segurança, foi solicitado que todos os dados de "Alta Renda" sejam protegidos de forma que, se o acesso ao BigQuery for comprometido, os dados ainda estejam ilegíveis sem uma chave externa. O que resolve isso?
A) Papéis do IAM.
B) Cloud KMS com CMEK (Customer-Managed Encryption Keys).
C) Mascaramento de dados via DLP.
D) Criptografia padrão gerenciada pelo Google.

**21.** Qual o papel do **Vertex AI** em uma arquitetura onde o BigQuery ML já está a ser usado para modelos simples?
A) Substituir o BigQuery ML para reduzir custos de treinamento.
B) Lidar com cenários de ML avançados, redes neurais complexas e MLOps (pipelines de retreino contínuo).
C) Atuar como o banco de dados principal para os modelos do BigQuery.
D) Realizar apenas a visualização dos dados (dashboards).

**22.** Uma empresa quer permitir que o BigQuery consulte dados em um bucket do Cloud Storage (formato CSV) sem importar os dados, pois eles mudam pouco e o volume é baixo. Qual a solução de **menor custo de configuração**?
A) BigLake.
B) External Tables.
C) Tabelas nativas do BigQuery.
D) Datastream.

**23.** Você está a usar o **Dataplex** para gerenciar um Data Lake. Você criou uma "Zona" chamada `Financeiro`. O que acontece com os dados que você associa a essa zona?
A) Eles são movidos fisicamente para um novo bucket.
B) Eles são catalogados e metadados são extraídos para governança centralizada.
C) Eles são criptografados com KMS automaticamente.
D) Eles são convertidos de CSV para Parquet automaticamente.

**24.** O que caracteriza o BigQuery como uma solução **Serverless**?
A) Você precisa configurar o número de CPUs e Memória para cada consulta.
B) O Google gerencia toda a infraestrutura e escala recursos automaticamente sob demanda.
C) Ele só funciona se não houver servidores na rede local da empresa.
D) Ele exige a instalação de um software de gerenciamento de nós.

**25.** Uma empresa de câmbio quer migrar o seu sistema de cotações que exige atualizações frequentes e sub-milissegundos de latência em gravações. O banco de dados atual não aguenta a carga. Eles buscam uma solução que suporte o padrão PostgreSQL com performance de classe empresarial. Qual a escolha?
A) AlloyDB.
B) Cloud Spanner.
C) BigQuery.
D) Cloud Storage.

**26.** Qual técnica do BigQuery permite que ele pule a leitura de blocos inteiros de dados baseando-se em um filtro de data, reduzindo o custo da consulta?
A) Clustering.
B) Partitioning (Pruning).
C) Indexação B-Tree.
D) Normalização.

**27.** O Data Loss Prevention (DLP) identificou que há nomes de clientes em uma coluna de comentários. Você decide usar a técnica de **Redação (Redaction)**. O que acontece com o dado original na visualização protegida?
A) O nome é substituído por um código aleatório (Token).
B) O nome é removido ou coberto (ex: substituído por [REDACTED]).
C) O dado é criptografado e pode ser aberto com uma chave KMS.
D) A linha inteira é eliminada da tabela.

**28.** No BigQuery ML, o que são os **Labels (Targets)**?
A) São os nomes das tabelas de origem.
B) É a coluna que o modelo tentará prever (ex: o valor da moeda).
C) São as variáveis explicativas (ex: idade, país, saldo).
D) É o nome dado ao modelo após o treinamento.

**29.** Você tem uma tabela particionada por "Dia" e clusterizada por "País". Qual consulta será mais eficiente em termos de custo?
A) SELECT * FROM tabela WHERE Pais = 'Portugal'.
B) SELECT * FROM tabela WHERE Data = '2023-10-01' AND Pais = 'Portugal'.
C) SELECT * FROM tabela WHERE Data > '20