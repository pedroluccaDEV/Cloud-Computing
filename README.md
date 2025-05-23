﻿# ☁️ Cloud Computing Portfolio – Pedro

Este repositório documenta minha jornada prática com **serviços de computação em nuvem**, com foco inicial em recursos da **AWS (Amazon Web Services)**. A ideia é manter uma organização por **categorias de serviços**, facilitando a manutenção e expansão do portfólio com novas experiências.

---

## 📁 Organização

Cada subdiretório contém um projeto real com exemplos práticos, código-fonte e documentação individual (`README.md`).

---

## 🔧 Categorias de Serviços AWS

### ⚙️ 1. Compute

#### 🔸 AWS Lambda

- Funções serverless criadas para manipular dados, processar requisições e integrar com outros serviços AWS.
- Utilização com API Gateway para expor rotas HTTP.
- Exemplo: Função para cadastrar e listar treinos com DynamoDB.

#### 🔸 Amazon EC2

- Lançamento e configuração de instâncias Linux.
- Acesso via SSH.
- Instalação de servidores e testes manuais de backend.

---

### 🗃️ 2. Storage

#### 🔸 Amazon S3 (Simple Storage Service)

- Hospedagem de arquivos estáticos (ex: sites em React).
- Criação e configuração de buckets.
- Políticas de acesso público e configuração CORS.

#### 🔸 Amazon DynamoDB

- Banco de dados NoSQL para armazenar dados estruturados.
- Utilizado em conjunto com Lambda e API Gateway.
- Experiência com tipos complexos como listas de objetos e valores `Decimal`.

---

### 🌐 3. Networking / API Management

#### 🔸 Amazon API Gateway

- Criação de APIs HTTP e REST para integração com serviços Lambda.
- Experiência com as duas abordagens:
  - **HTTP API**: mais simples, rápida de configurar, ideal para serverless.
  - **REST API**: mais completa, mas com interface mais complexa e mais passos.
- Aprendizado: HTTP API foi mais adequada para uso com Lambda + Dynamo.

---

## 💡 Projeto em Destaque

### 📋 API de Treinos com AWS Lambda + API Gateway + DynamoDB

**Objetivo:** Criar uma API serverless para registrar treinos de usuários.

**Funcionalidades:**
- `POST /workouts`: cadastra um novo treino com data, tipo e exercícios
- `GET /workouts`: retorna todos os treinos

**Serviços utilizados:**
- Lambda (backend)
- API Gateway (exposição de rotas)
- DynamoDB (persistência)

**Aprendizados:**
- Serialização com `Decimal` no DynamoDB
- Geração de UUIDs únicos
- Integração do Lambda com HTTP API
- Testes com Postman usando payload correto no body

---

## 🧠 Aprendizados Gerais

- Diferenças práticas entre REST API e HTTP API no API Gateway
- Manipulação de dados complexos no DynamoDB
- Deploy manual de funções Lambda e testes diretos
- Criação de políticas públicas de acesso em buckets S3
- Acesso a instâncias EC2 via SSH e controle de portas
- Debug de integrações usando o CloudWatch

---

## 📌 Próximos Passos

- Aprender e usar:
  - IAM e roles customizadas
  - RDS (banco relacional gerenciado)
  - VPC (configuração de redes privadas)
  - CloudFormation ou Terraform (infraestrutura como código)
  - Step Functions e SQS (orquestração e filas)
  - Monitoramento com CloudWatch em produção

---

## 📬 Contato

Se quiser Conversar ou ver mais do meu trabalho:

**LinkedIn:** [https://www.linkedin.com/in/pedroluccaga/](https://www.linkedin.com/in/pedroluccaga/)  
**Portfólio:** [em breve]
