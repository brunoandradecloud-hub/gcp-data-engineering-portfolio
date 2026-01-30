Com certeza, Engenheiro(a)! Manter a padronização é essencial para que o seu repositório de estudos fique organizado e profissional.

Segui exatamente o padrão visual do arquivo anterior, integrando as respostas e as explicações detalhadas logo abaixo de cada questão para manter o "ritmo de estudo" que você solicitou.

Aqui está o conteúdo pronto para o seu arquivo:

---

# 📝 Simulado de Certificação: Módulo 02 (50 Questões - Parte 2)

**Tema:** Processamento Serverless (Dataproc/Dataflow), Orquestração (Composer), Observabilidade e Governança.

---

**1.** Uma equipe técnica pequena precisa processar 800TB de logs históricos armazenados no Hadoop on-premises usando Spark. A empresa não tem capacidade para gerenciar clusters ou servidores. Qual é a melhor solução no Google Cloud?
A) Dataproc em modo Cluster tradicional (GCE)
B) Cloud Data Fusion com conectores Hadoop
C) Dataproc Serverless
D) BigQuery External Tables sobre HDFS

**Resposta: C**
**Explicação:** ✅ O **Dataproc Serverless** é a escolha ideal porque elimina a necessidade de gerenciar clusters. É perfeito para workloads Spark legados onde a equipe já conhece a tecnologia, mas quer focar no código, não na infraestrutura.

**2.** Você implementou um pipeline Dataflow que processa dados de sensores IoT em tempo real. Alguns registros chegam com formatos inválidos e estão causando falhas no pipeline inteiro. Qual é a melhor estratégia de resiliência?
A) Configurar retry infinito até o dado ser aceito
B) Implementar Dead Letter Queue (DLQ) usando Side Outputs
C) Descartar silenciosamente os registros inválidos
D) Parar o pipeline e corrigir os dados na origem

**Resposta: B**
**Explicação:** ✅ **Dead Letter Queue (DLQ)** via Side Outputs permite isolar dados problemáticos em um local separado (como um bucket) sem interromper o fluxo principal.

**3.** Sua arquitetura de Data Lake está evoluindo e novos campos estão sendo adicionados aos eventos de telemetria. Você precisa que o BigQuery aceite esses novos campos automaticamente sem quebrar as cargas. Qual configuração deve usar?
A) Schema Enforcement com validação estrita
B) Schema Evolution com ALLOW_FIELD_ADDITION
C) Schema Enforcement com bloqueio de novos campos
D) Desabilitar completamente a validação de schema

**Resposta: B**
**Explicação:** ✅ **Schema Evolution** permite que a tabela se adapte a novos dados. O `ALLOW_FIELD_ADDITION` é a configuração específica que permite ao BigQuery "aprender" as novas colunas conforme elas chegam.

**4.** Um pipeline Dataflow precisa processar tanto dados históricos (arquivos Parquet no GCS) quanto dados em tempo real (Pub/Sub). Como o Apache Beam lida com esses dois tipos de datasets?
A) Requer dois pipelines separados, um para cada tipo
B) Usa o mesmo código (modelo unificado) para Bounded e Unbounded
C) Só pode processar Unbounded (streaming) por vez
D) Exige conversão de Parquet para streaming antes

**Resposta: B**
**Explicação:** ✅ O mantra do **Apache Beam** é a unificação: o mesmo código processa dados finitos (Bounded/Batch) e infinitos (Unbounded/Streaming).

**5.** Um analista de negócios sem conhecimento de programação precisa criar um pipeline para conectar ao Oracle on-premises, mascarar CPFs e carregar no BigQuery. Qual ferramenta é mais adequada?
A) Dataflow SDK em Python
B) Cloud Data Fusion com nó Wrangler
C) Dataproc Serverless com PySpark
D) BigQuery Data Transfer Service

**Resposta: B**
**Explicação:** ✅ **Cloud Data Fusion** é a ferramenta visual (No-code) do GCP. O nó **Wrangler** permite limpar e transformar dados arrastando e soltando componentes, ideal para quem não programa.

---

**6.** Você está desenvolvendo um pipeline Dataflow customizado em Java para garantir exactly-once semantics e deduplicação complexa usando CombinePerKey. Qual tipo de template você deve criar para padronizar esse pipeline na empresa?
A) Classic Template
B) Flex Template (Docker)
C) Dataflow SQL Template
D) BigQuery Scheduled Query

**Resposta: B**
**Explicação:** ✅ **Flex Templates** utilizam containers Docker, o que permite gerenciar dependências complexas e garantir que o pipeline rode da mesma forma em qualquer ambiente.

**7.** Durante uma auditoria de custos, você identificou que um pipeline streaming Dataflow está rodando 24/7, mas o valor de negócio dos dados expira em 2 horas. Qual mudança reduziria custos drasticamente?
A) Migrar para processamento Batch executado a cada 2 horas
B) Aumentar o número de workers para processar mais rápido
C) Mudar de Dataflow para Dataproc tradicional
D) Manter streaming mas reduzir a RAM dos workers

**Resposta: A**
**Explicação:** ✅ Se o dado não precisa ser imediato (latência de 2h é aceitável), o **Batch** é muito mais barato, pois você só paga pelo tempo que as máquinas levam para processar e elas desligam logo em seguida.

**8.** Sua tabela BigQuery usa BigLake (Iceberg) sobre arquivos Parquet no GCS. A origem começou a enviar um novo campo 'user_segment'. O que acontece com a tabela BigLake?
A) A carga falha por Schema Mismatch
B) O campo é adicionado automaticamente via Schema Evolution
C) Os registros com novo campo são descartados
D) É necessário recriar toda a tabela manualmente

**Resposta: B**
**Explicação:** ✅ O **BigLake** com suporte a **Iceberg** permite que o esquema da tabela evolua nativamente no armazenamento (GCS) sem necessidade de comandos manuais de `ALTER TABLE`.

**9.** Um job Dataflow está falhando com erro 'Permission Denied' ao tentar escrever no BigQuery. Qual é a causa mais provável?
A) Falta de quota de BigQuery
B) Service Account do Dataflow sem papel 'BigQuery Data Editor'
C) BigQuery está em região diferente do Dataflow
D) O dataset não existe

**Resposta: B**
**Explicação:** ✅ Identidade é tudo no GCP. Se a **Service Account** (a "identidade" do serviço) não tem a permissão necessária para editar dados no BigQuery, o acesso será negado.

**10.** Você precisa orquestrar um fluxo: extrair do Oracle → processar no Dataflow → esperar conclusão → executar query no BigQuery → enviar e-mail. Qual ferramenta usar?
A) Cloud Workflows (YAML)
B) Cloud Composer (Apache Airflow)
C) Cloud Scheduler apenas
D) Cloud Functions encadeadas

**Resposta: B**
**Explicação:** ✅ O **Cloud Composer** (baseado em Airflow) é o "maestro" ideal para fluxos de dados complexos que envolvem múltiplas tecnologias e dependências.

---

**11.** Um arquivo de configuração é enviado para o GCS e deve disparar imediatamente uma análise via Vertex AI. O fluxo deve ser de baixíssimo custo quando ocioso. Qual orquestrador usar?
A) Cloud Composer
B) Cloud Workflows (pay-per-use, event-driven)
C) Dataflow streaming contínuo
D) Dataproc Serverless agendado

**Resposta: B**
**Explicação:** ✅ O **Cloud Workflows** é 100% serverless e você só paga pelas execuções. Se ninguém subir arquivos, o custo é **zero**, ao contrário do Composer que tem custo fixo de cluster.

**12.** Durante troubleshooting, você precisa investigar por que um worker do Dataflow falhou ao ler um arquivo no GCS. Onde você deve buscar essa informação?
A) Cloud Monitoring (métricas de CPU)
B) Cloud Logging (logs estruturados do job)
C) BigQuery audit logs
D) VPC Flow Logs

**Resposta: B**
**Explicação:** ✅ Se você quer saber o "porquê" (o erro textual), vá ao **Cloud Logging**. Se quer saber o "quanto" (números), vá ao Monitoring.

**13.** Você configurou alertas no Cloud Monitoring para notificar quando o 'System Lag' do Dataflow ultrapassar 5 minutos. O que esse alerta monitora?
A) Tempo que os workers levam para inicializar
B) Tempo de atraso entre o evento ocorrer e ser processado
C) Latência de rede entre GCS e BigQuery
D) Tempo de execução total do pipeline

**Resposta: B**
**Explicação:** ✅ **System Lag** é o termômetro do streaming. Ele diz o quão "atrasado" o seu pipeline está em relação ao tempo real.

**14.** Um desenvolvedor precisa depurar erros de código em pipelines, mas não deve ver quem acessou tabelas sensíveis. Qual papel IAM conceder?
A) Project Viewer
B) Logging Viewer
C) Private Logs Viewer
D) Logging Admin

**Resposta: B**
**Explicação:** ✅ O **Logging Viewer** permite ver logs de sistema (erros), mas oculta os logs de auditoria (quem acessou o quê), respeitando a privacidade.

**15.** Logs de acesso a dados (Data Access Audit Logs) estão desabilitados por padrão no BigQuery. Por quê?
A) São irrelevantes para auditoria
B) Causam aumento de custos de armazenamento de logs
C) Diminuem a performance do BigQuery
D) Apenas o Google pode visualizá-los

**Resposta: B**
**Explicação:** ✅ Como o BigQuery processa bilhões de linhas, registrar cada acesso individual geraria um volume de logs gigantesco e **caro**. Por isso, você deve habilitar apenas se for necessário.

---

**16.** Seu pipeline Dataflow em uma sub-rede privada não consegue ler arquivos do Cloud Storage. Qual configuração de rede está faltando?
A) Configurar NAT Gateway
B) Habilitar Private Google Access na sub-rede
C) Abrir firewall para porta 443
D) Criar VPN para GCS

**Resposta: B**
**Explicação:** ✅ O **Private Google Access** permite que recursos sem IP público (em redes privadas) conversem com as APIs do Google (como o Storage) através da rede interna.

**17.** Onde armazenar embeddings vetoriais para buscas de similaridade de baixa latência em um sistema de recomendação?
A) BigQuery (tabela normal)
B) Cloud Storage (arquivos Parquet)
C) Vertex AI Vector Search
D) Bigtable (chave-valor)

**Resposta: C**
**Explicação:** ✅ O **Vertex AI Vector Search** é um banco de dados especializado em vetores, capaz de comparar milhões de embeddings em milissegundos.

**18.** No Dataproc Serverless, por que o "Staging Bucket" é obrigatório?
A) Para backup automático
B) Para armazenar dados de shuffle e intermediários do Spark
C) Para criptografia em trânsito
D) Para aumentar a velocidade de leitura

**Resposta: B**
**Explicação:** ✅ Como não há um disco fixo (já que é serverless), o Spark usa o bucket de staging para "trocar figurinhas" (shuffle) entre os processos distribuídos.

**19.** Qual a diferença de sintaxe entre Dataflow e Dataproc para streaming?
A) Dataflow usa Beam (unificado); Dataproc exige Spark Structured Streaming (muda a lógica)
B) Dataflow só faz batch
C) Dataproc Serverless não suporta streaming
D) Ambos usam a mesma sintaxe

**Resposta: A**
**Explicação:** ✅ No **Beam**, o código é o mesmo. No **Spark**, você precisa usar APIs diferentes (RDD/Dataframe vs Structured Streaming) dependendo do modo.

**20.** Qual a principal vantagem do Dataproc Serverless sobre o tradicional no tempo de inicialização?
A) Mesmo tempo
B) Inicialização em segundos vs minutos
C) O tradicional é mais rápido
D) O serverless leva horas

**Resposta: B**
**Explicação:** ✅ O Serverless é muito mais ágil para começar a rodar o código, pois o Google já mantém "máquinas pré-aquecidas" para esses jobs.

---

**21.** O Cloud Data Fusion traduz o fluxo visual em qual tecnologia no backend?
A) Apache Beam
B) Apache Spark (executado no Dataproc)
C) SQL Nativo
D) Cloud Functions

**Resposta: B**
**Explicação:** ✅ Por trás da interface colorida do Data Fusion, o que roda de verdade é um job **Spark** dentro de um cluster Dataproc.

**22.** Qual o principal "ponto fraco" (trade-off) do Cloud Data Fusion para tarefas pequenas?
A) Poucos conectores
B) Cold start alto (15-20 minutos para subir o cluster)
C) Não processa dados estruturados
D) É gratuito

**Resposta: B**
**Explicação:** ✅ Se você tem um job de 1 minuto, esperar 15 minutos para o cluster subir é ineficiente. Use Data Fusion para processos longos e pesados.

**23.** Como proteger dados sensíveis que caíram em uma Dead Letter Queue (DLQ)?
A) Deixar o bucket público
B) Aplicar IAM restritivo e criptografia CMEK
C) Desabilitar o versionamento
D) Não precisa proteger

**Resposta: B**
**Explicação:** ✅ Dados que falharam (DLQ) costumam ser os mais sensíveis (erros de digitação de CPF, por exemplo). O bucket deve ser um cofre.

**24.** O que indica um crescimento rápido de mensagens em uma DLQ?
A) O pipeline está rápido demais
B) Possível Schema Drift (mudança de formato na origem)
C) O custo do storage caiu
D) Workers ociosos

**Resposta: B**
**Explicação:** ✅ Se o banco de origem mudou o formato da data e o seu pipeline não sabe ler, todos os dados serão rejeitados e enviados para a **DLQ**.

**25.** Qual linguagem oferece melhor performance para pipelines de escala Petabyte no Dataflow?
A) Python
B) Java (performance bruta na JVM)
C) SQL
D) Go

**Resposta: B**
**Explicação:** ✅ Embora Python seja mais popular, o **Java** ainda reina em performance extrema devido às otimizações da máquina virtual (JVM) e maturidade do SDK.

---

**26.** O Cloud Composer é baseado em qual tecnologia open-source?
A) Apache Kafka
B) Apache Airflow
C) Apache NiFi
D) Apache Oozie

**Resposta: B**
**Explicação:** ✅ Ele é o **Airflow** gerenciado pelo Google. Se você sabe Airflow, sabe Composer.

**27.** Em qual linguagem se escreve no Cloud Workflows?
A) Python
B) Java
C) YAML ou JSON
D) SQL

**Resposta: C**
**Explicação:** ✅ Diferente do Composer (Python), o **Workflows** é declarativo, focado em chamadas de API usando **YAML**.

**28.** Diferença de custo entre Composer e Workflows?
A) Ambos são iguais
B) Composer tem custo fixo (Cluster GKE); Workflows é pay-per-use
C) Workflows é mais caro
D) Ambos são gratuitos

**Resposta: B**
**Explicação:** ✅ O **Composer** é como um prédio (você paga o condomínio fixo). O **Workflows** é como um hotel (você paga apenas pelas noites que usar).

**29.** Qual log mostra quem fez um `SELECT` em uma tabela de salários?
A) Admin Activity Logs
B) Data Access Audit Logs
C) System Event Logs
D) VPC Flow Logs

**Resposta: B**
**Explicação:** ✅ **Data Access** é o log que "dedura" quem olhou para o dado.

**30.** Erro 'Quota Exceeded' ao criar workers do Dataflow. Solução?
A) Mudar permissão IAM
B) Solicitar aumento de quota de CPUs no console
C) Mudar a região
D) Desligar o firewall

**Resposta: B**
**Explicação:** ✅ Você atingiu o limite de "crédito" de máquinas que o Google te dá. Basta pedir mais no menu de Quotas.

---

**31.** Para economizar custos, quais logs devem ser filtrados em produção?
A) Todos
B) Filtrar DEBUG; manter INFO ou superior
C) Manter apenas DEBUG
D) Logar tudo

**Resposta: B**
**Explicação:** ✅ Logs de **DEBUG** são detalhados demais e caros. Em produção, você só quer saber se algo importante aconteceu (INFO) ou se deu erro (ERROR).

**32.** Como reter logs por 7 anos (compliance) se o Cloud Logging apaga antes?
A) Não é possível
B) Criar um Log Sink para Cloud Storage (Coldline) ou BigQuery
C) Tirar print dos logs
D) Exportar manualmente todo mês

**Resposta: B**
**Explicação:** ✅ O **Log Sink** é um "cano" que desvia os logs para um lugar onde eles podem ficar guardados para sempre por um custo muito baixo.

**33.** Qual métrica mede a taxa de escrita no BigQuery?
A) CPU Utilization
B) Storage Bytes
C) Uploaded Bytes
D) Query Duration

**Resposta: C**
**Explicação:** ✅ **Uploaded Bytes** te diz quantos dados por segundo estão entrando no seu Warehouse.

**34.** Vantagem do Structured Logging (JSON)?
A) Ocupa menos espaço
B) Permite busca e filtragem por campos específicos (ex: `jsonPayload.user_id`)
C) É mais bonito de ler
D) Não tem vantagem

**Resposta: B**
**Explicação:** ✅ Com JSON, o log vira uma pequena "tabela" onde você pode pesquisar valores específicos sem precisar ler o texto todo.

**35.** Impacto de processar dados em 'us-east1' rodando o job em 'europe-west1'?
A) Nenhum
B) Custo de Egress (saída de dados entre regiões) e latência
C) O Google bloqueia
D) Fica mais barato

**Resposta: B**
**Explicação:** ✅ Cruzar o oceano com dados custa caro e demora mais. **Sempre processe onde o dado está.**

---

**36.** No "Nível 3" de controle (Maturidade), quando usamos Dataflow?
A) Para ser rápido
B) Para garantir que o dado não entre "sujo" no storage (Controle Proativo)
C) Quando não sabemos programar
D) Para análises depois que o dado já entrou

**Resposta: B**
**Explicação:** ✅ O Nível 3 é o **Controle Proativo**. Você limpa o dado antes dele pisar no seu Data Lake.

**37.** BQML vs Vertex AI?
A) BQML é para modelos simples via SQL; Vertex é para modelos avançados e MLOps
B) BQML é melhor
C) Vertex só faz imagens
D) São iguais

**Resposta: A**
**Explicação:** ✅ Se você sabe SQL, já sabe fazer ML no BigQuery. Se precisa de redes neurais complexas, vá para o Vertex AI.

**38.** Onde criar um dashboard que mistura latência do Pub/Sub e Lag do Dataflow?
A) Cloud Logging
B) Cloud Monitoring (Dashboards Customizados)
C) BigQuery
D) Excel

**Resposta: B**
**Explicação:** ✅ O **Monitoring** é a central de comando para olhar a saúde de vários serviços ao mesmo tempo.

**39.** Como criar uma métrica que não existe (ex: contar erros específicos no log)?
A) Não dá
B) Criar uma Log-based Metric
C) Ligar para o suporte do Google
D) Usar o BigQuery

**Resposta: B**
**Explicação:** ✅ As **Log-based Metrics** transformam palavras nos logs em números e gráficos.

**40.** Papel para um Auditor que só pode investigar acessos passados:
A) BigQuery Data Editor
B) BigQuery Data Viewer + Private Logs Viewer
C) Project Owner
D) Logging Admin

**Resposta: B**
**Explicação:** ✅ Ele pode ver o dado (Viewer) e ver quem mais viu o dado (Private Logs), mas não pode estragar nada.

---

**41.** Foco principal do Dataproc Serverless?
A) Streaming 24/7
B) Batch massivo e esporádico
C) Imagens
D) Chatbots

**Resposta: B**
**Explicação:** ✅ Perfeito para aquele processamento pesado que roda uma vez por dia ou por mês.

**42.** O que acontece se um Uptime Check falha por 5 minutos?
A) Nada
B) Dispara alertas (E-mail, SMS, Webhook)
C) O pipeline explode
D) O Google deleta a conta

**Resposta: B**
**Explicação:** ✅ Ele é o seu "vigia". Se o serviço cair, ele te avisa imediatamente.

**43.** Qual formato de arquivo é binário e já vem com o "manual de instruções" (schema)?
A) CSV
B) Avro
C) TXT
D) JSON

**Resposta: B**
**Explicação:** ✅ O **Avro** é o favorito dos engenheiros porque é rápido e evita erros de interpretação de colunas.

**44.** Recurso para validar tudo antes de um job caro do Dataflow?
A) Run
B) Validate
C) Metrics
D) Logs

**Resposta: B**
**Explicação:** ✅ O **Validate** é o seu seguro contra erros bobos que podem custar caro se o job começar errado.

**45.** Diferença de uso: Composer vs Workflows?
A) Composer para dados complexos (Airflow); Workflows para microserviços e eventos leves
B) Sempre Composer
C) Sempre Workflows
D) São iguais

**Resposta: A**
**Explicação:** ✅ Use a ferramenta certa: trator (Composer) para terra pesada, bicicleta (Workflows) para entregas rápidas.

---

**46.** Diferencial do Data Fusion para o Dataflow?
A) É visual (No-code) e focado em conectores Enterprise (SAP, Oracle)
B) É mais rápido
C) É mais barato
D) Não tem diferença

**Resposta: A**
**Explicação:** ✅ O **Data Fusion** é para quem quer clicar e arrastar, especialmente integrando sistemas corporativos complexos.

**47.** Por que manter processamento e storage na mesma região?
A) Regra do Google
B) Evitar custo de Egress e reduzir latência
C) Fica mais bonito
D) Não faz diferença

**Resposta: B**
**Explicação:** ✅ Dinheiro e Velocidade. Mover dados entre regiões custa caro e leva tempo.

**48.** Onde ocorre o nó de 'Masking' no Data Fusion?
A) Depois do BigQuery
B) Durante a ingestão (antes de gravar)
C) Na tela do usuário
D) No Excel

**Resposta: B**
**Explicação:** ✅ Se você masca o dado na entrada, ele já nasce protegido dentro do seu ambiente.

**49.** O que garante que nenhum dado seja processado em dobro no Dataflow?
A) At-least-once
B) Exactly-once semantics
C) At-most-once
D) Sorte

**Resposta: B**
**Explicação:** ✅ O **Exactly-once** é a promessa do Dataflow: nem um dado a mais, nem um dado a menos.

**50.** Quer migrar Spark mas não quer gerenciar máquinas. Qual a escolha?
A) Reescrever tudo
B) Dataproc Serverless
C) Data Fusion
D) BigQuery

**Resposta: B**
**Explicação:** ✅ É o caminho mais curto (Lift-and-Shift) para levar seus scripts Spark para a nuvem com zero dor de cabeça de infraestrutura.

---

### 🗝️ Gabarito Rápido

| Q | R | Q | R | Q | R | Q | R | Q | R |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **1** | C | **11** | B | **21** | B | **31** | B | **41** | B |
| **2** | B | **12** | B | **22** | B | **32** | B | **42** | B |
| **3** | B | **13** | B | **23** | B | **33** | C | **43** | B |
| **4** | B | **14** | B | **24** | B | **34** | B | **44** | B |
| **5** | B | **15** | B | **25** | B | **35** | B | **45** | A |
| **6** | B | **16** | B | **26** | B | **36** | B | **46** | A |
| **7** | A | **17** | C | **27** | C | **37** | A | **47** | B |
| **8** | B | **18** | B | **28** | B | **38** | B | **48** | B |
| **9** | B | **19** | A | **29** | B | **39** | B | **49** | B |
| **10** | B | **20** | B | **30** | B | **40** | B | **50** | B |

---

**Gostaria que eu gerasse agora um resumo visual ou um mapa mental focando nas diferenças entre Composer e Workflows para o seu material de apoio?**