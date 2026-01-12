# Simulação de Cenário 01 — Criar Data Lakes e Data Warehouses no Google Cloud

## Objetivo

Este cenário consolida os principais conceitos do **Módulo 1 – Criar Data Lakes e Data Warehouses no Google Cloud**.

O objetivo é simular um problema realista no estilo da certificação, exigindo **tomada de decisão arquitetural**, e não foco em implementação ou código.

Ao final deste cenário, o leitor deve ser capaz de:
- Diferenciar sistemas operacionais e analíticos
- Aplicar corretamente a arquitetura de Data Lake (raw / processed / curated)
- Decidir quando utilizar tabelas nativas do BigQuery versus BigLake
- Separar workloads batch e streaming
- Justificar decisões arquiteturais com base nas boas práticas do Google Cloud

---

## Tópicos Abordados

- Sistemas legados e conceito de ODS
- Ingestão batch versus streaming
- Arquitetura de Data Lake no Cloud Storage
- Camada curated no BigQuery
- BigLake como padrão Lakehouse
- Otimização de custos e governança
- Separação entre workloads OLTP e OLAP

---

## Cenário de Negócio — Casa de Câmbio Digital

Uma empresa de câmbio digital atua no setor financeiro e possui mais de **10 anos de dados transacionais**, incluindo operações de câmbio e histórico de clientes.

### Estado Atual

- Todos os dados históricos e operacionais estão armazenados em um banco **Oracle**, que atua como um ODS legado.
- O sistema é confiável, porém fortemente acoplado, caro para escalar e inadequado para análises modernas.
- Relatórios são limitados, lentos e com pouca flexibilidade histórica.
- A empresa planeja lançar um **aplicativo global**, exigindo alta disponibilidade e baixa latência para transações financeiras.

---

## Requisitos

### Requisitos Analíticos (BI e Analytics)

- Acesso a todo o histórico de 10 anos
- Suporte a cargas analíticas diárias (D-1)
- Execução de consultas SQL complexas para dashboards e relatórios executivos
- Armazenamento de baixo custo para grandes volumes históricos
- Governança de dados e segurança em nível de coluna

### Requisitos Operacionais (Aplicação)

- Consistência forte para transações financeiras
- Escalabilidade global
- Baixa latência para consulta de cotações e métricas de uso
- Separação clara entre sistemas operacionais e analíticos

---

## Arquitetura Proposta

### 1. Ingestão de Dados

**Dados Históricos e Batch**
- Dados históricos são extraídos do Oracle e de fontes externas (CSV) por meio de pipelines batch gerenciados.
- O processamento segue o modelo D-1 para otimização de custos.

**Captura de Mudanças (CDC)**
- O Datastream captura mudanças contínuas no banco Oracle.
- Eventos de CDC são enviados para a plataforma analítica sem impactar o sistema de origem.

---

### 2. Data Lake — Cloud Storage

O Data Lake é implementado no **Cloud Storage** e organizado em camadas lógicas:

**Camada Raw**
- Armazena os dados exatamente como recebidos das fontes
- Preserva formatos originais para auditoria e reprocessamento

**Camada Processed**
- Aplica limpeza, padronização e tipagem
- Converte os dados para formatos colunares (Parquet), otimizados para análise

---

### 3. Camada Curated (Analítica)

A camada curated representa os dados prontos para consumo analítico.

**BigLake (Padrão Lakehouse)**
- Utilizado para grandes volumes de dados históricos majoritariamente imutáveis (~85 TB)
- Fornece acesso via SQL, governança centralizada e segurança em nível de coluna
- Permite que times de BI e Ciência de Dados utilizem os mesmos dados sem duplicação

**Tabelas Nativas do BigQuery**
- Armazenam dados consolidados e de acesso frequente
- Alimentam dashboards, relatórios executivos e análises interativas
- Otimizadas para performance analítica

---

### 4. Modernização Operacional

**Cloud Spanner (Sistema OLTP)**
- Atua como banco transacional do novo aplicativo global
- Garante consistência forte e escalabilidade horizontal
- Desacopla o workload da aplicação dos sistemas analíticos

**Cloud Bigtable (Acesso de Baixa Latência)**
- Armazena cotações de moedas em alta frequência e métricas de uso do aplicativo
- Otimizado para grandes volumes e acesso em milissegundos
- Suporta funcionalidades em tempo real do aplicativo

---

## Racional Arquitetural

- O ODS legado é mantido operacionalmente, enquanto a carga analítica é transferida para o Data Lake e BigQuery.
- Workloads batch e streaming são separados com base nos requisitos de latência.
- O BigLake reduz custos de armazenamento e evita duplicação de dados, mantendo governança.
- Tabelas nativas do BigQuery são utilizadas quando a performance analítica é crítica.
- Sistemas OLTP e OLAP permanecem isolados para garantir escalabilidade e estabilidade.

---

## Alinhamento com a Certificação

Este cenário reflete padrões comumente cobrados na **certificação Google Professional Data Engineer**:
- Escolha adequada de serviços de armazenamento e análise
- Arquiteturas escaláveis e eficientes em custo
- Aplicação de conceitos modernos de Data Lake e Lakehouse
- Justificativa clara de trade-offs arquiteturais

A solução prioriza decisões arquiteturais corretas, alinhando-se ao formato e às expectativas da prova.

