Com certeza! Para que você possa salvar o arquivo e manter o seu material de estudo organizado, formatei as 50 questões do **Módulo 02** seguindo exatamente o padrão visual e de estrutura do arquivo do Módulo 01 que você enviou.

Aqui está o conteúdo pronto para ser copiado e colado em um novo arquivo `.md`:

---

# 📝 Simulado de Certificação: Módulo 02 (50 Questões)

**Tema:** Processamento Serverless (Dataproc/Dataflow), Orquestração (Composer), Observabilidade e Governança.

---

**1.** A prefeitura possui 500TB de logs históricos no Hadoop local e quer processar esses dados no Google Cloud usando Spark, mas a equipe de infraestrutura é pequena e não pode gerenciar servidores ou clusters. Qual a melhor escolha?
A) Dataproc em modo Cluster (GCE).
B) Cloud Data Fusion.
C) Dataproc Serverless.
D) BigQuery External Tables.

**2.** Durante um job de Dataproc Serverless, você percebe que a escrita no BigQuery está falhando. Qual configuração técnica é obrigatória para que o conector Spark-BigQuery funcione corretamente?
A) Ativar o faturamento do projeto.
B) Definir um bucket temporário (Staging Bucket) no Cloud Storage.
C) Aumentar a RAM do nó mestre.
D) Converter os dados para formato PDF.

**3.** Um estagiário precisa criar um pipeline para filtrar cidadãos aprovados em um auxílio emergencial. Ele não tem experiência com programação em Python ou Java. O que você recomenda dentro do Dataflow para agilizar a entrega?
A) Criar um código customizado em Apache Beam.
B) Usar o Dataflow Job Builder UI (Templates No-code).
C) Instalar o Apache Airflow localmente.
D) Usar o Cloud Shell para editar arquivos YAML.

**4.** Na interface do Dataflow, antes de confirmar o início de um processamento de alto custo, qual recurso você deve utilizar para garantir que as permissões de rede e acesso aos buckets estão corretas?
A) Botão "Run Job".
B) Botão "Validate".
C) Aba "Logs".
D) Aba "Metrics".

**5.** Qual a principal diferença entre o Dataproc Serverless e o Dataflow para o Engenheiro de Dados?
A) Dataproc foca no ecossistema Apache Spark; Dataflow foca no modelo Apache Beam.
B) Dataproc é apenas para Streaming e Dataflow é apenas para Batch.
C) Dataflow exige gerenciar servidores físicos; Dataproc não.
D) Dataproc é visual e Dataflow é exclusivamente via linha de comando.

**6.** Por que o formato Avro é preferível para a ingestão de dados em pipelines de processamento massivo no Módulo 02?
A) Porque ele é legível por humanos como um arquivo CSV.
B) Por ser um formato binário que carrega o esquema (schema) junto com o dado.
C) Porque ele é o único formato que o BigQuery aceita.
D) Porque ele reduz o tamanho dos arquivos em 100%.

**7.** O contrato da licitação exige que o processamento dos dados ocorra exclusivamente na região de São Paulo (southamerica-east1). Onde você define essa restrição no Dataflow?
A) No código das transformações (Transform).
B) Nas variáveis de ambiente do BigQuery.
C) Na configuração de Região (Region) do Job durante a criação.
D) No nome do arquivo de saída.

**8.** Ao processar dados da frota de ambulâncias que enviam sinais constantes de GPS, o fluxo de dados não tem fim. Como chamamos tecnicamente esse conjunto de dados?
A) Bounded Dataset.
B) Unbounded Dataset.
C) Static Dataset.
D) Legacy Dataset.

**9.** Você precisa converter arquivos CSV da Secretaria de Saúde para tabelas no BigQuery de forma rápida. O Dataflow Job Builder UI é adequado para esta tarefa?
A) Não, ele só aceita arquivos binários.
B) Sim, ele possui templates prontos para ler CSV do Cloud Storage e gravar no BigQuery.
C) Sim, mas apenas se os arquivos tiverem menos de 1MB.
D) Não, o BigQuery não aceita dados vindos do Dataflow.

**10.** No Dataproc Serverless, o que acontece com a infraestrutura de computação após o término da execução de um Job Batch?
A) Ela continua ligada e gerando custos até ser desligada manualmente.
B) Ela é liberada automaticamente pelo Google (modelo Pay-per-use).
C) Você precisa entrar no console do GCE e deletar as instâncias.
D) Os dados processados são apagados junto com a infraestrutura.

**11.** O sistema precisa rodar uma auditoria complexa todo dia às 03:00 da manhã, envolvendo 10 passos interdependentes. Qual a ferramenta ideal para realizar esse agendamento (Scheduling) e controle de fluxo?
A) Cloud Workflows.
B) Cloud Composer (Apache Airflow).
C) Datastream.
D) Cloud Functions.

**12.** Qual linguagem de programação é utilizada para definir as DAGs (fluxos de trabalho) no Cloud Composer?
A) YAML.
B) SQL.
C) Python.
D) Java.

**13.** Você precisa de uma automação leve para reagir a um evento (ex: novo arquivo no bucket) e chamar uma API externa. Qual o melhor custo-benefício e simplicidade (Serverless)?
A) Cloud Composer.
B) Cloud Workflows.
C) Dataproc Serverless.
D) Looker.

**14.** Qual a principal desvantagem do Cloud Composer para projetos governamentais com orçamento extremamente reduzido?
A) Ele não suporta a linguagem Python.
B) Ele possui um custo fixo mínimo para manter o cluster (GKE) operando.
C) Ele é difícil de integrar com o BigQuery.
D) Ele só funciona para processamento em tempo real (Streaming).

**15.** No Cloud Workflows, como as etapas do processo são tecnicamente definidas?
A) Através de um grafo visual de arrastar e soltar.
B) Usando arquivos de configuração em formato YAML ou JSON.
C) Escrevendo scripts em Spark SQL.
D) Através de comandos de voz via assistente.

**16.** O conceito de "Backfilling" no Cloud Composer (Airflow) refere-se a:
A) Limpar os logs antigos do sistema.
B) Rodar o pipeline para datas passadas que não foram processadas por algum motivo.
C) Realizar o backup preventivo do banco de dados.
D) Aumentar o número de CPUs do cluster de forma manual.

**17.** O Cloud Workflows é considerado "Event-Driven". O que isso significa na prática da arquitetura?
A) Que ele roda baseado em um relógio de ponto fixo.
B) Que ele reage a gatilhos e eventos disparados pelo sistema em tempo real.
C) Que ele só funciona durante eventos de tecnologia.
D) Que ele exige intervenção humana manual para cada etapa do processo.

**18.** Qual ferramenta de orquestração do Google Cloud é baseada no projeto de código aberto Apache Airflow?
A) Cloud Data Fusion.
B) Cloud Workflows.
C) Cloud Composer.
D) Datastream.

**19.** Para um fluxo que exige "Retentativas" (Retries) automáticas com lógica de espera em caso de erro em uma API de terceiros, qual ferramenta é nativamente preparada para isso de forma leve?
A) Cloud Workflows.
B) BigQuery.
C) Cloud Storage.
D) Dataproc Serverless.

**20.** No contexto do Cloud Composer, o que representa uma DAG?
A) Um tipo de banco de dados NoSQL.
B) Um Grafo Acíclico Dirigido que define a sequência e dependência das tarefas.
C) Um erro crítico de sistema.
D) Uma chave de criptografia assimétrica.

**21.** Um pipeline do Dataflow falhou. Qual o primeiro lugar que o Engenheiro de Dados deve consultar para identificar o erro técnico detalhado?
A) Cloud Monitoring.
B) Cloud Logging.
C) Fatura do Google Cloud.
D) Portal de Transparência da prefeitura.

**22.** Você deseja visualizar um gráfico em tempo real do consumo de CPU dos seus jobs de processamento. Qual ferramenta fornece esse dashboard de métricas?
A) Cloud Logging.
B) Cloud Monitoring.
C) Cloud Workflows.
D) BigQuery ML.

**23.** No Google Cloud, o que são definidos como "Logs Estruturados"?
A) Registros de logs escritos em papel.
B) Registros formatados em JSON que permitem filtros e buscas avançadas por metadados.
C) Logs que são excluídos automaticamente a cada hora.
D) Logs que apenas os engenheiros do Google podem ler.

**24.** Um auditor precisa de um relatório técnico informando exatamente quem deletou uma tabela específica no BigQuery. Onde ele encontra essa informação?
A) Cloud Monitoring Metrics.
B) Cloud Audit Logs (dentro do Cloud Logging).
C) No código-fonte do pipeline de ingestão.
D) No histórico de navegação do Chrome do usuário.

**25.** Qual permissão de IAM permite que um técnico visualize logs operacionais comuns, mas o impede de visualizar logs com informações sensíveis de auditoria?
A) logging.privateLogViewer.
B) logging.viewer.
C) storage.admin.
D) bigquery.dataEditor.

**26.** Você configurou um alerta para ser notificado via e-mail se o erro de ingestão de dados ultrapassar 5%. Em qual ferramenta esse alerta foi configurado?
A) Cloud Monitoring.
B) Cloud Composer.
C) Dataflow UI.
D) Cloud KMS.

**27.** O erro "403 Forbidden" em um job de dados geralmente aponta para problemas relacionados a:
A) Conectividade de rede (Firewall).
B) Memória insuficiente (Out of Memory).
C) Permissões de acesso (IAM).
D) Falta de espaço em disco no Cloud Storage.

**28.** O erro "Connection Timeout" ou "Network Unreachable" sugere problemas de:
A) Senha de banco de dados incorreta.
B) Configuração de Rede (VPC, Firewall ou Sub-redes).
C) Arquivo CSV corrompido na origem.
D) Limite de armazenamento do BigQuery atingido.

**29.** Qual ferramenta do GCP permite simular o caminho de um pacote de rede para diagnosticar se um acesso será bloqueado por regras de firewall?
A) Connectivity Tests.
B) Cloud Workflows.
C) Cloud Shell.
D) Dataplex.

**30.** No serviço Cloud Logging, o que é definido como um "Sink"?
A) Uma ferramenta de limpeza física de servidores.
B) Um mecanismo para exportar logs automaticamente para o BigQuery ou Cloud Storage para análise.
C) Um erro fatal que interrompe a execução do sistema.
D) Uma chave de API secreta para acesso aos logs.

**31.** O sistema deve impedir que registros com o campo "Idade" preenchido como texto entrem na tabela final. Como chamamos essa trava de qualidade?
A) Schema Evolution.
B) Schema Enforcement.
C) Data Masking.
D) Data Encryption.

**32.** A Secretaria de Educação adicionou um novo campo "Nome_Escola" nos dados de origem. O pipeline aceitou o campo automaticamente sem falhas. Isso caracteriza o:
A) Schema Evolution.
B) Schema Enforcement.
C) Erro de segurança de dados.
D) Falha de validação de metadados.

**33.** Qual tecnologia permite que o BigQuery aplique permissões de segurança em nível de linha ou coluna em arquivos Parquet armazenados no Cloud Storage?
A) Dataproc Serverless.
B) BigLake (Tabelas com suporte a Iceberg/Delta).
C) Cloud Data Fusion.
D) Pub/Sub.

**34.** O Dataplex ajuda a organizar os dados da licitação em "Lakes" e "Zones". Qual a principal vantagem organizacional desta ferramenta?
A) Redução imediata de 50% nos custos de processamento.
B) Governança centralizada, qualidade de dados e facilidade de busca (Catálogo).
C) Substituição total da necessidade de linguagem SQL.
D) Permite minerar criptomoedas usando recursos ociosos.

**35.** Para ocultar automaticamente os últimos dígitos do CPF de um cidadão nos logs ou tabelas, qual ferramenta você integraria ao pipeline?
A) Data Loss Prevention (DLP).
B) Cloud Monitoring.
C) Cloud Composer.
D) Cloud Spanner.

**36.** No contexto de governação de dados, o que significa "Linhagem de Dados" (Data Lineage)?
A) O histórico de contratação dos engenheiros de dados.
B) O rastreamento da origem do dado e todas as transformações por onde ele passou.
C) O tempo de retenção do dado no disco rígido.
D) A estrutura hierárquica das pastas no Cloud Storage.

**37.** Qual o princípio de segurança que dita que cada sistema ou usuário deve ter apenas o acesso estritamente necessário para realizar sua função?
A) Princípio do Máximo Acesso.
B) Princípio do Menor Privilégio (Least Privilege).
C) Regra do Servidor Aberto.
D) Segurança por Obscuridade.

**38.** No Google Cloud, uma "Service Account" (Conta de Serviço) é utilizada para:
A) Criar uma conta de e-mail institucional para o cidadão.
B) Identificar e conceder permissões para que sistemas (como o Dataflow) acessem outros recursos.
C) Gerenciar o pagamento das faturas mensais do governo.
D) Realizar o atendimento via chat de suporte técnico.

**39.** O que acontece se você tentar executar um Job de Dataflow em uma região onde o seu projeto atingiu o limite de cota (Quota) de CPUs?
A) O Google Cloud aumenta a cota automaticamente sem custo.
B) O Job falha imediatamente com erro de cota insuficiente.
C) O Job é executado em velocidade reduzida.
D) Os dados são movidos automaticamente para outra conta de faturamento.

**40.** Qual a função fundamental do Cloud KMS no cenário da licitação de dados sensíveis?
A) Aumentar a velocidade de processamento do Spark.
B) Gerenciar as chaves de criptografia (CMEK) para proteger os dados em repouso.
C) Criar dashboards de BI para a prefeitura.
D) Monitorar o tráfego de rede em tempo real.

**41.** Por que é uma boa prática arquitetural colocar o bucket de staging e o job de Dataflow na mesma região geográfica?
A) Porque é proibido por lei utilizar regiões diferentes.
B) Para evitar custos de transferência de dados (egress) entre regiões e reduzir a latência.
C) Para garantir que os logs de erro sejam gerados em português.
D) Para aumentar artificialmente o custo da licitação.

**42.** Qual ferramenta é considerada "UI-First", facilitando a criação de pipelines para profissionais que estão migrando de ferramentas de ETL tradicionais?
A) Cloud Composer.
B) Dataflow Job Builder UI.
C) Cloud Shell.
D) gcloud CLI.

**43.** O BigQuery é classificado como um serviço "Serverless" primordialmente porque:
A) Ele não utiliza servidores físicos em nenhum local.
B) O usuário não precisa configurar, gerenciar ou escalar a infraestrutura de servidores.
C) Ele é um serviço totalmente gratuito para o setor público.
D) Ele só funciona se o usuário possuir uma conexão de internet dedicada.

**44.** Para processar dados de chat de cidadãos e identificar sentimentos em tempo real com baixa latência, qual a combinação ideal de ferramentas?
A) Pub/Sub + Dataflow Streaming.
B) Cloud Storage + Dataproc Batch.
C) Microsoft Excel + Cloud Workflows.
D) BigQuery ML + Cloud Composer.

**45.** O conceito de "Dead Letter Queue" (DLQ) em um pipeline de dados do Dataflow serve para:
A) Apagar mensagens de usuários que foram desativados.
B) Isolar registros que falharam no processamento para evitar a parada do pipeline e permitir auditoria posterior.
C) Enviar e-mails de marketing para endereços inválidos.
D) Aumentar a compressão de dados em tabelas de streaming.

**46.** Em relação ao custo operacional, qual a vantagem do Dataproc Serverless sobre a criação de um cluster Dataproc tradicional que permanece ligado 24/7?
A) Não existe vantagem de custo entre os dois modelos.
B) Você é cobrado apenas pelos segundos em que o job está efetivamente processando dados.
C) O modelo Serverless é mais caro, porém mais rápido.
D) O Google oferece descontos fixos apenas para o modelo de cluster gerenciado.

**47.** O que significa o termo "Pruning" em consultas SQL realizadas no BigQuery?
A) A deleção permanente de dados antigos para liberar espaço.
B) A capacidade de ler apenas as partições de dados necessárias, economizando tempo e custo de processamento.
C) A alteração do nome das colunas de uma tabela para nomes mais curtos.
D) A criptografia de campos sensíveis durante a consulta.

**48.** Se a licitação exige que o sistema seja resiliente a falhas catastróficas em um datacenter inteiro (Zona), qual configuração deve ser adotada?
A) Utilizar Recursos Regionais ou Multirregionais.
B) Utilizar Recursos Zonais exclusivos.
C) Armazenar os dados em um HD externo conectado via USB ao console.
D) Utilizar apenas o Cloud Shell para armazenamento de emergência.

**49.** No modelo de maturidade de dados do Módulo 02, o que marca a transição do nível manual para o nível automatizado?
A) A utilização de planilhas eletrônicas compartilhadas.
B) O uso de orquestradores (Composer/Workflows) para gerenciar o fluxo de dados sem intervenção humana.
C) A contratação de uma equipe maior de analistas de dados.
D) A migração de todos os dados para o formato PDF.

**50.** Qual a ferramenta padrão no Google Cloud para processar Petabytes de dados utilizando o modelo de programação e o SDK do Apache Beam?
A) Dataproc.
B) Dataflow.
C) Cloud Data Fusion.
D) BigQuery.

---

### 🗝️ Gabarito

| Q | R | Q | R | Q | R | Q | R | Q | R |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **1** | C | **11** | B | **21** | B | **31** | B | **41** | B |
| **2** | B | **12** | C | **22** | B | **32** | A | **42** | B |
| **3** | B | **13** | B | **23** | B | **33** | B | **43** | B |
| **4** | B | **14** | B | **24** | B | **34** | B | **44** | A |
| **5** | A | **15** | B | **25** | B | **35** | A | **45** | B |
| **6** | B | **16** | B | **26** | A | **36** | B | **46** | B |
| **7** | C | **17** | B | **27** | C | **37** | B | **47** | B |
| **8** | B | **18** | C | **28** | B | **38** | B | **48** | A |
| **9** | B | **19** | A | **29** | A | **39** | B | **49** | B |
| **10** | B | **20** | B | **30** | B | **40** | B | **50** | B |