# ✅ Notebook 00_configuração_aws_openai_s3 — Relatório Técnico

## 🎯 Objetivo
Estabelecer um ambiente Dev Container funcional para executar um pipeline de verificação de identidade com:
- Credenciais seguras via `.env`
- Integração com AWS (`boto3`)
- Integração com OpenAI (`openai`)
- Validação de infraestrutura e autenticação

---

## 📦 Estrutura do Container

- Nome do serviço Docker: `container_verificacao`
- Dockerfile baseado em `python:3.10-slim`
- Porta mapeada: `8899:8888`
- Volume: `.:/app`
- Ambiente acessado via **Dev Container** no VS Code
- Kernel Jupyter configurado com `ipykernel`

---

## 🪛 Etapas executadas

### 1. Construção do container

```bash
docker-compose build --no-cache
docker-compose up

Erro inicial: KeyError: 'ContainerConfig'
Solução: trocamos o nome da imagem para plano-verificacao, atualizamos o docker-compose.yml e limpamos imagens antigas.
2. Configuração como Dev Container

Criado o arquivo:

.devcontainer/devcontainer.json

Com referência ao docker-compose.yml, "workspaceFolder": "/app", e extensões Python + Jupyter.
3. Montagem do .env via notebook

Criado automaticamente a partir de:

    WRMELO_accessKeys.csv (AWS)

    OPEN AI - CHAVE.md (OpenAI)

Variáveis geradas:

    AWS_ACCESS_KEY_ID

    AWS_SECRET_ACCESS_KEY

    AWS_REGION

    OPENAI_API_KEY

    S3_BUCKET=staging-face-text

4. Testes de conectividade

    OpenAI: openai.models.list() → ✅ 50 modelos disponíveis

    AWS: s3.head_bucket(Bucket=...) → ❌ 404 Not Found

5. Solução para AWS S3

Erro: AccessDenied: CreateBucket
Causa: política IAM do usuário WRMELO negava s3:CreateBucket
Ações executadas:

    Acesso ao IAM: https://console.aws.amazon.com/iam

    Criação de política personalizada:

    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Sid": "S3StagingAccess",
          "Effect": "Allow",
          "Action": [
            "s3:CreateBucket",
            "s3:PutObject",
            "s3:GetObject",
            "s3:ListBucket"
          ],
          "Resource": [
            "arn:aws:s3:::staging-face-text",
            "arn:aws:s3:::staging-face-text/*"
          ]
        }
      ]
    }

    Alternativa usada: interface visual → permissão adicionada com CreateBucket, PutObject, GetObject, ListBucket

    Política nomeada: S3StagingAccess

6. Resultado final

    Célula 5: s3.head_bucket() → ✅ Bucket já existente

    Ambiente validado para uso com rekognition, textract, openai

    Notebook 00_configuração_aws_openai_s3.ipynb finalizado com sucesso

📌 Requisitos atendidos

    🔐 Autenticação com .env segura e replicável

    ✅ Integração com AWS e OpenAI confirmada

    🐳 Container persistente com acesso VS Code e Jupyter

    📚 Documentação versionável para continuidade

📁 Caminhos Relevantes

    /app/.env

    /app/notebooks/00_configuração_aws_openai_s3.ipynb

    ~/MBA_COGNITIVE_ENVIROMENTS/container-cognitive/docker-compose.yml

    ~/.aws/ (não utilizado, mas opcional para persistência global)