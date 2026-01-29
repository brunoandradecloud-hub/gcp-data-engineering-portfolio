Entendido, Engenheiro. Aqui está o conteúdo integral do arquivo `module_02_bullet_points_final.md`, sem qualquer alteração ou resumo no texto, aplicando rigorosamente a formatação de Markdown (`.md`) com a hierarquia de `#` e os espaçamentos solicitados.

---

# 📑 Guia de Estudos — Módulo 02 (Enriquecimento Técnico)

## 🏗️ 1. Níveis de Controle (Maturidade Arquitetural)

**O que é**
A estratégia de decisão que define o equilíbrio entre esforço de desenvolvimento e rigor técnico na qualidade dos dados. É o "termômetro" para escolher a ferramenta certa para o problema certo.

**Quando usar**

* **Nível 1 (Agilidade Máxima):** Use Cloud Data Fusion para ingestões rápidas de fontes legadas (SAP, SQL Server) ou quando o time não domina código. É o foco em produtividade visual.
* **Nível 2 (Correção Reativa):** Use BigQuery (SQL) para limpar dados que já foram persistidos. Ideal para ajustes de negócio via MERGE ou UPDATE agendados.
* **Nível 3 (Controle Proativo):** Use Dataflow (Beam SDK) ou Dataproc (Spark). Obrigatório quando o dado não pode entrar "sujo" no storage sob nenhuma hipótese.

**Onde se encaixa na arquitetura**
Na transição crítica entre a camada Raw (bruta) e a Processed/Curated (limpa).

**Exemplo prático**
Um sensor envia um campo de temperatura como "25C" (string) em vez de 25.0 (float).

* **Data Fusion (Nível 1):** Você usa o nó "Wrangler" para limpar visualmente. É rápido, mas se a origem mudar drasticamente, o pipeline quebra silenciosamente.
* **BigQuery (Nível 2):** Você carrega o dado bruto e roda um CAST via SQL. Você paga pelo armazenamento do dado errado e pelo processamento da correção.
* **Dataflow (Nível 3):** O código valida a string na entrada; se estiver fora do padrão, o registro é desviado para a DLQ antes de tocar no BigQuery.

**Ponto de prova / armadilha comum**

* ✅ **Recomendação:** Se a prova foca em "Non-coders" ou "Legacy connectivity", vá de Data Fusion. Se foca em "Exactly-once" e "Complex Logic", vá de Dataflow.
* ✅ **Data Quality:** Ferramentas como Dataplex Data Quality podem monitorar o storage, mas o controle de "Nível 3" no pipeline é a única forma de impedir a poluição do Data Lake.

---

## ⚖️ 2. Fundamentos: Bounded vs Unbounded Dataset

**O que é**
Classificação técnica da natureza do fluxo de dados. Bounded é um conjunto fixo (ex: ficheiro Parquet). Unbounded é um fluxo infinito (ex: mensagens do Pub/Sub).

**Quando usar**
Para escolher o modo de execução e o modelo de janelas.

* **Dataflow:** Trata ambos com o mesmo código (modelo Beam unificado).
* **Dataproc:** Exige o uso de Spark Structured Streaming para Unbounded, o que muda a sintaxe do código.

**Onde se encaixa na arquitetura**
Definição da estratégia de ingestão e custo operacional.

**Exemplo prático**
Consolidar vendas do mês para folha de pagamento (Bounded/Batch) vs. Monitorar transações para detecção de fraude em milissegundos (Unbounded/Streaming).

**Ponto de prova / armadilha comum**

* ✅ **Justificativa de Custo:** Batch é sempre mais barato porque desliga os recursos após o uso. Streaming exige workers ligados 24/7.
* ✅ **Cenário Ideal:** Streaming só faz sentido se o valor do dado para o negócio expira rapidamente (ex: uma oferta personalizada enquanto o cliente está na App).

---

## 🧬 3. Schema Management (Evolution vs Enforcement)

**O que é**

* **Enforcement:** Bloquear o dado se ele fugir do contrato.
* **Evolution:** Adaptar a tabela automaticamente quando o dado ganha novos campos.

**Quando usar**

* **Enforcement:** Na camada Curated/BigQuery, para não quebrar dashboards do Looker.
* **Evolution:** Na camada Processed/Data Lake, para não perder novos atributos de telemetria.

**Onde se encaixa na arquitetura**
Diretamente ligado aos tipos de tabelas e formatos:

* **BigLake & Iceberg Tables:** Suportam Schema Evolution robusto (ex: adicionar colunas ou renomear campos em ficheiros Parquet no GCS sem reescrever a tabela).
* **Native BigQuery:** Suporta adição de colunas via API/Job (ALLOW_FIELD_ADDITION).

**Exemplo prático**
Uma nova versão da App envia o campo user_segment.

* Com **Evolution**, o BigQuery cria a coluna automaticamente durante a carga.
* Com **Enforcement**, o Dataflow (Nível 3) rejeita o registo e envia para a DLQ por "Schema Mismatch", protegendo a integridade da tabela final.

**Ponto de prova / armadilha comum**

* ✅ **BigLake/Iceberg:** É a recomendação para cenários de "Open Data Standards" que precisam evoluir sem lock-in do fornecedor.
* ❌ **Armadilha:** Nenhuma tabela aceita mudar o tipo (ex: de INT para STRING) automaticamente; isso exige sempre reprocessamento.

---

## 🌊 4. Dataflow (Apache Beam, SDK & Templates)

**O que é**
O serviço de processamento mais "Professional" do GCP. Pode ser operado via Console (Templates) ou Desenvolvimento (SDK).

**Quando usar**

* **SDK (Java/Python):** Quando precisa de performance bruta, deduplicação customizada (CombinePerKey) e transformações complexas.
* **Templates (Classic/Flex):** Para padronizar pipelines entre equipas. Flex Templates são superiores pois usam Docker.

**Performance vs Amigabilidade no Console**

* **Dataflow SQL (Console):** Permite rodar pipelines via SQL diretamente na UI. É focado em performance (usa o motor Beam), mas é menos amigável para debug e não suporta lógicas complexas de Side Outputs (DLQ).
* **Console UI (Job Builder):** Muito amigável para filtros simples, mas escala mal em termos de manutenção de código.

**Exemplo prático**
Usar o Flex Template para criar um conversor padrão de "JSON para BigQuery" para toda a empresa, mas desenvolvido via SDK Java para garantir que a deduplicação ocorra antes de gravar no DW.

**Ponto de prova / armadilha comum**

* ✅ **Performance:** Java é geralmente mais performático para pipelines de escala petabyte. Python é o padrão para flexibilidade e Data Science.
* ✅ **Exactly-once:** É o mantra do Dataflow. Se a prova mencionar duplicatas ou perda de dados em falhas, a resposta envolve Dataflow.

---

## ⚡ 5. Dataproc Serverless (Apache Spark)

**O que é**
A evolução do Dataproc que elimina a necessidade de gerenciar clusters. Você fornece o código e o GCP gerencia o provisionamento efêmero dos recursos.

**Quando usar**

* **Workloads Spark Legados:** Quando já tem scripts Spark on-premises e quer movê-los para a nuvem sem reescrever.
* **Processamento Batch Massivo:** Excelente para transformações pesadas de fim de dia (D-1).
* **Data Science Scale:** Pré-processamento Spark distribuído antes do treino de modelos.

**Onde se encaixa na arquitetura**
Camada de processamento Batch de alta performance.

**Exemplo prático**
Reprocessar 5 anos de transações financeiras em arquivos Avro para recalcular o score de crédito. Se o cenário diz "Time already knows Spark", a resposta é Dataproc.

**Ponto de prova / armadilha comum**

* ✅ **Vantagem:** Inicialização muito mais rápida (segundos) que o Dataproc tradicional.
* ❌ **Armadilha:** Focado em Batch. Para streaming contínuo, o Dataflow é a recomendação técnica superior.

---

## 🧱 6. Cloud Data Fusion (Visual ETL)

**O que é**
Interface gráfica No-code/Low-code baseada no CDAP. Traduz o fluxo visual em um Job Spark executado no Dataproc.

**Quando usar**

* **Integração Enterprise:** Conectores nativos para SAP, Oracle, SQL Server e Salesforce.
* **Equipes de ETL Tradicional:** Analistas que não dominam Python/Java.
* **Wrangler:** Limpeza interativa de dados com feedback em tempo real.

**Onde se encaixa na arquitetura**
Camada de Ingestão e Transformação inicial.

**Exemplo prático**
Conectar ao SAP SuccessFactors, ocultar o CPF (PII) via nó "Masking" e salvar no BigQuery sem escrever código.

**Ponto de prova / armadilha comum**

* ✅ **Diferencial:** Catálogo de conectores mais vasto do GCP.
* ❌ **Performance:** O "cold start" é alto (15-20 min). Não use para tarefas pequenas e frequentes.

---

## 🧪 7. Dead Letter Queue (DLQ) & Resiliência

**O que é**
Estratégia de desvio para dados que falham na validação. O registro problemático é isolado com o motivo da falha anexado.

**Quando usar**
Sempre que a continuidade do pipeline for crítica e o dado de origem for imprevisível.

**Onde se encaixa na arquitetura**
Entre a camada de leitura e a camada de escrita final.

**Implementação**

* **Dataflow (SDK):** Usa-se Side Outputs (TaggedOutput).
* **Dataproc (Spark):** Implementado via try-catch ou filtragem de DataFrames.
* **Data Fusion:** Nós possuem uma porta de saída "Error" (ícone vermelho).

**Exemplo prático**
Pipeline espera price numérico, chega "R$ 10,00". O registro é salvo num bucket de erro com a mensagem: "Invalid format: R$".

**Ponto de prova / armadilha comum**

* ✅ **Segurança:** O acesso ao bucket da DLQ deve ser extremamente restrito via IAM (PII sensível).
* ✅ **Monitoramento:** DLQ crescendo rápido deve disparar um Alerta no Cloud Monitoring (Schema Drift).

---

## 🎼 8. Orquestração: Cloud Composer vs. Cloud Workflows

**O que é**

* **Composer:** Baseado em Apache Airflow. Gerencia tarefas complexas via Python (DAGs).
* **Workflows:** Orquestrador Serverless de baixa latência baseado em YAML/HTTP.

**Quando usar**

* **Composer:** Fluxo focado em Dados, dependências complexas, retries automáticos pesados.
* **Workflows:** Fluxo focado em Serviços/APIs, latência sub-segundo, preocupação extrema com custo.

**Onde se encaixa na arquitetura**
Camada de Orquestração (Control Plane).

**Exemplo prático**

* **Composer:** Disparar Dataflow -> Esperar -> Rodar Query BQ -> Enviar E-mail.
* **Workflows:** Arquivo cai no GCS -> Cloud Function -> Workflows chama API Vertex AI.

**Ponto de prova / armadilha comum**

* ❌ **Execução:** Nenhum dos dois processa dados. Eles apenas mandam outros serviços processarem.
* ✅ **Diferencial:** Composer é ideal para o ecossistema Python/Hadoop. Workflows é ideal para microserviços.

---

## 📊 9. Cloud Logging (Centralized Analysis)

**O que é**
Repositório centralizado de todos os eventos gerados pelos serviços do GCP.

**Quando usar**
Root Cause Analysis (RCA), Audit Logs e Log-based Metrics.

**Exemplo prático**
Configurar Dataflow para emitir Structured Logging em JSON para filtrar instantaneamente erros de uma venda específica.

---

## 📈 10. Cloud Monitoring (Proactive Alerting)

**O que é**
Ferramenta de telemetria focada em Métricas (CPU, Memória, Latência).

**Quando usar**
Alertas proativos (SMS/E-mail) e Dashboards de saúde (System Lag).

**Exemplo prático**
Criar alerta baseado no Dataflow System Lag. Se passar de 5 minutos, notificar o time de plantão.

---

## 🔐 11. IAM para Logs (Privacy & Security)

**O que é**
Controle granular de quem pode ler quais logs.

**Ponto de prova / armadilha comum**

* **Logs Viewer:** Acesso básico a logs operacionais e erros.
* **Private Logs Viewer:** Necessário para ver Data Access Audit Logs (quem fez SELECT em quais colunas). Pode conter PII.

---

## 🛠️ 12. Troubleshooting de Rede e Infraestrutura (IAM + VPC)

**O que é**
A habilidade de identificar falhas de "encanamento" que impedem o pipeline de rodar, independente do código estar correto.

**Quando usar**
Quando o Job falha no "Start" ou os Workers não conseguem se comunicar com o BigQuery/GCS.

**Principais Causas de Erro (Checklist PDE)**

1. **Service Account sem permissão:** O Dataflow roda com a conta de serviço do Compute Engine por padrão. Se ela não tiver Storage Object Viewer, o job falha ao ler o arquivo.
2. **Falta de Private Google Access:** Se seus Workers estão em uma sub-rede sem IP externo (Segurança), você precisa ativar o Private Google Access para que eles falhem ao tentar falar com a API do BigQuery.
3. **Cotas de Projeto:** Erros de "Quota Exceeded" no Monitoring indicam que você atingiu o limite de CPUs ou de escrita por segundo no BigQuery.

**Ponto de prova / armadilha comum**

* ✅ **Regionalidade:** Se o seu dado está no bucket em us-east1 e seu job Dataflow roda em europe-west1, você pagará Egress Cost e terá latência. A recomendação é sempre manter processamento e storage na mesma região.

---

**Engenheiro, arquivo processado com sucesso. Tudo pronto para amanhã iniciarmos o Módulo 03 e as questões de simulado?**