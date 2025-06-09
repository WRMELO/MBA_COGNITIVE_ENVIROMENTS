# Cloud Cognitive Environments
Aplicações com modelos LLM

## Requerimentos

* Langchain
* OpenAI
* Pandas
* Python Display
* DocArray
* Wikipedia
* XML to Dict


```python
# Apagar pasta do repositório
# Faça somente isso se não tenha clonado antes
!rm -rf fiap-ds-cloud-cognitive-environments
```


```python
# Clonar o repositório da aula
!git clone https://github.com/michelpf/fiap-ds-cloud-cognitive-environments
```

    Cloning into 'fiap-ds-cloud-cognitive-environments'...
    remote: Enumerating objects: 241, done.[K
    remote: Counting objects: 100% (69/69), done.[K
    remote: Compressing objects: 100% (53/53), done.[K
    remote: Total 241 (delta 26), reused 52 (delta 16), pack-reused 172[K
    Receiving objects: 100% (241/241), 186.80 MiB | 12.93 MiB/s, done.
    Resolving deltas: 100% (77/77), done.
    Updating files: 100% (62/62), done.
    


```python
%cd fiap-ds-cloud-cognitive-environments/aula-4-langchain/
```

    /content/fiap-ds-cloud-cognitive-environments/aula-4-langchain
    


```python
!pip install openai langchain-community langchain-core langchain tiktoken docarray wikipedia xmltodict langchain-openai chromadb
```

    Requirement already satisfied: openai in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (1.76.2)
    Requirement already satisfied: langchain-community in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (0.3.23)
    Requirement already satisfied: langchain-core in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (0.3.56)
    Requirement already satisfied: langchain in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (0.3.24)
    Requirement already satisfied: tiktoken in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (0.9.0)
    Requirement already satisfied: docarray in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (0.41.0)
    Requirement already satisfied: wikipedia in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (1.4.0)
    Requirement already satisfied: xmltodict in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (0.14.2)
    Requirement already satisfied: langchain-openai in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (0.3.15)
    Collecting chromadb
      Downloading chromadb-1.0.7-cp39-abi3-macosx_11_0_arm64.whl.metadata (6.9 kB)
    Requirement already satisfied: anyio<5,>=3.5.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from openai) (4.3.0)
    Requirement already satisfied: distro<2,>=1.7.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from openai) (1.9.0)
    Requirement already satisfied: httpx<1,>=0.23.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from openai) (0.27.0)
    Requirement already satisfied: jiter<1,>=0.4.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from openai) (0.9.0)
    Requirement already satisfied: pydantic<3,>=1.9.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from openai) (2.11.4)
    Requirement already satisfied: sniffio in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from openai) (1.3.1)
    Requirement already satisfied: tqdm>4 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from openai) (4.66.2)
    Requirement already satisfied: typing-extensions<5,>=4.11 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from openai) (4.13.2)
    Requirement already satisfied: idna>=2.8 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from anyio<5,>=3.5.0->openai) (3.6)
    Requirement already satisfied: exceptiongroup>=1.0.2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from anyio<5,>=3.5.0->openai) (1.2.0)
    Requirement already satisfied: certifi in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from httpx<1,>=0.23.0->openai) (2024.2.2)
    Requirement already satisfied: httpcore==1.* in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from httpx<1,>=0.23.0->openai) (1.0.4)
    Requirement already satisfied: h11<0.15,>=0.13 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from httpcore==1.*->httpx<1,>=0.23.0->openai) (0.14.0)
    Requirement already satisfied: annotated-types>=0.6.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from pydantic<3,>=1.9.0->openai) (0.6.0)
    Requirement already satisfied: pydantic-core==2.33.2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from pydantic<3,>=1.9.0->openai) (2.33.2)
    Requirement already satisfied: typing-inspection>=0.4.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from pydantic<3,>=1.9.0->openai) (0.4.0)
    Requirement already satisfied: SQLAlchemy<3,>=1.4 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-community) (2.0.40)
    Requirement already satisfied: requests<3,>=2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-community) (2.31.0)
    Requirement already satisfied: PyYAML>=5.3 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-community) (6.0.1)
    Requirement already satisfied: aiohttp<4.0.0,>=3.8.3 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-community) (3.9.3)
    Requirement already satisfied: tenacity!=8.4.0,<10,>=8.1.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-community) (8.5.0)
    Requirement already satisfied: dataclasses-json<0.7,>=0.5.7 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-community) (0.6.7)
    Requirement already satisfied: pydantic-settings<3.0.0,>=2.4.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-community) (2.9.1)
    Requirement already satisfied: langsmith<0.4,>=0.1.125 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-community) (0.3.39)
    Requirement already satisfied: httpx-sse<1.0.0,>=0.4.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-community) (0.4.0)
    Requirement already satisfied: numpy>=1.26.2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-community) (2.2.5)
    Requirement already satisfied: jsonpatch<2.0,>=1.33 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-core) (1.33)
    Requirement already satisfied: packaging<25,>=23.2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-core) (23.2)
    Requirement already satisfied: langchain-text-splitters<1.0.0,>=0.3.8 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain) (0.3.8)
    Requirement already satisfied: async-timeout<5.0.0,>=4.0.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain) (4.0.3)
    Requirement already satisfied: aiosignal>=1.1.2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from aiohttp<4.0.0,>=3.8.3->langchain-community) (1.3.1)
    Requirement already satisfied: attrs>=17.3.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from aiohttp<4.0.0,>=3.8.3->langchain-community) (23.2.0)
    Requirement already satisfied: frozenlist>=1.1.1 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from aiohttp<4.0.0,>=3.8.3->langchain-community) (1.4.1)
    Requirement already satisfied: multidict<7.0,>=4.5 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from aiohttp<4.0.0,>=3.8.3->langchain-community) (6.0.5)
    Requirement already satisfied: yarl<2.0,>=1.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from aiohttp<4.0.0,>=3.8.3->langchain-community) (1.9.4)
    Requirement already satisfied: marshmallow<4.0.0,>=3.18.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from dataclasses-json<0.7,>=0.5.7->langchain-community) (3.26.1)
    Requirement already satisfied: typing-inspect<1,>=0.4.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from dataclasses-json<0.7,>=0.5.7->langchain-community) (0.9.0)
    Requirement already satisfied: jsonpointer>=1.9 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from jsonpatch<2.0,>=1.33->langchain-core) (2.4)
    Requirement already satisfied: orjson<4.0.0,>=3.9.14 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langsmith<0.4,>=0.1.125->langchain-community) (3.9.14)
    Requirement already satisfied: requests-toolbelt<2.0.0,>=1.0.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langsmith<0.4,>=0.1.125->langchain-community) (1.0.0)
    Requirement already satisfied: zstandard<0.24.0,>=0.23.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langsmith<0.4,>=0.1.125->langchain-community) (0.23.0)
    Requirement already satisfied: python-dotenv>=0.21.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from pydantic-settings<3.0.0,>=2.4.0->langchain-community) (1.0.0)
    Requirement already satisfied: charset-normalizer<4,>=2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from requests<3,>=2->langchain-community) (3.3.2)
    Requirement already satisfied: urllib3<3,>=1.21.1 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from requests<3,>=2->langchain-community) (2.0.7)
    Requirement already satisfied: mypy-extensions>=0.3.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from typing-inspect<1,>=0.4.0->dataclasses-json<0.7,>=0.5.7->langchain-community) (1.1.0)
    Requirement already satisfied: regex>=2022.1.18 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from tiktoken) (2024.11.6)
    Requirement already satisfied: rich>=13.1.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from docarray) (13.7.0)
    Requirement already satisfied: types-requests>=2.28.11.6 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from docarray) (2.32.0.20250328)
    Requirement already satisfied: beautifulsoup4 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from wikipedia) (4.12.3)
    Collecting build>=1.0.3 (from chromadb)
      Downloading build-1.2.2.post1-py3-none-any.whl.metadata (6.5 kB)
    Collecting chroma-hnswlib==0.7.6 (from chromadb)
      Downloading chroma_hnswlib-0.7.6-cp310-cp310-macosx_11_0_arm64.whl.metadata (252 bytes)
    Collecting fastapi==0.115.9 (from chromadb)
      Downloading fastapi-0.115.9-py3-none-any.whl.metadata (27 kB)
    Collecting uvicorn>=0.18.3 (from uvicorn[standard]>=0.18.3->chromadb)
      Downloading uvicorn-0.34.2-py3-none-any.whl.metadata (6.5 kB)
    Collecting posthog>=2.4.0 (from chromadb)
      Downloading posthog-4.0.1-py2.py3-none-any.whl.metadata (3.0 kB)
    Collecting onnxruntime>=1.14.1 (from chromadb)
      Downloading onnxruntime-1.21.1-cp310-cp310-macosx_13_0_universal2.whl.metadata (4.5 kB)
    Collecting opentelemetry-api>=1.2.0 (from chromadb)
      Downloading opentelemetry_api-1.32.1-py3-none-any.whl.metadata (1.6 kB)
    Collecting opentelemetry-exporter-otlp-proto-grpc>=1.2.0 (from chromadb)
      Downloading opentelemetry_exporter_otlp_proto_grpc-1.32.1-py3-none-any.whl.metadata (2.5 kB)
    Collecting opentelemetry-instrumentation-fastapi>=0.41b0 (from chromadb)
      Downloading opentelemetry_instrumentation_fastapi-0.53b1-py3-none-any.whl.metadata (2.2 kB)
    Collecting opentelemetry-sdk>=1.2.0 (from chromadb)
      Downloading opentelemetry_sdk-1.32.1-py3-none-any.whl.metadata (1.6 kB)
    Collecting tokenizers>=0.13.2 (from chromadb)
      Downloading tokenizers-0.21.1-cp39-abi3-macosx_11_0_arm64.whl.metadata (6.8 kB)
    Collecting pypika>=0.48.9 (from chromadb)
      Downloading PyPika-0.48.9.tar.gz (67 kB)
      Installing build dependencies ... [?25ldone
    [?25h  Getting requirements to build wheel ... [?25ldone
    [?25h  Preparing metadata (pyproject.toml) ... [?25ldone
    [?25hRequirement already satisfied: overrides>=7.3.1 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from chromadb) (7.7.0)
    Collecting importlib-resources (from chromadb)
      Downloading importlib_resources-6.5.2-py3-none-any.whl.metadata (3.9 kB)
    Requirement already satisfied: grpcio>=1.58.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from chromadb) (1.65.4)
    Collecting bcrypt>=4.0.1 (from chromadb)
      Downloading bcrypt-4.3.0-cp39-abi3-macosx_10_12_universal2.whl.metadata (10 kB)
    Requirement already satisfied: typer>=0.9.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from chromadb) (0.9.0)
    Collecting kubernetes>=28.1.0 (from chromadb)
      Downloading kubernetes-32.0.1-py2.py3-none-any.whl.metadata (1.7 kB)
    Collecting mmh3>=4.0.1 (from chromadb)
      Downloading mmh3-5.1.0-cp310-cp310-macosx_11_0_arm64.whl.metadata (16 kB)
    Requirement already satisfied: jsonschema>=4.19.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from chromadb) (4.21.1)
    Collecting starlette<0.46.0,>=0.40.0 (from fastapi==0.115.9->chromadb)
      Downloading starlette-0.45.3-py3-none-any.whl.metadata (6.3 kB)
    Collecting pyproject_hooks (from build>=1.0.3->chromadb)
      Downloading pyproject_hooks-1.2.0-py3-none-any.whl.metadata (1.3 kB)
    Requirement already satisfied: tomli>=1.1.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from build>=1.0.3->chromadb) (2.0.1)
    Requirement already satisfied: jsonschema-specifications>=2023.03.6 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from jsonschema>=4.19.0->chromadb) (2023.12.1)
    Requirement already satisfied: referencing>=0.28.4 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from jsonschema>=4.19.0->chromadb) (0.33.0)
    Requirement already satisfied: rpds-py>=0.7.1 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from jsonschema>=4.19.0->chromadb) (0.18.0)
    Requirement already satisfied: six>=1.9.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from kubernetes>=28.1.0->chromadb) (1.16.0)
    Requirement already satisfied: python-dateutil>=2.5.3 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from kubernetes>=28.1.0->chromadb) (2.8.2)
    Collecting google-auth>=1.0.1 (from kubernetes>=28.1.0->chromadb)
      Downloading google_auth-2.39.0-py2.py3-none-any.whl.metadata (6.2 kB)
    Requirement already satisfied: websocket-client!=0.40.0,!=0.41.*,!=0.42.*,>=0.32.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from kubernetes>=28.1.0->chromadb) (1.7.0)
    Collecting requests-oauthlib (from kubernetes>=28.1.0->chromadb)
      Downloading requests_oauthlib-2.0.0-py2.py3-none-any.whl.metadata (11 kB)
    Collecting oauthlib>=3.2.2 (from kubernetes>=28.1.0->chromadb)
      Downloading oauthlib-3.2.2-py3-none-any.whl.metadata (7.5 kB)
    Collecting durationpy>=0.7 (from kubernetes>=28.1.0->chromadb)
      Downloading durationpy-0.9-py3-none-any.whl.metadata (338 bytes)
    Requirement already satisfied: cachetools<6.0,>=2.0.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from google-auth>=1.0.1->kubernetes>=28.1.0->chromadb) (5.4.0)
    Collecting pyasn1-modules>=0.2.1 (from google-auth>=1.0.1->kubernetes>=28.1.0->chromadb)
      Downloading pyasn1_modules-0.4.2-py3-none-any.whl.metadata (3.5 kB)
    Collecting rsa<5,>=3.1.4 (from google-auth>=1.0.1->kubernetes>=28.1.0->chromadb)
      Downloading rsa-4.9.1-py3-none-any.whl.metadata (5.6 kB)
    Collecting pyasn1>=0.1.3 (from rsa<5,>=3.1.4->google-auth>=1.0.1->kubernetes>=28.1.0->chromadb)
      Downloading pyasn1-0.6.1-py3-none-any.whl.metadata (8.4 kB)
    Collecting coloredlogs (from onnxruntime>=1.14.1->chromadb)
      Downloading coloredlogs-15.0.1-py2.py3-none-any.whl.metadata (12 kB)
    Requirement already satisfied: flatbuffers in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from onnxruntime>=1.14.1->chromadb) (24.3.25)
    Requirement already satisfied: protobuf in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from onnxruntime>=1.14.1->chromadb) (4.25.4)
    Collecting sympy (from onnxruntime>=1.14.1->chromadb)
      Downloading sympy-1.14.0-py3-none-any.whl.metadata (12 kB)
    Collecting deprecated>=1.2.6 (from opentelemetry-api>=1.2.0->chromadb)
      Downloading Deprecated-1.2.18-py2.py3-none-any.whl.metadata (5.7 kB)
    Requirement already satisfied: importlib-metadata<8.7.0,>=6.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from opentelemetry-api>=1.2.0->chromadb) (7.2.1)
    Requirement already satisfied: zipp>=0.5 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from importlib-metadata<8.7.0,>=6.0->opentelemetry-api>=1.2.0->chromadb) (3.21.0)
    Requirement already satisfied: wrapt<2,>=1.10 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from deprecated>=1.2.6->opentelemetry-api>=1.2.0->chromadb) (1.16.0)
    Collecting googleapis-common-protos~=1.52 (from opentelemetry-exporter-otlp-proto-grpc>=1.2.0->chromadb)
      Downloading googleapis_common_protos-1.70.0-py3-none-any.whl.metadata (9.3 kB)
    Collecting opentelemetry-exporter-otlp-proto-common==1.32.1 (from opentelemetry-exporter-otlp-proto-grpc>=1.2.0->chromadb)
      Downloading opentelemetry_exporter_otlp_proto_common-1.32.1-py3-none-any.whl.metadata (1.9 kB)
    Collecting opentelemetry-proto==1.32.1 (from opentelemetry-exporter-otlp-proto-grpc>=1.2.0->chromadb)
      Downloading opentelemetry_proto-1.32.1-py3-none-any.whl.metadata (2.4 kB)
    Collecting protobuf (from onnxruntime>=1.14.1->chromadb)
      Downloading protobuf-5.29.4-cp38-abi3-macosx_10_9_universal2.whl.metadata (592 bytes)
    Collecting opentelemetry-semantic-conventions==0.53b1 (from opentelemetry-sdk>=1.2.0->chromadb)
      Downloading opentelemetry_semantic_conventions-0.53b1-py3-none-any.whl.metadata (2.5 kB)
    Collecting opentelemetry-instrumentation-asgi==0.53b1 (from opentelemetry-instrumentation-fastapi>=0.41b0->chromadb)
      Downloading opentelemetry_instrumentation_asgi-0.53b1-py3-none-any.whl.metadata (2.1 kB)
    Collecting opentelemetry-instrumentation==0.53b1 (from opentelemetry-instrumentation-fastapi>=0.41b0->chromadb)
      Downloading opentelemetry_instrumentation-0.53b1-py3-none-any.whl.metadata (6.8 kB)
    Collecting opentelemetry-util-http==0.53b1 (from opentelemetry-instrumentation-fastapi>=0.41b0->chromadb)
      Downloading opentelemetry_util_http-0.53b1-py3-none-any.whl.metadata (2.6 kB)
    Collecting asgiref~=3.0 (from opentelemetry-instrumentation-asgi==0.53b1->opentelemetry-instrumentation-fastapi>=0.41b0->chromadb)
      Downloading asgiref-3.8.1-py3-none-any.whl.metadata (9.3 kB)
    Collecting backoff>=1.10.0 (from posthog>=2.4.0->chromadb)
      Downloading backoff-2.2.1-py3-none-any.whl.metadata (14 kB)
    Requirement already satisfied: markdown-it-py>=2.2.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from rich>=13.1.0->docarray) (3.0.0)
    Requirement already satisfied: pygments<3.0.0,>=2.13.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from rich>=13.1.0->docarray) (2.17.2)
    Requirement already satisfied: mdurl~=0.1 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from markdown-it-py>=2.2.0->rich>=13.1.0->docarray) (0.1.2)
    Collecting huggingface-hub<1.0,>=0.16.4 (from tokenizers>=0.13.2->chromadb)
      Downloading huggingface_hub-0.30.2-py3-none-any.whl.metadata (13 kB)
    Requirement already satisfied: filelock in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from huggingface-hub<1.0,>=0.16.4->tokenizers>=0.13.2->chromadb) (3.13.1)
    Requirement already satisfied: fsspec>=2023.5.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from huggingface-hub<1.0,>=0.16.4->tokenizers>=0.13.2->chromadb) (2024.2.0)
    Requirement already satisfied: click<9.0.0,>=7.1.1 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from typer>=0.9.0->chromadb) (8.1.7)
    Collecting httptools>=0.6.3 (from uvicorn[standard]>=0.18.3->chromadb)
      Downloading httptools-0.6.4-cp310-cp310-macosx_11_0_arm64.whl.metadata (3.6 kB)
    Collecting uvloop!=0.15.0,!=0.15.1,>=0.14.0 (from uvicorn[standard]>=0.18.3->chromadb)
      Downloading uvloop-0.21.0-cp310-cp310-macosx_10_9_universal2.whl.metadata (4.9 kB)
    Collecting watchfiles>=0.13 (from uvicorn[standard]>=0.18.3->chromadb)
      Downloading watchfiles-1.0.5-cp310-cp310-macosx_11_0_arm64.whl.metadata (4.9 kB)
    Collecting websockets>=10.4 (from uvicorn[standard]>=0.18.3->chromadb)
      Downloading websockets-15.0.1-cp310-cp310-macosx_11_0_arm64.whl.metadata (6.8 kB)
    Requirement already satisfied: soupsieve>1.2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from beautifulsoup4->wikipedia) (2.5)
    Collecting humanfriendly>=9.1 (from coloredlogs->onnxruntime>=1.14.1->chromadb)
      Downloading humanfriendly-10.0-py2.py3-none-any.whl.metadata (9.2 kB)
    Collecting mpmath<1.4,>=1.1.0 (from sympy->onnxruntime>=1.14.1->chromadb)
      Downloading mpmath-1.3.0-py3-none-any.whl.metadata (8.6 kB)
    Downloading chromadb-1.0.7-cp39-abi3-macosx_11_0_arm64.whl (16.9 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m16.9/16.9 MB[0m [31m10.7 MB/s[0m eta [36m0:00:00[0ma [36m0:00:01[0m
    [?25hDownloading chroma_hnswlib-0.7.6-cp310-cp310-macosx_11_0_arm64.whl (183 kB)
    Downloading fastapi-0.115.9-py3-none-any.whl (94 kB)
    Downloading starlette-0.45.3-py3-none-any.whl (71 kB)
    Downloading bcrypt-4.3.0-cp39-abi3-macosx_10_12_universal2.whl (498 kB)
    Downloading build-1.2.2.post1-py3-none-any.whl (22 kB)
    Downloading kubernetes-32.0.1-py2.py3-none-any.whl (2.0 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m2.0/2.0 MB[0m [31m11.6 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading durationpy-0.9-py3-none-any.whl (3.5 kB)
    Downloading google_auth-2.39.0-py2.py3-none-any.whl (212 kB)
    Downloading rsa-4.9.1-py3-none-any.whl (34 kB)
    Downloading mmh3-5.1.0-cp310-cp310-macosx_11_0_arm64.whl (40 kB)
    Downloading oauthlib-3.2.2-py3-none-any.whl (151 kB)
    Downloading onnxruntime-1.21.1-cp310-cp310-macosx_13_0_universal2.whl (33.6 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m33.6/33.6 MB[0m [31m7.9 MB/s[0m eta [36m0:00:00[0m00:01[0m00:01[0m
    [?25hDownloading opentelemetry_api-1.32.1-py3-none-any.whl (65 kB)
    Downloading Deprecated-1.2.18-py2.py3-none-any.whl (10.0 kB)
    Downloading opentelemetry_exporter_otlp_proto_grpc-1.32.1-py3-none-any.whl (18 kB)
    Downloading opentelemetry_exporter_otlp_proto_common-1.32.1-py3-none-any.whl (18 kB)
    Downloading opentelemetry_proto-1.32.1-py3-none-any.whl (55 kB)
    Downloading googleapis_common_protos-1.70.0-py3-none-any.whl (294 kB)
    Downloading opentelemetry_sdk-1.32.1-py3-none-any.whl (118 kB)
    Downloading opentelemetry_semantic_conventions-0.53b1-py3-none-any.whl (188 kB)
    Downloading protobuf-5.29.4-cp38-abi3-macosx_10_9_universal2.whl (417 kB)
    Downloading opentelemetry_instrumentation_fastapi-0.53b1-py3-none-any.whl (12 kB)
    Downloading opentelemetry_instrumentation-0.53b1-py3-none-any.whl (30 kB)
    Downloading opentelemetry_instrumentation_asgi-0.53b1-py3-none-any.whl (16 kB)
    Downloading opentelemetry_util_http-0.53b1-py3-none-any.whl (7.3 kB)
    Downloading asgiref-3.8.1-py3-none-any.whl (23 kB)
    Downloading posthog-4.0.1-py2.py3-none-any.whl (92 kB)
    Downloading backoff-2.2.1-py3-none-any.whl (15 kB)
    Downloading pyasn1-0.6.1-py3-none-any.whl (83 kB)
    Downloading pyasn1_modules-0.4.2-py3-none-any.whl (181 kB)
    Downloading tokenizers-0.21.1-cp39-abi3-macosx_11_0_arm64.whl (2.7 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m2.7/2.7 MB[0m [31m8.0 MB/s[0m eta [36m0:00:00[0ma [36m0:00:01[0m
    [?25hDownloading huggingface_hub-0.30.2-py3-none-any.whl (481 kB)
    Downloading uvicorn-0.34.2-py3-none-any.whl (62 kB)
    Downloading httptools-0.6.4-cp310-cp310-macosx_11_0_arm64.whl (103 kB)
    Downloading uvloop-0.21.0-cp310-cp310-macosx_10_9_universal2.whl (1.4 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m1.4/1.4 MB[0m [31m7.1 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading watchfiles-1.0.5-cp310-cp310-macosx_11_0_arm64.whl (395 kB)
    Downloading websockets-15.0.1-cp310-cp310-macosx_11_0_arm64.whl (173 kB)
    Downloading coloredlogs-15.0.1-py2.py3-none-any.whl (46 kB)
    Downloading humanfriendly-10.0-py2.py3-none-any.whl (86 kB)
    Downloading importlib_resources-6.5.2-py3-none-any.whl (37 kB)
    Downloading pyproject_hooks-1.2.0-py3-none-any.whl (10 kB)
    Downloading requests_oauthlib-2.0.0-py2.py3-none-any.whl (24 kB)
    Downloading sympy-1.14.0-py3-none-any.whl (6.3 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m6.3/6.3 MB[0m [31m9.6 MB/s[0m eta [36m0:00:00[0ma [36m0:00:01[0mm
    [?25hDownloading mpmath-1.3.0-py3-none-any.whl (536 kB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m536.2/536.2 kB[0m [31m9.6 MB/s[0m eta [36m0:00:00[0m
    [?25hBuilding wheels for collected packages: pypika
      Building wheel for pypika (pyproject.toml) ... [?25ldone
    [?25h  Created wheel for pypika: filename=pypika-0.48.9-py2.py3-none-any.whl size=53800 sha256=28ee388aa1f64c5ef6f29aa61524fc29da452268f15fc38138d994eb0eac6d69
      Stored in directory: /Users/michelpf/Library/Caches/pip/wheels/e1/26/51/d0bffb3d2fd82256676d7ad3003faea3bd6dddc9577af665f4
    Successfully built pypika
    Installing collected packages: pypika, mpmath, durationpy, websockets, uvloop, uvicorn, sympy, pyproject_hooks, pyasn1, protobuf, opentelemetry-util-http, oauthlib, mmh3, importlib-resources, humanfriendly, httptools, deprecated, chroma-hnswlib, bcrypt, backoff, asgiref, watchfiles, starlette, rsa, requests-oauthlib, pyasn1-modules, posthog, opentelemetry-proto, opentelemetry-api, huggingface-hub, googleapis-common-protos, coloredlogs, build, tokenizers, opentelemetry-semantic-conventions, opentelemetry-exporter-otlp-proto-common, onnxruntime, google-auth, fastapi, opentelemetry-sdk, opentelemetry-instrumentation, kubernetes, opentelemetry-instrumentation-asgi, opentelemetry-exporter-otlp-proto-grpc, opentelemetry-instrumentation-fastapi, chromadb
    [2K  Attempting uninstall: protobuf━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m 6/46[0m [sympy]
    [2K    Found existing installation: protobuf 4.25.4━━━━━━━━━━━━━━[0m [32m 6/46[0m [sympy]
    [2K    Uninstalling protobuf-4.25.4:━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m 6/46[0m [sympy]
    [2K      Successfully uninstalled protobuf-4.25.4━━━━━━━━━━━━━━━━[0m [32m 6/46[0m [sympy]
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m46/46[0m [chromadb]chromadb]lemetry-instrumentation-asgi]
    [1A[2K[31mERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
    streamlit 1.30.0 requires numpy<2,>=1.19.3, but you have numpy 2.2.5 which is incompatible.
    streamlit 1.30.0 requires protobuf<5,>=3.20, but you have protobuf 5.29.4 which is incompatible.
    tensorboard 2.17.0 requires protobuf!=4.24.0,<5.0.0,>=3.19.6, but you have protobuf 5.29.4 which is incompatible.
    tensorflow 2.17.0 requires numpy<2.0.0,>=1.23.5; python_version <= "3.11", but you have numpy 2.2.5 which is incompatible.
    tensorflow 2.17.0 requires protobuf!=4.21.0,!=4.21.1,!=4.21.2,!=4.21.3,!=4.21.4,!=4.21.5,<5.0.0dev,>=3.20.3, but you have protobuf 5.29.4 which is incompatible.[0m[31m
    [0mSuccessfully installed asgiref-3.8.1 backoff-2.2.1 bcrypt-4.3.0 build-1.2.2.post1 chroma-hnswlib-0.7.6 chromadb-1.0.7 coloredlogs-15.0.1 deprecated-1.2.18 durationpy-0.9 fastapi-0.115.9 google-auth-2.39.0 googleapis-common-protos-1.70.0 httptools-0.6.4 huggingface-hub-0.30.2 humanfriendly-10.0 importlib-resources-6.5.2 kubernetes-32.0.1 mmh3-5.1.0 mpmath-1.3.0 oauthlib-3.2.2 onnxruntime-1.21.1 opentelemetry-api-1.32.1 opentelemetry-exporter-otlp-proto-common-1.32.1 opentelemetry-exporter-otlp-proto-grpc-1.32.1 opentelemetry-instrumentation-0.53b1 opentelemetry-instrumentation-asgi-0.53b1 opentelemetry-instrumentation-fastapi-0.53b1 opentelemetry-proto-1.32.1 opentelemetry-sdk-1.32.1 opentelemetry-semantic-conventions-0.53b1 opentelemetry-util-http-0.53b1 posthog-4.0.1 protobuf-5.29.4 pyasn1-0.6.1 pyasn1-modules-0.4.2 pypika-0.48.9 pyproject_hooks-1.2.0 requests-oauthlib-2.0.0 rsa-4.9.1 starlette-0.45.3 sympy-1.14.0 tokenizers-0.21.1 uvicorn-0.34.2 uvloop-0.21.0 watchfiles-1.0.5 websockets-15.0.1
    


```python
import os
from openai import OpenAI
import langchain
from IPython.display import display, Markdown as display_markdown
import json
import pandas as pd
```

# Modelos de base

## OpenAI

Inscreva-se no [site](www.openai.com) para obter uma chave de API. No registro você terá 30 dias de uso gratuito.
Para experimentações pequenas o custo é baixo. Avalie com cuidado ao lançar uma aplicação em produção para incluir limites máximos de gastos.


```python
os.environ["OPENAI_API_KEY"] = "CHAVE"
```

Utilizaremos o modelo fundacional do ChatGPT 4.1 mini é ```gpt-4.1-mini``` conforme [documentação](https://platform.openai.com/docs/models/gpt-4.1-mini). Sempre revise a documentação pois os modelos podem se tornar obsoletos para a entrada de outros mais novos.

## Chat Completion

Permite criar prompts que conversam com o modelo selecionado. Por meio de roles definidas, podemos criar um prompt mais completo, especificando:

1. ```System```: papel de sistema, ajuda na formatação do tipo de resposta e de como o modelo deve se comportar. Definimos com esse papel aspectos de entrada e saída.
2. ```User```: papel de usuário, neste caso é a informação desejada pelo usuário, que vai seguir as regras definidas no papel anteriormente definido.
2. ```Assistant```: papel de assistente virtual (próprio modelo). Neste caso serve para definir um aspecto de memória.


```python
client = OpenAI(
    api_key=os.environ.get("OPENAI_API_KEY"),
)
```


```python
model = "gpt-4.1-mini"
```


```python
messages=[
        {"role": "system", "content": "Você é um professor de visão computacional que leciona para crianças de até 10 anos."},
        {"role": "user", "content": "Como funciona um detector de bordas de Canny, de forma bem resumida?"},
        {"role": "system", "content": "O resultado precisa ser formatado como Markdown."}
    ]
```


```python
response = client.chat.completions.create(
    model=model,
    messages=messages,
    temperature=0,
)
response
```




    ChatCompletion(id='chatcmpl-BSZS0IeAmp98xvnD0yLwbEYmIG8hJ', choices=[Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='Claro! Aqui vai uma explicação bem simples sobre o detector de bordas de Canny:\n\n---\n\n# Detector de Bordas de Canny (Explicação para crianças)\n\nImagina que você tem um desenho e quer encontrar onde as linhas começam e terminam. O detector de bordas de Canny é como um superdetetive que ajuda o computador a achar essas linhas no desenho.\n\nEle faz isso em 4 passos:\n\n1. **Deixar o desenho mais suave**: Apaga um pouco os detalhes para não confundir.\n2. **Encontrar mudanças fortes**: Procura onde a cor ou o brilho muda bastante, porque isso pode ser uma borda.\n3. **Achar as bordas mais importantes**: Decide quais mudanças são realmente bordas e quais são só barulho.\n4. **Conectar as bordas**: Junta as partes das linhas para formar bordas completas.\n\nAssim, o computador consegue ver as linhas do desenho, igual a gente!\n\n---\n\nSe quiser, posso explicar mais! 😊', refusal=None, role='assistant', annotations=[], audio=None, function_call=None, tool_calls=None))], created=1746149156, model='gpt-4.1-mini-2025-04-14', object='chat.completion', service_tier='default', system_fingerprint='fp_79b79be41f', usage=CompletionUsage(completion_tokens=209, prompt_tokens=61, total_tokens=270, completion_tokens_details=CompletionTokensDetails(accepted_prediction_tokens=0, audio_tokens=0, reasoning_tokens=0, rejected_prediction_tokens=0), prompt_tokens_details=PromptTokensDetails(audio_tokens=0, cached_tokens=0)))




```python
response.choices[0].message.content
```




    'Claro! Aqui vai uma explicação bem simples sobre o detector de bordas de Canny:\n\n---\n\n# Detector de Bordas de Canny (Explicação para crianças)\n\nImagina que você tem um desenho e quer encontrar onde as linhas começam e terminam. O detector de bordas de Canny é como um superdetetive que ajuda o computador a achar essas linhas no desenho.\n\nEle faz isso em 4 passos:\n\n1. **Deixar o desenho mais suave**: Apaga um pouco os detalhes para não confundir.\n2. **Encontrar mudanças fortes**: Procura onde a cor ou o brilho muda bastante, porque isso pode ser uma borda.\n3. **Achar as bordas mais importantes**: Decide quais mudanças são realmente bordas e quais são só barulho.\n4. **Conectar as bordas**: Junta as partes das linhas para formar bordas completas.\n\nAssim, o computador consegue ver as linhas do desenho, igual a gente!\n\n---\n\nSe quiser, posso explicar mais! 😊'




```python
display_markdown(response.choices[0].message.content)
```




Claro! Aqui vai uma explicação bem simples sobre o detector de bordas de Canny:

---

# Detector de Bordas de Canny (Explicação para crianças)

Imagina que você tem um desenho e quer encontrar onde as linhas começam e terminam. O detector de bordas de Canny é como um superdetetive que ajuda o computador a achar essas linhas no desenho.

Ele faz isso em 4 passos:

1. **Deixar o desenho mais suave**: Apaga um pouco os detalhes para não confundir.
2. **Encontrar mudanças fortes**: Procura onde a cor ou o brilho muda bastante, porque isso pode ser uma borda.
3. **Achar as bordas mais importantes**: Decide quais mudanças são realmente bordas e quais são só barulho.
4. **Conectar as bordas**: Junta as partes das linhas para formar bordas completas.

Assim, o computador consegue ver as linhas do desenho, igual a gente!

---

Se quiser, posso explicar mais! 😊




```python
messages=[
        {"role": "system", "content": "Você é um sistema que extrai 2 informações de um texto, primeiro se a frase for um elogio deverá armazenar is_compliment=True"},
        {"role": "system", "content": "Indique qual o elogio em uma variável compliment, por exemplo em 'A pessoa é bonita', is_compliment=True e compliment='bonita'. "},
        {"role": "user", "content": "Os carros elétricos são incríveis."},
        {"role": "system", "content": "O resultado precisa ser formatado como JSON."}
    ]
```


```python
response = client.chat.completions.create(
    model=model,
    messages=messages,
    temperature=0,
)
response
```




    ChatCompletion(id='chatcmpl-BSZS9yma4mrBhmNJIgM2s1HRWbG9T', choices=[Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='{\n  "is_compliment": true,\n  "compliment": "incríveis"\n}', refusal=None, role='assistant', annotations=[], audio=None, function_call=None, tool_calls=None))], created=1746149165, model='gpt-4.1-mini-2025-04-14', object='chat.completion', service_tier='default', system_fingerprint='fp_79b79be41f', usage=CompletionUsage(completion_tokens=22, prompt_tokens=97, total_tokens=119, completion_tokens_details=CompletionTokensDetails(accepted_prediction_tokens=0, audio_tokens=0, reasoning_tokens=0, rejected_prediction_tokens=0), prompt_tokens_details=PromptTokensDetails(audio_tokens=0, cached_tokens=0)))




```python
response.choices[0].message.content
```




    '{\n  "is_compliment": true,\n  "compliment": "incríveis"\n}'




```python
result = json.loads(response.choices[0].message.content)
result
```




    {'is_compliment': True, 'compliment': 'incríveis'}



# Langchain

É um framework com foco em integrar diferentes _foundationals model_, dentre eles o GPT (da OpenAI), Bard (do Google), WatsonX (da IBM) e outros mais. cada modelo e plataformas tem algumas particularidades, mas há um bom espaço para abstrações. É nesse nicho que o framework faz o seu papel de utilizar componentes e serviços comum a todos os modelos como encadeamento de ações, memória, acesso e dados externos e utilização de agentes.


```python
!pip install langchain_openai
```

    Collecting langchain_openai
      Downloading langchain_openai-0.3.15-py3-none-any.whl.metadata (2.3 kB)
    Requirement already satisfied: langchain-core<1.0.0,>=0.3.56 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain_openai) (0.3.56)
    Requirement already satisfied: openai<2.0.0,>=1.68.2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain_openai) (1.76.2)
    Requirement already satisfied: tiktoken<1,>=0.7 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain_openai) (0.9.0)
    Requirement already satisfied: langsmith<0.4,>=0.1.125 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-core<1.0.0,>=0.3.56->langchain_openai) (0.3.39)
    Requirement already satisfied: tenacity!=8.4.0,<10.0.0,>=8.1.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-core<1.0.0,>=0.3.56->langchain_openai) (8.5.0)
    Requirement already satisfied: jsonpatch<2.0,>=1.33 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-core<1.0.0,>=0.3.56->langchain_openai) (1.33)
    Requirement already satisfied: PyYAML>=5.3 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-core<1.0.0,>=0.3.56->langchain_openai) (6.0.1)
    Requirement already satisfied: packaging<25,>=23.2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-core<1.0.0,>=0.3.56->langchain_openai) (23.2)
    Requirement already satisfied: typing-extensions>=4.7 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-core<1.0.0,>=0.3.56->langchain_openai) (4.13.2)
    Requirement already satisfied: pydantic<3.0.0,>=2.5.2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchain-core<1.0.0,>=0.3.56->langchain_openai) (2.11.4)
    Requirement already satisfied: jsonpointer>=1.9 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from jsonpatch<2.0,>=1.33->langchain-core<1.0.0,>=0.3.56->langchain_openai) (2.4)
    Requirement already satisfied: httpx<1,>=0.23.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.56->langchain_openai) (0.27.0)
    Requirement already satisfied: orjson<4.0.0,>=3.9.14 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.56->langchain_openai) (3.9.14)
    Requirement already satisfied: requests<3,>=2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.56->langchain_openai) (2.31.0)
    Requirement already satisfied: requests-toolbelt<2.0.0,>=1.0.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.56->langchain_openai) (1.0.0)
    Requirement already satisfied: zstandard<0.24.0,>=0.23.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.56->langchain_openai) (0.23.0)
    Requirement already satisfied: anyio in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from httpx<1,>=0.23.0->langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.56->langchain_openai) (4.3.0)
    Requirement already satisfied: certifi in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from httpx<1,>=0.23.0->langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.56->langchain_openai) (2024.2.2)
    Requirement already satisfied: httpcore==1.* in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from httpx<1,>=0.23.0->langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.56->langchain_openai) (1.0.4)
    Requirement already satisfied: idna in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from httpx<1,>=0.23.0->langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.56->langchain_openai) (3.6)
    Requirement already satisfied: sniffio in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from httpx<1,>=0.23.0->langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.56->langchain_openai) (1.3.1)
    Requirement already satisfied: h11<0.15,>=0.13 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from httpcore==1.*->httpx<1,>=0.23.0->langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.56->langchain_openai) (0.14.0)
    Requirement already satisfied: distro<2,>=1.7.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from openai<2.0.0,>=1.68.2->langchain_openai) (1.9.0)
    Requirement already satisfied: jiter<1,>=0.4.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from openai<2.0.0,>=1.68.2->langchain_openai) (0.9.0)
    Requirement already satisfied: tqdm>4 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from openai<2.0.0,>=1.68.2->langchain_openai) (4.66.2)
    Requirement already satisfied: exceptiongroup>=1.0.2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from anyio->httpx<1,>=0.23.0->langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.56->langchain_openai) (1.2.0)
    Requirement already satisfied: annotated-types>=0.6.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from pydantic<3.0.0,>=2.5.2->langchain-core<1.0.0,>=0.3.56->langchain_openai) (0.6.0)
    Requirement already satisfied: pydantic-core==2.33.2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from pydantic<3.0.0,>=2.5.2->langchain-core<1.0.0,>=0.3.56->langchain_openai) (2.33.2)
    Requirement already satisfied: typing-inspection>=0.4.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from pydantic<3.0.0,>=2.5.2->langchain-core<1.0.0,>=0.3.56->langchain_openai) (0.4.0)
    Requirement already satisfied: charset-normalizer<4,>=2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from requests<3,>=2->langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.56->langchain_openai) (3.3.2)
    Requirement already satisfied: urllib3<3,>=1.21.1 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from requests<3,>=2->langsmith<0.4,>=0.1.125->langchain-core<1.0.0,>=0.3.56->langchain_openai) (2.0.7)
    Requirement already satisfied: regex>=2022.1.18 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from tiktoken<1,>=0.7->langchain_openai) (2024.11.6)
    Downloading langchain_openai-0.3.15-py3-none-any.whl (62 kB)
    Installing collected packages: langchain_openai
    Successfully installed langchain_openai-0.3.15
    


```python
from langchain_openai import ChatOpenAI
```

## Templates de prompt

Templates ajudam a criar uma forma padronizada sempre que for utilizado um determindo prompt para atividades repetitivas. Por exemplo, traduções, resumos, modificações de tom, etc.

Utilizaremos nos templates parâmetros que são inseridos dentro dos prompts que trazem os dados que são dinâmicos, enquanto parte do prompt continua constante.

A _temperatura_ do chat é o grau de criatividade dado a conversão. Quanto menor, menor menos criativo e randômico ele será, em tese diminui as possibilidades de alucinações, embora os resultados podem ficar mais simples.

Vamos manter zero para extrair o máximo de informação útil, com o mínimo de ruídos.


```python
openai_chat = ChatOpenAI(temperature=0.0, model=model)
openai_chat
```




    ChatOpenAI(client=<openai.resources.chat.completions.completions.Completions object at 0x10ec5ff40>, async_client=<openai.resources.chat.completions.completions.AsyncCompletions object at 0x10ef06350>, root_client=<openai.OpenAI object at 0x10dadc700>, root_async_client=<openai.AsyncOpenAI object at 0x10ec5ffa0>, model_name='gpt-4.1-mini', temperature=0.0, model_kwargs={}, openai_api_key=SecretStr('**********'))



Vamos criar prompts para analisar comentários de clientes de modo a toda avaliação seja excluído palavras de ofensa.


```python
style = """
Tom calmo, sintético e respeitoso, sem incluir nenhuma ofensa ou algo parecido.
"""
```


```python
template_string = """
Analise a revisão de cliente para que sejam resumidas em no máximo 300 caracteres.
Utilize o seguinte estilo {style}.
A revisão é a seguinte: {customer_review}
"""
```


```python
from langchain.prompts import ChatPromptTemplate

prompt_template = ChatPromptTemplate.from_template(template_string)
```


```python
prompt_template.messages[0].prompt
```




    PromptTemplate(input_variables=['customer_review', 'style'], input_types={}, partial_variables={}, template='\nAnalise a revisão de cliente para que sejam resumidas em no máximo 300 caracteres.\nUtilize o seguinte estilo {style}.\nA revisão é a seguinte: {customer_review}\n')




```python
prompt_template.messages[0].prompt.input_variables
```




    ['customer_review', 'style']




```python
customer_review = """
Comprei um brinquedo que não presta! Utilizei todas as instruções
mas não funcionada nada! Que droga!
Se soubesse que fosse assim não teria comprado nessa loja ruim!!!
"""
```


```python
customer_messages = prompt_template.format_messages(
                    style=style,
                    customer_review=customer_review)
```

Analisando o resultado com a avaliação de exemplo.


```python
customer_messages
```




    [HumanMessage(content='\nAnalise a revisão de cliente para que sejam resumidas em no máximo 300 caracteres.\nUtilize o seguinte estilo \nTom calmo, sintético e respeitoso, sem incluir nenhuma ofensa ou algo parecido.\n.\nA revisão é a seguinte: \nComprei um brinquedo que não presta! Utilizei todas as instruções\nmas não funcionada nada! Que droga!\nSe soubesse que fosse assim não teria comprado nessa loja ruim!!!\n\n', additional_kwargs={}, response_metadata={})]




```python
customer_response = openai_chat.invoke(customer_messages)
```


```python
customer_response.content
```




    'O produto não funcionou conforme as instruções, o que gerou insatisfação. Esperava uma experiência melhor ao comprar na loja.'



Vamos obter uma base de revisões de clientes em sites de comércio eletrônico.
Com esta base vamos sintetizar as reclamações para padronizar análises, removendo palavras e expressões que impactariam muito pouco na avaliação como um todo.


```python
!git clone https://github.com/michelpf/dataset-customer-evaluations
```

    Cloning into 'dataset-customer-evaluations'...
    remote: Enumerating objects: 11, done.[K
    remote: Counting objects: 100% (11/11), done.[K
    remote: Compressing objects: 100% (10/10), done.[K
    remote: Total 11 (delta 1), reused 7 (delta 0), pack-reused 0 (from 0)[K
    Receiving objects: 100% (11/11), 2.52 MiB | 6.33 MiB/s, done.
    Resolving deltas: 100% (1/1), done.
    


```python
df = pd.read_csv("dataset-customer-evaluations/dataset/am_scrape_final.csv")
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Search Query</th>
      <th>Product Title</th>
      <th>Link</th>
      <th>Review</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>smartphone</td>
      <td>Smartphone Xiaomi Note 12 4G 128GB 6GB Ram (VE...</td>
      <td>https://www.amazon.com.br/dp/B0BZ7RJDHD</td>
      <td>Com a necessidade de comprar um celular custo ...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>smartphone</td>
      <td>Smartphone Xiaomi Note 12 4G 128GB 6GB Ram (VE...</td>
      <td>https://www.amazon.com.br/dp/B0BZ7RJDHD</td>
      <td>Minha experiência de 10 dias de uso com o Xiao...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>smartphone</td>
      <td>Smartphone Xiaomi Note 12 4G 128GB 6GB Ram (VE...</td>
      <td>https://www.amazon.com.br/dp/B0BZ7RJDHD</td>
      <td>Smartphone de qualidade como já esperava, boas...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>smartphone</td>
      <td>Smartphone Xiaomi Note 12 4G 128GB 6GB Ram (VE...</td>
      <td>https://www.amazon.com.br/dp/B0BZ7RJDHD</td>
      <td>atendeu mto minhas expectativas. Antes  eu usa...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>smartphone</td>
      <td>Smartphone Xiaomi Note 12 4G 128GB 6GB Ram (VE...</td>
      <td>https://www.amazon.com.br/dp/B0BZ7RJDHD</td>
      <td>Gostei muito do celular, tem resposta rápida e...</td>
    </tr>
  </tbody>
</table>
</div>



Note que as revisões podem ser extensas, mas não necessáriamente contém informações relevantes em todas as frases.


```python
df.iloc[0]["Review"]
```




    'Com a necessidade de comprar um celular custo benefício comecei pesquisando os modelos que mais vendem no mercado e me deparei com os líderes de sempre: Samsung, Motorola...Apple não é custo-benefício aqui no Brasil.Eu já tive smartphones dessas marcas supracitadas, mas nunca da Xiaomi.Por conseguinte, analisei vários vídeos e tinham varias opções (não cabem ser citadas agora) que entregavam uma boa qualidade de apenas algumas características, porém o conjunto completo deixava sempre a desejar.Partindo da premissa que eu saí de um celular no seguinte estado:-Marca: Samsung-Modelo: Gran prime-Ano de lançamento: 2015-Armazenamento: apenas 8gb de memória interna.Considerando também o valor do mercado atual dos Smartphones: CATASTRÓFICO.Associo que a escolha da marca condiz com o meu objetivo:- Precisava de uma boa tela ( essa tela é a melhor do mercado para esses celulares de entrada).- Tinha a necessidade de uma boa bateria ( essa faz jus à marca, sem contar com o carregamento ultra rápido que eles ainda oferecem).- Buscava uma boa câmera ( não tira as melhores fotos do mercado, mas nenhum dos celulares dessa categoria vem com as mesmas funções dos top de linha).- Visava um celular que não travasse ( esse tem 6gb de memória ram, roda até os jogos mais pesados da play story como: Call Of Duty, Genshin Impact, bem como já cheguei até a jogar um pouco de The Legend of Neverland, não roda com os gráficos no máximo sem leg, pois essa não é uma das propostas da faixa de preço desse aparelho, mas dá pra jogar todos os jogos tranquilamente em modo mais light).Portanto, a aquisição desse aparelho está suprindo minhas necessidades e veio melhor do que eu pensava.Já fiz até a aquisição de outro para uma amiga.Comprem sem medo e boa sorte com o produto.'




```python
customer_messages = prompt_template.format_messages(
                    style=style,
                    customer_review=df.iloc[0]["Review"])

customer_response = openai_chat.invoke(customer_messages)
```


```python
customer_response.content
```




    'O celular escolhido oferece boa tela, bateria duradoura com carregamento rápido e desempenho satisfatório para jogos leves, atendendo bem às necessidades do usuário. Apesar de não ter a melhor câmera, o aparelho supera expectativas na faixa de preço. Recomendo a compra com confiança.'




```python
df.iloc[5]["Review"]
```




    'Gente, é barato e muito bom mesmo; pelo menos até o momento rs. Comprei com 6 g RAM e estou amando, o bicho tá parecendo um foguete de rápido kkkkk.  Estou usando há um mês e meio; o verde é lindo 😍, e uma das coisas que mais me chamou atenção até o momento, é a durabilidade da bateria. Castiguei ele com aplicativos que consomem muita energia e a bateria aguentou 20 horas numa boa, coloquei para carregar pq iria sair e não queria levar o carregador, mas dura muuuuuito, o meu anterior (Zenfone, que tbm é muito bom, a bateria não aguentava 10 horas ou menos). Para carregar, também é mega rápido. E como utilizo o celular na maior parte do tempo para trabalho, consigo visualizar diversos documentos na gigante tela dele rs. Enfim ... até o momento, só elogios para o aparelho, vale a pena comprar.Obs* apenas demora par reiniciar, se estiver com pressa, ferrou! Mas considero isso um inconveniente bobo rs.'



Passando no prompt para resumir e também remover eventuais expressões e agressões.


```python
customer_messages = prompt_template.format_messages(
                    style=style,
                    customer_review=df.iloc[5]["Review"])

customer_response = openai_chat.invoke(customer_messages)
customer_response.content
```




    'O aparelho é rápido, com 6 GB de RAM e bateria duradoura, aguentando até 20 horas mesmo com uso intenso. A tela grande facilita o trabalho e o carregamento é rápido. O design verde é bonito. O único ponto negativo é a demora para reiniciar, mas não compromete a experiência.'



## Memória e contexto

Adiciona um efeito de contexto sobre as conversas realizadas, tornando a conversa (modelo chat) mais integrado podendo controlar a janela de memória em cada interação.

A memória sempre é passada ao modelo de LLM, portanto existem técnicas que tornam essa passagem mais inteligente, como a limitação de posições, tokens ou até mesmo o uso de sumários.


```python
from langchain.chains import ConversationChain
from langchain.memory import ConversationBufferMemory
```


```python
model = "gpt-3.5-turbo"
```

Conversação com memória simples.


```python
chat_openai = ChatOpenAI(temperature=0.0, model=model)

memory = ConversationBufferMemory()

conversation = ConversationChain(llm = chat_openai, memory = memory, verbose=True)
```


```python
conversation.predict(input="Olá, meu nome é Michel e sou o professor na FIAP")
```

    
    
    [1m> Entering new ConversationChain chain...[0m
    Prompt after formatting:
    [32;1m[1;3mThe following is a friendly conversation between a human and an AI. The AI is talkative and provides lots of specific details from its context. If the AI does not know the answer to a question, it truthfully says it does not know.
    
    Current conversation:
    
    Human: Olá, meu nome é Michel e sou o professor na FIAP
    AI:[0m
    
    [1m> Finished chain.[0m
    




    'Olá, Michel! Que prazer falar com você. A FIAP é uma instituição incrível, conhecida por sua excelência em tecnologia e inovação. Como professor aí, você deve estar envolvido com muitos projetos interessantes. Em que área você atua? Posso ajudar com algo específico relacionado ao seu trabalho ou à FIAP?'




```python
conversation.predict(input="Você conhece a FIAP?")
```

    
    
    [1m> Entering new ConversationChain chain...[0m
    Prompt after formatting:
    [32;1m[1;3mThe following is a friendly conversation between a human and an AI. The AI is talkative and provides lots of specific details from its context. If the AI does not know the answer to a question, it truthfully says it does not know.
    
    Current conversation:
    Human: Olá, meu nome é Michel e sou o professor na FIAP
    AI: Olá, Michel! Que prazer falar com você. A FIAP é uma instituição incrível, conhecida por sua excelência em tecnologia e inovação. Como professor aí, você deve estar envolvido com muitos projetos interessantes. Em que área você atua? Posso ajudar com algo específico relacionado ao seu trabalho ou à FIAP?
    Human: Você conhece a FIAP?
    AI:[0m
    
    [1m> Finished chain.[0m
    




    'Sim, conheço a FIAP! A FIAP, ou Faculdade de Informática e Administração Paulista, é uma instituição de ensino superior bastante renomada no Brasil, especialmente nas áreas de tecnologia, inovação e negócios. Fundada em 1972, a FIAP tem uma forte reputação por seus cursos de graduação, pós-graduação e MBA, além de ser reconhecida por sua conexão com o mercado de trabalho e parcerias com empresas de tecnologia. A instituição também é conhecida por incentivar o empreendedorismo e a pesquisa aplicada, oferecendo laboratórios modernos e programas de incubação de startups. Você gostaria que eu falasse mais sobre algum curso específico, projetos de pesquisa ou iniciativas da FIAP?'




```python
conversation.predict(input="Qual é o meu nome e onde eu trabalho?")
```

    
    
    [1m> Entering new ConversationChain chain...[0m
    Prompt after formatting:
    [32;1m[1;3mThe following is a friendly conversation between a human and an AI. The AI is talkative and provides lots of specific details from its context. If the AI does not know the answer to a question, it truthfully says it does not know.
    
    Current conversation:
    Human: Olá, meu nome é Michel e sou o professor na FIAP
    AI: Olá, Michel! Que prazer falar com você. A FIAP é uma instituição incrível, conhecida por sua excelência em tecnologia e inovação. Como professor aí, você deve estar envolvido com muitos projetos interessantes. Em que área você atua? Posso ajudar com algo específico relacionado ao seu trabalho ou à FIAP?
    Human: Você conhece a FIAP?
    AI: Sim, conheço a FIAP! A FIAP, ou Faculdade de Informática e Administração Paulista, é uma instituição de ensino superior bastante renomada no Brasil, especialmente nas áreas de tecnologia, inovação e negócios. Fundada em 1972, a FIAP tem uma forte reputação por seus cursos de graduação, pós-graduação e MBA, além de ser reconhecida por sua conexão com o mercado de trabalho e parcerias com empresas de tecnologia. A instituição também é conhecida por incentivar o empreendedorismo e a pesquisa aplicada, oferecendo laboratórios modernos e programas de incubação de startups. Você gostaria que eu falasse mais sobre algum curso específico, projetos de pesquisa ou iniciativas da FIAP?
    Human: Qual é o meu nome e onde eu trabalho?
    AI:[0m
    
    [1m> Finished chain.[0m
    




    'Seu nome é Michel e você trabalha como professor na FIAP. Posso ajudar com mais alguma coisa relacionada ao seu trabalho ou à instituição?'




```python
print(memory.buffer)
```

    Human: Olá, meu nome é Michel e sou o professor na FIAP
    AI: Olá, Michel! Que prazer falar com você. A FIAP é uma instituição incrível, conhecida por sua excelência em tecnologia e inovação. Como professor aí, você deve estar envolvido com muitos projetos interessantes. Em que área você atua? Posso ajudar com algo específico relacionado ao seu trabalho ou à FIAP?
    Human: Você conhece a FIAP?
    AI: Sim, conheço a FIAP! A FIAP, ou Faculdade de Informática e Administração Paulista, é uma instituição de ensino superior bastante renomada no Brasil, especialmente nas áreas de tecnologia, inovação e negócios. Fundada em 1972, a FIAP tem uma forte reputação por seus cursos de graduação, pós-graduação e MBA, além de ser reconhecida por sua conexão com o mercado de trabalho e parcerias com empresas de tecnologia. A instituição também é conhecida por incentivar o empreendedorismo e a pesquisa aplicada, oferecendo laboratórios modernos e programas de incubação de startups. Você gostaria que eu falasse mais sobre algum curso específico, projetos de pesquisa ou iniciativas da FIAP?
    Human: Qual é o meu nome e onde eu trabalho?
    AI: Seu nome é Michel e você trabalha como professor na FIAP. Posso ajudar com mais alguma coisa relacionada ao seu trabalho ou à instituição?
    


```python
memory.load_memory_variables({})
```




    {'history': 'Human: Olá, meu nome é Michel e sou o professor na FIAP\nAI: Olá, Michel! Que prazer falar com você. A FIAP é uma instituição incrível, conhecida por sua excelência em tecnologia e inovação. Como professor aí, você deve estar envolvido com muitos projetos interessantes. Em que área você atua? Posso ajudar com algo específico relacionado ao seu trabalho ou à FIAP?\nHuman: Você conhece a FIAP?\nAI: Sim, conheço a FIAP! A FIAP, ou Faculdade de Informática e Administração Paulista, é uma instituição de ensino superior bastante renomada no Brasil, especialmente nas áreas de tecnologia, inovação e negócios. Fundada em 1972, a FIAP tem uma forte reputação por seus cursos de graduação, pós-graduação e MBA, além de ser reconhecida por sua conexão com o mercado de trabalho e parcerias com empresas de tecnologia. A instituição também é conhecida por incentivar o empreendedorismo e a pesquisa aplicada, oferecendo laboratórios modernos e programas de incubação de startups. Você gostaria que eu falasse mais sobre algum curso específico, projetos de pesquisa ou iniciativas da FIAP?\nHuman: Qual é o meu nome e onde eu trabalho?\nAI: Seu nome é Michel e você trabalha como professor na FIAP. Posso ajudar com mais alguma coisa relacionada ao seu trabalho ou à instituição?'}




```python
memory.save_context({"input": "Eu tenho dois filhos, uma de 10 anos e um de 6 anos."},
                    {"output": "Legal, então você tem um casal."})
```


```python
memory.load_memory_variables({})
```




    {'history': 'Human: Olá, meu nome é Michel e sou o professor na FIAP\nAI: Olá, Michel! Que prazer falar com você. A FIAP é uma instituição incrível, conhecida por sua excelência em tecnologia e inovação. Como professor aí, você deve estar envolvido com muitos projetos interessantes. Em que área você atua? Posso ajudar com algo específico relacionado ao seu trabalho ou à FIAP?\nHuman: Você conhece a FIAP?\nAI: Sim, conheço a FIAP! A FIAP, ou Faculdade de Informática e Administração Paulista, é uma instituição de ensino superior bastante renomada no Brasil, especialmente nas áreas de tecnologia, inovação e negócios. Fundada em 1972, a FIAP tem uma forte reputação por seus cursos de graduação, pós-graduação e MBA, além de ser reconhecida por sua conexão com o mercado de trabalho e parcerias com empresas de tecnologia. A instituição também é conhecida por incentivar o empreendedorismo e a pesquisa aplicada, oferecendo laboratórios modernos e programas de incubação de startups. Você gostaria que eu falasse mais sobre algum curso específico, projetos de pesquisa ou iniciativas da FIAP?\nHuman: Qual é o meu nome e onde eu trabalho?\nAI: Seu nome é Michel e você trabalha como professor na FIAP. Posso ajudar com mais alguma coisa relacionada ao seu trabalho ou à instituição?\nHuman: Eu tenho dois filhos, uma de 10 anos e um de 6 anos.\nAI: Legal, então você tem um casal.'}




```python
conversation.predict(input="Em que ano meus filhos nasceram, sabendo que estamos em 2023?")
```

    
    
    [1m> Entering new ConversationChain chain...[0m
    Prompt after formatting:
    [32;1m[1;3mThe following is a friendly conversation between a human and an AI. The AI is talkative and provides lots of specific details from its context. If the AI does not know the answer to a question, it truthfully says it does not know.
    
    Current conversation:
    Human: Olá, meu nome é Michel e sou o professor na FIAP
    AI: Olá, Michel! Que prazer falar com você. A FIAP é uma instituição incrível, conhecida por sua excelência em tecnologia e inovação. Como professor aí, você deve estar envolvido com muitos projetos interessantes. Em que área você atua? Posso ajudar com algo específico relacionado ao seu trabalho ou à FIAP?
    Human: Você conhece a FIAP?
    AI: Sim, conheço a FIAP! A FIAP, ou Faculdade de Informática e Administração Paulista, é uma instituição de ensino superior bastante renomada no Brasil, especialmente nas áreas de tecnologia, inovação e negócios. Fundada em 1972, a FIAP tem uma forte reputação por seus cursos de graduação, pós-graduação e MBA, além de ser reconhecida por sua conexão com o mercado de trabalho e parcerias com empresas de tecnologia. A instituição também é conhecida por incentivar o empreendedorismo e a pesquisa aplicada, oferecendo laboratórios modernos e programas de incubação de startups. Você gostaria que eu falasse mais sobre algum curso específico, projetos de pesquisa ou iniciativas da FIAP?
    Human: Qual é o meu nome e onde eu trabalho?
    AI: Seu nome é Michel e você trabalha como professor na FIAP. Posso ajudar com mais alguma coisa relacionada ao seu trabalho ou à instituição?
    Human: Eu tenho dois filhos, uma de 10 anos e um de 6 anos.
    AI: Legal, então você tem um casal.
    Human: Em que ano meus filhos nasceram, sabendo que estamos em 2023?
    AI:[0m
    
    [1m> Finished chain.[0m
    




    'Se estamos em 2023, seu filho de 10 anos provavelmente nasceu em 2013, e sua filha de 6 anos provavelmente nasceu em 2017. Claro, isso pode variar um pouco dependendo do mês de nascimento deles, mas essas seriam as estimativas mais prováveis! Posso ajudar com mais alguma coisa?'




```python
from langchain.memory import ConversationBufferWindowMemory
```

Adicionando limitação de entradas (valor k).


```python
memory_limited = ConversationBufferWindowMemory(k=1)
```


```python
memory_limited.save_context({"input": "Estudei Engenharia Elétrica na FEI em São Bernardo do Campo."},
                    {"output": "Muito bom! É uma faculdade reconhecida nesta área."})

memory_limited.save_context({"input": "Eu nasci na cidade de São Paulo."},
                    {"output": "Legal!"})
```


```python
memory_limited.load_memory_variables({})
```




    {'history': 'Human: Eu nasci na cidade de São Paulo.\nAI: Legal!'}




```python
conversation = ConversationChain(llm = chat_openai, memory = memory_limited, verbose=True)
```


```python
conversation.predict(input="Onde eu me estudei?")
```

    
    
    [1m> Entering new ConversationChain chain...[0m
    Prompt after formatting:
    [32;1m[1;3mThe following is a friendly conversation between a human and an AI. The AI is talkative and provides lots of specific details from its context. If the AI does not know the answer to a question, it truthfully says it does not know.
    
    Current conversation:
    Human: Eu nasci na cidade de São Paulo.
    AI: Legal!
    Human: Onde eu me estudei?
    AI:[0m
    
    [1m> Finished chain.[0m
    




    'Eu não sei onde você estudou, mas posso te contar que São Paulo tem muitas opções de escolas e universidades renomadas, como a Universidade de São Paulo (USP), a Universidade Estadual Paulista (UNESP) e o Colégio Bandeirantes, que é bastante tradicional. Se quiser, posso ajudar a encontrar informações sobre instituições de ensino na sua região!'




```python
conversation.predict(input="Onde eu nasci?")
```

    
    
    [1m> Entering new ConversationChain chain...[0m
    Prompt after formatting:
    [32;1m[1;3mThe following is a friendly conversation between a human and an AI. The AI is talkative and provides lots of specific details from its context. If the AI does not know the answer to a question, it truthfully says it does not know.
    
    Current conversation:
    Human: Onde eu me estudei?
    AI: Eu não sei onde você estudou, mas posso te contar que São Paulo tem muitas opções de escolas e universidades renomadas, como a Universidade de São Paulo (USP), a Universidade Estadual Paulista (UNESP) e o Colégio Bandeirantes, que é bastante tradicional. Se quiser, posso ajudar a encontrar informações sobre instituições de ensino na sua região!
    Human: Onde eu nasci?
    AI:[0m
    
    [1m> Finished chain.[0m
    




    'Eu não sei onde você nasceu, pois não tenho acesso a informações pessoais suas. Mas, se quiser, posso compartilhar curiosidades sobre diferentes cidades e estados do Brasil, ou ajudar a encontrar dados históricos e culturais sobre o lugar onde você nasceu!'




```python
from langchain.memory import ConversationTokenBufferMemory
```

Adicioando limitação por tokens.


```python
memory_limited = ConversationTokenBufferMemory(llm=chat_openai, max_token_limit=10)
```


```python
memory_limited.save_context({"input": "Estudei Engenharia Elétrica na FEI em São Bernardo do Campo."},
                    {"output": "Muito bom! É uma faculdade reconhecida nesta área."})

memory_limited.save_context({"input": "Eu nasci na cidade de São Paulo."},
                    {"output": "Legal!"})
```


```python
memory_limited.load_memory_variables({})
```




    {'history': 'AI: Legal!'}



Utilizando sumarização para carregar a memória. Esta funcionalidade utilizada o próprio LLM para gerar o resumo e o carrega como efeito de memória.


```python
from langchain.memory import ConversationSummaryBufferMemory
```


```python
memory_limited = ConversationSummaryBufferMemory(llm=chat_openai, max_token_limit=100)
```


```python
memory_limited.save_context(
    {"input": "Estudei Engenharia Elétrica na FEI em São Bernardo do Campo, referência na área e que formou muitos profissionais."},
    {"output": "Muito bom! É uma faculdade reconhecida nesta área."})

memory_limited.save_context(
    {"input": "Eu nasci na cidade de São Paulo, capital do Estado. Esta cidade é uma das maiores do Brasil.\
    Nesta cidadade as pessoas costumam ir ao trabalho utilizando transporte público.\
    O transporte público mais preferido é o metrô, seguido do trem e do ônibus.\
    Muitas pessoas optam por ir de carro, mas as restrições como o rodízio de veículos tornam a necessidade \
    de ter mais de um carro ou ir de transporte público em algum dia da semana."},
    {"output": "Legal!"})
```


```python
memory_limited.load_memory_variables({})
```




    {'history': 'System: O humano compartilha que estudou Engenharia Elétrica na FEI em São Bernardo do Campo, uma faculdade de referência na área. O AI elogia a faculdade. O humano então fala sobre ter nascido em São Paulo, uma das maiores cidades do Brasil, onde as pessoas costumam utilizar o transporte público para ir ao trabalho, sendo o metrô o meio mais preferido, seguido pelo trem e ônibus. Muitas pessoas também optam por ir de carro, mas as restrições como o rodízio de veículos tornam necessário ter mais de um carro ou utilizar o transporte público em algum dia da semana.\nAI: Legal!'}




```python
conversation = ConversationChain(
    llm=chat_openai,
    memory = memory_limited,
    verbose=True
)
```


```python
conversation.predict(input="Onde eu me formei?")
```

    
    
    [1m> Entering new ConversationChain chain...[0m
    Prompt after formatting:
    [32;1m[1;3mThe following is a friendly conversation between a human and an AI. The AI is talkative and provides lots of specific details from its context. If the AI does not know the answer to a question, it truthfully says it does not know.
    
    Current conversation:
    System: O humano compartilha que estudou Engenharia Elétrica na FEI em São Bernardo do Campo, uma faculdade de referência na área. O AI elogia a faculdade. O humano então fala sobre ter nascido em São Paulo, uma das maiores cidades do Brasil, onde as pessoas costumam utilizar o transporte público para ir ao trabalho, sendo o metrô o meio mais preferido, seguido pelo trem e ônibus. Muitas pessoas também optam por ir de carro, mas as restrições como o rodízio de veículos tornam necessário ter mais de um carro ou utilizar o transporte público em algum dia da semana.
    AI: Legal!
    Human: Onde eu me formei?
    AI:[0m
    
    [1m> Finished chain.[0m
    




    'Você se formou na FEI em Engenharia Elétrica.'




```python
memory_limited.chat_memory.messages
```




    [AIMessage(content='Legal!', additional_kwargs={}, response_metadata={}),
     HumanMessage(content='Onde eu me formei?', additional_kwargs={}, response_metadata={}),
     AIMessage(content='Você se formou na FEI em Engenharia Elétrica.', additional_kwargs={}, response_metadata={})]



##Dados externos com word embeddings

O framework permite criar embeddings a partir de arquivos externos para tornar mais leve a quantidade de dados que é enviado aos LLMs.

Com este técnica, suponha que tenhamos um livro com 1000 páginas, mas somente 1 página é necessária para as análises, os embeddings ajudarão a separar a parte de interesse off-line e enviará somente a página para o modelo de LLM.

Vamos utilizar o mesmo dataset de avaliações de clientes.


```python
!git clone https://github.com/michelpf/dataset-customer-evaluations
```

    fatal: destination path 'dataset-customer-evaluations' already exists and is not an empty directory.
    

Neste caso, vamos utilizar os dados do Mercado Livre.


```python
df = pd.read_csv("dataset-customer-evaluations/dataset/ml_scrape_final.csv")
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Pesquisa</th>
      <th>Titulo</th>
      <th>Link</th>
      <th>Comentario</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>smartphone</td>
      <td>Smartphone Samsung Galaxy A14 Dual 6.6 128gb P...</td>
      <td>https://produto.mercadolivre.com.br/MLB-331518...</td>
      <td>A foto fica amarelada quando eu vou fotografar...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>smartphone</td>
      <td>Smartphone Samsung Galaxy A14 Dual 6.6 128gb P...</td>
      <td>https://produto.mercadolivre.com.br/MLB-331518...</td>
      <td>👏🏼👏🏼👏🏼👏🏼👏🏼👏🏼.</td>
    </tr>
    <tr>
      <th>2</th>
      <td>smartphone</td>
      <td>Smartphone Samsung Galaxy A14 Dual 6.6 128gb P...</td>
      <td>https://produto.mercadolivre.com.br/MLB-331518...</td>
      <td>Muito bom.</td>
    </tr>
    <tr>
      <th>3</th>
      <td>smartphone</td>
      <td>Smartphone Samsung Galaxy A14 Dual 6.6 128gb P...</td>
      <td>https://produto.mercadolivre.com.br/MLB-331518...</td>
      <td>Produto muito bom dei de presente pra meu filh...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>smartphone</td>
      <td>Smartphone Samsung Galaxy A14 Dual 6.6 128gb P...</td>
      <td>https://produto.mercadolivre.com.br/MLB-331518...</td>
      <td>Recomendo.</td>
    </tr>
  </tbody>
</table>
</div>



Para o arquivo não ficar muito grande, vamos limitar o produto para smartphone.


```python
df.query("Pesquisa == 'smartphone'").to_csv("smartphone_review.csv")
```


```python
from langchain_openai import ChatOpenAI
from langchain.document_loaders import CSVLoader
from langchain.vectorstores import DocArrayInMemorySearch
from langchain.indexes import VectorstoreIndexCreator
from langchain_openai import OpenAIEmbeddings
from langchain_community.vectorstores import Chroma
```

Carregamos o arquivo no CSVLoader. Cada tipo de arquivo tem um loader diferente.


```python
loader = CSVLoader(file_path="smartphone_review.csv")
```

É criado um armazenamento vetorial, que são os embeddings com índices associados para as buscas.


```python
embedding_model = OpenAIEmbeddings(chunk_size=10)
index = VectorstoreIndexCreator(vectorstore_cls=Chroma, embedding=embedding_model).from_loaders([loader])
```


```python
query = "List 3 revisões muito ruins de clientes, listando com o modelo e o link do produto."
```


```python
chat_openai = ChatOpenAI(temperature=0.0, model=model)
```


```python
response = index.query(query, verbose=True, llm=chat_openai)
```

    
    
    [1m> Entering new RetrievalQA chain...[0m
    
    [1m> Finished chain.[0m
    


```python
display_markdown(response)
```




1. **Smartphone Motorola Moto G22 Dual 6,5 128gb 4gb Ram Azul**
   - **Comentário:** O smartphone é muito lento, realmente se alguém tivesse me dito isso antes não teria comprado.
   - **Link:** [Ver produto](https://produto.mercadolivre.com.br/MLB-2660980219-smartphone-motorola-moto-g22-dual-65-128gb-4gb-ram-azul-_JM#position=48&search_layout=stack&type=item&tracking_id=4703b69b-1b0f-4cc2-b281-a641f81b3281)

2. **Smartphone Moto E13 32gb Tela 6.5'' 2gb Ram Grafite Motorola**
   - **Comentário:** O áudio do celular estar ruim e os vídeos sair ruim tbm.
   - **Link:** [Ver produto](https://produto.mercadolivre.com.br/MLB-3140442935-smartphone-moto-e13-32gb-tela-65-2gb-ram-grafite-motorola-_JM#position=54&search_layout=stack&type=item&tracking_id=4703b69b-1b0f-4cc2-b281-a641f81b3281)

3. **Smartphone Samsung Galaxy A14 Dual 6.6 128gb Preto 4gb Ram**
   - **Comentário:** A foto fica amarelada quando eu vou fotografar com celular.
   - **Link:** [Ver produto](https://produto.mercadolivre.com.br/MLB-3315181641-smartphone-samsung-galaxy-a14-dual-66-128gb-preto-4gb-ram-_JM#position=46&search_layout=stack&type=item&tracking_id=4703b69b-1b0f-4cc2-b281-a641f81b3281)



Também podemos citar as fontes dos documentos utilizados.


```python
response = index.query_with_sources(query, verbose=True, llm=chat_openai)
```

    
    
    [1m> Entering new RetrievalQAWithSourcesChain chain...[0m
    
    [1m> Finished chain.[0m
    


```python
response
```




    {'question': 'List 3 revisões muito ruins de clientes, listando com o modelo e o link do produto.',
     'answer': "1. Modelo: Smartphone Motorola Moto G22 Dual 6,5 128gb 4gb Ram Azul\n   Link: https://produto.mercadolivre.com.br/MLB-2660980219-smartphone-motorola-moto-g22-dual-65-128gb-4gb-ram-azul-_JM#position=48&search_layout=stack&type=item&tracking_id=4703b69b-1b0f-4cc2-b281-a641f81b3281\n   Revisão Ruim: O smartphone é muito lento, realmente se alguém tivesse me dito isso antes não teria comprado.\n\n2. Modelo: Smartphone Moto E13 32gb Tela 6.5'' 2gb Ram Grafite Motorola\n   Link: https://produto.mercadolivre.com.br/MLB-3140442935-smartphone-moto-e13-32gb-tela-65-2gb-ram-grafite-motorola-_JM#position=54&search_layout=stack&type=item&tracking_id=4703b69b-1b0f-4cc2-b281-a641f81b3281\n   Revisão Ruim: O áudio do celular estar ruim e os vídeos sair ruim tbm.\n\n3. Modelo: Smartphone Samsung Galaxy A14 Dual 6.6 128gb Preto 4gb Ram\n   Link: https://produto.mercadolivre.com.br/MLB-3315181641-smartphone-samsung-galaxy-a14-dual-66-128gb-preto-4gb-ram-_JM#position=46&search_layout=stack&type=item&tracking_id=4703b69b-1b0f-4cc2-b281-a641f81b3281\n   Revisão Ruim: A foto fica amarelada quando eu vou fotografar com celular.\n",
     'sources': 'smartphone_review.csv'}




```python
chat_openai = ChatOpenAI(temperature=0.0, model=model)
```

Adicionamente podemos utilizar um loader de PDF e utilizar outros tipos de indexação offline.


```python
!pip install pypdf faiss-cpu
```

    Collecting pypdf
      Downloading pypdf-5.4.0-py3-none-any.whl.metadata (7.3 kB)
    Collecting faiss-cpu
      Downloading faiss_cpu-1.11.0-cp310-cp310-macosx_14_0_arm64.whl.metadata (4.8 kB)
    Requirement already satisfied: typing_extensions>=4.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from pypdf) (4.13.2)
    Requirement already satisfied: numpy<3.0,>=1.25.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from faiss-cpu) (2.2.5)
    Requirement already satisfied: packaging in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from faiss-cpu) (23.2)
    Downloading pypdf-5.4.0-py3-none-any.whl (302 kB)
    Downloading faiss_cpu-1.11.0-cp310-cp310-macosx_14_0_arm64.whl (3.3 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m3.3/3.3 MB[0m [31m6.0 MB/s[0m eta [36m0:00:00[0ma [36m0:00:01[0m
    [?25hInstalling collected packages: pypdf, faiss-cpu
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m2/2[0m [faiss-cpu]/2[0m [faiss-cpu]
    [1A[2KSuccessfully installed faiss-cpu-1.11.0 pypdf-5.4.0
    


```python
from langchain.document_loaders import PyPDFLoader

loader = PyPDFLoader("docs/SM-A146M_Emb_BR_Rev.1.2.pdf")
pages = loader.load_and_split()
```

A indexação offline [FAISS](https://python.langchain.com/docs/integrations/vectorstores/faiss) é do Facebook para buscar similiaridade nos textos.


```python
from langchain.vectorstores import FAISS
from langchain_openai import OpenAIEmbeddings

faiss_index = FAISS.from_documents(pages, OpenAIEmbeddings(chunk_size=10))
```


```python
query = "O que fazer se o aparelho não liga?"
docs = faiss_index.similarity_search(query)
docs
```




    [Document(id='68bf9fda-7498-42f7-9ca2-ed46d6d35c4f', metadata={'producer': 'PyPDF', 'creator': 'PyPDF', 'creationdate': '2023-02-13T14:42:22-04:00', 'moddate': '2023-02-13T14:42:22-04:00', 'source': 'docs/SM-A146M_Emb_BR_Rev.1.2.pdf', 'total_pages': 114, 'page': 12, 'page_label': '13'}, page_content='Primeiros passos\n13\nLigar ou desligar seu aparelho\nSiga todos os avisos e instruções recomendadas pelo pessoal autorizado em áreas \nonde aparelhos sem fio são proibidos, tais como aviões e hospitais.\nTecla Lateral\nLigar o aparelho\nMantenha pressionada a Tecla Lateral por alguns segundos para ligar o aparelho.\nDesligar o aparelho\n1 Para desligar o aparelho, mantenha as teclas Lateral e Diminuir volume pressionadas \nsimultaneamente. Como alternativa, abra o painel de notificações e toque em \n .\n2 Toque em Desligar.\nPara reiniciar o aparelho, toque em Reiniciar.\nForçar reinício\nSe o seu aparelho estiver travado e sem operação, mantenha as teclas Lateral e \nDiminuir volume pressionadas simultaneamente por aproximadamente 7 segundos \npara reiniciá-lo.\nConfiguração inicial\nAo ligar pela primeira vez ou após executar uma restauração de dados, siga as instruções \nna tela para configurá-lo.\nSe você não se conectar a uma rede Wi-Fi, você não conseguirá definir algumas \nfunções do aparelho durante a configuração inicial.'),
     Document(id='ef598c17-0184-4f31-a607-765f6d4c81ba', metadata={'producer': 'PyPDF', 'creator': 'PyPDF', 'creationdate': '2023-02-13T14:42:22-04:00', 'moddate': '2023-02-13T14:42:22-04:00', 'source': 'docs/SM-A146M_Emb_BR_Rev.1.2.pdf', 'total_pages': 114, 'page': 107, 'page_label': '108'}, page_content='Apêndice\n108\nApêndice\nSolução de problemas\nAntes de contatar a Central de Atendimento Samsung, tente as seguintes soluções. \nAlgumas situações podem não se aplicar ao seu aparelho.\nVocê também pode usar o Samsung Members para resolver qualquer problema que \nencontre enquanto usa o aparelho.\nAo ligar seu aparelho ou enquanto o usa, a inserção de um dos \nseguintes códigos pode ser solicitada:\n•  Senha: quando a função de bloqueio do aparelho está ativada, você precisa inserir \na senha que configurou para desbloqueá-lo.\n•  PIN: ao usar o aparelho pela primeira vez ou quando a solicitação de PIN está \nativada, você precisa inserir o PIN fornecido com seu chip. Você pode desativar \nessa função. Na tela de configurações, toque em Segurança e privacidade → \nOutras config. de segurança → Conf. bloqueio cartão SIM.\n•  PUK: seu chip bloqueia normalmente como resultado de inserir seu PIN \nincorretamente várias vezes. Você deve inserir o PUK fornecido pela sua operadora \nde serviços.\n•  PIN2: ao acessar um menu que requer o PIN2, precisa ser inserido o PIN2 fornecido \ncom o chip. Para maiores detalhes, contate sua operadora de serviços.\nSeu aparelho exibe mensagens de erro de rede ou falha no serviço\n•  Quando você está em áreas com sinal fraco ou recepção fraca, você pode perder a \nrecepção do sinal. Vá para outra área e tente novamente. Ao se mover, mensagens \nde erro podem aparecer repetidamente.\n•  Você pode não acessar algumas opções sem um plano de dados. Para maiores \ndetalhes, contate sua operadora de serviços.\nSeu aparelho não liga\nQuando a bateria estiver completamente descarregada, seu aparelho não ligará. \nCarregue a bateria completamente antes de ligar o aparelho.'),
     Document(id='624d8391-7c4c-443d-ab22-b70c4fd17b8b', metadata={'producer': 'PyPDF', 'creator': 'PyPDF', 'creationdate': '2023-02-13T14:42:22-04:00', 'moddate': '2023-02-13T14:42:22-04:00', 'source': 'docs/SM-A146M_Emb_BR_Rev.1.2.pdf', 'total_pages': 114, 'page': 109, 'page_label': '110'}, page_content='Apêndice\n110\nAs chamadas recebidas não são conectadas\n•  Certifique-se de que acessou a rede de telefonia correta.\n•  Certifique-se de que não configurou a restrição de chamada para o número que \nestá ligando.\n•  Certifique-se de que você não configurou a restrição de chamada para o número \nque está recebendo a chamada.\nAs pessoas não conseguem ouvi-lo durante uma chamada\n•  Certifique-se de que você não está bloqueando o microfone.\n•  Certifique-se de que o microfone está próximo à sua boca.\n•  Se você estiver utilizando um fone de ouvido, certifique-se de que ele está \ncorretamente conectado.\nO som ecoa durante uma chamada\nAjuste o volume ao pressionar a Tecla Volume ou vá para outra área.\nA rede móvel ou a internet é desconectada muitas vezes ou a \nqualidade do áudio é ruim\n•  Certifique-se de que você não está bloqueando a antena interna do aparelho.\n•  Quando você está em áreas com sinal fraco ou recepção fraca, você pode perder a \nrecepção do sinal. Você poderá ter problemas de conectividade devido a problemas \ncom a estação rádio base da operadora. Vá para outra área e tente novamente.\n•  Quando utilizar o aparelho em movimento, os serviços de rede sem fio poderão ser \ndesativados devido a problemas com a rede da operadora.\nA bateria não carrega corretamente (Em caso de utilizar \ncarregadores Samsung)\n•  Certifique-se de que o carregador está conectado corretamente.\n•  Visite um Centro de Serviços Samsung para trocar a bateria.\n•  Para carregadores não Samsung, contate o fabricante do carregador.'),
     Document(id='b09eb8aa-81ef-4329-b11f-d84f47c2e6b5', metadata={'producer': 'PyPDF', 'creator': 'PyPDF', 'creationdate': '2023-02-13T14:42:22-04:00', 'moddate': '2023-02-13T14:42:22-04:00', 'source': 'docs/SM-A146M_Emb_BR_Rev.1.2.pdf', 'total_pages': 114, 'page': 7, 'page_label': '8'}, page_content='Primeiros passos\n8\n • Para economizar energia, desconecte o carregador da tomada quando não \nestiver em uso. O carregador não possui um botão de liga e desliga, então você \ndeve retirar da tomada para evitar desperdício de energia. O carregador deve \npermanecer na tomada e facilmente acessível enquanto carregar.\n • Ao usar um carregador, é recomendável usar um aprovado que garanta o \ndesempenho do carregamento.\n • Se a bateria estiver completamente descarregada, você não conseguirá ligar o \naparelho, mesmo que o carregador esteja conectado. Aguarde a bateria carregar \npor alguns minutos antes de tentar ligar o aparelho.\n • Se utilizar vários aplicativos ao mesmo tempo, tais como aplicativos de rede ou \naplicativos que precisem da conexão de outro aparelho, a bateria descarregará \nrapidamente. Para evitar desconexão da rede ou esgotar a bateria durante \numa transferência de dados, use sempre esses aplicativos após carregar \ncompletamente a bateria.\n • Usar uma fonte de energia diferente do carregador USB, por exemplo, um \ncomputador, pode resultar em lentidão ao carregar a bateria devido à corrente \nelétrica ser mais baixa.\n • O aparelho pode ser utilizado enquanto carrega, porém pode levar mais tempo \npara carregar a bateria completamente.\n • Se o aparelho receber uma fonte de alimentação instável enquanto carrega, o \ntouch screen pode não funcionar. Se isto acontecer, desconecte o carregador.\n • O aparelho pode aquecer enquanto carrega. Isto é normal e não deve afetar a \nvida útil ou desempenho dele. Se a bateria aquecer mais do que o normal, o \ncarregador pode parar de funcionar.\n • Se o seu aparelho não carregar adequadamente, leve para um Centro de Serviços \nSamsung.')]




```python
for doc in docs:
    print(str(doc.metadata["page"]) + ":", doc.page_content)
```

    12: Primeiros passos
    13
    Ligar ou desligar seu aparelho
    Siga todos os avisos e instruções recomendadas pelo pessoal autorizado em áreas 
    onde aparelhos sem fio são proibidos, tais como aviões e hospitais.
    Tecla Lateral
    Ligar o aparelho
    Mantenha pressionada a Tecla Lateral por alguns segundos para ligar o aparelho.
    Desligar o aparelho
    1 Para desligar o aparelho, mantenha as teclas Lateral e Diminuir volume pressionadas 
    simultaneamente. Como alternativa, abra o painel de notificações e toque em 
     .
    2 Toque em Desligar.
    Para reiniciar o aparelho, toque em Reiniciar.
    Forçar reinício
    Se o seu aparelho estiver travado e sem operação, mantenha as teclas Lateral e 
    Diminuir volume pressionadas simultaneamente por aproximadamente 7 segundos 
    para reiniciá-lo.
    Configuração inicial
    Ao ligar pela primeira vez ou após executar uma restauração de dados, siga as instruções 
    na tela para configurá-lo.
    Se você não se conectar a uma rede Wi-Fi, você não conseguirá definir algumas 
    funções do aparelho durante a configuração inicial.
    107: Apêndice
    108
    Apêndice
    Solução de problemas
    Antes de contatar a Central de Atendimento Samsung, tente as seguintes soluções. 
    Algumas situações podem não se aplicar ao seu aparelho.
    Você também pode usar o Samsung Members para resolver qualquer problema que 
    encontre enquanto usa o aparelho.
    Ao ligar seu aparelho ou enquanto o usa, a inserção de um dos 
    seguintes códigos pode ser solicitada:
    •  Senha: quando a função de bloqueio do aparelho está ativada, você precisa inserir 
    a senha que configurou para desbloqueá-lo.
    •  PIN: ao usar o aparelho pela primeira vez ou quando a solicitação de PIN está 
    ativada, você precisa inserir o PIN fornecido com seu chip. Você pode desativar 
    essa função. Na tela de configurações, toque em Segurança e privacidade → 
    Outras config. de segurança → Conf. bloqueio cartão SIM.
    •  PUK: seu chip bloqueia normalmente como resultado de inserir seu PIN 
    incorretamente várias vezes. Você deve inserir o PUK fornecido pela sua operadora 
    de serviços.
    •  PIN2: ao acessar um menu que requer o PIN2, precisa ser inserido o PIN2 fornecido 
    com o chip. Para maiores detalhes, contate sua operadora de serviços.
    Seu aparelho exibe mensagens de erro de rede ou falha no serviço
    •  Quando você está em áreas com sinal fraco ou recepção fraca, você pode perder a 
    recepção do sinal. Vá para outra área e tente novamente. Ao se mover, mensagens 
    de erro podem aparecer repetidamente.
    •  Você pode não acessar algumas opções sem um plano de dados. Para maiores 
    detalhes, contate sua operadora de serviços.
    Seu aparelho não liga
    Quando a bateria estiver completamente descarregada, seu aparelho não ligará. 
    Carregue a bateria completamente antes de ligar o aparelho.
    109: Apêndice
    110
    As chamadas recebidas não são conectadas
    •  Certifique-se de que acessou a rede de telefonia correta.
    •  Certifique-se de que não configurou a restrição de chamada para o número que 
    está ligando.
    •  Certifique-se de que você não configurou a restrição de chamada para o número 
    que está recebendo a chamada.
    As pessoas não conseguem ouvi-lo durante uma chamada
    •  Certifique-se de que você não está bloqueando o microfone.
    •  Certifique-se de que o microfone está próximo à sua boca.
    •  Se você estiver utilizando um fone de ouvido, certifique-se de que ele está 
    corretamente conectado.
    O som ecoa durante uma chamada
    Ajuste o volume ao pressionar a Tecla Volume ou vá para outra área.
    A rede móvel ou a internet é desconectada muitas vezes ou a 
    qualidade do áudio é ruim
    •  Certifique-se de que você não está bloqueando a antena interna do aparelho.
    •  Quando você está em áreas com sinal fraco ou recepção fraca, você pode perder a 
    recepção do sinal. Você poderá ter problemas de conectividade devido a problemas 
    com a estação rádio base da operadora. Vá para outra área e tente novamente.
    •  Quando utilizar o aparelho em movimento, os serviços de rede sem fio poderão ser 
    desativados devido a problemas com a rede da operadora.
    A bateria não carrega corretamente (Em caso de utilizar 
    carregadores Samsung)
    •  Certifique-se de que o carregador está conectado corretamente.
    •  Visite um Centro de Serviços Samsung para trocar a bateria.
    •  Para carregadores não Samsung, contate o fabricante do carregador.
    7: Primeiros passos
    8
     • Para economizar energia, desconecte o carregador da tomada quando não 
    estiver em uso. O carregador não possui um botão de liga e desliga, então você 
    deve retirar da tomada para evitar desperdício de energia. O carregador deve 
    permanecer na tomada e facilmente acessível enquanto carregar.
     • Ao usar um carregador, é recomendável usar um aprovado que garanta o 
    desempenho do carregamento.
     • Se a bateria estiver completamente descarregada, você não conseguirá ligar o 
    aparelho, mesmo que o carregador esteja conectado. Aguarde a bateria carregar 
    por alguns minutos antes de tentar ligar o aparelho.
     • Se utilizar vários aplicativos ao mesmo tempo, tais como aplicativos de rede ou 
    aplicativos que precisem da conexão de outro aparelho, a bateria descarregará 
    rapidamente. Para evitar desconexão da rede ou esgotar a bateria durante 
    uma transferência de dados, use sempre esses aplicativos após carregar 
    completamente a bateria.
     • Usar uma fonte de energia diferente do carregador USB, por exemplo, um 
    computador, pode resultar em lentidão ao carregar a bateria devido à corrente 
    elétrica ser mais baixa.
     • O aparelho pode ser utilizado enquanto carrega, porém pode levar mais tempo 
    para carregar a bateria completamente.
     • Se o aparelho receber uma fonte de alimentação instável enquanto carrega, o 
    touch screen pode não funcionar. Se isto acontecer, desconecte o carregador.
     • O aparelho pode aquecer enquanto carrega. Isto é normal e não deve afetar a 
    vida útil ou desempenho dele. Se a bateria aquecer mais do que o normal, o 
    carregador pode parar de funcionar.
     • Se o seu aparelho não carregar adequadamente, leve para um Centro de Serviços 
    Samsung.
    

Ou podemos fazer a mesma coisa que fizemos com o arquivo CSV e criar os embeddings e depois buscar pelo índice, trazendo o modelo de LLM para ajudar na busca do mesmo.


```python
embedding_model = OpenAIEmbeddings(chunk_size=10)
index = VectorstoreIndexCreator(vectorstore_cls=FAISS, embedding=embedding_model).from_loaders([loader])
```


```python
query = "O que fazer se o aparelho não ligar?"
```


```python
response = index.query(query, llm=chat_openai)
```


```python
display_markdown(response)
```




Se o seu aparelho não ligar, mesmo estando conectado ao carregador, pode ser necessário aguardar alguns minutos para permitir que a bateria se recarregue o suficiente para ligar o aparelho. Se mesmo após esperar um tempo a situação persistir, você pode tentar forçar o reinício do aparelho mantendo as teclas Lateral e Diminuir volume pressionadas por mais de 7 segundos. Se ainda assim o problema não for resolvido, você pode tentar restaurar o aparelho para as configurações de fábrica. Lembre-se de fazer um backup de todos os dados importantes antes de executar essa ação.




```python
response = index.query_with_sources(query, llm=chat_openai)
```


```python
response
```




    {'question': 'O que fazer se o aparelho não ligar?',
     'answer': 'Se o aparelho não ligar, é recomendado forçar um reinício mantendo as teclas Lateral e Diminuir volume pressionadas por mais de 7 segundos. Se o problema persistir, é aconselhável executar uma restauração para o padrão de fábrica. Certifique-se de fazer um backup de todos os dados importantes antes de prosseguir com a restauração.\n',
     'sources': 'docs/SM-A146M_Emb_BR_Rev.1.2.pdf'}



## Agentes

Os agentes são ações que podem ser disparados a partir dos modelos LLM.

A idéia principal é que cada agente possui um "pensamento", uma determinação de qual "ação" realizar e depois delegar ao "agente" especificado, dentro das descrições passadas.


```python
!pip install langchainhub
```

    Collecting langchainhub
      Downloading langchainhub-0.1.21-py3-none-any.whl.metadata (659 bytes)
    Requirement already satisfied: packaging<25,>=23.2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchainhub) (23.2)
    Requirement already satisfied: requests<3,>=2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchainhub) (2.31.0)
    Requirement already satisfied: types-requests<3.0.0.0,>=2.31.0.2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from langchainhub) (2.32.0.20250328)
    Requirement already satisfied: charset-normalizer<4,>=2 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from requests<3,>=2->langchainhub) (3.3.2)
    Requirement already satisfied: idna<4,>=2.5 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from requests<3,>=2->langchainhub) (3.6)
    Requirement already satisfied: urllib3<3,>=1.21.1 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from requests<3,>=2->langchainhub) (2.0.7)
    Requirement already satisfied: certifi>=2017.4.17 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from requests<3,>=2->langchainhub) (2024.2.2)
    Downloading langchainhub-0.1.21-py3-none-any.whl (5.2 kB)
    Installing collected packages: langchainhub
    Successfully installed langchainhub-0.1.21
    


```python
from langchain.agents import load_tools
from langchain.agents import AgentType, Tool, create_json_chat_agent, create_react_agent, AgentExecutor
from langchain_openai import ChatOpenAI
```


```python
from langchain import hub

prompt = hub.pull("hwchase17/react-chat-json")
```

    /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages/langsmith/client.py:272: LangSmithMissingAPIKeyWarning: API key must be provided when using hosted LangSmith API
      warnings.warn(
    


```python
model = "gpt-4.1-mini"
```


```python
chat_openai = ChatOpenAI(temperature=0, model=model)
```

Vamos carregar a ferramenta (ou agente) para operações matemáticas.


```python
!pip install numexpr
```

    Collecting numexpr
      Downloading numexpr-2.10.2-cp310-cp310-macosx_11_0_arm64.whl.metadata (8.1 kB)
    Requirement already satisfied: numpy>=1.23.0 in /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages (from numexpr) (2.2.5)
    Downloading numexpr-2.10.2-cp310-cp310-macosx_11_0_arm64.whl (134 kB)
    Installing collected packages: numexpr
    Successfully installed numexpr-2.10.2
    


```python
tools = load_tools(["llm-math"], llm=chat_openai)
```


```python
agent = create_json_chat_agent(
    chat_openai,
    tools,
    prompt)
```


```python
agent_executor = AgentExecutor(
    agent=agent, tools=tools, verbose=True, handle_parsing_errors=True
)
```


```python
agent_executor.invoke({"input": "Quanto é raiz quadrada de 5 menos 0,75?"})
```

    
    
    [1m> Entering new AgentExecutor chain...[0m
    [32;1m[1;3m```json
    {
      "action": "Calculator",
      "action_input": "sqrt(5) - 0.75"
    }
    ```[0m[36;1m[1;3mAnswer: 1.4860679774997898[0m[32;1m[1;3m```json
    {
      "action": "Final Answer",
      "action_input": "A raiz quadrada de 5 menos 0,75 é aproximadamente 1,49."
    }
    ```[0m
    
    [1m> Finished chain.[0m
    




    {'input': 'Quanto é raiz quadrada de 5 menos 0,75?',
     'output': 'A raiz quadrada de 5 menos 0,75 é aproximadamente 1,49.'}



Podemos escolher outra ferramenta, neste caso o Wikipedia, para interagir com os dados armazenados.


```python
tools = load_tools(["wikipedia"], llm=chat_openai)
```


```python
prompt = hub.pull("hwchase17/react")
```

    /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages/langsmith/client.py:272: LangSmithMissingAPIKeyWarning: API key must be provided when using hosted LangSmith API
      warnings.warn(
    


```python
agent = create_react_agent(
    chat_openai,
    tools,
    prompt)
```


```python
agent_executor = AgentExecutor(
    agent=agent, tools=tools, verbose=True, handle_parsing_errors=True
)
```


```python
agent_executor.invoke({"input": "Quem é o fundador da Gurgel?"})
```

    
    
    [1m> Entering new AgentExecutor chain...[0m
    [32;1m[1;3mQuestion: Quem é o fundador da Gurgel?
    Thought: Para responder a essa pergunta, vou buscar informações sobre a Gurgel no Wikipedia para identificar quem é o fundador da empresa.
    Action: wikipedia
    Action Input: Gurgel (empresa)[0m[36;1m[1;3mPage: Flávio Rocha
    Summary: Flávio Gurgel Rocha (born 14 February 1958) is a Brazilian former federal deputy and businessman, current CEO and Chairman of Lojas Riachuelo, one of the largest retailers in the country.
    
    Page: Social security in Brazil
    Summary: Social security in Brazil has its origins in the 1824 Constitution, through a 'public aid' system supported by private initiatives, such as the Santa Casa de Misericórdia. Social security, along with public health and social assistance, forms part of the broader social welfare system. The Instituto Nacional do Seguro Social (National Social Security Institute - INSS), responsible for managing social security benefits, was established by Decree No. 99,350 on June 27, 1990. The INSS resulted from the merger of the Instituto de Administração Financeira da Previdência e Assistência Social (IAPAS), founded in 1977, and the Instituto Nacional de Previdência Social (INPS), created in 1966.
    
    
    
    Page: Automotive industry in Brazil
    Summary: The Brazilian automotive industry is coordinated by the Associação Nacional dos Fabricantes de Veículos Automotores (Anfavea), created in 1956, which includes automakers (cars, light vehicles, trucks, buses and agriculture machines) with factories in Brazil. Anfavea is part of the Organisation Internationale des Constructeurs d'Automobiles (OICA), based in Paris. In 2021, the annual production exceeded 2.2 million vehicles, the 8th largest in the world.
    Most large global automotive companies are present in Brazil, such as: BMW, BYD, Chery, Fiat, Ford, Geely, General Motors, Honda, Hyundai, JAC Motors, Kia, Land Rover, Lexus, Lifan, Mercedes-Benz, Mitsubishi, Nissan Motors, Renault, Stellantis, Subaru, Toyota, Volkswagen, Volvo Trucks, among others, as well as national companies such as Agrale, Marcopolo, Randon, and more. In the past there were national brands such as DKW Vemag, FNM, Gurgel, and Troller. Some traditionally produced modern equipped replicas of older models.[0m[32;1m[1;3mThought: The search for "Gurgel (empresa)" did not return a direct answer about the founder of Gurgel, but it did mention the company in the context of the Brazilian automotive industry. I know Gurgel is a Brazilian car manufacturer. I will try searching specifically for "Gurgel fundador" to find the founder's name.
    
    Action: wikipedia
    Action Input: Gurgel fundador[0m[36;1m[1;3mPage: João do Amaral Gurgel
    Summary: João Augusto Conrado do Amaral Gurgel (March 26, 1926 – January 30, 2009) was a Brazilian engineer and businessman. He founded Gurgel Motores, a Brazilian automobile manufacturer, in 1969, with the aim of producing vehicles entirely made in Brazil. The company's initial models featured fiberglass bodies mounted on Volkswagen Beetle (Fusca) chassis and machinery.
    During the São Paulo International Motor Show in 1974, Gurgel presented the Itaipu, a two-seater minicar that became the first electric vehicle developed in Latin America. Later on, he introduced the BR-800, the first fully designed and manufactured car made in Brazil.
    However, Gurgel faced challenges over the years and eventually ceased operations in 1996.
    
    Page: Deaths in January 2009
    Summary: 
    
    Page: 2017 in Brazil
    Summary: Events in the year 2017 in Brazil.[0m[32;1m[1;3mThought: The search for "Gurgel fundador" returned a clear answer that João Augusto Conrado do Amaral Gurgel is the founder of Gurgel Motores, a Brazilian automobile manufacturer.
    Final Answer: O fundador da Gurgel é João Augusto Conrado do Amaral Gurgel.[0m
    
    [1m> Finished chain.[0m
    




    {'input': 'Quem é o fundador da Gurgel?',
     'output': 'O fundador da Gurgel é João Augusto Conrado do Amaral Gurgel.'}



Existem diversos tipos de ferramentas que podem ser utilizadas, como por exemplo o acesso ao Pubmed, um repositório de artigos médicos.


```python
from langchain_community.tools.pubmed.tool import PubmedQueryRun
```


```python
tool = PubmedQueryRun()

tool.run("Febre Familiar do Mediterrâneo")
```




    'Published: 2017-03-23\nTitle: Clinical outcomes and survival in AA amyloidosis patients.\nCopyright Information: Copyright © 2017. Published by Elsevier Editora Ltda.\nSummary::\nAIM: Amyloid A amyloidosis is a rare complication of chronic inflammatory conditions. Most patients with amyloid A amyloidosis present with nephropathy and it leads to renal failure and death. We studied clinical characteristics and survival in patients with amyloid A amyloidosis.\nMETHODS: A total of 81 patients (51 males, 30 females) with renal biopsy proven amyloid A amyloidosis were analyzed retrospectively. The patients were divided into good and poor outcomes groups according to survival results.\nRESULTS: Most of the patients (55.6%) had nephrotic range proteinuria at diagnosis. Most frequent underlying disorders were familial Mediterranean fever (21.2%) and rheumatoid arthritis (10.6%) in the good outcome group and malignancy (20%) in the poor outcome group. Only diastolic blood pressure in the good outcome group and phosphorus level in the poor outcome group was higher. Serum creatinine levels increased after treatment in both groups, while proteinuria in the good outcome group decreased. Increase in serum creatinine and decrease in estimated glomerular filtration rate of the poor outcome group were more significant in the good outcome group. At the time of diagnosis 18.5% and 27.2% of all patients had advanced chronic kidney disease (stage 4 and 5, respectively). Median duration of renal survival was 65±3.54 months. Among all patients, 27.1% were started dialysis treatment during the follow-up period and 7.4% of all patients underwent kidney transplantation. Higher levels of systolic blood pressure [hazard ratios 1.03, 95% confidence interval: 1-1.06, p=0.036], serum creatinine (hazard ratios 1.25, 95% confidence interval: 1.07-1.46, p=0.006) and urinary protein excretion (hazard ratios 1.08, 95% confidence interval: 1.01-1.16, p=0.027) were predictors of end-stage renal disease. Median'




```python
prompt = hub.pull("hwchase17/react")
```

    /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages/langsmith/client.py:272: LangSmithMissingAPIKeyWarning: API key must be provided when using hosted LangSmith API
      warnings.warn(
    


```python
pubmed_tool = PubmedQueryRun()

tool = Tool(
    name="PubMed",
    func=pubmed_tool.run,
    description="Use esta ferramenta para pesquisar artigos científicos no PubMed. Forneça um termo de busca como entrada."
)

```


```python
agent = create_react_agent(
    chat_openai,
    [tool],
    prompt)
```


```python
agent_executor = AgentExecutor(
    agent=agent, tools=[tool], verbose=True, handle_parsing_errors=True
)
```


```python
agent_executor.invoke({"input": "Relacione a Febre Familiar do Mediterrâneo com Alergias."})
```

    
    
    [1m> Entering new AgentExecutor chain...[0m
    [32;1m[1;3mThought: Para relacionar a Febre Familiar do Mediterrâneo (FFM) com alergias, preciso buscar artigos científicos que abordem a associação entre essa doença autoinflamatória e manifestações alérgicas ou condições alérgicas. Vou pesquisar no PubMed usando termos que combinem "Familial Mediterranean Fever" e "allergies" ou "allergic reactions".
    
    Action: PubMed
    Action Input: "Familial Mediterranean Fever AND allergies"
    [0m[36;1m[1;3mPublished: 2025-04-29
    Title: Pediatric Systemic Autoinflammatory Disorders: An Overview.
    Copyright Information: © 2025. The Author(s), under exclusive licence to Springer Science+Business Media, LLC, part of Springer Nature.
    Summary::
    PURPOSE OF REVIEW: Systemic autoinflammatory disorders (SAIDs) are a group of diseases that are characterized by recurrent or persistent unprovoked attacks of inflammation resulting from innate immunity dysregulation and leading to significant sequelae in many cases. The concept of autoinflammatory disorders has been widely studied in the last 28 years since the genetic mutation responsible for familial Mediterranean fever (FMF) was discovered. These disorders are mainly hereditary autoinflammatory diseases with key immunological pathways affected and particularly involving inflammasomes, nuclear factor-κB dysregulation and interferon upregulation. This article serves as an overview of pediatric systemic autoinflammatory disorders, their presentation, workup, complications, and therapeutic management.
    RECENT FINDINGS: Advances in genetic analysis have allowed for the rapid identification of mutations responsible for many autoinflammatory disorders. Advances in biomolecular techniques, which have allowed for identifying key players such as inflammasomes, have led to treatment options that have significantly improved morbidity and mortality in affected patients. This review provides an overview of the proposed pathogenesis, presenting features, potential complications and suggested therapies of systemic autoinflammatory disorders. Providers should have a high clinical suspicion for autoinflammatory disorders in children who present with fever, a heightened inflammatory response and negative evaluation for an infectious, malignant, and autoimmune etiology. Understanding and identifying these disorders in a timely manner and implementing prompt treatment allow for the best possible outcome for these patients.
    
    Published: 2025-04-07
    Title: [0m[32;1m[1;3mThought: O artigo encontrado é uma revisão geral sobre doenças autoinflamatórias sistêmicas pediátricas, incluindo a Febre Familiar do Mediterrâneo (FFM), mas não menciona especificamente a relação entre FFM e alergias. Para aprofundar, preciso buscar artigos que abordem diretamente a associação entre FFM e manifestações alérgicas, como asma, dermatite atópica ou outras reações alérgicas.
    
    Action: PubMed
    Action Input: "Familial Mediterranean Fever AND allergic diseases"
    [0m[36;1m[1;3mPublished: 2025-04-23
    Title: Amyloidosis in Human Inborn Errors of Immunity Predicts Poor Prognosis.
    Copyright Information: © 2025. The Author(s).
    Summary::
    PURPOSE: Chronic inflammation in inborn errors of immunity(IEI) caused by the infections or immune dysregulation is associated with the amyloid A (AA) amyloidosis development. This study aims to analyze the clinical characteristics, management strategies, and outcomes of patients with IEI complicated by AA amyloidosis, focusing on demographics, disease manifestations, treatment modalities, and survival rates.
    METHODS: Thirteen patients diagnosed with IEI and AA amyloidosis, along with an additional 10 patients previously reported from Türkiye, were reviewed retrospectively.
    RESULTS: The median ages at diagnosis of IEI and amyloidosis were 20 years (2-61) and 25 years (7-70), respectively. Renal (74%) and gastrointestinal involvement (44%) were the most common, followed by skin(9%), pulmonary (9%), and cardiac involvement (9%). Primary antibody deficiencies(48%), combined immunodeficiencies(31%), hyperimmunoglobulin E syndrome(9%), congenital neutropenia (4%), autoinflammatory disorders (4%), and chronic mucocutaneous candidiasis (4%) were the IEI types associated with amyloidosis. Bronchiectasis (74%) and malignancy (17%) were observed in given ratio of patients. Treatment modalities for amyloidosis include colchicine (n = 12, 52%), steroids (n = 5, 22%) and tocilizumab (n = 2, 9%) without significant benefit. Thirteen patients (57%) died with a median age of 24 years (8-45), predominantly due to sepsis (52%). Familial Mediterranean fever (FMF) gene analysis was negative in all patients except for one, who had a heterozygous MEFV gene defect (M694V).
    CONCLUSION: AA amyloidosis in IEI is associated with severe morbidity and mortality. Early diagnosis and management of IEI are crucial to prevent amyloidosis development. However, colchicine appears ineffective once amyloidosis has occurred, highlighting the need fo[0m[32;1m[1;3mThought: O segundo artigo aborda a associação de doenças imunológicas hereditárias com complicações como amiloidose, mencionando a Febre Familiar do Mediterrâneo (FFM) como uma das condições autoinflamatórias, mas não discute diretamente a relação entre FFM e alergias. Até agora, não encontrei evidências claras ou estudos que relacionem diretamente FFM com alergias ou doenças alérgicas. Vou tentar uma busca mais específica por termos como "Familial Mediterranean Fever AND asthma" ou "Familial Mediterranean Fever AND atopy" para verificar se há alguma associação com manifestações alérgicas específicas.
    
    Action: PubMed  
    Action Input: "Familial Mediterranean Fever AND asthma"
    [0m[36;1m[1;3mPublished: 2025-04-29
    Title: Pediatric Systemic Autoinflammatory Disorders: An Overview.
    Copyright Information: © 2025. The Author(s), under exclusive licence to Springer Science+Business Media, LLC, part of Springer Nature.
    Summary::
    PURPOSE OF REVIEW: Systemic autoinflammatory disorders (SAIDs) are a group of diseases that are characterized by recurrent or persistent unprovoked attacks of inflammation resulting from innate immunity dysregulation and leading to significant sequelae in many cases. The concept of autoinflammatory disorders has been widely studied in the last 28 years since the genetic mutation responsible for familial Mediterranean fever (FMF) was discovered. These disorders are mainly hereditary autoinflammatory diseases with key immunological pathways affected and particularly involving inflammasomes, nuclear factor-κB dysregulation and interferon upregulation. This article serves as an overview of pediatric systemic autoinflammatory disorders, their presentation, workup, complications, and therapeutic management.
    RECENT FINDINGS: Advances in genetic analysis have allowed for the rapid identification of mutations responsible for many autoinflammatory disorders. Advances in biomolecular techniques, which have allowed for identifying key players such as inflammasomes, have led to treatment options that have significantly improved morbidity and mortality in affected patients. This review provides an overview of the proposed pathogenesis, presenting features, potential complications and suggested therapies of systemic autoinflammatory disorders. Providers should have a high clinical suspicion for autoinflammatory disorders in children who present with fever, a heightened inflammatory response and negative evaluation for an infectious, malignant, and autoimmune etiology. Understanding and identifying these disorders in a timely manner and implementing prompt treatment allow for the best possible outcome for these patients.
    
    Published: 2024-05-23
    Title: [0m[32;1m[1;3mThought: As buscas realizadas não retornaram evidências claras ou estudos que relacionem diretamente a Febre Familiar do Mediterrâneo (FFM) com alergias ou doenças alérgicas específicas como asma ou atopia. A FFM é uma doença autoinflamatória hereditária caracterizada por episódios recorrentes de inflamação, mas não há dados robustos que indiquem uma associação direta com alergias. A literatura disponível destaca mais a disfunção da imunidade inata e inflamação do que uma predisposição a reações alérgicas mediadas por imunidade adaptativa, típicas das alergias.
    
    Final Answer: A Febre Familiar do Mediterrâneo (FFM) é uma doença autoinflamatória hereditária caracterizada por episódios recorrentes de inflamação devido à disfunção da imunidade inata. Até o momento, não há evidências científicas claras que relacionem diretamente a FFM com alergias ou doenças alérgicas, como asma ou dermatite atópica. Portanto, a FFM não é considerada uma condição associada a alergias, e suas manifestações clínicas estão mais relacionadas a processos inflamatórios autoinflamatórios do que a reações alérgicas.[0m
    
    [1m> Finished chain.[0m
    




    {'input': 'Relacione a Febre Familiar do Mediterrâneo com Alergias.',
     'output': 'A Febre Familiar do Mediterrâneo (FFM) é uma doença autoinflamatória hereditária caracterizada por episódios recorrentes de inflamação devido à disfunção da imunidade inata. Até o momento, não há evidências científicas claras que relacionem diretamente a FFM com alergias ou doenças alérgicas, como asma ou dermatite atópica. Portanto, a FFM não é considerada uma condição associada a alergias, e suas manifestações clínicas estão mais relacionadas a processos inflamatórios autoinflamatórios do que a reações alérgicas.'}



E por fim, podemos criar nossa própria ferramenta.
O exemplo a seguir será de consumir 2 APIs de testes (mock) que traz uma lista de produtos ou um produto individual, de acordo com um identificador.


```python
import requests
from langchain.agents import tool
```

Incluímos a anotação "@tool" na função e obrigatóriamente precisa ter entrada e saída em String.

Se não for utiliar a entrada, envie um parâmetro vazio.

Importante é detalhar no comentário da função sobre o que faz e qual parâmetro necessita.


```python
retorno = requests.get("https://dummyjson.com/products")
products = json.loads(retorno.content)
products
```




    {'products': [{'id': 1,
       'title': 'Essence Mascara Lash Princess',
       'description': 'The Essence Mascara Lash Princess is a popular mascara known for its volumizing and lengthening effects. Achieve dramatic lashes with this long-lasting and cruelty-free formula.',
       'category': 'beauty',
       'price': 9.99,
       'discountPercentage': 10.48,
       'rating': 2.56,
       'stock': 99,
       'tags': ['beauty', 'mascara'],
       'brand': 'Essence',
       'sku': 'BEA-ESS-ESS-001',
       'weight': 4,
       'dimensions': {'width': 15.14, 'height': 13.08, 'depth': 22.99},
       'warrantyInformation': '1 week warranty',
       'shippingInformation': 'Ships in 3-5 business days',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 3,
         'comment': 'Would not recommend!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Eleanor Collins',
         'reviewerEmail': 'eleanor.collins@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Very satisfied!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Lucas Gordon',
         'reviewerEmail': 'lucas.gordon@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Highly impressed!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Eleanor Collins',
         'reviewerEmail': 'eleanor.collins@x.dummyjson.com'}],
       'returnPolicy': 'No return policy',
       'minimumOrderQuantity': 48,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '5784719087687',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/beauty/essence-mascara-lash-princess/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/beauty/essence-mascara-lash-princess/thumbnail.webp'},
      {'id': 2,
       'title': 'Eyeshadow Palette with Mirror',
       'description': "The Eyeshadow Palette with Mirror offers a versatile range of eyeshadow shades for creating stunning eye looks. With a built-in mirror, it's convenient for on-the-go makeup application.",
       'category': 'beauty',
       'price': 19.99,
       'discountPercentage': 18.19,
       'rating': 2.86,
       'stock': 34,
       'tags': ['beauty', 'eyeshadow'],
       'brand': 'Glamour Beauty',
       'sku': 'BEA-GLA-EYE-002',
       'weight': 9,
       'dimensions': {'width': 9.26, 'height': 22.47, 'depth': 27.67},
       'warrantyInformation': '1 year warranty',
       'shippingInformation': 'Ships in 2 weeks',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 5,
         'comment': 'Great product!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Savannah Gomez',
         'reviewerEmail': 'savannah.gomez@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Awesome product!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Christian Perez',
         'reviewerEmail': 'christian.perez@x.dummyjson.com'},
        {'rating': 1,
         'comment': 'Poor quality!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Nicholas Bailey',
         'reviewerEmail': 'nicholas.bailey@x.dummyjson.com'}],
       'returnPolicy': '7 days return policy',
       'minimumOrderQuantity': 20,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '9170275171413',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/beauty/eyeshadow-palette-with-mirror/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/beauty/eyeshadow-palette-with-mirror/thumbnail.webp'},
      {'id': 3,
       'title': 'Powder Canister',
       'description': 'The Powder Canister is a finely milled setting powder designed to set makeup and control shine. With a lightweight and translucent formula, it provides a smooth and matte finish.',
       'category': 'beauty',
       'price': 14.99,
       'discountPercentage': 9.84,
       'rating': 4.64,
       'stock': 89,
       'tags': ['beauty', 'face powder'],
       'brand': 'Velvet Touch',
       'sku': 'BEA-VEL-POW-003',
       'weight': 8,
       'dimensions': {'width': 29.27, 'height': 27.93, 'depth': 20.59},
       'warrantyInformation': '3 months warranty',
       'shippingInformation': 'Ships in 1-2 business days',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 4,
         'comment': 'Would buy again!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Alexander Jones',
         'reviewerEmail': 'alexander.jones@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Highly impressed!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Elijah Cruz',
         'reviewerEmail': 'elijah.cruz@x.dummyjson.com'},
        {'rating': 1,
         'comment': 'Very dissatisfied!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Avery Perez',
         'reviewerEmail': 'avery.perez@x.dummyjson.com'}],
       'returnPolicy': 'No return policy',
       'minimumOrderQuantity': 22,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '8418883906837',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/beauty/powder-canister/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/beauty/powder-canister/thumbnail.webp'},
      {'id': 4,
       'title': 'Red Lipstick',
       'description': 'The Red Lipstick is a classic and bold choice for adding a pop of color to your lips. With a creamy and pigmented formula, it provides a vibrant and long-lasting finish.',
       'category': 'beauty',
       'price': 12.99,
       'discountPercentage': 12.16,
       'rating': 4.36,
       'stock': 91,
       'tags': ['beauty', 'lipstick'],
       'brand': 'Chic Cosmetics',
       'sku': 'BEA-CHI-LIP-004',
       'weight': 1,
       'dimensions': {'width': 18.11, 'height': 28.38, 'depth': 22.17},
       'warrantyInformation': '3 year warranty',
       'shippingInformation': 'Ships in 1 week',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 4,
         'comment': 'Great product!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Liam Garcia',
         'reviewerEmail': 'liam.garcia@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Great product!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Ruby Andrews',
         'reviewerEmail': 'ruby.andrews@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Would buy again!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Clara Berry',
         'reviewerEmail': 'clara.berry@x.dummyjson.com'}],
       'returnPolicy': '7 days return policy',
       'minimumOrderQuantity': 40,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '9467746727219',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/beauty/red-lipstick/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/beauty/red-lipstick/thumbnail.webp'},
      {'id': 5,
       'title': 'Red Nail Polish',
       'description': 'The Red Nail Polish offers a rich and glossy red hue for vibrant and polished nails. With a quick-drying formula, it provides a salon-quality finish at home.',
       'category': 'beauty',
       'price': 8.99,
       'discountPercentage': 11.44,
       'rating': 4.32,
       'stock': 79,
       'tags': ['beauty', 'nail polish'],
       'brand': 'Nail Couture',
       'sku': 'BEA-NAI-NAI-005',
       'weight': 8,
       'dimensions': {'width': 21.63, 'height': 16.48, 'depth': 29.84},
       'warrantyInformation': '1 month warranty',
       'shippingInformation': 'Ships overnight',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 2,
         'comment': 'Poor quality!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Benjamin Wilson',
         'reviewerEmail': 'benjamin.wilson@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Great product!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Liam Smith',
         'reviewerEmail': 'liam.smith@x.dummyjson.com'},
        {'rating': 1,
         'comment': 'Very unhappy with my purchase!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Clara Berry',
         'reviewerEmail': 'clara.berry@x.dummyjson.com'}],
       'returnPolicy': 'No return policy',
       'minimumOrderQuantity': 22,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '4063010628104',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/beauty/red-nail-polish/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/beauty/red-nail-polish/thumbnail.webp'},
      {'id': 6,
       'title': 'Calvin Klein CK One',
       'description': "CK One by Calvin Klein is a classic unisex fragrance, known for its fresh and clean scent. It's a versatile fragrance suitable for everyday wear.",
       'category': 'fragrances',
       'price': 49.99,
       'discountPercentage': 1.89,
       'rating': 4.37,
       'stock': 29,
       'tags': ['fragrances', 'perfumes'],
       'brand': 'Calvin Klein',
       'sku': 'FRA-CAL-CAL-006',
       'weight': 7,
       'dimensions': {'width': 29.36, 'height': 27.76, 'depth': 20.72},
       'warrantyInformation': '1 week warranty',
       'shippingInformation': 'Ships overnight',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 2,
         'comment': 'Very disappointed!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Layla Young',
         'reviewerEmail': 'layla.young@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Fast shipping!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Daniel Cook',
         'reviewerEmail': 'daniel.cook@x.dummyjson.com'},
        {'rating': 3,
         'comment': 'Not as described!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Jacob Cooper',
         'reviewerEmail': 'jacob.cooper@x.dummyjson.com'}],
       'returnPolicy': '90 days return policy',
       'minimumOrderQuantity': 9,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '2451534060749',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/fragrances/calvin-klein-ck-one/1.webp',
        'https://cdn.dummyjson.com/product-images/fragrances/calvin-klein-ck-one/2.webp',
        'https://cdn.dummyjson.com/product-images/fragrances/calvin-klein-ck-one/3.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/fragrances/calvin-klein-ck-one/thumbnail.webp'},
      {'id': 7,
       'title': 'Chanel Coco Noir Eau De',
       'description': 'Coco Noir by Chanel is an elegant and mysterious fragrance, featuring notes of grapefruit, rose, and sandalwood. Perfect for evening occasions.',
       'category': 'fragrances',
       'price': 129.99,
       'discountPercentage': 16.51,
       'rating': 4.26,
       'stock': 58,
       'tags': ['fragrances', 'perfumes'],
       'brand': 'Chanel',
       'sku': 'FRA-CHA-CHA-007',
       'weight': 7,
       'dimensions': {'width': 24.5, 'height': 25.7, 'depth': 25.98},
       'warrantyInformation': '3 year warranty',
       'shippingInformation': 'Ships overnight',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 4,
         'comment': 'Highly impressed!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Ruby Andrews',
         'reviewerEmail': 'ruby.andrews@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Awesome product!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Leah Henderson',
         'reviewerEmail': 'leah.henderson@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Very happy with my purchase!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Xavier Wright',
         'reviewerEmail': 'xavier.wright@x.dummyjson.com'}],
       'returnPolicy': 'No return policy',
       'minimumOrderQuantity': 1,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '4091737746820',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/fragrances/chanel-coco-noir-eau-de/1.webp',
        'https://cdn.dummyjson.com/product-images/fragrances/chanel-coco-noir-eau-de/2.webp',
        'https://cdn.dummyjson.com/product-images/fragrances/chanel-coco-noir-eau-de/3.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/fragrances/chanel-coco-noir-eau-de/thumbnail.webp'},
      {'id': 8,
       'title': "Dior J'adore",
       'description': "J'adore by Dior is a luxurious and floral fragrance, known for its blend of ylang-ylang, rose, and jasmine. It embodies femininity and sophistication.",
       'category': 'fragrances',
       'price': 89.99,
       'discountPercentage': 14.72,
       'rating': 3.8,
       'stock': 98,
       'tags': ['fragrances', 'perfumes'],
       'brand': 'Dior',
       'sku': 'FRA-DIO-DIO-008',
       'weight': 4,
       'dimensions': {'width': 27.67, 'height': 28.28, 'depth': 11.83},
       'warrantyInformation': '1 week warranty',
       'shippingInformation': 'Ships in 2 weeks',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 5,
         'comment': 'Great value for money!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Nicholas Bailey',
         'reviewerEmail': 'nicholas.bailey@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Great value for money!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Penelope Harper',
         'reviewerEmail': 'penelope.harper@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Great product!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Emma Miller',
         'reviewerEmail': 'emma.miller@x.dummyjson.com'}],
       'returnPolicy': '7 days return policy',
       'minimumOrderQuantity': 10,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '1445086097250',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ["https://cdn.dummyjson.com/product-images/fragrances/dior-j'adore/1.webp",
        "https://cdn.dummyjson.com/product-images/fragrances/dior-j'adore/2.webp",
        "https://cdn.dummyjson.com/product-images/fragrances/dior-j'adore/3.webp"],
       'thumbnail': "https://cdn.dummyjson.com/product-images/fragrances/dior-j'adore/thumbnail.webp"},
      {'id': 9,
       'title': 'Dolce Shine Eau de',
       'description': "Dolce Shine by Dolce & Gabbana is a vibrant and fruity fragrance, featuring notes of mango, jasmine, and blonde woods. It's a joyful and youthful scent.",
       'category': 'fragrances',
       'price': 69.99,
       'discountPercentage': 0.62,
       'rating': 3.96,
       'stock': 4,
       'tags': ['fragrances', 'perfumes'],
       'brand': 'Dolce & Gabbana',
       'sku': 'FRA-DOL-DOL-009',
       'weight': 6,
       'dimensions': {'width': 27.28, 'height': 29.88, 'depth': 18.3},
       'warrantyInformation': '3 year warranty',
       'shippingInformation': 'Ships in 1 month',
       'availabilityStatus': 'Low Stock',
       'reviews': [{'rating': 4,
         'comment': 'Would buy again!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Mateo Bennett',
         'reviewerEmail': 'mateo.bennett@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Highly recommended!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Nolan Gonzalez',
         'reviewerEmail': 'nolan.gonzalez@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Very happy with my purchase!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Aurora Lawson',
         'reviewerEmail': 'aurora.lawson@x.dummyjson.com'}],
       'returnPolicy': '7 days return policy',
       'minimumOrderQuantity': 2,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '3023868210708',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/fragrances/dolce-shine-eau-de/1.webp',
        'https://cdn.dummyjson.com/product-images/fragrances/dolce-shine-eau-de/2.webp',
        'https://cdn.dummyjson.com/product-images/fragrances/dolce-shine-eau-de/3.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/fragrances/dolce-shine-eau-de/thumbnail.webp'},
      {'id': 10,
       'title': 'Gucci Bloom Eau de',
       'description': "Gucci Bloom by Gucci is a floral and captivating fragrance, with notes of tuberose, jasmine, and Rangoon creeper. It's a modern and romantic scent.",
       'category': 'fragrances',
       'price': 79.99,
       'discountPercentage': 14.39,
       'rating': 2.74,
       'stock': 91,
       'tags': ['fragrances', 'perfumes'],
       'brand': 'Gucci',
       'sku': 'FRA-GUC-GUC-010',
       'weight': 7,
       'dimensions': {'width': 20.92, 'height': 21.68, 'depth': 11.2},
       'warrantyInformation': '6 months warranty',
       'shippingInformation': 'Ships overnight',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 1,
         'comment': 'Very dissatisfied!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Cameron Perez',
         'reviewerEmail': 'cameron.perez@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Very happy with my purchase!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Daniel Cook',
         'reviewerEmail': 'daniel.cook@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Highly impressed!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Addison Wright',
         'reviewerEmail': 'addison.wright@x.dummyjson.com'}],
       'returnPolicy': 'No return policy',
       'minimumOrderQuantity': 2,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '3170832177880',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/fragrances/gucci-bloom-eau-de/1.webp',
        'https://cdn.dummyjson.com/product-images/fragrances/gucci-bloom-eau-de/2.webp',
        'https://cdn.dummyjson.com/product-images/fragrances/gucci-bloom-eau-de/3.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/fragrances/gucci-bloom-eau-de/thumbnail.webp'},
      {'id': 11,
       'title': 'Annibale Colombo Bed',
       'description': 'The Annibale Colombo Bed is a luxurious and elegant bed frame, crafted with high-quality materials for a comfortable and stylish bedroom.',
       'category': 'furniture',
       'price': 1899.99,
       'discountPercentage': 8.57,
       'rating': 4.77,
       'stock': 88,
       'tags': ['furniture', 'beds'],
       'brand': 'Annibale Colombo',
       'sku': 'FUR-ANN-ANN-011',
       'weight': 10,
       'dimensions': {'width': 28.16, 'height': 25.36, 'depth': 17.28},
       'warrantyInformation': '1 year warranty',
       'shippingInformation': 'Ships in 1 month',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 2,
         'comment': 'Would not recommend!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Christopher West',
         'reviewerEmail': 'christopher.west@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Highly impressed!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Vivian Carter',
         'reviewerEmail': 'vivian.carter@x.dummyjson.com'},
        {'rating': 1,
         'comment': 'Poor quality!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Mason Wright',
         'reviewerEmail': 'mason.wright@x.dummyjson.com'}],
       'returnPolicy': 'No return policy',
       'minimumOrderQuantity': 1,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '3610757456581',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/furniture/annibale-colombo-bed/1.webp',
        'https://cdn.dummyjson.com/product-images/furniture/annibale-colombo-bed/2.webp',
        'https://cdn.dummyjson.com/product-images/furniture/annibale-colombo-bed/3.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/furniture/annibale-colombo-bed/thumbnail.webp'},
      {'id': 12,
       'title': 'Annibale Colombo Sofa',
       'description': 'The Annibale Colombo Sofa is a sophisticated and comfortable seating option, featuring exquisite design and premium upholstery for your living room.',
       'category': 'furniture',
       'price': 2499.99,
       'discountPercentage': 14.4,
       'rating': 3.92,
       'stock': 60,
       'tags': ['furniture', 'sofas'],
       'brand': 'Annibale Colombo',
       'sku': 'FUR-ANN-ANN-012',
       'weight': 6,
       'dimensions': {'width': 12.75, 'height': 20.55, 'depth': 19.06},
       'warrantyInformation': 'Lifetime warranty',
       'shippingInformation': 'Ships in 1 week',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 3,
         'comment': 'Very unhappy with my purchase!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Christian Perez',
         'reviewerEmail': 'christian.perez@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Fast shipping!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Lillian Bishop',
         'reviewerEmail': 'lillian.bishop@x.dummyjson.com'},
        {'rating': 1,
         'comment': 'Poor quality!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Lillian Simmons',
         'reviewerEmail': 'lillian.simmons@x.dummyjson.com'}],
       'returnPolicy': '7 days return policy',
       'minimumOrderQuantity': 1,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '1777662847736',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/furniture/annibale-colombo-sofa/1.webp',
        'https://cdn.dummyjson.com/product-images/furniture/annibale-colombo-sofa/2.webp',
        'https://cdn.dummyjson.com/product-images/furniture/annibale-colombo-sofa/3.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/furniture/annibale-colombo-sofa/thumbnail.webp'},
      {'id': 13,
       'title': 'Bedside Table African Cherry',
       'description': 'The Bedside Table in African Cherry is a stylish and functional addition to your bedroom, providing convenient storage space and a touch of elegance.',
       'category': 'furniture',
       'price': 299.99,
       'discountPercentage': 19.09,
       'rating': 2.87,
       'stock': 64,
       'tags': ['furniture', 'bedside tables'],
       'brand': 'Furniture Co.',
       'sku': 'FUR-FUR-BED-013',
       'weight': 2,
       'dimensions': {'width': 13.47, 'height': 24.99, 'depth': 27.35},
       'warrantyInformation': '5 year warranty',
       'shippingInformation': 'Ships overnight',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 4,
         'comment': 'Excellent quality!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Aaliyah Hanson',
         'reviewerEmail': 'aaliyah.hanson@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Excellent quality!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Liam Smith',
         'reviewerEmail': 'liam.smith@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Highly recommended!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Avery Barnes',
         'reviewerEmail': 'avery.barnes@x.dummyjson.com'}],
       'returnPolicy': '7 days return policy',
       'minimumOrderQuantity': 3,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '6441287925979',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/furniture/bedside-table-african-cherry/1.webp',
        'https://cdn.dummyjson.com/product-images/furniture/bedside-table-african-cherry/2.webp',
        'https://cdn.dummyjson.com/product-images/furniture/bedside-table-african-cherry/3.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/furniture/bedside-table-african-cherry/thumbnail.webp'},
      {'id': 14,
       'title': 'Knoll Saarinen Executive Conference Chair',
       'description': 'The Knoll Saarinen Executive Conference Chair is a modern and ergonomic chair, perfect for your office or conference room with its timeless design.',
       'category': 'furniture',
       'price': 499.99,
       'discountPercentage': 2.01,
       'rating': 4.88,
       'stock': 26,
       'tags': ['furniture', 'office chairs'],
       'brand': 'Knoll',
       'sku': 'FUR-KNO-KNO-014',
       'weight': 10,
       'dimensions': {'width': 13.81, 'height': 7.5, 'depth': 5.62},
       'warrantyInformation': '2 year warranty',
       'shippingInformation': 'Ships overnight',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 2,
         'comment': 'Waste of money!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Ella Cook',
         'reviewerEmail': 'ella.cook@x.dummyjson.com'},
        {'rating': 2,
         'comment': 'Very dissatisfied!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Clara Berry',
         'reviewerEmail': 'clara.berry@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Would buy again!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Elena Long',
         'reviewerEmail': 'elena.long@x.dummyjson.com'}],
       'returnPolicy': '60 days return policy',
       'minimumOrderQuantity': 5,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '8919386859966',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/furniture/knoll-saarinen-executive-conference-chair/1.webp',
        'https://cdn.dummyjson.com/product-images/furniture/knoll-saarinen-executive-conference-chair/2.webp',
        'https://cdn.dummyjson.com/product-images/furniture/knoll-saarinen-executive-conference-chair/3.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/furniture/knoll-saarinen-executive-conference-chair/thumbnail.webp'},
      {'id': 15,
       'title': 'Wooden Bathroom Sink With Mirror',
       'description': 'The Wooden Bathroom Sink with Mirror is a unique and stylish addition to your bathroom, featuring a wooden sink countertop and a matching mirror.',
       'category': 'furniture',
       'price': 799.99,
       'discountPercentage': 8.8,
       'rating': 3.59,
       'stock': 7,
       'tags': ['furniture', 'bathroom'],
       'brand': 'Bath Trends',
       'sku': 'FUR-BAT-WOO-015',
       'weight': 10,
       'dimensions': {'width': 7.98, 'height': 8.88, 'depth': 28.46},
       'warrantyInformation': '3 year warranty',
       'shippingInformation': 'Ships in 3-5 business days',
       'availabilityStatus': 'Low Stock',
       'reviews': [{'rating': 4,
         'comment': 'Fast shipping!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Logan Torres',
         'reviewerEmail': 'logan.torres@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Very pleased!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Aria Parker',
         'reviewerEmail': 'aria.parker@x.dummyjson.com'},
        {'rating': 3,
         'comment': 'Poor quality!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Dylan Wells',
         'reviewerEmail': 'dylan.wells@x.dummyjson.com'}],
       'returnPolicy': '60 days return policy',
       'minimumOrderQuantity': 2,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '1958104402873',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/furniture/wooden-bathroom-sink-with-mirror/1.webp',
        'https://cdn.dummyjson.com/product-images/furniture/wooden-bathroom-sink-with-mirror/2.webp',
        'https://cdn.dummyjson.com/product-images/furniture/wooden-bathroom-sink-with-mirror/3.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/furniture/wooden-bathroom-sink-with-mirror/thumbnail.webp'},
      {'id': 16,
       'title': 'Apple',
       'description': 'Fresh and crisp apples, perfect for snacking or incorporating into various recipes.',
       'category': 'groceries',
       'price': 1.99,
       'discountPercentage': 12.62,
       'rating': 4.19,
       'stock': 8,
       'tags': ['fruits'],
       'sku': 'GRO-BRD-APP-016',
       'weight': 9,
       'dimensions': {'width': 13.66, 'height': 11.01, 'depth': 9.73},
       'warrantyInformation': '3 year warranty',
       'shippingInformation': 'Ships in 2 weeks',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 5,
         'comment': 'Very satisfied!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Sophia Brown',
         'reviewerEmail': 'sophia.brown@x.dummyjson.com'},
        {'rating': 1,
         'comment': 'Very dissatisfied!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Scarlett Bowman',
         'reviewerEmail': 'scarlett.bowman@x.dummyjson.com'},
        {'rating': 3,
         'comment': 'Very unhappy with my purchase!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'William Gonzalez',
         'reviewerEmail': 'william.gonzalez@x.dummyjson.com'}],
       'returnPolicy': '90 days return policy',
       'minimumOrderQuantity': 7,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '7962803553314',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/groceries/apple/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/groceries/apple/thumbnail.webp'},
      {'id': 17,
       'title': 'Beef Steak',
       'description': 'High-quality beef steak, great for grilling or cooking to your preferred level of doneness.',
       'category': 'groceries',
       'price': 12.99,
       'discountPercentage': 9.61,
       'rating': 4.47,
       'stock': 86,
       'tags': ['meat'],
       'sku': 'GRO-BRD-BEE-017',
       'weight': 10,
       'dimensions': {'width': 18.9, 'height': 5.77, 'depth': 18.57},
       'warrantyInformation': '3 year warranty',
       'shippingInformation': 'Ships overnight',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 3,
         'comment': 'Would not recommend!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Eleanor Tyler',
         'reviewerEmail': 'eleanor.tyler@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Fast shipping!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Alexander Jones',
         'reviewerEmail': 'alexander.jones@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Great value for money!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Natalie Harris',
         'reviewerEmail': 'natalie.harris@x.dummyjson.com'}],
       'returnPolicy': '60 days return policy',
       'minimumOrderQuantity': 43,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '5640063409695',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/groceries/beef-steak/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/groceries/beef-steak/thumbnail.webp'},
      {'id': 18,
       'title': 'Cat Food',
       'description': 'Nutritious cat food formulated to meet the dietary needs of your feline friend.',
       'category': 'groceries',
       'price': 8.99,
       'discountPercentage': 9.58,
       'rating': 3.13,
       'stock': 46,
       'tags': ['pet supplies', 'cat food'],
       'sku': 'GRO-BRD-FOO-018',
       'weight': 10,
       'dimensions': {'width': 18.08, 'height': 9.26, 'depth': 21.86},
       'warrantyInformation': '1 year warranty',
       'shippingInformation': 'Ships overnight',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 3,
         'comment': 'Would not recommend!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Noah Lewis',
         'reviewerEmail': 'noah.lewis@x.dummyjson.com'},
        {'rating': 3,
         'comment': 'Very unhappy with my purchase!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Ruby Andrews',
         'reviewerEmail': 'ruby.andrews@x.dummyjson.com'},
        {'rating': 2,
         'comment': 'Very disappointed!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Ethan Thompson',
         'reviewerEmail': 'ethan.thompson@x.dummyjson.com'}],
       'returnPolicy': 'No return policy',
       'minimumOrderQuantity': 18,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '1483991328610',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/groceries/cat-food/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/groceries/cat-food/thumbnail.webp'},
      {'id': 19,
       'title': 'Chicken Meat',
       'description': 'Fresh and tender chicken meat, suitable for various culinary preparations.',
       'category': 'groceries',
       'price': 9.99,
       'discountPercentage': 13.7,
       'rating': 3.19,
       'stock': 97,
       'tags': ['meat'],
       'sku': 'GRO-BRD-CHI-019',
       'weight': 1,
       'dimensions': {'width': 11.03, 'height': 22.11, 'depth': 16.01},
       'warrantyInformation': '1 year warranty',
       'shippingInformation': 'Ships in 1 month',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 5,
         'comment': 'Great product!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Mateo Bennett',
         'reviewerEmail': 'mateo.bennett@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Highly recommended!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Jackson Evans',
         'reviewerEmail': 'jackson.evans@x.dummyjson.com'},
        {'rating': 3,
         'comment': 'Not worth the price!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Sadie Morales',
         'reviewerEmail': 'sadie.morales@x.dummyjson.com'}],
       'returnPolicy': '7 days return policy',
       'minimumOrderQuantity': 22,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '8829514594521',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/groceries/chicken-meat/1.webp',
        'https://cdn.dummyjson.com/product-images/groceries/chicken-meat/2.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/groceries/chicken-meat/thumbnail.webp'},
      {'id': 20,
       'title': 'Cooking Oil',
       'description': 'Versatile cooking oil suitable for frying, sautéing, and various culinary applications.',
       'category': 'groceries',
       'price': 4.99,
       'discountPercentage': 9.33,
       'rating': 4.8,
       'stock': 10,
       'tags': ['cooking essentials'],
       'sku': 'GRO-BRD-COO-020',
       'weight': 5,
       'dimensions': {'width': 19.95, 'height': 27.54, 'depth': 24.86},
       'warrantyInformation': 'Lifetime warranty',
       'shippingInformation': 'Ships in 1-2 business days',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 5,
         'comment': 'Very happy with my purchase!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Victoria McDonald',
         'reviewerEmail': 'victoria.mcdonald@x.dummyjson.com'},
        {'rating': 2,
         'comment': 'Would not recommend!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Hazel Evans',
         'reviewerEmail': 'hazel.evans@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Would buy again!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Zoe Bennett',
         'reviewerEmail': 'zoe.bennett@x.dummyjson.com'}],
       'returnPolicy': '30 days return policy',
       'minimumOrderQuantity': 46,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '4874727824518',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/groceries/cooking-oil/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/groceries/cooking-oil/thumbnail.webp'},
      {'id': 21,
       'title': 'Cucumber',
       'description': 'Crisp and hydrating cucumbers, ideal for salads, snacks, or as a refreshing side.',
       'category': 'groceries',
       'price': 1.49,
       'discountPercentage': 0.16,
       'rating': 4.07,
       'stock': 84,
       'tags': ['vegetables'],
       'sku': 'GRO-BRD-CUC-021',
       'weight': 4,
       'dimensions': {'width': 12.8, 'height': 28.38, 'depth': 21.34},
       'warrantyInformation': '2 year warranty',
       'shippingInformation': 'Ships in 1-2 business days',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 4,
         'comment': 'Great product!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Lincoln Kelly',
         'reviewerEmail': 'lincoln.kelly@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Great value for money!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Savannah Gomez',
         'reviewerEmail': 'savannah.gomez@x.dummyjson.com'},
        {'rating': 2,
         'comment': 'Poor quality!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'James Davis',
         'reviewerEmail': 'james.davis@x.dummyjson.com'}],
       'returnPolicy': '7 days return policy',
       'minimumOrderQuantity': 2,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '5300066378225',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/groceries/cucumber/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/groceries/cucumber/thumbnail.webp'},
      {'id': 22,
       'title': 'Dog Food',
       'description': 'Specially formulated dog food designed to provide essential nutrients for your canine companion.',
       'category': 'groceries',
       'price': 10.99,
       'discountPercentage': 10.27,
       'rating': 4.55,
       'stock': 71,
       'tags': ['pet supplies', 'dog food'],
       'sku': 'GRO-BRD-FOO-022',
       'weight': 10,
       'dimensions': {'width': 16.93, 'height': 27.15, 'depth': 9.29},
       'warrantyInformation': 'No warranty',
       'shippingInformation': 'Ships in 1-2 business days',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 5,
         'comment': 'Excellent quality!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Nicholas Edwards',
         'reviewerEmail': 'nicholas.edwards@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Awesome product!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Zachary Lee',
         'reviewerEmail': 'zachary.lee@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Great product!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Nova Cooper',
         'reviewerEmail': 'nova.cooper@x.dummyjson.com'}],
       'returnPolicy': '60 days return policy',
       'minimumOrderQuantity': 43,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '5906686116469',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/groceries/dog-food/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/groceries/dog-food/thumbnail.webp'},
      {'id': 23,
       'title': 'Eggs',
       'description': 'Fresh eggs, a versatile ingredient for baking, cooking, or breakfast.',
       'category': 'groceries',
       'price': 2.99,
       'discountPercentage': 11.05,
       'rating': 2.53,
       'stock': 9,
       'tags': ['dairy'],
       'sku': 'GRO-BRD-EGG-023',
       'weight': 2,
       'dimensions': {'width': 11.42, 'height': 7.44, 'depth': 16.95},
       'warrantyInformation': '1 week warranty',
       'shippingInformation': 'Ships in 1 week',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 3,
         'comment': 'Disappointing product!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Penelope King',
         'reviewerEmail': 'penelope.king@x.dummyjson.com'},
        {'rating': 3,
         'comment': 'Poor quality!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Eleanor Tyler',
         'reviewerEmail': 'eleanor.tyler@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Very pleased!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Benjamin Foster',
         'reviewerEmail': 'benjamin.foster@x.dummyjson.com'}],
       'returnPolicy': 'No return policy',
       'minimumOrderQuantity': 32,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '3478638588469',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/groceries/eggs/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/groceries/eggs/thumbnail.webp'},
      {'id': 24,
       'title': 'Fish Steak',
       'description': 'Quality fish steak, suitable for grilling, baking, or pan-searing.',
       'category': 'groceries',
       'price': 14.99,
       'discountPercentage': 4.23,
       'rating': 3.78,
       'stock': 74,
       'tags': ['seafood'],
       'sku': 'GRO-BRD-FIS-024',
       'weight': 6,
       'dimensions': {'width': 14.95, 'height': 26.31, 'depth': 11.27},
       'warrantyInformation': '1 month warranty',
       'shippingInformation': 'Ships in 3-5 business days',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 2,
         'comment': 'Would not buy again!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Caleb Perkins',
         'reviewerEmail': 'caleb.perkins@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Excellent quality!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Isabella Jackson',
         'reviewerEmail': 'isabella.jackson@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Great value for money!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Nathan Dixon',
         'reviewerEmail': 'nathan.dixon@x.dummyjson.com'}],
       'returnPolicy': '60 days return policy',
       'minimumOrderQuantity': 50,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '9595036192098',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/groceries/fish-steak/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/groceries/fish-steak/thumbnail.webp'},
      {'id': 25,
       'title': 'Green Bell Pepper',
       'description': 'Fresh and vibrant green bell pepper, perfect for adding color and flavor to your dishes.',
       'category': 'groceries',
       'price': 1.29,
       'discountPercentage': 0.16,
       'rating': 3.25,
       'stock': 33,
       'tags': ['vegetables'],
       'sku': 'GRO-BRD-GRE-025',
       'weight': 2,
       'dimensions': {'width': 15.33, 'height': 26.65, 'depth': 14.44},
       'warrantyInformation': '1 month warranty',
       'shippingInformation': 'Ships in 1 week',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 4,
         'comment': 'Highly recommended!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Avery Carter',
         'reviewerEmail': 'avery.carter@x.dummyjson.com'},
        {'rating': 3,
         'comment': 'Would not recommend!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Henry Hill',
         'reviewerEmail': 'henry.hill@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Excellent quality!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Addison Wright',
         'reviewerEmail': 'addison.wright@x.dummyjson.com'}],
       'returnPolicy': '30 days return policy',
       'minimumOrderQuantity': 12,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '2365227493323',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/groceries/green-bell-pepper/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/groceries/green-bell-pepper/thumbnail.webp'},
      {'id': 26,
       'title': 'Green Chili Pepper',
       'description': 'Spicy green chili pepper, ideal for adding heat to your favorite recipes.',
       'category': 'groceries',
       'price': 0.99,
       'discountPercentage': 1,
       'rating': 3.66,
       'stock': 3,
       'tags': ['vegetables'],
       'sku': 'GRO-BRD-GRE-026',
       'weight': 7,
       'dimensions': {'width': 15.38, 'height': 18.12, 'depth': 19.92},
       'warrantyInformation': '2 year warranty',
       'shippingInformation': 'Ships in 1 week',
       'availabilityStatus': 'Low Stock',
       'reviews': [{'rating': 4,
         'comment': 'Great product!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Luna Russell',
         'reviewerEmail': 'luna.russell@x.dummyjson.com'},
        {'rating': 1,
         'comment': 'Waste of money!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Noah Lewis',
         'reviewerEmail': 'noah.lewis@x.dummyjson.com'},
        {'rating': 3,
         'comment': 'Very disappointed!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Clara Berry',
         'reviewerEmail': 'clara.berry@x.dummyjson.com'}],
       'returnPolicy': '30 days return policy',
       'minimumOrderQuantity': 39,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '9335000538563',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/groceries/green-chili-pepper/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/groceries/green-chili-pepper/thumbnail.webp'},
      {'id': 27,
       'title': 'Honey Jar',
       'description': 'Pure and natural honey in a convenient jar, perfect for sweetening beverages or drizzling over food.',
       'category': 'groceries',
       'price': 6.99,
       'discountPercentage': 14.4,
       'rating': 3.97,
       'stock': 34,
       'tags': ['condiments'],
       'sku': 'GRO-BRD-HON-027',
       'weight': 2,
       'dimensions': {'width': 9.28, 'height': 21.72, 'depth': 17.74},
       'warrantyInformation': '1 month warranty',
       'shippingInformation': 'Ships in 1-2 business days',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 1,
         'comment': 'Very disappointed!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Autumn Gomez',
         'reviewerEmail': 'autumn.gomez@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Highly impressed!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Benjamin Wilson',
         'reviewerEmail': 'benjamin.wilson@x.dummyjson.com'},
        {'rating': 2,
         'comment': 'Very disappointed!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Nicholas Edwards',
         'reviewerEmail': 'nicholas.edwards@x.dummyjson.com'}],
       'returnPolicy': '90 days return policy',
       'minimumOrderQuantity': 47,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '6354306346329',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/groceries/honey-jar/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/groceries/honey-jar/thumbnail.webp'},
      {'id': 28,
       'title': 'Ice Cream',
       'description': 'Creamy and delicious ice cream, available in various flavors for a delightful treat.',
       'category': 'groceries',
       'price': 5.49,
       'discountPercentage': 8.69,
       'rating': 3.39,
       'stock': 27,
       'tags': ['desserts'],
       'sku': 'GRO-BRD-CRE-028',
       'weight': 1,
       'dimensions': {'width': 14.83, 'height': 15.07, 'depth': 24.2},
       'warrantyInformation': '1 month warranty',
       'shippingInformation': 'Ships in 2 weeks',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 5,
         'comment': 'Very pleased!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Elijah Cruz',
         'reviewerEmail': 'elijah.cruz@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Excellent quality!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Jace Smith',
         'reviewerEmail': 'jace.smith@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Highly impressed!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Sadie Morales',
         'reviewerEmail': 'sadie.morales@x.dummyjson.com'}],
       'returnPolicy': 'No return policy',
       'minimumOrderQuantity': 42,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '0788954559076',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/groceries/ice-cream/1.webp',
        'https://cdn.dummyjson.com/product-images/groceries/ice-cream/2.webp',
        'https://cdn.dummyjson.com/product-images/groceries/ice-cream/3.webp',
        'https://cdn.dummyjson.com/product-images/groceries/ice-cream/4.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/groceries/ice-cream/thumbnail.webp'},
      {'id': 29,
       'title': 'Juice',
       'description': 'Refreshing fruit juice, packed with vitamins and great for staying hydrated.',
       'category': 'groceries',
       'price': 3.99,
       'discountPercentage': 12.06,
       'rating': 3.94,
       'stock': 50,
       'tags': ['beverages'],
       'sku': 'GRO-BRD-JUI-029',
       'weight': 1,
       'dimensions': {'width': 18.56, 'height': 21.46, 'depth': 28.02},
       'warrantyInformation': '6 months warranty',
       'shippingInformation': 'Ships in 1 week',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 5,
         'comment': 'Excellent quality!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Nolan Gonzalez',
         'reviewerEmail': 'nolan.gonzalez@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Would buy again!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Bella Grant',
         'reviewerEmail': 'bella.grant@x.dummyjson.com'},
        {'rating': 5,
         'comment': 'Awesome product!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Aria Flores',
         'reviewerEmail': 'aria.flores@x.dummyjson.com'}],
       'returnPolicy': 'No return policy',
       'minimumOrderQuantity': 25,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '6936112580956',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/groceries/juice/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/groceries/juice/thumbnail.webp'},
      {'id': 30,
       'title': 'Kiwi',
       'description': 'Nutrient-rich kiwi, perfect for snacking or adding a tropical twist to your dishes.',
       'category': 'groceries',
       'price': 2.49,
       'discountPercentage': 15.22,
       'rating': 4.93,
       'stock': 99,
       'tags': ['fruits'],
       'sku': 'GRO-BRD-KIW-030',
       'weight': 5,
       'dimensions': {'width': 19.4, 'height': 18.67, 'depth': 17.13},
       'warrantyInformation': '6 months warranty',
       'shippingInformation': 'Ships overnight',
       'availabilityStatus': 'In Stock',
       'reviews': [{'rating': 4,
         'comment': 'Highly recommended!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Emily Brown',
         'reviewerEmail': 'emily.brown@x.dummyjson.com'},
        {'rating': 2,
         'comment': 'Would not buy again!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Jackson Morales',
         'reviewerEmail': 'jackson.morales@x.dummyjson.com'},
        {'rating': 4,
         'comment': 'Fast shipping!',
         'date': '2025-04-30T09:41:02.053Z',
         'reviewerName': 'Nora Russell',
         'reviewerEmail': 'nora.russell@x.dummyjson.com'}],
       'returnPolicy': '7 days return policy',
       'minimumOrderQuantity': 30,
       'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
        'updatedAt': '2025-04-30T09:41:02.053Z',
        'barcode': '2530169917252',
        'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
       'images': ['https://cdn.dummyjson.com/product-images/groceries/kiwi/1.webp'],
       'thumbnail': 'https://cdn.dummyjson.com/product-images/groceries/kiwi/thumbnail.webp'}],
     'total': 194,
     'skip': 0,
     'limit': 30}




```python
import requests

@tool
def products(text: str) -> str:
    """Retorna lista de produtos de uma loja de
    comércio eletrônico chamada ShopAI.
    Não requer parâmetro de entrada, se necessário informe vazio.
    O resultado será na forma JSON somente com o campo title e price."""

    retorno = requests.get("https://dummyjson.com/products")
    items = json.loads(retorno.content)
    items = items["products"][:3]

    return items
```


```python
products("")
```

    /var/folders/wg/hmxmh5ds0x73wpdxzt6w7k340000gn/T/ipykernel_11510/2952740257.py:1: LangChainDeprecationWarning: The method `BaseTool.__call__` was deprecated in langchain-core 0.1.47 and will be removed in 1.0. Use :meth:`~invoke` instead.
      products("")
    




    [{'id': 1,
      'title': 'Essence Mascara Lash Princess',
      'description': 'The Essence Mascara Lash Princess is a popular mascara known for its volumizing and lengthening effects. Achieve dramatic lashes with this long-lasting and cruelty-free formula.',
      'category': 'beauty',
      'price': 9.99,
      'discountPercentage': 10.48,
      'rating': 2.56,
      'stock': 99,
      'tags': ['beauty', 'mascara'],
      'brand': 'Essence',
      'sku': 'BEA-ESS-ESS-001',
      'weight': 4,
      'dimensions': {'width': 15.14, 'height': 13.08, 'depth': 22.99},
      'warrantyInformation': '1 week warranty',
      'shippingInformation': 'Ships in 3-5 business days',
      'availabilityStatus': 'In Stock',
      'reviews': [{'rating': 3,
        'comment': 'Would not recommend!',
        'date': '2025-04-30T09:41:02.053Z',
        'reviewerName': 'Eleanor Collins',
        'reviewerEmail': 'eleanor.collins@x.dummyjson.com'},
       {'rating': 4,
        'comment': 'Very satisfied!',
        'date': '2025-04-30T09:41:02.053Z',
        'reviewerName': 'Lucas Gordon',
        'reviewerEmail': 'lucas.gordon@x.dummyjson.com'},
       {'rating': 5,
        'comment': 'Highly impressed!',
        'date': '2025-04-30T09:41:02.053Z',
        'reviewerName': 'Eleanor Collins',
        'reviewerEmail': 'eleanor.collins@x.dummyjson.com'}],
      'returnPolicy': 'No return policy',
      'minimumOrderQuantity': 48,
      'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
       'updatedAt': '2025-04-30T09:41:02.053Z',
       'barcode': '5784719087687',
       'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
      'images': ['https://cdn.dummyjson.com/product-images/beauty/essence-mascara-lash-princess/1.webp'],
      'thumbnail': 'https://cdn.dummyjson.com/product-images/beauty/essence-mascara-lash-princess/thumbnail.webp'},
     {'id': 2,
      'title': 'Eyeshadow Palette with Mirror',
      'description': "The Eyeshadow Palette with Mirror offers a versatile range of eyeshadow shades for creating stunning eye looks. With a built-in mirror, it's convenient for on-the-go makeup application.",
      'category': 'beauty',
      'price': 19.99,
      'discountPercentage': 18.19,
      'rating': 2.86,
      'stock': 34,
      'tags': ['beauty', 'eyeshadow'],
      'brand': 'Glamour Beauty',
      'sku': 'BEA-GLA-EYE-002',
      'weight': 9,
      'dimensions': {'width': 9.26, 'height': 22.47, 'depth': 27.67},
      'warrantyInformation': '1 year warranty',
      'shippingInformation': 'Ships in 2 weeks',
      'availabilityStatus': 'In Stock',
      'reviews': [{'rating': 5,
        'comment': 'Great product!',
        'date': '2025-04-30T09:41:02.053Z',
        'reviewerName': 'Savannah Gomez',
        'reviewerEmail': 'savannah.gomez@x.dummyjson.com'},
       {'rating': 4,
        'comment': 'Awesome product!',
        'date': '2025-04-30T09:41:02.053Z',
        'reviewerName': 'Christian Perez',
        'reviewerEmail': 'christian.perez@x.dummyjson.com'},
       {'rating': 1,
        'comment': 'Poor quality!',
        'date': '2025-04-30T09:41:02.053Z',
        'reviewerName': 'Nicholas Bailey',
        'reviewerEmail': 'nicholas.bailey@x.dummyjson.com'}],
      'returnPolicy': '7 days return policy',
      'minimumOrderQuantity': 20,
      'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
       'updatedAt': '2025-04-30T09:41:02.053Z',
       'barcode': '9170275171413',
       'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
      'images': ['https://cdn.dummyjson.com/product-images/beauty/eyeshadow-palette-with-mirror/1.webp'],
      'thumbnail': 'https://cdn.dummyjson.com/product-images/beauty/eyeshadow-palette-with-mirror/thumbnail.webp'},
     {'id': 3,
      'title': 'Powder Canister',
      'description': 'The Powder Canister is a finely milled setting powder designed to set makeup and control shine. With a lightweight and translucent formula, it provides a smooth and matte finish.',
      'category': 'beauty',
      'price': 14.99,
      'discountPercentage': 9.84,
      'rating': 4.64,
      'stock': 89,
      'tags': ['beauty', 'face powder'],
      'brand': 'Velvet Touch',
      'sku': 'BEA-VEL-POW-003',
      'weight': 8,
      'dimensions': {'width': 29.27, 'height': 27.93, 'depth': 20.59},
      'warrantyInformation': '3 months warranty',
      'shippingInformation': 'Ships in 1-2 business days',
      'availabilityStatus': 'In Stock',
      'reviews': [{'rating': 4,
        'comment': 'Would buy again!',
        'date': '2025-04-30T09:41:02.053Z',
        'reviewerName': 'Alexander Jones',
        'reviewerEmail': 'alexander.jones@x.dummyjson.com'},
       {'rating': 5,
        'comment': 'Highly impressed!',
        'date': '2025-04-30T09:41:02.053Z',
        'reviewerName': 'Elijah Cruz',
        'reviewerEmail': 'elijah.cruz@x.dummyjson.com'},
       {'rating': 1,
        'comment': 'Very dissatisfied!',
        'date': '2025-04-30T09:41:02.053Z',
        'reviewerName': 'Avery Perez',
        'reviewerEmail': 'avery.perez@x.dummyjson.com'}],
      'returnPolicy': 'No return policy',
      'minimumOrderQuantity': 22,
      'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
       'updatedAt': '2025-04-30T09:41:02.053Z',
       'barcode': '8418883906837',
       'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
      'images': ['https://cdn.dummyjson.com/product-images/beauty/powder-canister/1.webp'],
      'thumbnail': 'https://cdn.dummyjson.com/product-images/beauty/powder-canister/thumbnail.webp'}]




```python
@tool
def product(text: str) -> str:
    """Retorna o produto baseado na sua identificação.
    Ela é numérica, mas deve ser enviado no formato texto e é obrigatória ser passado.
    Esse é um produto do comércio eletrônico chamada ShopAI.
    O resultado será na forma JSON ou dicionário."""

    retorno = requests.get("https://dummyjson.com/products/"+text)
    item = json.loads(retorno.content)

    return item
```


```python
product("1")
```




    {'id': 1,
     'title': 'Essence Mascara Lash Princess',
     'description': 'The Essence Mascara Lash Princess is a popular mascara known for its volumizing and lengthening effects. Achieve dramatic lashes with this long-lasting and cruelty-free formula.',
     'category': 'beauty',
     'price': 9.99,
     'discountPercentage': 10.48,
     'rating': 2.56,
     'stock': 99,
     'tags': ['beauty', 'mascara'],
     'brand': 'Essence',
     'sku': 'BEA-ESS-ESS-001',
     'weight': 4,
     'dimensions': {'width': 15.14, 'height': 13.08, 'depth': 22.99},
     'warrantyInformation': '1 week warranty',
     'shippingInformation': 'Ships in 3-5 business days',
     'availabilityStatus': 'In Stock',
     'reviews': [{'rating': 3,
       'comment': 'Would not recommend!',
       'date': '2025-04-30T09:41:02.053Z',
       'reviewerName': 'Eleanor Collins',
       'reviewerEmail': 'eleanor.collins@x.dummyjson.com'},
      {'rating': 4,
       'comment': 'Very satisfied!',
       'date': '2025-04-30T09:41:02.053Z',
       'reviewerName': 'Lucas Gordon',
       'reviewerEmail': 'lucas.gordon@x.dummyjson.com'},
      {'rating': 5,
       'comment': 'Highly impressed!',
       'date': '2025-04-30T09:41:02.053Z',
       'reviewerName': 'Eleanor Collins',
       'reviewerEmail': 'eleanor.collins@x.dummyjson.com'}],
     'returnPolicy': 'No return policy',
     'minimumOrderQuantity': 48,
     'meta': {'createdAt': '2025-04-30T09:41:02.053Z',
      'updatedAt': '2025-04-30T09:41:02.053Z',
      'barcode': '5784719087687',
      'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'},
     'images': ['https://cdn.dummyjson.com/product-images/beauty/essence-mascara-lash-princess/1.webp'],
     'thumbnail': 'https://cdn.dummyjson.com/product-images/beauty/essence-mascara-lash-princess/thumbnail.webp'}



Iniciamos o agente como os anteriores.


```python
prompt = hub.pull("hwchase17/react")
```

    /Users/michelpf/.pyenv/versions/3.10.13/lib/python3.10/site-packages/langsmith/client.py:272: LangSmithMissingAPIKeyWarning: API key must be provided when using hosted LangSmith API
      warnings.warn(
    


```python
tools = [products, product]
```


```python
agent = create_react_agent(
    chat_openai,
    tools,
    prompt)
```


```python
agent_executor = AgentExecutor(
    agent=agent, tools=tools, verbose=True, handle_parsing_errors=True
)
```

Pelas descrições, o modelo LLM será capaz de realizar um "planejamento" sobre que ação tomar e quais dados precisa para cada ação.


```python
agent_executor.invoke({"input": "Traga a lista de produtos da loja ShopAI."})
```

    
    
    [1m> Entering new AgentExecutor chain...[0m
    [32;1m[1;3mThought: Para responder à pergunta, preciso obter a lista de produtos da loja ShopAI usando a função products.
    Action: products
    Action Input: [0m[36;1m[1;3m[{'id': 1, 'title': 'Essence Mascara Lash Princess', 'description': 'The Essence Mascara Lash Princess is a popular mascara known for its volumizing and lengthening effects. Achieve dramatic lashes with this long-lasting and cruelty-free formula.', 'category': 'beauty', 'price': 9.99, 'discountPercentage': 10.48, 'rating': 2.56, 'stock': 99, 'tags': ['beauty', 'mascara'], 'brand': 'Essence', 'sku': 'BEA-ESS-ESS-001', 'weight': 4, 'dimensions': {'width': 15.14, 'height': 13.08, 'depth': 22.99}, 'warrantyInformation': '1 week warranty', 'shippingInformation': 'Ships in 3-5 business days', 'availabilityStatus': 'In Stock', 'reviews': [{'rating': 3, 'comment': 'Would not recommend!', 'date': '2025-04-30T09:41:02.053Z', 'reviewerName': 'Eleanor Collins', 'reviewerEmail': 'eleanor.collins@x.dummyjson.com'}, {'rating': 4, 'comment': 'Very satisfied!', 'date': '2025-04-30T09:41:02.053Z', 'reviewerName': 'Lucas Gordon', 'reviewerEmail': 'lucas.gordon@x.dummyjson.com'}, {'rating': 5, 'comment': 'Highly impressed!', 'date': '2025-04-30T09:41:02.053Z', 'reviewerName': 'Eleanor Collins', 'reviewerEmail': 'eleanor.collins@x.dummyjson.com'}], 'returnPolicy': 'No return policy', 'minimumOrderQuantity': 48, 'meta': {'createdAt': '2025-04-30T09:41:02.053Z', 'updatedAt': '2025-04-30T09:41:02.053Z', 'barcode': '5784719087687', 'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'}, 'images': ['https://cdn.dummyjson.com/product-images/beauty/essence-mascara-lash-princess/1.webp'], 'thumbnail': 'https://cdn.dummyjson.com/product-images/beauty/essence-mascara-lash-princess/thumbnail.webp'}, {'id': 2, 'title': 'Eyeshadow Palette with Mirror', 'description': "The Eyeshadow Palette with Mirror offers a versatile range of eyeshadow shades for creating stunning eye looks. With a built-in mirror, it's convenient for on-the-go makeup application.", 'category': 'beauty', 'price': 19.99, 'discountPercentage': 18.19, 'rating': 2.86, 'stock': 34, 'tags': ['beauty', 'eyeshadow'], 'brand': 'Glamour Beauty', 'sku': 'BEA-GLA-EYE-002', 'weight': 9, 'dimensions': {'width': 9.26, 'height': 22.47, 'depth': 27.67}, 'warrantyInformation': '1 year warranty', 'shippingInformation': 'Ships in 2 weeks', 'availabilityStatus': 'In Stock', 'reviews': [{'rating': 5, 'comment': 'Great product!', 'date': '2025-04-30T09:41:02.053Z', 'reviewerName': 'Savannah Gomez', 'reviewerEmail': 'savannah.gomez@x.dummyjson.com'}, {'rating': 4, 'comment': 'Awesome product!', 'date': '2025-04-30T09:41:02.053Z', 'reviewerName': 'Christian Perez', 'reviewerEmail': 'christian.perez@x.dummyjson.com'}, {'rating': 1, 'comment': 'Poor quality!', 'date': '2025-04-30T09:41:02.053Z', 'reviewerName': 'Nicholas Bailey', 'reviewerEmail': 'nicholas.bailey@x.dummyjson.com'}], 'returnPolicy': '7 days return policy', 'minimumOrderQuantity': 20, 'meta': {'createdAt': '2025-04-30T09:41:02.053Z', 'updatedAt': '2025-04-30T09:41:02.053Z', 'barcode': '9170275171413', 'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'}, 'images': ['https://cdn.dummyjson.com/product-images/beauty/eyeshadow-palette-with-mirror/1.webp'], 'thumbnail': 'https://cdn.dummyjson.com/product-images/beauty/eyeshadow-palette-with-mirror/thumbnail.webp'}, {'id': 3, 'title': 'Powder Canister', 'description': 'The Powder Canister is a finely milled setting powder designed to set makeup and control shine. With a lightweight and translucent formula, it provides a smooth and matte finish.', 'category': 'beauty', 'price': 14.99, 'discountPercentage': 9.84, 'rating': 4.64, 'stock': 89, 'tags': ['beauty', 'face powder'], 'brand': 'Velvet Touch', 'sku': 'BEA-VEL-POW-003', 'weight': 8, 'dimensions': {'width': 29.27, 'height': 27.93, 'depth': 20.59}, 'warrantyInformation': '3 months warranty', 'shippingInformation': 'Ships in 1-2 business days', 'availabilityStatus': 'In Stock', 'reviews': [{'rating': 4, 'comment': 'Would buy again!', 'date': '2025-04-30T09:41:02.053Z', 'reviewerName': 'Alexander Jones', 'reviewerEmail': 'alexander.jones@x.dummyjson.com'}, {'rating': 5, 'comment': 'Highly impressed!', 'date': '2025-04-30T09:41:02.053Z', 'reviewerName': 'Elijah Cruz', 'reviewerEmail': 'elijah.cruz@x.dummyjson.com'}, {'rating': 1, 'comment': 'Very dissatisfied!', 'date': '2025-04-30T09:41:02.053Z', 'reviewerName': 'Avery Perez', 'reviewerEmail': 'avery.perez@x.dummyjson.com'}], 'returnPolicy': 'No return policy', 'minimumOrderQuantity': 22, 'meta': {'createdAt': '2025-04-30T09:41:02.053Z', 'updatedAt': '2025-04-30T09:41:02.053Z', 'barcode': '8418883906837', 'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'}, 'images': ['https://cdn.dummyjson.com/product-images/beauty/powder-canister/1.webp'], 'thumbnail': 'https://cdn.dummyjson.com/product-images/beauty/powder-canister/thumbnail.webp'}][0m[32;1m[1;3mThought: Já tenho a lista de produtos da loja ShopAI, com título e preço de cada um.
    Final Answer: A lista de produtos da loja ShopAI é:
    1. Essence Mascara Lash Princess - R$ 9,99
    2. Eyeshadow Palette with Mirror - R$ 19,99
    3. Powder Canister - R$ 14,99[0m
    
    [1m> Finished chain.[0m
    




    {'input': 'Traga a lista de produtos da loja ShopAI.',
     'output': 'A lista de produtos da loja ShopAI é:\n1. Essence Mascara Lash Princess - R$ 9,99\n2. Eyeshadow Palette with Mirror - R$ 19,99\n3. Powder Canister - R$ 14,99'}




```python
agent_executor.invoke({"input": "Traga o produto com identificação 1 da loja ShopAI."})
```

    
    
    [1m> Entering new AgentExecutor chain...[0m
    [32;1m[1;3mThought: Vou buscar o produto com a identificação "1" na loja ShopAI usando a função product.
    Action: product
    Action Input: "1"[0m[33;1m[1;3m{'id': 1, 'title': 'Essence Mascara Lash Princess', 'description': 'The Essence Mascara Lash Princess is a popular mascara known for its volumizing and lengthening effects. Achieve dramatic lashes with this long-lasting and cruelty-free formula.', 'category': 'beauty', 'price': 9.99, 'discountPercentage': 10.48, 'rating': 2.56, 'stock': 99, 'tags': ['beauty', 'mascara'], 'brand': 'Essence', 'sku': 'BEA-ESS-ESS-001', 'weight': 4, 'dimensions': {'width': 15.14, 'height': 13.08, 'depth': 22.99}, 'warrantyInformation': '1 week warranty', 'shippingInformation': 'Ships in 3-5 business days', 'availabilityStatus': 'In Stock', 'reviews': [{'rating': 3, 'comment': 'Would not recommend!', 'date': '2025-04-30T09:41:02.053Z', 'reviewerName': 'Eleanor Collins', 'reviewerEmail': 'eleanor.collins@x.dummyjson.com'}, {'rating': 4, 'comment': 'Very satisfied!', 'date': '2025-04-30T09:41:02.053Z', 'reviewerName': 'Lucas Gordon', 'reviewerEmail': 'lucas.gordon@x.dummyjson.com'}, {'rating': 5, 'comment': 'Highly impressed!', 'date': '2025-04-30T09:41:02.053Z', 'reviewerName': 'Eleanor Collins', 'reviewerEmail': 'eleanor.collins@x.dummyjson.com'}], 'returnPolicy': 'No return policy', 'minimumOrderQuantity': 48, 'meta': {'createdAt': '2025-04-30T09:41:02.053Z', 'updatedAt': '2025-04-30T09:41:02.053Z', 'barcode': '5784719087687', 'qrCode': 'https://cdn.dummyjson.com/public/qr-code.png'}, 'images': ['https://cdn.dummyjson.com/product-images/beauty/essence-mascara-lash-princess/1.webp'], 'thumbnail': 'https://cdn.dummyjson.com/product-images/beauty/essence-mascara-lash-princess/thumbnail.webp'}[0m[32;1m[1;3mThought: Já obtive as informações do produto com identificação 1, que é a Essence Mascara Lash Princess, incluindo detalhes como preço, descrição, avaliações e imagens.
    Final Answer: O produto com identificação 1 é a Essence Mascara Lash Princess. É uma máscara de cílios conhecida por seus efeitos de volume e alongamento, com preço de 9,99. Está disponível em estoque, possui garantia de 1 semana e não aceita devolução. Aqui está a imagem do produto: https://cdn.dummyjson.com/product-images/beauty/essence-mascara-lash-princess/1.webp[0m
    
    [1m> Finished chain.[0m
    




    {'input': 'Traga o produto com identificação 1 da loja ShopAI.',
     'output': 'O produto com identificação 1 é a Essence Mascara Lash Princess. É uma máscara de cílios conhecida por seus efeitos de volume e alongamento, com preço de 9,99. Está disponível em estoque, possui garantia de 1 semana e não aceita devolução. Aqui está a imagem do produto: https://cdn.dummyjson.com/product-images/beauty/essence-mascara-lash-princess/1.webp'}


