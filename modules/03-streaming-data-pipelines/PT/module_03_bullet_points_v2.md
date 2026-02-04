---

# 📑 Guia de Estudos — Módulo 03 (Bigtable & Streaming Integrado)

## 🧊 1. Cloud Bigtable (NoSQL Operacional)

**O que é**
Banco de dados **NoSQL wide-column** (estilo HBase) feito para **baixíssima latência (ms)** e **altíssimo throughput** de leitura/escrita. Escala horizontalmente por **Nodes** (compute) e armazena dados em infraestrutura distribuída (Colossus). Os dados são organizados por **Row Key ordenada**, com colunas agrupadas em **Column Families** e suporte a **múltiplas versões por timestamp**.

**Quando usar**

* **Volume Massivo:** Terabytes a Petabytes, com crescimento contínuo.
* **Alta Vazão:** telemetria/IoT, logs operacionais, eventos em tempo real com milhares a milhões de writes por segundo (dependendo do cluster e key design).
* **Time-Series Operacional:** histórico curto e acesso previsível por entidade e tempo (ex.: “últimas 24h/7d por device”).
* **Serving/Lookup:** quando um sistema precisa responder em **milissegundos** para leituras por chave (ex.: “estado atual da ambulância”, “flags do usuário”).
* **Workload previsível:** você sabe as queries principais (lookup por key, prefix/range scan por key).

**Onde se encaixa na arquitetura**
Camada **Operational Serving (Online Store)**: Bigtable serve aplicações e dashboards operacionais (baixa latência), enquanto **BigQuery/BigLake** ficam responsáveis por **analytics/histórico longo**, auditoria e ML offline. Em streaming, Bigtable costuma receber dados “curados/acionáveis” (estado, agregados, flags), não o raw infinito.

**Exemplo prático**

* **Ambulâncias (Estado Atual):** tabela `ambulance_state`, Row Key `ambulance_id`, colunas com `last_lat`, `last_lon`, `status`, `last_seen_ts`.
* **Telemetria Recente:** tabela `ambulance_telemetry`, Row Key `ambulance_id#reversed_ts`, TTL 30 dias, leitura por prefixo do `ambulance_id`.
* **Moderation:** tabela `user_flags`, Row Key `user_id`, colunas `flag_reason`, `last_message_ts`, `action`.

**Ponto de prova / armadilha comum**
❌ Bigtable não é para **SQL ad hoc**, **JOINs**, agregações complexas e exploração BI.
✅ **Dica de Ouro:** se a questão pede **latência de milissegundos para servir um App**, a resposta é **Bigtable**, não BigQuery.
⚠️ O “segredo” não é só o serviço: é o **design da Row Key** e o padrão de acesso.

---

## 🔑 2. Design de Row Key (O Segredo da Performance)

**O que é**
A **Row Key** é o índice primário do Bigtable. Como as linhas são armazenadas **ordenadas lexicograficamente**, a Row Key define:

1. **Como os tablets são formados**,
2. **Como o tráfego se distribui**,
3. **Como prefix/range scans performam**,
4. Se você terá ou não **hotspotting**.

**Quando usar**
Sempre. Em Bigtable, “modelagem” começa por: **quais são minhas queries mais comuns?** A Row Key precisa refletir o padrão de leitura/escrita.

**Onde se encaixa na arquitetura**
É a decisão central da camada de Serving: conecta **padrão de acesso** → **layout físico (tablets)** → **latência/custo**. Uma Row Key mal desenhada pode derrubar performance mesmo com muitos nós.

**Exemplo prático**
**Padrões de Row Key por objetivo:**

* **Lookup (último estado):** `ambulance_id`

  * leitura: `ReadRow(ambulance_id)`
  * atualização: sobrescreve “estado atual”.
* **Histórico por entidade (range scan):** `ambulance_id#reversed_timestamp`

  * leitura por prefixo `ambulance_id#` → pega eventos recentes primeiro.
* **Agregados por região/janela:** `region#YYYYMMDDHH`

  * leitura rápida do agregado de uma janela.
* **Anti-hotspot (alto volume em poucas chaves):** `shard#ambulance_id#timestamp`

  * shard pode ser `00..63` via hash(ambulance_id) ou hash(event_id).

**Boas práticas (regras de ouro)**

* **Evitar timestamp puro no início:** `timestamp#id` tende a criar hotspot de ingestão (todo mundo escreve no mesmo “fim” do range).
* **Reversed timestamp:** usar `Long.MAX_VALUE - timestamp` (ou formato equivalente) para que o mais recente fique “no topo” do range por entidade.
* **Sharding/Salting:** prefixo `shard#` para espalhar escrita por mais tablets/nós quando o tráfego é desigual.
* **Design guiado por query:** defina sua chave para sua leitura mais crítica.

### (Add-on PDE) Técnica de “Reversal / Reverse Domain”

**O que é**
Inversão de domínio na key para distribuir melhor dados hierárquicos (domínios/namespaces). Ex.: `com.google.maps` em vez de `google.com/maps`.

**Quando usar**
Quando muitas chaves compartilham o mesmo prefixo “dominante” (ex.: `google.com/*`), causando concentração de range e piorando distribuição.

**Onde se encaixa na arquitetura**
Ajuda tanto a distribuição de tablets quanto a permitir **buscas por prefixo** eficientes por TLD/domínio.

**Exemplo prático**

* RUIM: `google.com/search#timestamp#...` → muitas entradas com `google.com` agrupadas.
* BOM: `com.google.search#timestamp#...`, `com.google.maps#timestamp#...`

**Ponto de prova / armadilha comum**
✅ Resolve hotspot causado por **prefixos dominantes** (domínio/namespace).
⚠️ Não resolve hotspot de **timestamp crescente na frente** — para isso use reversed timestamp e/ou sharding.

**Ponto de prova / armadilha comum (geral de Row Key)**
✅ Bigtable é ótimo para **buscas por prefixo** (“todas as linhas que começam com `amb_001#`”).
⚠️ Sharding/salting tem trade-off: para ler “tudo do `ambulance_id`”, você pode ter que consultar **N shards** (mais round trips/complexidade).
⚠️ Aumentar partitions/tablets/nós não corrige sozinho uma key que concentra tráfego.

---

## 🧩 3. Tablets, Split e Hotspotting (Como Bigtable escala de verdade)

**O que é**
Bigtable divide a tabela em **tablets** (ranges contíguos de Row Key). Tablets podem sofrer **split** quando crescem e o sistema redistribui tablets entre nós para paralelismo. Em prática, hotspot é quando **um ou poucos tablets** recebem a maior parte do tráfego.

**Quando usar**

* Em qualquer cenário de alta vazão/baixa latência (telemetria/IoT).
* Quando houver sintomas: latência alta, throttling, “picos” em subset de keys, performance desigual.

**Onde se encaixa na arquitetura**
Parte do raciocínio de performance/observabilidade do Serving: “Row Key define tablets; tablets definem distribuição de carga”.

**Exemplo prático**
Se sua key começa por timestamp (crescente), a escrita cai sempre num intervalo pequeno de chaves recentes → um tablet vira gargalo mesmo com cluster grande.

**Ponto de prova / armadilha comum**
❌ “Só aumenta nós” pode ser resposta errada se o problema é concentração de tráfego por key/range.
✅ Hotspot quase sempre aponta para: **Row Key** + **padrão de acesso** (e depois capacity).

---

## 🧹 4. Garbage Collection (GC) no Bigtable

**O que é**
Políticas automáticas de retenção por **Column Family** para remover dados antigos ou versões excedentes, mantendo tabela enxuta. Duas políticas principais:

* **Age-based (TTL)**: remove após X dias.
* **Version-based**: mantém as últimas N versões por célula.

**Quando usar**

* Quando Bigtable é **operacional** (serving) e não precisa guardar “histórico infinito”.
* Para controlar custo, evitar rows gigantes e manter leituras rápidas.
* Para separar retenções diferentes por tipo de dado (state vs telemetry vs debug).

**Onde se encaixa na arquitetura**
Contrato de retenção da camada Serving. Histórico longo costuma ir para **BigQuery/BigLake** (lakehouse/warehouse), enquanto Bigtable mantém janela operacional.

**Exemplo prático**

* Family `state`: max versions = 1 (último estado sempre).
* Family `telemetry`: TTL 30 dias (investigação recente).
* Family `debug`: TTL 7 dias (ruído).

**Ponto de prova / armadilha comum**
❌ “GC é ruim porque perde histórico” → Bigtable não é o repositório histórico definitivo; é serving.
✅ GC evita rows “chunky” e excesso de versões que aumentam bytes lidos e degradam latência.

---

## 🧭 5. Diagnóstico de Performance (Cloud Monitoring vs Key Visualizer)

**O que é**

* **Cloud Monitoring (macro):** visão geral do serviço — latência (read/write), QPS, erros, throttling, saturação/uso de recursos.
* **Key Visualizer (micro):** visão granular por ranges de Row Key — “mapa de calor” que evidencia **hotspots**.

**Quando usar**

* Comece pelo **Cloud Monitoring** para triagem: “é read ou write?”, “é global?”, “tem throttling?”, “há pico de tráfego?”.
* Use o **Key Visualizer** para confirmar hotspot e entender se o problema é **key design** ou distribuição de carga por ranges.

**Onde se encaixa na arquitetura**
Observabilidade do Serving:

* Monitoring para detectar impacto (SLA)
* Visualizer para explicar causa raiz (RCA)

**Exemplo prático**

* CPU global baixa, mas latência alta para subset de usuários → suspeita de hotspot/padrão de acesso.
* CPU global alta + throttling global → gargalo de capacity (nodes) pode ser real.
* Latência alta com bytes lidos altos → rows largas/versions demais/GC mal ajustado.

**Ponto de prova / armadilha comum**
❌ Diagnosticar tudo como falta de nós.
✅ Se é localizado por key/range, a resposta correta envolve **Row Key** (hotspot) e não apenas scale-up.

---

## 🔗 6. Integração Pub/Sub → Bigtable (Por que não é “direct path” como BigQuery)

**O que é**
BigQuery possui ingestão direta (Pub/Sub subscription escreve em tabela). Bigtable **não** tem um “subscription type” nativo equivalente. Para escrever em Bigtable vindo do Pub/Sub, você usa um writer intermediário.

**Quando usar**

* **Alta vazão / robustez / backpressure / transformações:** Pub/Sub → **Dataflow** → Bigtable.
* **Volume moderado / lógica simples:** Pub/Sub → **Cloud Run/Functions** → Bigtable.

**Onde se encaixa na arquitetura**
Pub/Sub (event bus) → processamento/controle (Dataflow/app) → Bigtable (serving). Normalmente Bigtable recebe “estado/flags/agregados”, não raw infinito.

**Exemplo prático**

* Ambulâncias → Pub/Sub → Dataflow (valida schema, dedup, normaliza) → Bigtable `ambulance_state`.
* Eventos esporádicos → Pub/Sub Push → Cloud Run → upsert em Bigtable `user_flags`.

**Ponto de prova / armadilha comum**
❌ Assumir que “Bigtable também tem direct ingestion igual BigQuery”.
✅ Bigtable exige writer consciente (Row Key, families, idempotência).

---

## ♾️ 7. Integração Pub/Sub → BigQuery → Bigtable (Continuous Queries / Reverse ETL)

**O que é**
Padrão de **ELT near real-time + reverse ETL**: Pub/Sub aterrissa no BigQuery (raw), uma **Continuous Query** processa incrementalmente e exporta resultado para Bigtable (serving).

**Quando usar**

* Quando a lógica de transformação é bem expressa em **SQL** (filtrar, classificar, agregar, aplicar regras).
* Quando você quer BigQuery como “motor de decisão” e Bigtable como “cache operacional”/online store.

**Onde se encaixa na arquitetura**
Pub/Sub (ingress) → BigQuery raw (landing/auditoria) → Continuous Query (curadoria/decisão) → Bigtable (serving ms).

**Exemplo prático**

* Moderation: BigQuery classifica mensagens e exporta “flag por user_id” para Bigtable.
* Ambulâncias: BigQuery calcula flags/alertas e mantém Bigtable com “estado operacional por id”.

**Ponto de prova / armadilha comum**
⚠️ Não substituir Dataflow quando precisa de streaming avançado (event time, state complexo, late data sofisticado).
✅ Este padrão brilha quando o “cérebro” pode ser SQL e o serving precisa ser rápido.

---

## 🧩 8. App Profiles (Roteamento, Prioridade, Consistência e Isolamento)

**O que é**
App profiles definem como o tráfego do Bigtable é roteado e priorizado: ajudam a separar workloads e, em ambientes multi-cluster, definir roteamento entre clusters.

**Quando usar**

* Para isolar **serving crítico** de workloads de background (exports, scans).
* Para resiliência/latência em multi-cluster (failover, roteamento).

**Onde se encaixa na arquitetura**
Controle de performance/consistência da camada Serving: garante SLAs e evita que jobs pesados degradem o app.

**Exemplo prático**

* App profile “serving-high”: leituras do app operacional.
* App profile “export-low”: reverse ETL/exports rodando em baixa prioridade.

### (Add-on PDE) Single-Cluster Routing vs Multi-Cluster Routing

**O que é**

* **Single-Cluster Routing:** força tráfego em um cluster específico.
* **Multi-Cluster Routing:** envia a requisição para o cluster mais próximo/adequado (melhor latência), porém com implicações de consistência devido à replicação.

**Quando usar**

* **Multi-Cluster (eventual):** melhor latência global, tolera atraso de replicação.
* **Single-Cluster (read-your-writes):** quando você precisa garantir leitura imediata do que acabou de escrever.

**Onde se encaixa na arquitetura**
Trade-off de **latência vs consistência** em arquiteturas multi-região.

**Exemplo prático**

* Operador atualiza flag do usuário e precisa ver imediatamente → Single-Cluster.
* Dashboard global aceita pequeno atraso → Multi-Cluster.

**Ponto de prova / armadilha comum**
⚠️ Multi-cluster pode apresentar **consistência eventual**: escreveu no cluster A e leu no cluster B logo depois → pode não ver a atualização.
✅ Se a questão pede “leitura imediata após escrita”, justificativa envolve **rotear para o mesmo cluster** (Single-Cluster) ou aceitar eventual (Multi-Cluster).

---

## 🏢 9. BigQuery Editions / Reservations (o erro que você viu no lab)

**O que é**
Alguns recursos (como integrações avançadas/exports e Continuous Queries) podem exigir **BigQuery Editions** e **Capacity Reservations** (slots reservados). Jobs são classificados por tipo (ex.: `QUERY`, `CONTINUOUS`).

**Quando usar**
Quando o cenário pede execução contínua ou recursos que não rodam em modelo totalmente on-demand. Em labs, isso aparece como requisito “escondido”.

**Onde se encaixa na arquitetura**
Camada de analytics/decisão (BigQuery) com operação controlada: reservas dão previsibilidade de capacidade e habilitam features.

**Exemplo prático**

* `EXPORT DATA ... format='CLOUD_BIGTABLE'` rodando como query job.
* Continuous Query exigindo capacidade dedicada para rodar continuamente.

**Ponto de prova / armadilha comum**
⚠️ **FAILED_PRECONDITION:** não misturar `CONTINUOUS` com tipos não-continuous na mesma reservation (precisa separar).
✅ Entender `QUERY` vs `CONTINUOUS` evita ficar preso em erro de capacidade/configuração.

---
