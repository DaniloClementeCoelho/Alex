##### IMPORTANTE : SEMPRE RODAR 
terraform destroy
NO FINAL PARA EVITAR CUSTOS NA AWS 

------- GUIA DE CUSTO FEITO PELO CODEX --------

O que mais merece atenção
Os maiores riscos de custo no projeto costumam ser:

Aurora Serverless v2
App Runner
SageMaker endpoint
chamadas de modelo no Bedrock
CloudWatch logs/monitoring acumulando com o tempo
Por guia

Guide 2 - SageMaker

Recurso principal: endpoint de embeddings
Risco de custo: médio
terraform destroy em terraform/2_sagemaker:
normalmente remove o endpoint/model/config criados por esse diretório
Verificar no console:
SageMaker > Endpoints
Se continuar existindo, pode continuar gerando custo
Guide 3 - Ingestion

Recursos principais:
Lambda
API Gateway
IAM relacionados
S3 Vectors bucket/index
Risco de custo:
Lambda/API Gateway: baixo, se sem uso
S3 Vectors: depende de armazenamento/uso
terraform destroy em terraform/3_ingestion:
geralmente remove Lambda/API Gateway/IAM
provavelmente não remove o S3 Vectors se ele foi criado manualmente
Verificar no console:
S3 > Vector buckets
Guide 4 - Researcher

Recursos principais:
App Runner
ECR
scheduler opcional
Risco de custo: alto/moderado
terraform destroy em terraform/4_researcher:
deve remover infraestrutura criada ali
Verificar no console:
App Runner
EventBridge
Lambda do scheduler
ECR
App Runner é um dos pontos que eu mais checaria
Guide 5 - Database

Recurso principal:
Aurora Serverless v2
Risco de custo: alto
terraform destroy em terraform/5_database:
esse é o mais importante para economizar
Verificar no console:
RDS > Databases / Clusters
Secrets Manager
Se o Aurora continuar lá, o custo continua sendo relevante
Guide 6 - Agents

Recursos principais:
5 Lambdas
SQS
S3 de pacotes
CloudWatch logs
Risco de custo: geralmente baixo a moderado
terraform destroy em terraform/6_agents:
remove a maior parte desses recursos
Verificar no console:
Lambda
SQS
S3
CloudWatch log groups
Guide 7 - Frontend

Recursos principais:
S3 site bucket
CloudFront
API Gateway
Lambda API
Risco de custo: baixo a moderado
terraform destroy em terraform/7_frontend:
deve remover esses recursos
Verificar no console:
CloudFront
S3
API Gateway
Lambda
Guide 8 - Enterprise

Recursos principais:
dashboards
alarms
observability extras
Risco de custo: normalmente baixo, mas pode crescer com logs/monitoring
Verificar no console:
CloudWatch
X-Ray / integrações extras, se usadas
LangFuse não é AWS, mas pode ter custo próprio
O que pode ficar mesmo depois de terraform destroy
Esses são os itens para suspeitar:

recursos criados manualmente no console
S3 Vectors bucket/index
secrets/tokens em outros serviços
imagens em ECR
logs no CloudWatch
snapshots/backups eventualmente preservados
qualquer recurso que falhou ao destruir e ficou órfão
Ordem prática para zerar custo quase todo
Se você quiser pausar o projeto por um tempo, eu faria a checagem nesta ordem:

terraform/5_database

maior impacto: Aurora
terraform/4_researcher

App Runner e scheduler
terraform/2_sagemaker

endpoint de embeddings
terraform/6_agents

Lambdas e SQS
terraform/7_frontend

CloudFront, API, site
terraform/3_ingestion

Lambda/API da ingestão
checagem manual do S3 Vectors

bucket e index
Checklist final no console AWS
Depois de destruir, confira manualmente:

RDS
SageMaker Endpoints
App Runner
Lambda
API Gateway
CloudFront
SQS
ECR
S3
S3 Vector buckets
CloudWatch Logs
Bedrock usage/costs
Billing / Cost Explorer
Resumo mais importante
Se você quiser economizar rápido:

destrua primeiro Aurora
depois App Runner
depois SageMaker
e lembre que S3 Vectors pode precisar de limpeza manual

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Alex - the Agentic Learning Equities Explainer

## Multi-agent Enterprise-Grade SaaS Financial Planner

![Course Image](assets/alex.png)

_If you're looking at this in Cursor, please right click on the filename in the Explorer on the left, and select "Open preview", to view it in formatted glory._

### Welcome to The Capstone Project for Week 3 and Week 4!

#### The directories:

1. **guides** - this is where you will live - step by step guides to deploy to production
2. **backend** - the agent code, organized into subdirectories, each a uv project (as is the backend parent directory)
3. **frontend** - a NextJS React frontend integrated with Clerk
4. **terraform** - separate terraform subdirectories with state for each part
5. **scripts** - the final deployment script

#### Order of play:

##### Week 3

- On Week 3 Day 3, we will do 1_permissions and 2_sagemaker
- On Week 3 Day 4, we will do 3_ingest
- On Week 3 Day 5, we will do 4_researcher

##### Week 4

- On Week 4 Day 1, we will do 5_database
- On Week 4 Day 2, we will do 6_agents
- On Week 4 Day 3, we will do 7_frontend
- On Week 4 Day 4, we will do 8_enterprise

#### Keep in mind

- Please submit your community_contributions, including links to your repos, in the production repo community_contributions folder
- Regularly do a git pull to get the latest code
- Reach out in Udemy or email (ed@edwarddonner.com) if I can help! This is a gigantic project and I am here to help you deliver it!