# 📑 Guia de Estudos — Versão FULL (Streaming + Mensageria + BigQuery + AI + Bigtable)

---

## 📨 1) Cloud Pub/Sub (Topics, Subscriptions, Ack/Retention, Fan-out, Push/Pull)

**O que é**

* Serviço de mensageria **serverless** para ingestão e distribuição de eventos.
* Conceitos centrais: **Topic** (publicação), **Subscription** (consumo), **fan-out** (1 topic → N subscribers).

**Quando usar**

* Quando você quer **desacoplamento** entre produtores e consumidores.
* Quando precisa de **escala automática** e baixa operação.
* Quando o padrão é “event-driven” (IoT, logs, eventos de apps).

**Como se encaixa na arquitetura**

* Camada de **Event Ingestion / Event Bus**: fontes → Pub/Sub → (processamento: Dataflow/Run) → DW/Lake (BigQuery/BigLake/GCS) + ações operacionais.

**Exemplos práticos**

* Ambulâncias publicam telemetria (`ambulance_id`, `event_timestamp`) em `ambulance-telemetry-topic`.
* Sensores de rios publicam em `river-level-topic`.
* Fan-out:

  * Subscription A → Dataflow (curadoria/analytics)
  * Subscription B → Cloud Run (alertas)
  * Subscription C → auditoria/raw

**Ponto de prova / armadilha comum**

* **Delivery** é tipicamente **at-least-once** → duplicatas podem acontecer → dedup/idempotência é obrigatório no design.
* **Ordering** não é global; só “por chave” quando usar **ordering keys**.
* **Push vs Pull (escala):**

  * **Push** (Pub/Sub → endpoint): ótimo para eventos esporádicos e simplicidade.
  * **Pull** (consumidor puxa): melhor para **alta vazão** e controle de ritmo/backpressure (ex.: Dataflow).
* Pegadinha: “Pull garante exactly-once” → **não automaticamente**; exactly-once efetivo depende de **pipeline + dedup/idempotência + sink**.

---

## 🔥 2) Pub/Sub vs Kafka (critério de escolha)

**O que é**

* **Pub/Sub**: mensageria serverless (elasticidade, fan-out, baixa operação).
* **Kafka**: **distributed log** com partitions, offsets, consumer groups, retenção e replay como núcleo.

**Quando usar**

* **Pub/Sub**: cloud-native, time-to-market, pouca operação, integração direta com Dataflow/BigQuery.
* **Kafka**: ecossistema Kafka-first, replay operacional frequente, necessidade de controle fino de partitions e compatibilidade multi-plataforma.

**Como se encaixa na arquitetura**

* Ambos são “front door/backbone” de eventos.
* A diferença é onde você quer colocar o peso:

  * Pub/Sub → mais “managed/serverless”
  * Kafka → mais “controle/operabilidade do log”

**Exemplos práticos**

* Pub/Sub em GCP: IoT → Pub/Sub → Dataflow → BigQuery.
* Kafka corporativo: topic central, vários consumer groups (fraude, billing, analytics), replay via offsets.

**Ponto de prova / armadilha comum**

* Não reduzir a escolha a “domínio de infra”. A prova cobra **trade-off**: operação, replay, ordering, escala, custo/previsibilidade.
* “Preciso de ordering” → nenhum garante ordering global; Kafka garante **por partition**, Pub/Sub só por **ordering key**.

---

## 🧱 3) Apache Kafka (fundamentos: partitions, consumer groups, offsets, replay, infra)

**O que é**

* Plataforma de streaming baseada em **log append-only** distribuído.
* Elementos: **topics**, **partitions**, **consumer groups**, **offsets**, **retention**.

**Quando usar**

* Quando replay via offsets é parte do “modo normal” de operar (reprocessamento frequente).
* Quando você precisa de controle explícito de **partitions** para escala e ordering por entidade.
* Quando o ecossistema (Connect/Schema Registry/Streams) é um requisito do ambiente.

**Como se encaixa na arquitetura**

* Kafka atua como **backbone**: produtores → Kafka → múltiplos consumidores (pipelines, microservices, analytics).
* O “processamento de stream” pode ser Kafka Streams / Flink / Spark / Dataflow (dependendo do stack).

**Exemplos práticos**

* `telemetry-topic` particionado por `ambulance_id`: garante ordem por ambulância (dentro da mesma partition).
* Consumer group “analytics” lê e grava em DW; consumer group “alerts” lê e dispara incidentes.

**Ponto de prova / armadilha comum**

* **Partitions não são faixas de ID.** O roteamento típico é `hash(key) % N`.
* **Crescimento de IDs não realoca histórico**: novos IDs só caem em alguma partition existente.
* **1 consumer por partition** vale **dentro do mesmo consumer group** (vários consumer groups podem ler o mesmo topic).
* Tuning/infra existe (mesmo managed): sizing, throughput, partitions, retention, rebalances, etc.

---

## 🧩 4) Kafka Partitions (paralelismo/ordering e “o que acontece quando eu aumento partitions”)

**O que é**

* **Partition** = unidade de **ordering** e **paralelismo** no Kafka.
* Ordering é garantido **dentro da partition** (não no topic inteiro).

**Quando usar**

* Quando você precisa de ordering por chave (`device_id`, `ambulance_id`, `sensor_id`).
* Quando você quer escalar consumo com consumer group (paralelismo limitado pelo número de partitions).

**Como se encaixa na arquitetura**

* Partitions definem:

  * teto de paralelismo por consumer group
  * distribuição de storage/IO entre brokers
  * “onde” a ordenação é mantida

**Exemplos práticos**

* 10 partitions e key=`ambulance_id`:

  * 10 consumers no mesmo group → até 10 partitions consumidas em paralelo
  * 20 consumers no mesmo group → 10 ativos e 10 ociosos
* Crescer de 100 para 200 IDs **não** muda “faixas”; continua hash → partition.

**Ponto de prova / armadilha comum**

* **Aumentar partitions** muda o mapeamento `hash % N`: a mesma key pode cair em outra partition no futuro → pode afetar expectativas de ordering “histórico”.
* Hot partitions: chaves “famosas” concentram carga (soluções incluem chave composta/salting, dependendo do caso).
* Partitions demais aumentam overhead (metadata, rebalances, arquivos/segmentos).

---

## 🧠 5) Dataflow Streaming (por que aparece no meio do Pub/Sub/Kafka)

**O que é**

* Serviço serverless de processamento (Apache Beam) para **unbounded data**.
* Recursos centrais: **event time**, **windowing**, **triggers**, **state/timers**, **late data**, DLQ/side outputs.

**Quando usar**

* Quando a prova mencionar: **event time**, janelas, dados tardios, dedup, enriquecimento, pipelines robustos.
* Quando você quer stream processing cloud-native com baixa operação.

**Como se encaixa na arquitetura**

* Camada de **Stream Processing**:

  * Pub/Sub/Kafka → Dataflow → BigQuery/BigLake/GCS (+ alertas e outros sinks)

**Exemplos práticos**

* Telemetria → Dataflow valida schema, normaliza timestamps, dedup por `event_id`, grava em BigQuery curated.
* Sensor de rio → janela por event time para médias e alerta por threshold.

**Ponto de prova / armadilha comum**

* Pub/Sub/Kafka entregam eventos; **não resolvem** event-time correctness.
* Exactly-once end-to-end depende do desenho: geralmente você mira **at-least-once + idempotência/dedup**.

---

## 🏢 6) BigQuery — APIs/Paths de ingestão (por que isso importa)

**O que é**

* BigQuery é DW serverless, mas tem múltiplas formas de ingestão com trade-offs de **latência, custo, throughput e garantias**.
* Principais “portas” que você precisa reconhecer na prova:

  * **Load Jobs (batch)**
  * **Storage Write API (streaming moderno)**
  * **Legacy Streaming (`tabledata.insertAll`)**
  * **Direct ingestion (Pub/Sub → BigQuery)**

**Quando usar**

* **Batch Load**: quando latência pode ser minutos/horas e você quer custo/eficiência.
* **Storage Write API**: quando precisa de near real-time com alto throughput (frequente via Dataflow).
* **Legacy streaming**: legados/compatibilidade (evitar para greenfield).
* **Direct ingestion**: menor custo/menor operação quando dado já vem limpo e você não precisa de transformação no caminho.

**Como se encaixa na arquitetura**

* Camada de **Analytics/Serving**:

  * pipelines (Dataflow/Connect/apps) escrevem no BigQuery via uma dessas APIs/paths.
* A escolha do path impacta custo e comportamento operacional.

**Exemplos práticos**

* IoT em tempo real: Dataflow → BigQuery usando **Storage Write API**.
* Batch lakehouse: GCS (Parquet) → BigQuery via **Load Job**.
* Evento limpo: Pub/Sub → BigQuery via **Direct ingestion**.

**Ponto de prova / armadilha comum**

* “Streaming é sempre melhor” → não. Sem requisito de latência, **batch** costuma ser a melhor resposta.
* “Exactly-once” não é propriedade do BigQuery sozinho; depende do path e do desenho (dedup/idempotência).

---

## 🧷 7) Pub/Sub → BigQuery (Direct Path / Direct Ingestion)

**O que é**

* Ingestão **direta** do Pub/Sub para BigQuery **sem Dataflow** no meio.

**Quando usar**

* Quando você quer **menor custo + menor operação + baixa latência** e o payload já está **limpo/estruturado**.
* Quando sua meta é “landing rápido” com mínima lógica.

**Como se encaixa na arquitetura**

* Atalho de ingestão para a camada analítica: Pub/Sub (event bus) → BigQuery (raw landing/staging).

**Exemplos práticos**

* Eventos de auditoria já prontos no schema final indo direto para uma tabela particionada no BigQuery.
* Telemetria “sem enriquecimento” apenas para consulta e auditoria.

**Ponto de prova / armadilha comum**

* Não faz transformação real (em geral, apenas **mapeamento de campos/schema**).
* **Dica de prova**: “streaming no BigQuery com o menor custo e dado já limpo” → **Direct ingestion**, não Dataflow.

---

## 🧷 8) Kafka → BigQuery (faz sentido? sim — e aqui estão os padrões)

**O que é**

* Padrões de ingestão de eventos Kafka para BigQuery, normalmente via:

  1. **Kafka Connect (BigQuery Sink Connector)**
  2. **Dataflow (KafkaIO → BigQuery)**
  3. Aplicação custom (menos comum para alto volume)
* Independente do “meio”, a escrita final no BigQuery usa uma **API de write** (destaque abaixo).

**Quando usar**

* Quando seu backbone é Kafka e você quer analytics/BI no BigQuery.
* Quando precisa reter replay no Kafka, mas servir consultas no DW.

**Como se encaixa na arquitetura**

* Kafka (event backbone) → (Connect/Dataflow/app) → BigQuery (DW).
* Se você precisa de transformação/quality/dedup/event time: preferir **Dataflow** entre Kafka e BigQuery.
* Se você quer “sink direto e simples”: **Kafka Connect** costuma ser suficiente.

**Exemplos práticos**

* **Kafka Connect**: `telemetry-topic` → BigQuery dataset `raw_telemetry` (baixa lógica, rápida implementação).
* **Kafka → Dataflow**: aplica validação, normalização, dedup por `event_id`, enriquece com lookup e grava em `curated_telemetry`.

**Ponto de prova / armadilha comum**

* **Dado duplicado**: Kafka e muitos sinks são at-least-once → o pipeline precisa de idempotência/dedup no BigQuery (MERGE/upsert, chave natural, etc.).
* **BigQuery Write APIs (destacar na resposta):**

  * **Storage Write API**: caminho moderno e preferível para streaming/alto throughput (muito comum quando Dataflow está no meio).
  * **Legacy `insertAll`**: legado/compatibilidade (evitar em greenfield).
  * **Load Jobs**: se você “bufferiza” em arquivos (menos comum em Kafka direto, mas possível em designs batch).
* Pegadinha: “Kafka tem replay então dispensa lake” → não necessariamente; o lake ainda é fonte para batch/curadoria e custo.

---

## ♾️ 9) BigQuery Continuous Queries

**O que é**

* SQL que roda continuamente para processar dados conforme chegam, materializando tabelas derivadas e mantendo resultados near real-time.

**Quando usar**

* Quando a transformação é majoritariamente **SQL** e você quer reduzir o “peso” de pipeline.
* Para agregações contínuas e roteamento simples no DW.

**Como se encaixa na arquitetura**

* BigQuery raw/landing → Continuous Query → BigQuery curated/aggregated (ELT near real-time).

**Exemplos práticos**

* Atualizar continuamente uma tabela “incidentes por região” a partir de eventos de sensores.
* Materializar métricas por ambulância em janelas simples (quando adequado).

**Ponto de prova / armadilha comum**

* Não substituir Dataflow quando você precisa de streaming avançado (state/late data/janelas complexas).
* Projetar downstream para tolerar reprocessamento/duplicidade se o design exigir.

---

## 🤖 10) ADK (Agent Development Kit) + BigQuery

**O que é**

* Framework para criar **agents** que usam ferramentas (executar SQL, inspecionar metadados) para automação e experiência “conversacional” em cima de dados.

**Quando usar**

* Para camada “assistiva/operacional”: gerar SQL, investigar anomalias, automatizar runbooks e responder perguntas com governança.

**Como se encaixa na arquitetura**

* Camada acima do DW: o agent consulta BigQuery (e pode acionar ações), mas não substitui pipeline.

**Exemplos práticos**

* Agent que detecta queda de ingestão (via query) e alerta com contexto.
* Agent que monta queries e explica drift/anomalias para times de operação.

**Ponto de prova / armadilha comum**

* Segurança: aplicar **least privilege** (service account, allowlist de datasets/ações) para não virar “DBA automático”.

---

## 🧊 11) Bigtable Data Boost (Isolamento de workload)

**O que é**

* Mecanismo de leitura analítica com **compute serverless separado**, preservando o cluster principal para serving/escritas.

**Quando usar**

* Quando você tem Bigtable com tráfego transacional crítico e precisa rodar leituras pesadas/analytics sem degradar SLA.

**Como se encaixa na arquitetura**

* Bigtable serving (telemetria/escritas) + Data Boost para leituras analíticas (jobs/ML/scans) sem “roubar” capacidade do cluster.

**Exemplos práticos**

* Job pesado lendo Bigtable para features/ML sem afetar o app das ambulâncias (escrita contínua).
* Scans para auditoria/analytics com isolamento.

**Ponto de prova / armadilha comum**

* Data Boost é “salvador” do analytics porque **isola workload**; não substitui modelagem de chave/row design e não resolve write path.
