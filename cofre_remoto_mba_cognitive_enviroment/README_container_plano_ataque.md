# 📦 Container: `container_plano_ataque`
> Documentação de criação e inicialização do container local para execução do projeto de validação de identidade com AWS + OpenAI

---

## 🔹 Objetivo
Criar um container Docker autônomo, baseado em Python 3.10, com suporte a bibliotecas e dependências necessárias para um pipeline completo de verificação de identidade baseado em imagens e texto usando **Amazon Rekognition**, **Amazon Textract**, **OpenAI** e **Langchain**.

---

## 📁 Diretório de Trabalho
```bash
/home/wrm/MBA_COGNITIVE_ENVIROMENTS/container-cognitive
```

---

## 📄 Arquivos Criados

### Dockerfile
```Dockerfile
FROM python:3.10-slim

RUN apt-get update && apt-get install -y \
    curl \
    wget \
    git \
    build-essential \
    libgl1-mesa-glx \
    libglib2.0-0 \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY requirements.txt .
RUN pip install --upgrade pip && pip install -r requirements.txt

COPY . .

CMD ["bash"]
```

---

### requirements.txt
```text
boto3==1.34.79
matplotlib==3.10.3
pandas==2.3.0
requests==2.32.4
Pillow==11.2.1
openai==1.85.0
langchain==0.1.20
docarray==0.40.0
wikipedia==1.4.0
xmltodict==0.13.0
```

---

## 🔁 Reconfiguração do Container para Projeto de Validação de Identidade (AWS + OpenAI)

### Contexto
O container foi adaptado para refletir o plano de trabalho detalhado no documento `PROPOSIÇÃO PARA SOLUÇÃO.md`, com foco em:

- Validação de imagem (selfie vs. documento)
- Detecção de emoção e atributos faciais
- Extração de texto com OCR
- Integração com LLMs para sumarização ou validações textuais

---

## 🔧 Ações Executadas

1. Reescrita do `requirements.txt` com bibliotecas específicas
2. Atualização do `Dockerfile` com suporte a dependências visuais
3. Rebuild manual:
    ```bash
    docker build -t plano-ataque:latest .
    ```
4. Execução manual com rede Docker:
    ```bash
    docker run -it --rm \
      --name container_plano_ataque \
      --network infraestrutura_infra_net \
      -v $(pwd):/app \
      plano-ataque:latest
    ```

---

## ✅ Status Atual
| Etapa                      | Status |
|----------------------------|--------|
| Container funcional        | ✅     |
| Jupyter e Kernel operando | ✅     |
| Bibliotecas corretas       | ✅     |
| Ambiente pronto para AWS/OpenAI | ✅ |

---

## 🧩 Próximos Passos
- Criar `.env` com as chaves da AWS e OpenAI
- Validar chamadas `boto3` e `openai`
- Implementar notebooks de validação facial, OCR e lógica antifraude

---

## 📁 Referência do Plano
Baseado no documento: **PROPOSIÇÃO PARA SOLUÇÃO.md**
