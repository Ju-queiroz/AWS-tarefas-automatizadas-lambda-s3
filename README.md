# AWS-tarefas-automatizadas-lambda-s3
# 📘 Resumo e Anotações do Módulo — AWS (S3, Lambda, DynamoDB e LocalStack)

---

## ☁️ Entendendo o Amazon S3
- O **Amazon S3 (Simple Storage Service)** é um serviço de **armazenamento de objetos**.  
- Permite guardar arquivos (imagens, vídeos, documentos, logs, backups, etc.) de forma **escalável, segura e durável**.  
- Cada arquivo armazenado é chamado de **objeto**, e fica dentro de um **bucket**.  
- Principais características:  
  - **Alta durabilidade (99,999999999%)**.  
  - **Controle de acesso** com políticas (IAM, ACLs, Bucket Policies).  
  - Suporte a **ciclo de vida** (Lifecycle), para mover ou excluir dados automaticamente.  
  - Integração com diversos serviços AWS (Lambda, CloudFront, Athena, etc.).  

---

## ⚡ Entendendo o AWS Lambda
- O **AWS Lambda** é um serviço de **computação serverless**, que executa código sem necessidade de gerenciar servidores.  
- Principais pontos:  
  - Suporta várias linguagens (Python, Node.js, Java, Go, etc.).  
  - É executado em resposta a **eventos** (ex.: upload no S3, inserção no DynamoDB, chamada HTTP via API Gateway).  
  - Cobra apenas pelo tempo de execução e quantidade de requisições.  
  - Escalabilidade automática → múltiplas funções podem rodar em paralelo.  
- Uso comum:  
  - Processar arquivos.  
  - Automatizar tarefas.  
  - Construir APIs serverless.  

---

## 📂 Upload de Arquivos + DynamoDB
- Fluxo prático comum:  
  1. Usuário faz **upload de arquivo** no bucket S3.  
  2. Esse evento dispara uma **função Lambda**.  
  3. A Lambda processa o arquivo (ex.: extrair dados, gerar logs).  
  4. Os metadados ou registros são salvos em uma tabela **DynamoDB**.  
- O **Amazon DynamoDB** é um banco de dados **NoSQL totalmente gerenciado**, com alta performance, escalabilidade e baixa latência.  
- Benefícios dessa integração:  
  - Automatização do pipeline de dados.  
  - Persistência das informações processadas.  
  - Desacoplamento de componentes (S3 → Lambda → DynamoDB).  

---

## 🛠️ Configurando AWS localmente com LocalStack
- O **LocalStack** é uma ferramenta que emula serviços da AWS localmente.  
- Permite testar aplicações **sem custos** e **sem precisar de internet**.  
- Serviços suportados: S3, Lambda, DynamoDB, API Gateway, SQS, entre outros.  
- Passos básicos:  
  1. Instalar LocalStack (via pip ou Docker).  
  2. Configurar a AWS CLI para apontar para o endpoint do LocalStack.  
  3. Criar buckets, tabelas, funções etc. localmente, simulando a nuvem.  
- Benefícios:  
  - Desenvolvimento rápido.  
  - Testes locais antes de subir para AWS real.  
  - Integração em pipelines de CI/CD.  

---

## 🔨 Criando os Recursos
- Exemplo prático com LocalStack:  
  - Criar um bucket S3:  
    ```bash
    awslocal s3 mb s3://meu-bucket-local
    ```
  - Criar uma tabela DynamoDB:  
    ```bash
    awslocal dynamodb create-table \
      --table-name Arquivos \
      --attribute-definitions AttributeName=ID,AttributeType=S \
      --key-schema AttributeName=ID,KeyType=HASH \
      --billing-mode PAY_PER_REQUEST
    ```
  - Criar uma função Lambda de teste:  
    ```bash
    awslocal lambda create-function \
      --function-name HelloWorld \
      --runtime python3.9 \
      --role arn:aws:iam::000000000000:role/lambda-role \
      --handler lambda_function.lambda_handler \
      --zip-file fileb://function.zip
    ```

---
