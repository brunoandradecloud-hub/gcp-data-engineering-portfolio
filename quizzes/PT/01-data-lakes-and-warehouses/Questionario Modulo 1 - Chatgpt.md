# 📝 Simulado – Módulo 01
**Google Professional Data Engineer**
(50 questões – Versão ChatGPT)

---

**1.** Uma empresa de câmbio mantém 12 anos de dados históricos que não sofrem atualização e são usados apenas para auditorias ocasionais. O principal objetivo é minimizar custo de armazenamento e consulta. Qual abordagem é mais adequada?
A) Carregar os dados em tabelas nativas do BigQuery particionadas
B) Armazenar os dados no Cloud Storage e acessá-los via External Tables
C) Replicar os dados continuamente usando Datastream
D) Migrar os dados para Cloud Spanner para alta durabilidade

**2.** Durante o desenho de uma arquitetura, você decide separar claramente dados brutos, dados tratados e dados prontos para consumo. Qual característica define corretamente a camada Raw?
A) Dados já anonimizados e prontos para análise
B) Dados com regras de negócio aplicadas
C) Dados imutáveis armazenados como chegaram da fonte
D) Dados agregados em modelo dimensional

**3.** Uma equipe precisa capturar alterações contínuas (insert, update, delete) de um banco Oracle sem impactar a performance do sistema de origem. Qual solução atende melhor esse requisito?
A) Dataflow em modo batch
B) BigQuery Data Transfer Service
C) Datastream
D) Cloud Scheduler

**4.** Qual é a principal responsabilidade do Dataflow em um pipeline de CDC baseado em Datastream?
A) Detectar mudanças no banco de origem
B) Armazenar os logs de transação
C) Processar e aplicar as mudanças no destino (BigQuery/GCS)
D) Gerenciar as chaves de criptografia

**5.** Você precisa de um banco de dados para um sistema de cotações de moedas globais que exige escalabilidade horizontal ilimitada e consistência forte (Strong Consistency). Qual banco escolher?
A) Cloud SQL
B) AlloyDB
C) Cloud Spanner
D) Cloud Bigtable

**6.** Uma empresa deseja usar o BigQuery para analisar dados no Cloud Storage sem precisar movê-los fisicamente, mas mantendo a governança de segurança por coluna. Qual tecnologia habilita isso?
A) External Tables simples
B) BigLake
C) Datastream
D) Cloud Functions

**7.** Em qual camada do Data Lake os dados devem ser convertidos para formatos colunares (como Parquet ou Avro) e ter os tipos de dados validados?
A) Raw
B) Processed
C) Curated
D) Landing Zone

**8.** Analistas de marketing precisam realizar consultas SQL em tempo real sobre o saldo dos clientes que está em um banco AlloyDB. Qual recurso do BigQuery permite isso sem ingestão de dados?
A) BQML
B) External Query (Federated Query)
C) Exportação agendada
D) Dataplex

**9.** Ao projetar uma tabela no BigQuery para otimizar custos de consultas que filtram por "País" e "Data", qual a melhor prática?
A) Particionar por País e Data
B) Particionar por Data e Clusterizar por País
C) Clusterizar por Data e Particionar por País
D) Não usar particionamento em tabelas grandes

**10.** Qual serviço é responsável por catalogar dados e aplicar políticas de governança centralizadas em ambientes de Data Lake e Data Warehouse?
A) Cloud KMS
B) Dataplex
C) Data Loss Prevention (DLP)
D) Vertex AI

**11.** Qual ferramenta você usaria para identificar automaticamente números de cartões de crédito em uma coluna de comentários no BigQuery?
A) Cloud KMS
B) Cloud IAM
C) Data Loss Prevention (DLP)
D) BigQuery ML

**12.** O BigQuery ML (BQML) é ideal para qual tipo de usuário?
A) Desenvolvedores Java
B) Analistas e Engenheiros que dominam SQL
C) Apenas Data Scientists especialistas em PyTorch
D) Administradores de Redes

**13.** Se uma empresa exige que o Google não tenha qualquer acesso às chaves que criptografam os dados, o que deve ser usado?
A) Criptografia padrão do Google
B) Customer-Managed Encryption Keys (CMEK) via KMS
C) Senhas manuais em tabelas
D) Firewall de rede

**14.** Qual é a principal vantagem do AlloyDB em relação ao Cloud SQL para workloads PostgreSQL?
A) É gratuito
B) Maior performance em processamento analítico e transacional integrado
C) Não suporta replicação
D) Só funciona com dados em CSV

**15.** O que acontece ao utilizar o comando CREATE OR REPLACE MODEL no BigQuery ML?
A) O modelo aprende incrementalmente
B) O modelo anterior é deletado e um novo é treinado do zero
C) Apenas os novos dados são processados
D) O BigQuery cobra apenas pelo armazenamento, não pelo treino

**16.** Para uma busca por similaridade semântica (ex: achar reclamações parecidas), qual técnica é recomendada?
A) Operador SQL LIKE
B) Embeddings + Vector Search
C) Índices Primários
D) Normalização de tabelas

**17.** Qual serviço permite visualizar a linhagem de dados (Data Lineage) no Google Cloud?
A) Cloud Storage
B) Datastream
C) Dataplex
D) Cloud Spanner

**18.** Qual a principal diferença entre BigQuery e Cloud Bigtable?
A) BigQuery é para transações; Bigtable é para analytics
B) BigQuery é para analytics (OLAP); Bigtable é NoSQL de baixa latência
C) BigQuery é pago; Bigtable é gratuito
D) Não há diferença

**19.** Em um cenário de Lakehouse, por que usar o formato Iceberg?
A) Para compactar imagens
B) Para permitir transações ACID e evolução de esquema em arquivos no GCS
C) Para substituir o BigQuery permanentemente
D) Para armazenar apenas dados brutos

**20.** Qual estratégia de ingestão é a mais econômica para dados que só precisam estar disponíveis uma vez por dia?
A) Streaming constante
B) Ingestão em Batch (Load Jobs)
C) Datastream
D) Pub/Sub

**21.** Qual o objetivo principal de usar o Cloud Storage como base para o Data Lake?
A) Substituir o BigQuery
B) Armazenamento de baixo custo e alta escalabilidade para dados brutos
C) Executar consultas SQL mais rápidas que o BigQuery
D) Garantir consistência forte global

**22.** Você precisa processar dados sensíveis e mascará-los antes que cheguem à camada Curated. Onde esse processo deve ocorrer?
A) Camada Raw
B) Camada Processed
C) Diretamente no Looker
D) No banco de origem

**23.** O que define uma tabela "Clusterizada" no BigQuery?
A) Dados divididos por faixas de tempo
B) Dados organizados fisicamente com base no conteúdo de colunas específicas para otimizar filtros
C) Dados armazenados em múltiplos buckets do GCS
D) Tabelas que não podem ser excluídas

**24.** Qual a principal limitação das External Tables em comparação com Tabelas Nativas?
A) Não podem ler Parquet
B) Performance de consulta geralmente inferior
C) Custam mais caro para armazenar
D) Não suportam segurança por linha

**25.** O que é o "Pruning" no contexto de tabelas particionadas?
A) Exclusão de dados antigos
B) Capacidade do BigQuery de ignorar partições que não atendem ao filtro da consulta
C) Compressão de colunas não utilizadas
D) Processo de limpeza de metadados

**26.** Qual banco de dados é recomendado para armazenar métricas de séries temporais com alta taxa de escrita (milhões de eventos por segundo)?
A) Cloud SQL
B) Cloud Spanner
C) Cloud Bigtable
D) AlloyDB

**27.** O Dataplex permite organizar dados em "Lakes" e "Zones". Qual a vantagem disso?
A) Melhora a velocidade de escrita no GCS
B) Permite aplicar governança e permissões de forma lógica e centralizada
C) Elimina a necessidade de usar o IAM
D) Converte automaticamente CSV para JSON

**28.** Em uma arquitetura moderna, onde os modelos de Machine Learning costumam consumir dados?
A) Camada Raw
B) Camada Curated ou Processed
C) Diretamente do Datastream
D) Do Cloud KMS

**29.** O que é necessário para usar o Vector Search no BigQuery?
A) Converter os dados em Embeddings numéricos
B) Usar apenas tabelas CSV
C) Desativar o faturamento da conta
D) Ter uma instância do Cloud SQL ativa

**30.** Qual a principal função do Cloud KMS na arquitetura de dados?
A) Processar logs de auditoria
B) Gerenciar e proteger as chaves que criptografam os dados em repouso
C) Substituir o DLP
D) Monitorar o tráfego de rede

**31.** Se você precisa de alta disponibilidade (99,999%) para um sistema de pagamentos global, qual a escolha óbvia?
A) Cloud SQL
B) AlloyDB
C) Cloud Spanner
D) Bigtable

**32.** O que diferencia o BigLake de uma External Table comum?
A) BigLake é gratuito
B) BigLake suporta governança avançada (segurança nível de coluna) e formatos abertos como Iceberg
C) External Tables não suportam Cloud Storage
D) Não há diferença técnica

**33.** Por que o CDC (Change Data Capture) é preferível a dumps diários de banco de dados?
A) Porque é mais barato
B) Porque permite atualizações em tempo quase real sem sobrecarregar a origem
C) Porque elimina a necessidade de Dataflow
D) Porque dumps diários não funcionam com Oracle

**34.** Qual ferramenta do Google Cloud é usada para criar pipelines ETL/ELT tanto em Batch quanto em Streaming?
A) Datastream
B) Dataflow
C) BigQuery ML
D) Pub/Sub

**35.** O que é um "Embedding" no contexto de IA no BigQuery?
A) Uma forma de comprimir tabelas
B) Uma representação vetorial numérica de um dado (texto/imagem) que captura seu significado
C) Um tipo de índice de banco de dados
D) Uma chave de criptografia do KMS

**36.** Qual o papel do Pub/Sub em uma arquitetura de streaming?
A) Armazenar dados permanentemente
B) Atuar como um buffer/mensageria entre a origem e o processamento
C) Substituir o Cloud Storage
D) Executar consultas SQL

**37.** No BigQuery, o que acontece se você não filtrar uma tabela particionada pela coluna de partição?
A) A consulta falha
B) O BigQuery realiza um "Full Table Scan", aumentando o custo e tempo
C) O BigQuery adivinha a partição correta
D) O custo é o mesmo

**38.** Qual a principal característica de uma solução "Serverless" como o BigQuery?
A) Você precisa gerenciar o sistema operacional
B) A infraestrutura escala automaticamente e você paga pelo uso
C) Não utiliza servidores físicos no data center
D) É limitado a 1TB de dados

**39.** O Vertex AI é necessário para qual cenário?
A) Criar uma tabela no BigQuery
B) MLOps avançado, treinamento customizado e gerenciamento de modelos em produção
C) Fazer ingestão de CSV
D) Criptografar buckets

**40.** Qual a função da Camada Processed no Data Lake?
A) Armazenar logs brutos para sempre
B) Fornecer dados limpos, tipados e organizados para consumo ou modelagem
C) Exibir dashboards para o CEO
D) Armazenar chaves privadas

**41.** O que é o Federated Query (External Query)?
A) Uma forma de unir dois datasets do BigQuery
B) Capacidade do BigQuery de enviar uma consulta SQL para bancos operacionais (Cloud SQL/AlloyDB/Spanner)
C) Um sistema de busca do Google
D) Uma forma de migrar dados para o S3

**42.** Por que usar Parquet no Cloud Storage em vez de CSV para análise?
A) CSV é maior e mais lento para ler em escala
B) Parquet é um formato colunar que otimiza a leitura de colunas específicas
C) BigQuery não lê CSV
D) Parquet é mais fácil de ler no Excel

**43.** O que é "Data Lineage"?
A) O histórico de quem acessou o dado
B) O rastreamento da jornada do dado, desde a origem até o destino final
C) A hierarquia de pastas no GCS
D) O tempo de vida de uma tabela

**44.** Qual a principal vantagem do Cloud Spanner em relação ao Cloud SQL?
A) Mais barato
B) Escalabilidade horizontal global com consistência forte
C) Não requer SQL
D) É um banco NoSQL

**45.** Qual é um benefício do Lakehouse?
A) Eliminar Data Lakes
B) Unir governança e flexibilidade de formatos abertos com performance de Warehouse
C) Substituir BigQuery
D) Eliminar BI

**46.** Qual componente NÃO faz parte do pipeline de streaming direto?
A) Datastream
B) Dataflow
C) Pub/Sub
D) Dataplex (ele é governança, não transporte)

**47.** Qual erro arquitetural é comum em provas?
A) Usar BigQuery ML para modelos simples
B) Usar Dataflow para streaming
C) Usar External Tables para dados que sofrem atualizações constantes e exigem alta performance
D) Usar Datastream para CDC

**48.** Qual camada aplica regras técnicas iniciais e limpeza (Data Cleansing)?
A) Raw
B) Processed
C) Curated
D) ML

**49.** Qual tecnologia fornece segurança por linha e coluna em dados no Cloud Storage acessados pelo BigQuery?
A) External Tables
B) BigLake
C) DLP
D) IAM apenas

**50.** Qual decisão demonstra melhor alinhamento com custo, escalabilidade e governança para dados analíticos?
A) Tudo em BigQuery Native
B) Tudo em Cloud Storage sem metadados
C) Lakehouse com BigLake + BigQuery
D) Cloud Spanner para tudo

---

## 🗝️ Gabarito

1-B
2-C  
3-C  
4-C
5-C
6-B
7-B
8-B
9-B
10-B
11-C
12-B
13-B
14-B
15-B
16-B
17-C
18-B
19-B
20-B
21-B
22-B
23-B
24-B
25-B
26-C
27-B
28-B
29-A
30-B
31-C
32-B
33-B
34-B
35-B
36-B
37-B
38-B
39-B
40-B
41-B
42-B
43-B
44-B
45-B
46-D
47-C
48-B
49-B
50-C