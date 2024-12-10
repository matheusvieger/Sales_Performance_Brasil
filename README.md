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

1. Desenvolvimento de uma arquitetura Lambda em nuvem a partir de uma modelagem Wide Table.
2. Seguindo os critérios de segurança, garantir toda a documentação do projeto e seu versionamento em GitHub.

Todo o projeto será dividido em *sprints* com datas de duração variável de acordo com a necessidade do projeto.

# Planejamento das Sprints
## Sprint 1

**Duração**: 2 dias

**Objetivos**:
- Definição do Problema:
- Escolha da base de dados:
- Desenho da Arquitetura: foi desenvolvido uma arquitetura em lambda e sua representação foi feita via draw.io. O desenho pode ser consultado acessando a seguir [ADICIONAR UM LINK PARA TER ACESSO AO DRAW.IO]
- Escolha Stack de Ferramentas: será usado o ambiente da AWS para desenvolvimento da arquitetura. Segue relação de ferramentas que serão usadas:
  - **_Data Ingest_**: para enviar os dados ao *Batch Layer* será usado o AWS Glue; para o *Speed Layer*, AWS Kinesis.
  - **_Batch Layer_**: neste acesso, usaremos a metodologia *Medallion Architecture*, construída a parte do AWS S3 e, para cada etapa da transformação, será usado o AWS Glue.
  - **_Speed Layer_**: entendendo a agilidade que essa camada precisa, será usado o AWS Lambda como serviço *serverless* para que, assim que seja recebido um novo dado, ele será diretamente armazenado no AWS S3.
  - **_Serving Layer_**: esta camada, responsável pelas consultas, usaremos a AWS Athena como solução.
  - **_Consumer_**: as equipes de análise e ciências de dados poderão usar as ferramentas de sua preferência. Nossa recomendação é usar o Power Bi para criação de *dashboards* inteligentes seguindo o modelo de negócio e o AWS Sagemaker para construção de modelos preditivos.
  - **_Policies e Governance_**: algumas ferramentas serão usados nessa camada.
    - Calculadora dos custos de manutenção de toda a arquitetura: AWS Pricing Calculator.
    - Monitoramento da Arquitetura: Amazon CloudWatch.
    - 
