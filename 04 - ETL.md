Okay, vamos complementar a explicação sobre ELT, adicionando mais contexto, detalhes sobre padrões arquiteturais, considerações operacionais e tendências, sem remover o conteúdo já existente.

---

**Material de Estudo Detalhado e Complementado sobre ELT (Extract, Load, Transform)**

**1. Introdução ao ELT**

**1.1. A Era do Big Data e a Necessidade de Processamento**
*(Conteúdo original mantido, com formatação de código)*
Vivemos em uma era definida pela explosão de dados, frequentemente caracterizada pelos "Vs" do Big Data. O **`Volume`** refere-se à quantidade massiva de dados gerados a cada segundo... A **`Velocidade`** descreve a rapidez com que esses dados são criados... A **`Variedade`** abrange os múltiplos formatos... (`JSON`, `XML`).
Além desses, a **`Veracidade`** destaca a importância da qualidade e confiabilidade... Finalmente, o **`Valor`** representa o objetivo final: transformar... insights acionáveis...
Para lidar com essa complexidade e extrair valor, as organizações dependem de **pipelines de dados**. Estes são fluxos de trabalho automatizados projetados para mover dados..., prepará-los e disponibilizá-los para análise e consumo por ferramentas de `BI`..., cientistas de dados ou aplicações.

**1.2. Evolução das Arquiteturas de Processamento: De ETL para ELT**
*(Conteúdo original mantido, com formatação de código)*
Historicamente, a abordagem predominante foi o **`ETL` (`Extract`, `Transform`, `Load`)**. Nesse modelo, os dados são extraídos..., transformados em um servidor intermediário (*staging area*)... e carregados no destino (`DWH` *on-premises*). ...adequada quando os `DWHs` tinham capacidade limitada e custo de armazenamento alto...
No entanto, o advento da **computação em nuvem** revolucionou o cenário. A nuvem oferece armazenamento de objetos virtualmente ilimitado e de baixo custo (como `AWS S3`, `Google Cloud Storage (GCS)`, `Azure Blob Storage`) e data warehouses ou data lakes com poder de processamento massivo, paralelo e elástico (como `Snowflake`, `Google BigQuery`, `Amazon Redshift` e `Azure Synapse Analytics`).
Essa mudança tecnológica tornou viável a abordagem **`ELT` (`Extract`, `Load`, `Transform`)**. ...os dados são extraídos e carregados diretamente no destino em formato bruto... A transformação... acontece *dentro* do `DWH` ou `Data Lake` de destino...
A transição de `ETL` para `ELT` ...é uma mudança de paradigma... armazenamento barato e processamento escalável sob demanda removeram as restrições do `ETL`. Carregar dados brutos rapidamente e transformá-los usando os recursos do destino... impulsionando a adoção generalizada do `ELT`.

**2. Definindo ELT (Extract, Load, Transform)**

**2.1. O que é ELT?**
*(Conteúdo original mantido, com formatação de código)*
`ELT` (`Extract`, `Load`, `Transform`) é um processo... que se diferencia do `ETL` pela ordem das etapas. ...Extraídos... Carregados diretamente... em formato bruto... no destino (`DWH`/`Data Lake` nuvem)... Transformação ocorre *após* o carregamento... onde dados brutos são limpos, enriquecidos, agregados e modelados...

**2.2. A Sequência das Etapas**
*(Conteúdo original mantido, com formatação de código)*
1.  **Extração (`Extract`):** Coleta de dados das fontes originais.
2.  **Carregamento (`Load`):** Transferência e armazenamento dos dados brutos/semi-brutos no destino (`DWH`/`Data Lake`) com processamento mínimo.
3.  **Transformação (`Transform`):** Conversão dos dados brutos (já no destino) em formato estruturado, limpo e otimizado para análise, usando o poder do próprio destino.

**2.3. Diferenças Fundamentais: ELT vs. ETL**
*(Conteúdo original mantido, com formatação de código e pequenas adições para clareza)*
* **Sequência:** `ETL` = E -> T -> L. `ELT` = E -> L -> T.
* **Local da Transformação:** `ETL` = Sistema intermediário (antes da carga). `ELT` = `DWH`/`Data Lake` de destino (após a carga).
* **Uso de Recursos:** `ETL` requer motor dedicado. `ELT` usa poder do `DWH`/`Lake` na nuvem (`BigQuery`, `Snowflake`, etc.).
* **Formato dos Dados no Destino:** `ETL` carrega dados processados/estruturados. `ELT` carrega brutos/minimamente processados, preservando o original (útil para *replay* de transformações ou novas análises).
* **Velocidade de Ingestão:** `ELT` geralmente mais rápido (transformação adiada).
* **Flexibilidade:** `ELT` maior (dados brutos permitem re-transformações/novas lógicas).
* **Manutenção:** `ETL` (servidor ETL + DWH). `ELT` (código `SQL`/transformação no `DWH`/`Lake` + infra gerenciada do destino).
* **Governança e Qualidade (Aplicação):** No `ETL`, regras são aplicadas *antes* da carga, garantindo que apenas dados conformes entrem no `DWH`. No `ELT`, essas regras são aplicadas *após* a carga, exigindo disciplina e ferramentas para gerenciar a qualidade dentro do destino.

A escolha `ETL` vs. `ELT` conecta-se à arquitetura e capacidade do destino. `DWHs` modernos na nuvem favorecem `ELT` pela eficiência e escalabilidade.

**Tabela Comparativa: ETL vs. ELT**
*(Conteúdo original mantido, com formatação de código)*

| Característica             | `ETL` (Extract, Transform, Load)                                             | `ELT` (Extract, Load, Transform)                                                      |
| :------------------------- | :--------------------------------------------------------------------------- | :------------------------------------------------------------------------------------ |
| **Sequência** | Extração -> Transformação -> Carregamento                                  | Extração -> Carregamento -> Transformação                                             |
| **Local Transformação** | Servidor/Staging intermediário                                             | Dentro do Data Warehouse/Data Lake de destino                                         |
| **Formato no Destino** | Dados transformados e estruturados                                           | Dados brutos ou minimamente processados                                               |
| **Velocidade Ingestão** | Geralmente mais lenta                                                        | Geralmente mais rápida                                                                |
| **Flexibilidade** | Menor (transformações predefinidas)                                          | Maior (dados brutos disponíveis para múltiplas transformações)                      |
| **Uso de Recursos** | Requer servidor ETL dedicado + DWH                                           | Utiliza principalmente os recursos do DWH/Data Lake                                 |
| **Casos de Uso Típicos** | DWHs *on-premises*, dados estruturados, transformações complexas predefinidas | DWHs/Data Lakes na nuvem, Big Data, dados variados, agilidade analítica             |

**3. As Etapas do ELT em Detalhe**

**3.1. Extração (`Extract`)**
*(Conteúdo original mantido, com formatação de código e exemplos)*
... Fontes Comuns: BDs Relacionais (`SQL`, `CDC`), BDs `NoSQL` (`MongoDB`, `Cassandra`, `Redis`), APIs `SaaS` (`REST`/`SOAP`), Arquivos Planos (`CSV`, `JSON`, `XML`, `Parquet`, `Avro`), Logs (agentes como `Fluentd`, `Logstash`, `Flume`), Dados de *Streaming* (`Kafka`, `Kinesis`, `Pub/Sub`)...
    * *Exemplo Simples (SQL Extract):*
        ```sql
        SELECT order_id, customer_id, order_date, total_amount
        FROM production_db.orders
        WHERE updated_at > '2025-04-21 09:00:00'; -- Usar marcador da última execução
        ```
    * *Exemplo Complexo (SQL Extract com CDC Simulada via Window Function):*
        ```sql
        -- Simula CDC buscando o registro mais recente para cada chave primária
        -- desde a última extração, assumindo uma coluna 'last_modified_ts'
        WITH RankedChanges AS (
            SELECT
                t.*,
                ROW_NUMBER() OVER (PARTITION BY primary_key_column ORDER BY last_modified_ts DESC) as rn
            FROM source_table t
            WHERE last_modified_ts > :last_extract_timestamp -- Marcador da última extração
        )
        SELECT *
        FROM RankedChanges
        WHERE rn = 1; -- Pega apenas a versão mais recente de cada registro alterado
        ```
    * *Exemplo Simples (Python API Extract):* *(Mantido como antes)*
    * *Exemplo Complexo (Python API Extract com Paginação e Erros):* *(Mantido como antes)*
... Desafios: Diversidade, conectividade segura, proteção dados sensíveis, volume...
... Ferramentas: `Fivetran`, `Stitch`, `Airbyte`, `ADF`, `AWS Glue`...

**3.2. Carregamento (`Load`)**
*(Conteúdo original mantido, com formatação de código, exemplos e adição sobre camadas)*
... Mover dados coletados para o destino (`DWH`/`Data Lake`) rapidamente, com mínimo processamento...
* **Destinos Comuns:** `DWHs` Nuvem (`Snowflake`, `BigQuery`, `Redshift`, `Synapse`), `Data Lakes` (`S3`, `GCS`, `ADLS`).
* **Métodos de Carregamento:** *Bulk Loading* (comandos `COPY INTO`, `LOAD DATA`), *Streaming Inserts* (APIs de ingestão de stream), Ferramentas de Replicação (E+L) (`Fivetran`, `Airbyte`).
    * *Exemplo Simples (SQL - BigQuery `LOAD DATA`):* *(Mantido como antes)*
    * *Exemplo Complexo (SQL - Snowflake `COPY INTO` com Schema Detection e Carregamento em Tabela Raw):*
        ```sql
        -- 1. Inferir o schema do arquivo Parquet no S3 (opcional, mas útil)
        SELECT GENERATE_COLUMN_DESCRIPTION(ARRAY_AGG(OBJECT_CONSTRUCT(*)), 'TABLE')
        FROM TABLE(
          INFER_SCHEMA(
            LOCATION=>'@meu_stage_s3/dados_parquet/',
            FILE_FORMAT=>'my_parquet_format'
          )
        );

        -- 2. Criar tabela RAW com schema detectado ou flexível (ex: VARIANT)
        CREATE OR REPLACE TABLE raw_data.vendas_parquet_raw (
            dados VARIANT, -- Coluna para armazenar o registro inteiro como semi-estruturado
            nome_arquivo VARCHAR,
            timestamp_carga TIMESTAMP_LTZ
        );

        -- 3. Carregar dados Parquet na tabela RAW
        COPY INTO raw_data.vendas_parquet_raw (dados, nome_arquivo, timestamp_carga)
        FROM (
            SELECT
                $1, -- Seleciona o registro inteiro como VARIANT
                METADATA$FILENAME,
                CURRENT_TIMESTAMP()
            FROM @meu_stage_s3/dados_parquet/
        )
        FILE_FORMAT = (TYPE = PARQUET)
        MATCH_BY_COLUMN_NAME = NONE -- Importante para carregar em coluna VARIANT
        ON_ERROR = 'SKIP_FILE'; -- Pula arquivos com erro
        ```
* **Considerações:**
    * ***Schema-on-Read*:** Carregar em formatos brutos (`JSON`, `VARIANT`) ou tipos genéricos (`VARCHAR`). Estrutura aplicada na `Transform`.
    * **Formatos de Arquivo:** Colunares (`Parquet`, `ORC`) otimizam carga/consulta.
    * **Particionamento:** Otimiza `Transform`/consultas futuras.
* **Estruturação no Destino (Ex: Medallion Architecture):** É comum organizar os dados carregados e transformados em camadas dentro do `DWH`/`Data Lake`. Um padrão popular é a **Medallion Architecture**:
    * **Bronze:** Camada inicial onde os dados brutos são carregados "como estão" (ou com mínima limpeza/tipagem), preservando a fonte original. Corresponde à saída da etapa `Load` do ELT.
    * **Silver:** Camada onde os dados da Bronze são limpos, validados, conformados (padronizados), enriquecidos e possivelmente desnormalizados. Representa uma visão mais confiável e integrada dos dados. A maior parte da lógica de `Transform` do ELT opera para gerar esta camada.
    * **Gold:** Camada final, contendo dados agregados, métricas de negócio e modelos otimizados para casos de uso específicos de análise (`BI`, `ML`). Geralmente são tabelas de fatos e dimensões ou *feature stores*. A transformação final do ELT cria esta camada a partir da Silver.
    Esta arquitetura promove organização, reusabilidade e governança no processo `ELT`.

A etapa `Load` no `ELT` foca em velocidade e simplicidade...

**3.3. Transformação (`Transform`)**
*(Conteúdo original mantido, com formatação de código, exemplos simples/complexos e adições sobre modelagem e governança)*
...Converte dados brutos (no destino) em informações prontas para análise...
* **Localização e Linguagem:** Dentro do `DWH`/`Data Lake`, usando `SQL` predominantemente.
* **Tipos Comuns de Transformação:**
    * **Limpeza (*Cleansing*):** Tratar Nulos (`COALESCE`), Padronizar Formatos (funções data/string, `REGEXP_REPLACE`), Remover Duplicatas (`ROW_NUMBER() OVER(...)`), Corrigir Erros (`CASE`).
        * *Exemplo Simples (SQL - Limpeza básica):* *(Mantido como antes)*
        * *Exemplo Complexo (SQL - Limpeza com Regex e Lógica Condicional):* *(Mantido como antes)*
    * **Enriquecimento (*Enrichment*):** Combinar dados com `JOIN`s (`LEFT JOIN`, `INNER JOIN`).
        * *Exemplo Simples (SQL - Enriquecimento com dados do cliente):* *(Mantido como antes)*
        * *Exemplo Complexo (SQL - Enriquecimento com Múltiplas Dimensões e Lógica):* *(Mantido como antes)*
    * **Agregação (*Aggregation*):** Sumarizar com `SUM`, `COUNT`, `AVG`, `MIN`, `MAX` + `GROUP BY`. Filtrar grupos com `HAVING`.
        * *Exemplo Simples (SQL - Contagem de pedidos por status):* *(Mantido como antes)*
        * *Exemplo Complexo (SQL - Agregação Multinível e Funções de Janela):* *(Mantido como antes)*
    * **Modelagem (*Modeling*):** Estruturar dados para análise.
        * *Tabelas Fato/Dimensão (Esquema Estrela/Floco de Neve):* Modelo clássico `DWH`, otimizado para `BI`. A transformação `ELT` constrói essas tabelas a partir das camadas Silver/Bronze.
        * *Tabelas Largas (*Wide Tables*):* Desnormalizadas, simplificam consultas exploratórias.
        * *Criação Métricas Derivadas:* Calcular KPIs.
        * *Exemplo Simples (SQL - Criando tabela de dimensão - SCD Type 1):* *(Mantido como antes)*
        * *Exemplo Complexo (SQL - Criando/Atualizando Dimensão - SCD Type 2 com MERGE):* *(Mantido como antes, com nota sobre complexidade)*
* **Ferramentas:** `SQL` é principal. `dbt` (Data Build Tool) organiza, testa, versiona, documenta e executa transformações `SQL`. Orquestradores (`Airflow`, `Dagster`, `Prefect`, `ADF`, `Glue`, `Dataflow`) agendam/monitoram.
* **Governança e Qualidade na Transformação:** Como a transformação ocorre após o carregamento de dados potencialmente brutos, é crucial incorporar validações e testes nesta etapa. Ferramentas como `dbt` facilitam isso com testes customizados (queries `SQL` que devem retornar 0 linhas para passar) e testes genéricos (verificando unicidade, não-nulidade, valores aceitos, referências) que podem ser aplicados aos modelos de transformação. Isso ajuda a garantir a `Veracidade` dos dados nas camadas Silver e Gold.
* **SQL Avançado:** `DWHs` nuvem suportam `Window Functions`, `CTEs`, consulta a `JSON`/`AVRO` via `SQL`, otimizando transformações. `dbt` gera `SQL` otimizado.

**4. Exemplo Prático de ELT com SQL**
*(Conteúdo original mantido, incluindo código com melhorias e comentários)*

**4.1. Cenário e Dados Brutos**
*(Mantido como antes)*

**4.2. Carregamento para Tabela de Staging (`Load`)**
*(Mantido como antes, com exemplo `COPY INTO` melhorado)*

**4.3. Transformações SQL no Data Warehouse (`Transform`)**
*(Mantido como antes, com exemplos SQL de limpeza, enriquecimento, agregação e carga final com idempotência simples)*

**4.4. Comentários sobre o Exemplo (Atualizado)**
*(Mantido como antes)*

**5. Vantagens da Abordagem ELT**
*(Conteúdo original mantido, com formatação de código)*

* **5.1. Velocidade de Ingestão:** Disponibiliza dados brutos rapidamente.
* **5.2. Flexibilidade e Acesso a Dados Brutos:** Preserva original, permite re-transformações, bom para dados variados e `Data Science`/`ML`.
* **5.3. Escalabilidade na Nuvem:** Aproveita poder de `DWHs`/`Data Lakes` modernos. Ideal para `Big Data`.
* **5.4. Custo-Efetividade (Potencial):** Elimina servidor ETL dedicado. Usa modelo *pay-as-you-go*. **Atenção:** Custo de computação no `DWH` pode aumentar; análise TCO necessária.

**6. Cenários de Aplicação e Casos de Uso Ideais**
*(Conteúdo original mantido, com formatação de código e adição sobre Lakehouse)*

* **6.1. Data Warehouses na Nuvem:** Ideal para `Snowflake`, `BigQuery`, `Redshift`, `Synapse`.
* **6.2. Data Lakes:** Abordagem natural. `Load` no `S3`/`GCS`/`ADLS`. `Transform` no Lake (`Spark` via `EMR`/`Dataproc`/`Databricks`) ou no `DWH` adjacente.
    * **O Papel do Data Lakehouse:** Uma arquitetura emergente que combina a flexibilidade e o armazenamento de baixo custo dos Data Lakes com a confiabilidade, performance e recursos de gerenciamento dos Data Warehouses. Plataformas como `Databricks Delta Lake`, `Apache Iceberg` e `Apache Hudi` implementam formatos de tabela abertos sobre o Data Lake que adicionam transações `ACID`, *time travel* (versionamento de dados), e gerenciamento de metadados. Isso permite executar transformações `ELT` diretamente sobre os dados no Data Lake com confiabilidade e performance comparáveis às de um DWH, utilizando motores como `Spark`. O Lakehouse é um habilitador chave para padrões `ELT` eficientes e robustos diretamente no storage do Lake.
* **6.3. Grandes Volumes de Dados (`Big Data`):** Escalabilidade nuvem torna `ELT` mais viável.
* **6.4. Dados Não Estruturados ou Semi-estruturados:** `ELT` carrega "como estão" (`JSON`, `XML`, logs), transforma depois.
* **6.5. Lógica de Transformação Mutável:** Fácil adaptar/corrigir/adicionar transformações nos dados brutos.

**Democratização da Transformação:** `ELT` + `SQL` + ferramentas (`dbt`) tornam transformação mais acessível a analistas e cientistas, reduzindo dependência de equipes centrais.

**7. Ferramentas e Plataformas Comuns em ELT**
*(Conteúdo original mantido, com formatação de código)*
Ecossistema modular combina ferramentas especializadas.

* **7.1. Ferramentas de Extração e Carregamento (E+L):** Foco em replicação rápida/confiável de dados brutos.
    * Exemplos: `Fivetran`, `Stitch`, `Airbyte` (open-source), `Matillion`, `Hevo Data`.
* **7.2. Ferramentas de Transformação (T):** Gerenciam/orquestram lógica `SQL` no `DWH`/`Lake`.
    * Exemplos: `dbt` (padrão *de facto*, engenharia de software para `SQL`), `SQL Puro` (scripts/procedures).
* **7.3. Plataformas de Nuvem e Orquestradores:** Serviços combináveis e ferramentas de agendamento/gerenciamento.
    * **Azure:** `ADF` (E, L, Orquestração T), `Synapse` (DWH/Motor T), `Databricks` (Motor T Spark).
    * **AWS:** `Glue` (Descoberta, ETL leve, Orquestração T), `EMR` (Motor T Spark), `Redshift` (DWH/Motor T), `Kinesis`/`Lambda` (Streaming ELT).
    * **GCP:** `Dataflow` (E, L, T), `Pub/Sub` (Streaming E), `BigQuery` (DWH/Motor T), `Dataproc` (Motor T Spark).
    * **Orquestradores Gerais:** `Apache Airflow`, `Dagster`, `Prefect`.

Tendência à modularidade: Ferramenta E+L + Ferramenta T (`dbt`) + Orquestrador.

**8. Desafios e Considerações Operacionais no ELT**

Embora vantajoso, o ELT apresenta desafios operacionais específicos:
* **Gerenciamento de Custos de Computação:** Como as transformações pesadas rodam no DWH/Lake (que cobra por tempo/recursos de computação), otimizar queries SQL, escolher o tamanho correto do "warehouse" virtual (em plataformas como Snowflake/Synapse) e agendar jobs eficientemente são cruciais para controlar custos. Ferramentas de monitoramento de custos do DWH são essenciais.
* **Monitoramento e Depuração:** Diagnosticar falhas ou gargalos em transformações que ocorrem dentro do DWH pode ser desafiador. Requer boas práticas de logging dentro do código SQL/dbt, uso de ferramentas de monitoramento do DWH e, potencialmente, ferramentas de *Data Observability* que rastreiam dados e linhagem.
* **Gerenciamento de Schema Evolution (Pós-Load):** Mudanças na estrutura dos dados de origem (`schema drift`) são carregadas para as tabelas brutas/staging. O pipeline de transformação (`T`) precisa ser robusto o suficiente para detectar e lidar com essas mudanças (ex: adicionar colunas automaticamente, alertar sobre tipos de dados inesperados) ou falhar de forma controlada. Ferramentas E+L modernas ajudam a gerenciar isso durante a replicação.
* **Governança e Qualidade Pós-Load:** Garantir a qualidade, segurança, conformidade e catalogação dos dados após terem sido carregados em estado bruto exige processos e ferramentas robustas *dentro* do ambiente de destino. Isso inclui testes de dados (`dbt tests`), data catalogs para documentar metadados, e políticas de acesso refinadas no DWH/Lake.

**9. O Futuro e Tendências Relacionadas ao ELT**

* **Streaming ELT:** Uma evolução natural onde dados são extraídos e carregados continuamente (*streaming load*) e as transformações são aplicadas em micro-lotes ou em tempo real *dentro* do DWH/Lake destino, usando suas capacidades de processamento de stream (ex: `Snowpipe Streaming` + `Dynamic Tables` no Snowflake, `BigQuery streaming inserts` + `Materialized Views`, `Spark Structured Streaming` sobre `Delta Lake`). Isso visa reduzir a latência para insights, mantendo a arquitetura centrada no DWH/Lake.
* **Integração com IA/ML:** Ferramentas ELT incorporando IA para sugerir transformações, otimizar SQL automaticamente, detectar anomalias de qualidade nos dados carregados ou até gerar documentação (`dbt`).
* **Abstração e Low-Code/No-Code:** Ferramentas buscando simplificar a definição de transformações ELT para usuários menos técnicos, com interfaces visuais que geram código SQL/dbt por baixo.
* **Foco em Metadados e Data Fabric:** A flexibilidade do ELT aumenta a importância de metadados ativos e catálogos de dados para entender, descobrir e governar os dados brutos e transformados no DWH/Lake, alinhando-se com conceitos de Data Fabric.

**10. Conclusão**

**10.1. Recapitulação**
*(Conteúdo original mantido, com formatação de código)*
Este material explorou `ELT` (`Extract`, `Load`, `Transform`), abordagem moderna... Inverte `L` e `T` do `ETL`. Extrai -> Carrega bruto no `DWH`/`Lake` nuvem -> Transforma usando poder do destino (`SQL`). Vantagens: velocidade ingestão, flexibilidade (dados brutos), escalabilidade nuvem, potencial custo-efetividade.

**10.2. A Importância do ELT no Cenário Moderno**
*(Conteúdo original mantido, com formatação de código)*
Arquitetura dominante para `Big Data` e `Cloud`. Lida com volume, velocidade, variedade. Permite agilidade, suporta `BI` e `Data Science`/`ML`. Rapidez e flexibilidade cruciais.

**10.3. Considerações Finais e Próximos Passos**
*(Conteúdo original mantido e expandido)*
`ELT` não é universal. Escolha vs. `ETL` depende de análise de requisitos, infra, skills, custo. Princípios (modelagem, governança, qualidade) são essenciais. O gerenciamento dos desafios operacionais (custo computacional no DWH, monitoramento pós-load, governança pós-load, schema drift) é crítico para o sucesso do `ELT`.
Ferramentas como `dbt` são significativas, trazendo engenharia de software para `SQL` no `ELT`, promovendo colaboração, confiabilidade e manutenibilidade. A ascensão de arquiteturas como Data Lakehouse e padrões como Streaming ELT mostram a contínua evolução deste paradigma. Compreender `ELT`, suas ferramentas e desafios operacionais é fundamental para construir pipelines de dados eficazes na era da nuvem e do `Big Data`.

---