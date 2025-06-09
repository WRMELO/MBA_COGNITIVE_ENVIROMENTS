
# Objetivo

Na imagem você vê um slide estruturado assim:
![[Pasted image 20250608090919.png]]
1. **Cabeçalho**
    
    - Texto grande “FACE & TEXT EXTRACTION” (com “TEXT” destacado em vermelho).
        
2. **Parágrafo descritivo**
    
    - Explica que, após um teste de vivacidade (liveness) com taxa abaixo de 90%, é necessária uma nova validação “do self” da pessoa junto ao documento.
        
3. **Três etapas ilustradas com imagens e legendas**
    
    |Passo|Imagem|Descrição|
    |---|---|---|
    |**1**|Carteira Nacional de Habilitação (CNH)|**Extração da face, nome e CPF** do documento oficial.|
    |**2**|Duas fotos de rosto lado a lado|**Comparação de faces**: exige ≥ 90% de similaridade entre selfie e foto da CNH.|
    |**3**|Conta de serviço (fatura ou boleto)|**Extração de nome e endereço**: o nome extraído deve coincidir com o da CNH.|
    
4. **Objetivo prático**
    
    - Montar um fluxo onde:
        
        1. Leio o documento (CNH), extraio campos textuais (nome, CPF) e recorto a face.
            
        2. Comparo esse recorte com uma selfie do cliente, validando similaridade ≥ 90%.
            
        3. Valido dados de endereço em documento secundário (conta de luz, água etc.), conferindo se o nome bate exatamente com o da CNH.
            
5. **Contexto de fraude**
    
    - Esse processo foi demandado porque alguns clientes reclamaram de recusas indevidas de crédito pessoal; o teste de vivacidade insuficiente (< 90%) disparou a necessidade de reforçar a validação de identidade.
        

---

Esse é o detalhamento prático dos elementos e do fluxo apresentado na imagem.


---
# Plano de Ataque Detalhado

Este documento apresenta um roteiro passo a passo para implementar o fluxo de validação de identidade via **Face & Text Extraction**. Pode ser colado no Obsidian para discussão e ajustes.

---

## 🎯 Objetivo

Construir um pipeline que:

1. Extraia face, nome e CPF da CNH.
    
2. Compare a face extraída com selfie do cliente (≥ 90% de similaridade).
    
3. Extraia nome e endereço de documento secundário e valide consistência com a CNH.
    

## 📦 Pré-requisitos

- Conta AWS configurada (Rekognition, Textract, S3) ou OpenAI multimodal habilitada.
    
- Permissões IAM:
    
    - `rekognition:DetectFaces`, `rekognition:CompareFaces`
        
    - `textract:AnalyzeDocument`
        
    - `s3:GetObject`, `s3:PutObject` no bucket `staging-face-text`
        
- Python 3.10+ e bibliotecas:
    
    ```bash
    pip install boto3 openai face_recognition pillow pandas numpy 
    ```
    
- Estrutura de pastas do projeto:
    
    ```text
    face-text-extraction/
    ├── notebooks/
    │   ├── 01_setup.ipynb
    │   ├── 02_extract_cnh.ipynb
    │   ├── 03_compare_face.ipynb
    │   ├── 04_extract_secondary.ipynb
    │   └── 05_reporting.ipynb
    ├── src/
    │   ├── extractors.py
    │   ├── comparators.py
    │   └── utils.py
    ├── data/
    │   ├── raw/
    │   └── processed/
    └── requirements.txt
    ```
    

## 🚀 Fases do Pipeline

|Fase|Descrição|Notebook|LLM Sugerido|
|---|---|---|---|
|**0. Configuração Inicial**|Configurar credenciais AWS, OpenAI e definir bucket S3|00_configuração_aws_openai_s3.ipynb|o4-mini-high|
|**1. Setup**|Teste de credenciais, instalação de libs e estrutura de pastas|01_setup.ipynb|o4-mini-high|
|**2. Extração CNH**|Upload da imagem, extração de texto (nome, CPF) e recorte da face|02_extract_cnh.ipynb|GPT-4.5|
|**3. Comparação de Faces**|Carregamento da selfie, cálculo de embeddings e verificação de similaridade (≥ 90%)|03_compare_face.ipynb|GPT-4o|
|**4. Extração Secundária**|Extração de nome e endereço de documento (fatura ou boleto)|04_extract_secondary.ipynb|GPT-4.5|
|**5. Relatórios & Decisão**|Consolidação dos resultados, lógica de aprovação/reprovação e geração de Markdown|05_reporting.ipynb|GPT-4.5|
|**6. Testes & CI/CD**|Unit tests, simulação LocalStack e pipeline GitHub Actions|--|GPT-4.1|

---

### 0. Configuração Inicial (00_configuração_aws_openai_s3.ipynb)

1. **Criar notebook** `00_configuração_aws_openai_s3.ipynb` na pasta raiz do projeto.
    
2. Inserir as seguintes células:
    

#### Célula 1: Instalar dependências

```python
!pip install boto3 openai python-dotenv
```

#### Célula 2: Criar arquivo de ambiente `.env`

```bash
%%bash
cat > .env << 'EOF'
AWS_ACCESS_KEY_ID=<COLOQUE_SEU_ACCESS_KEY_ID>
AWS_SECRET_ACCESS_KEY=<COLOQUE_SEU_SECRET_ACCESS_KEY>
AWS_REGION=<REGIÃO_AWS>
OPENAI_API_KEY=<SUA_OPENAI_API_KEY>
S3_BUCKET=<NOME_DO_SEU_BUCKET>
EOF
```

#### Célula 3: Carregar variáveis de ambiente

```python
from dotenv import load_dotenv
import os

load_dotenv('.env')

required = [
    "AWS_ACCESS_KEY_ID",
    "AWS_SECRET_ACCESS_KEY",
    "AWS_REGION",
    "OPENAI_API_KEY",
    "S3_BUCKET"
]
missing = [var for var in required if not os.getenv(var)]
if missing:
    raise EnvironmentError(f"Variáveis faltando: {missing}")
print("✅ Variáveis de ambiente carregadas com sucesso")
```

#### Célula 4: Testar AWS e OpenAI Connectivity

```python
import boto3
import os
import openai

# AWS S3 Test
s3 = boto3.client(
    's3',
    aws_access_key_id=os.getenv('AWS_ACCESS_KEY_ID'),
    aws_secret_access_key=os.getenv('AWS_SECRET_ACCESS_KEY'),
    region_name=os.getenv('AWS_REGION')
)
buckets = s3.list_buckets()
print("Buckets S3 disponíveis:", [b['Name'] for b in buckets.get('Buckets', [])])

# OpenAI Test
openai.api_key = os.getenv('OPENAI_API_KEY')
models = openai.Model.list()
print("Modelos OpenAI disponíveis:", len(models['data']))
```

> **LLM:** o4-mini-high para gerar scripts de configuração e verificação de ambiente.

---
