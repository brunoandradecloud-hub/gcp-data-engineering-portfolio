# 📑 Guia de Estudos — Módulo 02

## ⚡ Serverless for Apache Spark (Dataproc Serverless)

- Conceito de **Serverless for Apache Spark** e como ele elimina a necessidade de gerenciar clusters.
- Diferença entre **Dataproc tradicional (cluster gerenciado)** e **Dataproc Serverless (batch jobs)**.
- Execução de **jobs batch Spark** usando templates prontos.
- Uso de **Cloud Storage** como staging e origem de dados.
- Leitura de arquivos **Avro** no Spark.
- Integração nativa entre **Spark e BigQuery** usando o connector spark-bigquery.
- Criação automática de **datasets e tabelas no BigQuery** via job Spark.
- Uso de **variáveis de ambiente** para configurar projetos, regiões e dependências.
- Importância de **buckets temporários** para carga de dados no BigQuery.
- Monitoramento da execução via logs e status do job.
- Validação dos dados carregados usando **queries SQL no BigQuery**.

## ⚡ Dataflow — Batch Pipeline (Job Builder UI)

- Introdução ao **Dataflow como serviço serverless de processamento de dados**.
- Diferença entre **Dataflow (Beam)** e **Spark (Dataproc Serverless)**.
- Criação de pipelines **batch sem código** usando a **Job Builder UI**.
- Leitura de dados CSV diretamente do **Cloud Storage**.
- Uso de transformações prontas, como **Filter (Python)**.
- Aplicação de regras simples de negócio (ex: filtrar registros com status = Approved).
- Escrita dos resultados em **BigQuery**, com criação automática de datasets e tabelas.
- Configuração correta de **staging bucket** para execução do Dataflow.
- Importância da **região do job** e alinhamento com recursos do projeto.
- Uso do botão **Validate** para evitar erros antes da criação do job.
- Monitoramento visual do pipeline (Graph View e Logs).
- Gerenciamento do ciclo de vida do job (executar, cancelar, excluir).
- Entendimento de erros comuns relacionados a **região e constraints do projeto**.

## 🧠 Aprendizados-chave do dia

- Serverless reduz drasticamente a complexidade operacional.
- Spark e Dataflow resolvem problemas semelhantes, mas com abordagens diferentes.
- BigQuery é o destino central para analytics no GCP.
- Configuração de região é crítica em serviços gerenciados.
- UI-first (Dataflow Job Builder) acelera onboarding e aprendizado.
