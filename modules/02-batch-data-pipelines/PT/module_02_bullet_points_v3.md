# 📑 Guia de Estudos — Módulo 02 (Deduplicação, Data Quality e Maturidade)

## 🏗️ Níveis de Controle de Dados (Maturidade Arquitetural)

**O que é** A hierarquia de decisão técnica sobre onde e como tratar a qualidade do dado, separando soluções paliativas de soluções de engenharia profissional.

**Quando usar** * **Nível 1 (UI/Templates):** Ingestão rápida "Lift & Shift" para dados simples sem necessidade de lógica de limpeza.  
* **Nível 2 (SQL/MERGE):** Correção reativa após a carga; útil para pequenos ajustes, mas escala mal e gera custo de reprocessamento.  
* **Nível 3 (Beam SDK):** Controle proativo; obrigatório para Exactly-once, deduplicação em voo e governança de ponta a ponta.

**Onde se encaixa na arquitetura** Na camada de processamento que transforma e move os dados da Raw para a Processed/Curated.

**Exemplo prático** Impedir que um registro duplicado chegue ao BigQuery via código Beam (Nível 3) em vez de pagar por um script SQL de limpeza agendado (Nível 2).

**Ponto de prova / armadilha comum** ❌ A prova oferece "Dataflow Templates" como solução para lógicas de validação complexas.  
✅ O padrão Professional PDE exige Apache Beam SDK para garantir "Exactly-once" e prevenir a entrada de lixo no storage.

---

## 🧹 Deduplicação Proativa (In-flight Dedup)

**O que é** A estratégia de garantir a unicidade do dado no pipeline de processamento antes da persistência final.

**Quando usar** Sempre que for necessário evitar o "inchaço" de storage e reduzir custos operacionais de DML (MERGE) no Data Warehouse.

**Onde se encaixa na arquitetura** Dentro da lógica do Dataflow usando as transformações nativas `beam.Distinct()` ou `beam.CombinePerKey`.

**Exemplo prático** Remover logs de eventos idênticos disparados por falhas de rede antes que eles ocupem espaço nas tabelas analíticas.

**Ponto de prova / armadilha comum** ❌ "Vou rodar um DISTINCT após a carga no BigQuery."  
✅ Deduplicar no Beam SDK é a resposta correta para escala e economia de slots do BigQuery.

---

## 🧠 GroupByKey vs. CombinePerKey

**O que é** Operações fundamentais de agregação e redução de dados dentro do motor do Dataflow.

**Quando usar** * **GroupByKey:** Quando o volume de dados por chave é pequeno e o processamento exige todos os elementos juntos.  
* **CombinePerKey:** Para deduplicação e agregações massivas; realiza redução parcial nos workers de origem.

**Onde se encaixa na arquitetura** Núcleo de processamento do pipeline de dados.

**Exemplo prático** Contar bilhões de acessos por URL: `CombinePerKey` reduz a contagem localmente antes de enviar o total, evitando gargalos de rede.

**Ponto de prova / armadilha comum** ❌ `GroupByKey` é a solução recomendada para deduplicação em larga escala.  
✅ `GroupByKey` gera alto shuffle e estoura memória (OOM); `CombinePerKey` é o padrão para performance e escala.

---

## 🧬 Schema como Contrato

**O que é** A filosofia de que o código do pipeline (Schema as Code) é o juiz final que impõe a estrutura do dado antes do consumo.

**Quando usar** Para garantir que a camada Curated nunca receba dados malformados que quebrem Dashboards ou modelos de ML.

**Onde se encaixa na arquitetura** Na lógica de validação do Dataflow entre as camadas Raw e Processed.

**Exemplo prático** Um pipeline que aceita campos extras na Raw (flexível), mas falha propositalmente ou desvia o dado se o campo "ID_CLIENTE" não for numérico na Curated (rígido).

**Ponto de prova / armadilha comum** ✅ Raw tolera mudança (Schema-on-read); Curated exige contrato (Schema-on-write). O Beam é quem executa esse contrato.

---

## 🪟 Facade View Pattern

**O que é** Criação de uma camada de abstração via Views no BigQuery para desacoplar as tabelas físicas dos consumidores.

**Quando usar** Para permitir mudanças estruturais no banco de dados sem interromper dashboards ou APIs.

**Onde se encaixa na arquitetura** Camada de Apresentação/Consumo (Presentation Layer).

**Exemplo prático** A View `v_vendas_atual` aponta para `tabela_vendas_2024`. Ao virar o ano, você altera a View para `tabela_vendas_2025` e o Looker continua funcionando sem edições.

**Ponto de prova / armadilha comum** ❌ Usar Views para aumentar a velocidade de processamento de queries.  
✅ Use para "avoid breaking dashboards", "abstract complexity" e fornecer uma "stable interface".

---

## ⚠️ Dead Letter Queue (DLQ Real)

**O que é** Mecanismo de desvio técnico para capturar registros que falharam durante o processamento para posterior auditoria.

**Quando usar** Cenários onde o dado não pode ser simplesmente descartado, exigindo análise do motivo da falha.

**Onde se encaixa na arquitetura** Implementado via Side Outputs (`TaggedOutput`) no código Beam, salvando o erro + o dado original no GCS ou BQ.

**Exemplo prático** Se um registro de venda falha na conversão de moeda, ele é enviado para um bucket de "erros" com o stacktrace da exceção anexado.

**Ponto de prova / armadilha comum** ❌ Templates da UI oferecem visibilidade total de erros por registro.  
✅ Somente o SDK permite um DLQ granular que salva a causa exata da falha e permite reprocessamento cirúrgico.

---

## ⚔️ Comparativo: Beam SDK vs. Dataflow UI

| Recurso | Apache Beam SDK (Código) | Dataflow UI / Templates |
| :--- | :--- | :--- |
| **Lógica** | Customizada e Proativa | Rígida e Reativa |
| **Deduplicação** | Nativa (antes da escrita) | Manual (SQL pós-carga) |
| **Tratamento de Erro** | DLQ com log de exceção | Erros genéricos de Runner |
| **Maturidade** | Profissional (Nível 3) | Operacional (Nível 1) |