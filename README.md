# Sales Performance Brasil

Este repositório abriga o projeto final do curso de MBA de Engenharia de Dados ministrado na Universidade Presbiteriana Mackenzie. 
É obrigatório para obtenção parcial da nota da matéria de *Hands-On* - Engenharia de Dados em *Big Data*

# Sobre a Empresa

A Sales Performance Brasil é uma empresa de vanguarda, especializada em oferecer soluções sofisticadas nas áreas de engenharia e ciência de dados. Nosso objetivo é fornecer um serviço abrangente aos clientes, que vai desde o provisionamento de arquiteturas em nuvem até a realização de análises avançadas de dados, passando por todos os aspectos de governança, documentação, ADR e pipelines. Nosso trabalho é focado em solucionar desafios complexos e transformar dados em insights valiosos e aplicáveis. Contamos com uma equipe de especialistas dedicados e apaixonados por dados, oferecendo soluções sob medida para diferentes setores da indústria.

## Serviços Oferecidos:
- Governança de Dados: implementamos processos e boas práticas de governança através de consultoria especializada, garantindo a documentação e a segurança das informações de forma adequada.
- Consultoria em Arquitetura Baseada em Dados: desenvolvemos soluções em nuvem para aprimorar a tomada de decisões da sua empresa com base em dados, promovendo eficiência e agilidade.
- Análise Avançada de Dados: aplicamos as ferramentas mais avançadas e bem avaliadas no quadrante mágico do Gartner para extrair e apresentar insights valiosos a partir de grandes volumes de dados, tanto estruturados quanto não estruturados.
- Desenvolvimento de Modelos de Machine Learning: criamos modelos preditivos e analíticos personalizados para melhorar a eficiência dos processos e apoiar decisões mais assertivas.

# Definição do Projeto

Fomos contratados pela empresa **Olist** que atua no _e-commerce_ brasileiro, no setor de vendas focado no público varejo. Em suma, o **Olist** é a maior loja de departamentos do mercado brasileiro e conecta pequenas empresas de todo o Brasil aos canais sem complicações e com um único contrato. Esses comerciantes podem vender seus produtos por meio da **Olist Store** e enviá-los diretamente aos clientes usando os parceiros logísticos do Olist.  

O projeto consiste em levar uma de suas bases de dados para o ambiente _cloud_ e nos passaram algumas tabelas com informações sobre clientes, vendedores e produtos vendidos, vinculando data e geolocalização da venda (mais detalhes serão abordados a seguir). Nosso projeto consiste em dois principais objetivos:

1. Desenvolvimento de uma arquitetura Lambda em nuvem a partir de uma modelagem Wide Table.
2. Seguindo os critérios de segurança, garantir toda a documentação do projeto e seu versionamento em GitHub.

Todo o projeto será dividido em *sprints* com datas de duração variável de acordo com a necessidade do projeto.

# Planejamento das Sprints
## Sprint 1

**Duração**: 2 dias

**Objetivos**:
- Definição do Problema: consultoria a empresa **Olist** na ambientação em _Cloud_ de sua base de dados de vendas.
- Escolha da base de dados: a base de dados foi fornecida pela empresa. Consiste em 8 tabelas de dados estruturados, com a seguinte estrutura:

  ![Base de Dados Olist](https://i.imgur.com/HRhd2Y0.png)

  Para entendermos melhor a estrutura deste banco, segue um resumo dos metadados:
1. **olist_customers_dataset.csv**: este conjunto de dados contém informações sobre o cliente e sua localização.

| Coluna                   | Descrição                                                                            |
|--------------------------|--------------------------------------------------------------------------------------|
| customer_id              | Chave para o conjunto de dados de pedidos. Cada pedido tem um customer_id exclusivo. |
| customer_unique_id       | Identificador exclusivo de um cliente.                                               |
| customer_zip_code_prefix | Primeiros cinco dígitos do CEP do cliente.                                           |
| custumer_city            | Nome da cidade do cliente.                                                           |
| customer_state           | Estado do cliente.                                                                   |

2. **olist_geolocation_dataset.csv**: este conjunto de dados possui informações de CEPs brasileiros e suas coordenadas latitude/longitude (pode ser usado pela equipe de BI para mapas de vendas).

| Coluna                      | Descrição                   |
|-----------------------------|-----------------------------|
| geolocation_zip_code_prefix | Primeiros 5 dígitos do CEP. |
| geolocation_lat             | Latitude.                   |
| geolocation_lng             | Longitude.                  |
| geolocation_city            | Nome da cidade.             |
| geolocation_state           | Nome do estado              |

3. **olist_order_items_dataset.csv**: este conjunto de dados inclui dados sobre os itens comprados em cada pedido.

| Coluna                        | Descrição                                                                                                    |
|-------------------------------|--------------------------------------------------------------------------------------------------------------|
| order_id                      | Identificador exclusivo do pedido.                                                                           |
| order_item_id                 | Número sequencial que identifica o número de itens incluídos no mesmo pedido.                                |
| product_id                    | Identificador exclusivo do produto.                                                                          |
| seller_id                     | Identificador exclusivo do vendedor.                                                                         |
| shipping_limit_date           | Mostra a data limite de envio do vendedor para processar o pedido para o parceiro logístico.                 |
| preco                         | Preço do Item.                                                                                               |
| freight_value                 | Valor do frete por item (se um pedido tiver mais de um item, o valor do frete será dividido entre os itens). |

4. **olist_order_payments_dataset.csv**: este conjunto de dados inclui dados sobre os itens comprados em cada pedido.

| Coluna              | Descrição                                                                                                         |
|---------------------|-------------------------------------------------------------------------------------------------------------------|
| order_id            | Identificador exclusivo do pedido.                                                                                |
| payment_sequential  | Um cliente pode pagar um pedido com mais de um método de pagamento. Se ele fizer isso, uma sequência será criada. |
| payment_type        | Método de pagamento escolhido pelo cliente.                                                                       |
| payment_installment | Número de parcelas escolhidas pelo cliente.                                                                       |
| payment_value       | Valor da transação.                                                                                               |

5. **olist_order_reviews_dataset.csv**: depois que um cliente compra o produto na Olist Store, um vendedor é notificado para atender a esse pedido. Assim que o cliente recebe o produto, ou a data estimada de entrega é devida, o cliente recebe uma pesquisa de satisfação por e-mail onde pode dar uma nota para a experiência de compra e anotar alguns comentários - NPS do serviço:

| Coluna                  | Descrição                                                            |
|-------------------------|----------------------------------------------------------------------|
| review_id               | Identificador exclusivo do review.                                   |
| order_id                | Identificador exclusivo do pedido.                                   |
| review_score            | Nota de vai de 0 a 5 dada pelo cliente na pesquisa de satisfação.    |
| review_comment_title    | Título do comentário deixado pelo cliente na pesquisa, em português. |
| review_comment_message  | Comentário deixado pelo cliente na pesquisa, em português.           |
| review_creation_date    | Data na qual a pesquisa foi enviada pelo cliente.                    |
| review_comment_message  | Comentário deixado pelo cliente na pesquisa, em português.           |
| review_answer_timestamp | Mostra o carimbo de data/hora da resposta da pesquisa de satisfação. |

6. **olist_products_dataset.csv**: este conjunto de dados inclui dados sobre os produtos vendidos pelo Olist.

| Coluna                     | Descrição                                               |
|----------------------------|---------------------------------------------------------|
| product_id                 | Identificador exclusivo do produto.                     |
| product_category_name      | Categoria raiz do produto, em português.                |
| product_name_lenght        | Número de caracteres extraídos do nome do produto.      |
| product_description_lenght | Número de caracteres extraídos da descrição do produto. |
| product_photos_qty         | Número de fotos publicadas do produto.                  |
| product_weight_g           | Peso do produto medido em gramas.                       |
| product_lenght_cm          | Comprimento do produto medido em centímetros.           |
| product_height_cm          | Altura do produto medida em centímetros.                |
| product_width_cm           | Largura do produto medida em centímetros.               |

7. **olist_sellers_dataset.csv**: este conjunto de dados inclui dados sobre os vendedores que atenderam aos pedidos feitos no Olist.

| Coluna                 | Descrição                               |
|------------------------|-----------------------------------------|
| seller_id              | Identificador exclusivo do vendedor.    |
| seller_zip_code_prefix | Primeiros 5 dígitos do CEP do vendedor. |
| seller_city            | Nome da cidade do vendedor.             |
| seller_state           | Estado do vendedor.                     |

8. **product_category_name_translation.csv**: esta tabela traduz a coluna _product_category_name_ da tabela _olist_products_dataset.csv_ para o inglês.

| Coluna                        | Descrição                                   |
|-------------------------------|---------------------------------------------|
| product_category_name         | Nome da categoria do produto, em português. |
| product_category_name_english | Nome da categoria do produto, em inglês.    |

9. **olist_order_dataset.csv**: esta é a tabela principal, de onde todas as outras se originam.

| Coluna                        | Descrição                                                                                      |
|-------------------------------|------------------------------------------------------------------------------------------------|
| order_id                      | Identificador exclusivo do pedido.                                                             |
| customer_id                   | Chave para o conjunto de dados de pedidos. Cada pedido tem um customer_id exclusivo.           |
| order_status                  | Referência ao status do pedido (entregue, enviado, etc).                                       |
| order_purchase_timestamp      | Mostra o carimbo de data/hora da compra.                                                       |
| order_approved_at             | Mostra o carimbo de data/hora de aprovação do pagamento.                                       |
| order_delivered_carrier_date  | Mostra o carimbo de data/hora de postagem da ordem. Quando foi entregue ao parceiro logístico. |
| order_delivered_customer_date | Mostra a data real de entrega do pedido ao cliente.                                            |
| order_estimated_delivery_date | Mostra a data estimada de entrega que foi informada ao cliente no momento da compra.           |

- Desenho da Arquitetura: foi desenvolvido uma arquitetura em lambda, um padrão de design popular para processamento de grandes volumes de dados, especialmente em aplicações de Big Data. Essa arquitetura combina o processamento em lote (batch) com o processamento em tempo real para oferecer uma visão completa e consistente dos dados. Sua representação foi feita via draw.io. O desenho segue: ![Arquitetura do Projeto](https://github.com/matheusvieger/Sales_Performance_Brasil/blob/main/Arquitetura_Stack_Ferramentas.png?raw=true)
- Escolha Stack de Ferramentas: será usado o ambiente da AWS para desenvolvimento da arquitetura, então toda a estrutura do projeto será usada no ambiente provido pela Amazon. Segue relação de ferramentas que serão usadas:
  - **_Data Ingest_**: para enviar os dados ao *Batch Layer* será usado o AWS Glue; para o *Speed Layer*, AWS Kinesis.
  - **_Batch Layer_**: neste acesso, usaremos a metodologia *Medallion Architecture*, construída a parte do AWS S3 e, para cada etapa da transformação, será usado o AWS Glue.
  - **_Speed Layer_**: entendendo a agilidade que essa camada precisa, será usado o AWS Lambda como serviço *serverless* para que, assim que seja recebido um novo dado, ele será diretamente armazenado no AWS S3.
  - **_Serving Layer_**: esta camada, responsável pelas consultas, usaremos a AWS Athena como solução.
  - **_Consumer_**: as equipes de análise e ciências de dados poderão usar as ferramentas de sua preferência. Nossa recomendação é usar o Power Bi para criação de *dashboards* inteligentes seguindo o modelo de negócio e o AWS Sagemaker para construção de modelos preditivos.
  - **_Policies e Governance_**: algumas ferramentas serão usados nessa camada.
    - Calculadora dos custos de manutenção de toda a arquitetura: AWS Pricing Calculator.
    - Monitoramento da Arquitetura: Amazon CloudWatch.
    - Controle de Acessos: AWS Secrets Manager.
    - Versionamento, documentação e gerenciamento do projeto: Github.

## Sprint 2

**Duração**: 7 dias

**Objetivos**:
- Criação de Kanban (Tarefas e Responsáveis): para realização desta estrutura, responsabilizada pela Angélica, foi criado uma base de card e tarefas, com prazos, na plataforma do Trello com as seguintes listas de tarefas:
    - **Backlog**: todas as atividades a serem realizadas a pedido do cliente;
    - **A fazer**: as atividades que ainda não foram iniciadas, com a marcação do seu responsável;
    - **Em andamento**: quando o(s) responsável(is) já iniciaram a tratativa;
    - **Revisão do código**: quando uma tarefa exige a criação de um código, como uma ETL, haverá sempre dois responsáveis para que um sempre valide o código criado. Esta etapa mostra que o segundo responsável estará validando a estrutura criada pelo colega;
    - **Fase de Teste**: após a revisão do código, ele deve ser testado e validado para entendermos sua usabilidade, facilidade e eficiência. Esta etapa designa este momento.
    - **Concluído**: quando uma atividade for finalizada, o *card* será enviado a esta etapa para entenrdermos o que já está finalizado, a agilidade da entrega e quando podemos iniciar uma nova etapa.

- Modelagem das Camadas de Dados: esta tarefa foi realizada pelo Gustavo e Fábio. Os participantes fizeram o desenvolvimento do projeto em sistema AWS. Abaixo, algumas imagens do projeto realizado dentro do ambimente:
    - Datalake:
![Datalake](https://github.com/matheusvieger/Sales_Performance_Brasil/blob/main/Modelagem/DL.jpg)

    - Camada Bronze:
![Bronze](https://github.com/matheusvieger/Sales_Performance_Brasil/blob/main/Modelagem/Brz.jpg)

    - Camada Silver:
![Silver](https://github.com/matheusvieger/Sales_Performance_Brasil/blob/main/Modelagem/Slv.jpg)

    - Camada Gold:
![Gold](https://github.com/matheusvieger/Sales_Performance_Brasil/blob/main/Modelagem/Gld.jpg)


## Sprint 3

**Duração**: 7 dias

# MVP - Sales Performance Brasil

## Objetivo
Desenvolver uma arquitetura de dados em nuvem utilizando AWS S3 e AWS Glue, focada nas camadas Bronze, Silver e Gold, para processar e analisar os dados da Olist.

## Estrutura do Projeto

### 1. Camada Bronze
**Objetivo:** Armazenar dados brutos exatamente como são recebidos.

- **Ingestão de Dados:**
  - AWS Glue para ingestão em lote.
- **Armazenamento:**
  - Dados brutos armazenados no bucket S3 Bronze.

### 2. Camada Silver
**Objetivo:** Limpar e transformar os dados brutos para um formato mais estruturado.

- **Transformação:**
  - Jobs do AWS Glue para limpar e transformar os dados da camada Bronze.
- **Armazenamento:**
  - Dados transformados armazenados no bucket S3 Silver.

### 3. Camada Gold
**Objetivo:** Otimizar os dados para análise, agregando e enriquecendo as informações.

- **Transformação Avançada:**
  - Jobs do AWS Glue para agregar e enriquecer os dados da camada Silver.
- **Armazenamento:**
  - Dados otimizados armazenados no bucket S3 Gold.


  ## Planner MVP

### 01/12/2024: Configuração Inicial e Ingestão de Dados
**Responsáveis:** Gustavo e Fabio

- **Configuração do Ambiente AWS:**
  - Criação de buckets S3 para as camadas Bronze, Silver e Gold.

- **Ingestão de Dados Brutos (Camada Bronze):**
  - Configuração de jobs no AWS Glue para ingestão em lote.
  - Armazenamento dos dados brutos no bucket S3 Bronze.

### 02/12/2024: Transformação de Dados (Camada Silver)
**Responsáveis:** Gustavo e Fabio

- **Transformação Inicial:**
  - Criação de jobs no AWS Glue para limpar e transformar os dados da camada Bronze.
  - Remoção de duplicatas e correção de erros.
  - Armazenamento dos dados transformados no bucket S3 Silver.

- **Validação dos Dados Silver:**
  - Verificação da integridade e qualidade dos dados transformados.
  - Ajustes necessários nos jobs do AWS Glue.

### 03/12/2024: Otimização e Enriquecimento de Dados (Camada Gold)
**Responsáveis:** Gustavo e Fabio

- **Transformação Avançada:**
  - Criação de jobs no AWS Glue para agregar e enriquecer os dados da camada Silver.
  - Realização de cálculos e agregações.
  - Armazenamento dos dados otimizados no bucket S3 Gold.

- **Validação dos Dados Gold:**
  - Verificação da integridade e qualidade dos dados otimizados.
  - Ajustes necessários nos jobs do AWS Glue.


## Sprint 4 e 5

**Duração**: 2 dias

## 1. Análise e Design
- **Análise de Requisitos:** Identificar dados e objetivos de cada camada.
- **Desenho da Arquitetura:** Criar diagramas de fluxo de dados e definir estrutura dos buckets S3.
- **Modelagem Wide Table:** Planejar a estrutura da Wide Table para consolidar dados de várias fontes.

## 2. Configuração
- **Configuração do Ambiente AWS:**
  - Criar buckets S3 para Bronze, Silver e Gold.
  - Configurar AWS Glue.
- **Segurança e Governança:**
  - Implementar políticas de segurança nos buckets S3.


## 3. Desenvolvimento
- **Jobs do AWS Glue:**
  - **Camada Bronze:** Ingestão de dados brutos e armazenamento no bucket S3 Bronze.
  - **Camada Silver:** Limpeza e transformação dos dados da camada Bronze e armazenamento no bucket S3 Silver.
  - **Camada Gold:** Agregação e enriquecimento dos dados da camada Silver e armazenamento no bucket S3 Gold.
- **Wide Table:**
  - Consolidar dados de várias tabelas em uma Wide Table na camada Gold.
- **Batch Layer:**
  - Implementar a Batch Layer usando AWS Glue para processar grandes volumes de dados periodicamente.
- **Scripts de Transformação:** Escrever e testar scripts em PySpark.

## 4. Teste e Validação
- **Teste de Ingestão:** Verificar armazenamento correto na camada Bronze.
- **Teste de Transformação:** Validar limpeza e transformação na camada Silver e otimização na camada Gold.
- **Validação da Wide Table:** Garantir que a Wide Table consolida corretamente os dados.
- **Validação da Batch Layer:** Testar o processamento em lote para garantir eficiência.
- **Validação de Qualidade:** Verificar integridade e qualidade dos dados.

## Ferramentas Utilizadas
- **AWS Glue:** Ingestão e transformação.
- **AWS S3:** Armazenamento.


### Script AWS Glue para Criar a Wide Table

```python
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job

args = getResolvedOptions(sys.argv, ['JOB_NAME'])
sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session
job = Job(glueContext)
job.init(args['JOB_NAME'], args)

# Carregar dados da camada Silver
customers = glueContext.create_dynamic_frame.from_catalog(database = "olist_db", table_name = "silver_customers")
orders = glueContext.create_dynamic_frame.from_catalog(database = "olist_db", table_name = "silver_orders")
products = glueContext.create_dynamic_frame.from_catalog(database = "olist_db", table_name = "silver_products")
sellers = glueContext.create_dynamic_frame.from_catalog(database = "olist_db", table_name = "silver_sellers")
payments = glueContext.create_dynamic_frame.from_catalog(database = "olist_db", table_name = "silver_payments")
reviews = glueContext.create_dynamic_frame.from_catalog(database = "olist_db", table_name = "silver_reviews")

# Unir dados para criar a Wide Table
wide_table = Join.apply(orders, customers, 'customer_id', 'customer_id')
wide_table = Join.apply(wide_table, products, 'product_id', 'product_id')
wide_table = Join.apply(wide_table, sellers, 'seller_id', 'seller_id')
wide_table = Join.apply(wide_table, payments, 'order_id', 'order_id')
wide_table = Join.apply(wide_table, reviews, 'order_id', 'order_id')

# Salvar a Wide Table na camada Gold
glueContext.write_dynamic_frame.from_options(frame = wide_table, connection_type = "s3", connection_options = {"path": "s3://bucket-gold/wide_table/"}, format = "parquet")

job.commit()








