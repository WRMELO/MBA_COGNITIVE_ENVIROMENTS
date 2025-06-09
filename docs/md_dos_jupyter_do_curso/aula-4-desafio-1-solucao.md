# Desafio 1

Revise as avaliações da Amazon utilizando LLM e extraia as seguintes informações: resumo em até 300 caracteres em tom calmo, análise de sentimento (positivo, negativou ou neutro) e uma pontuação de -10 até +10 em relação ao sentimento detectado, sendo -10 mais negativo, 0 neutro e + 10 mais positivo.

Utilize o dataset disponibilizado neste repositório https://github.com/michelpf/dataset-customer-evaluations.


```python
!pip install openai langchain tiktoken docarray wikipedia xmltodict langchain-openai
```

    Collecting openai
      Downloading openai-1.9.0-py3-none-any.whl (223 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m223.4/223.4 kB[0m [31m1.3 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting langchain
      Downloading langchain-0.1.2-py3-none-any.whl (803 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m803.6/803.6 kB[0m [31m25.5 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting tiktoken
      Downloading tiktoken-0.5.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (2.0 MB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m2.0/2.0 MB[0m [31m51.6 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting docarray
      Downloading docarray-0.40.0-py3-none-any.whl (270 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m270.2/270.2 kB[0m [31m20.2 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting wikipedia
      Downloading wikipedia-1.4.0.tar.gz (27 kB)
      Preparing metadata (setup.py) ... [?25l[?25hdone
    Collecting xmltodict
      Downloading xmltodict-0.13.0-py2.py3-none-any.whl (10.0 kB)
    Collecting langchain-openai
      Downloading langchain_openai-0.0.3-py3-none-any.whl (28 kB)
    Requirement already satisfied: anyio<5,>=3.5.0 in /usr/local/lib/python3.10/dist-packages (from openai) (3.7.1)
    Requirement already satisfied: distro<2,>=1.7.0 in /usr/lib/python3/dist-packages (from openai) (1.7.0)
    Collecting httpx<1,>=0.23.0 (from openai)
      Downloading httpx-0.26.0-py3-none-any.whl (75 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m75.9/75.9 kB[0m [31m6.3 MB/s[0m eta [36m0:00:00[0m
    [?25hRequirement already satisfied: pydantic<3,>=1.9.0 in /usr/local/lib/python3.10/dist-packages (from openai) (1.10.13)
    Requirement already satisfied: sniffio in /usr/local/lib/python3.10/dist-packages (from openai) (1.3.0)
    Requirement already satisfied: tqdm>4 in /usr/local/lib/python3.10/dist-packages (from openai) (4.66.1)
    Collecting typing-extensions<5,>=4.7 (from openai)
      Downloading typing_extensions-4.9.0-py3-none-any.whl (32 kB)
    Requirement already satisfied: PyYAML>=5.3 in /usr/local/lib/python3.10/dist-packages (from langchain) (6.0.1)
    Requirement already satisfied: SQLAlchemy<3,>=1.4 in /usr/local/lib/python3.10/dist-packages (from langchain) (2.0.24)
    Requirement already satisfied: aiohttp<4.0.0,>=3.8.3 in /usr/local/lib/python3.10/dist-packages (from langchain) (3.9.1)
    Requirement already satisfied: async-timeout<5.0.0,>=4.0.0 in /usr/local/lib/python3.10/dist-packages (from langchain) (4.0.3)
    Collecting dataclasses-json<0.7,>=0.5.7 (from langchain)
      Downloading dataclasses_json-0.6.3-py3-none-any.whl (28 kB)
    Collecting jsonpatch<2.0,>=1.33 (from langchain)
      Downloading jsonpatch-1.33-py2.py3-none-any.whl (12 kB)
    Collecting langchain-community<0.1,>=0.0.14 (from langchain)
      Downloading langchain_community-0.0.14-py3-none-any.whl (1.6 MB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m1.6/1.6 MB[0m [31m23.6 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting langchain-core<0.2,>=0.1.14 (from langchain)
      Downloading langchain_core-0.1.14-py3-none-any.whl (229 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m229.5/229.5 kB[0m [31m10.5 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting langsmith<0.0.84,>=0.0.83 (from langchain)
      Downloading langsmith-0.0.83-py3-none-any.whl (49 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m49.3/49.3 kB[0m [31m2.2 MB/s[0m eta [36m0:00:00[0m
    [?25hRequirement already satisfied: numpy<2,>=1 in /usr/local/lib/python3.10/dist-packages (from langchain) (1.23.5)
    Requirement already satisfied: requests<3,>=2 in /usr/local/lib/python3.10/dist-packages (from langchain) (2.31.0)
    Requirement already satisfied: tenacity<9.0.0,>=8.1.0 in /usr/local/lib/python3.10/dist-packages (from langchain) (8.2.3)
    Requirement already satisfied: regex>=2022.1.18 in /usr/local/lib/python3.10/dist-packages (from tiktoken) (2023.6.3)
    Collecting orjson>=3.8.2 (from docarray)
      Downloading orjson-3.9.12-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (139 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m139.8/139.8 kB[0m [31m2.3 MB/s[0m eta [36m0:00:00[0m
    [?25hRequirement already satisfied: rich>=13.1.0 in /usr/local/lib/python3.10/dist-packages (from docarray) (13.7.0)
    Collecting types-requests>=2.28.11.6 (from docarray)
      Downloading types_requests-2.31.0.20240106-py3-none-any.whl (14 kB)
    Collecting typing-inspect>=0.8.0 (from docarray)
      Downloading typing_inspect-0.9.0-py3-none-any.whl (8.8 kB)
    Requirement already satisfied: beautifulsoup4 in /usr/local/lib/python3.10/dist-packages (from wikipedia) (4.11.2)
    Requirement already satisfied: attrs>=17.3.0 in /usr/local/lib/python3.10/dist-packages (from aiohttp<4.0.0,>=3.8.3->langchain) (23.2.0)
    Requirement already satisfied: multidict<7.0,>=4.5 in /usr/local/lib/python3.10/dist-packages (from aiohttp<4.0.0,>=3.8.3->langchain) (6.0.4)
    Requirement already satisfied: yarl<2.0,>=1.0 in /usr/local/lib/python3.10/dist-packages (from aiohttp<4.0.0,>=3.8.3->langchain) (1.9.4)
    Requirement already satisfied: frozenlist>=1.1.1 in /usr/local/lib/python3.10/dist-packages (from aiohttp<4.0.0,>=3.8.3->langchain) (1.4.1)
    Requirement already satisfied: aiosignal>=1.1.2 in /usr/local/lib/python3.10/dist-packages (from aiohttp<4.0.0,>=3.8.3->langchain) (1.3.1)
    Requirement already satisfied: idna>=2.8 in /usr/local/lib/python3.10/dist-packages (from anyio<5,>=3.5.0->openai) (3.6)
    Requirement already satisfied: exceptiongroup in /usr/local/lib/python3.10/dist-packages (from anyio<5,>=3.5.0->openai) (1.2.0)
    Collecting marshmallow<4.0.0,>=3.18.0 (from dataclasses-json<0.7,>=0.5.7->langchain)
      Downloading marshmallow-3.20.2-py3-none-any.whl (49 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m49.4/49.4 kB[0m [31m2.6 MB/s[0m eta [36m0:00:00[0m
    [?25hRequirement already satisfied: certifi in /usr/local/lib/python3.10/dist-packages (from httpx<1,>=0.23.0->openai) (2023.11.17)
    Collecting httpcore==1.* (from httpx<1,>=0.23.0->openai)
      Downloading httpcore-1.0.2-py3-none-any.whl (76 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m76.9/76.9 kB[0m [31m4.1 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting h11<0.15,>=0.13 (from httpcore==1.*->httpx<1,>=0.23.0->openai)
      Downloading h11-0.14.0-py3-none-any.whl (58 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m58.3/58.3 kB[0m [31m6.1 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting jsonpointer>=1.9 (from jsonpatch<2.0,>=1.33->langchain)
      Downloading jsonpointer-2.4-py2.py3-none-any.whl (7.8 kB)
    Requirement already satisfied: packaging<24.0,>=23.2 in /usr/local/lib/python3.10/dist-packages (from langchain-core<0.2,>=0.1.14->langchain) (23.2)
    Requirement already satisfied: charset-normalizer<4,>=2 in /usr/local/lib/python3.10/dist-packages (from requests<3,>=2->langchain) (3.3.2)
    Requirement already satisfied: urllib3<3,>=1.21.1 in /usr/local/lib/python3.10/dist-packages (from requests<3,>=2->langchain) (2.0.7)
    Requirement already satisfied: markdown-it-py>=2.2.0 in /usr/local/lib/python3.10/dist-packages (from rich>=13.1.0->docarray) (3.0.0)
    Requirement already satisfied: pygments<3.0.0,>=2.13.0 in /usr/local/lib/python3.10/dist-packages (from rich>=13.1.0->docarray) (2.16.1)
    Requirement already satisfied: greenlet!=0.4.17 in /usr/local/lib/python3.10/dist-packages (from SQLAlchemy<3,>=1.4->langchain) (3.0.3)
    Collecting mypy-extensions>=0.3.0 (from typing-inspect>=0.8.0->docarray)
      Downloading mypy_extensions-1.0.0-py3-none-any.whl (4.7 kB)
    Requirement already satisfied: soupsieve>1.2 in /usr/local/lib/python3.10/dist-packages (from beautifulsoup4->wikipedia) (2.5)
    Requirement already satisfied: mdurl~=0.1 in /usr/local/lib/python3.10/dist-packages (from markdown-it-py>=2.2.0->rich>=13.1.0->docarray) (0.1.2)
    Building wheels for collected packages: wikipedia
      Building wheel for wikipedia (setup.py) ... [?25l[?25hdone
      Created wheel for wikipedia: filename=wikipedia-1.4.0-py3-none-any.whl size=11678 sha256=0ec51284dbe4f9c3855ae2b439a0f99880d55fc9d6586b7697838176aaf2f2ed
      Stored in directory: /root/.cache/pip/wheels/5e/b6/c5/93f3dec388ae76edc830cb42901bb0232504dfc0df02fc50de
    Successfully built wikipedia
    Installing collected packages: xmltodict, typing-extensions, types-requests, orjson, mypy-extensions, marshmallow, jsonpointer, h11, wikipedia, typing-inspect, tiktoken, jsonpatch, httpcore, langsmith, httpx, docarray, dataclasses-json, openai, langchain-core, langchain-openai, langchain-community, langchain
      Attempting uninstall: typing-extensions
        Found existing installation: typing_extensions 4.5.0
        Uninstalling typing_extensions-4.5.0:
          Successfully uninstalled typing_extensions-4.5.0
    [31mERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
    llmx 0.0.15a0 requires cohere, which is not installed.
    tensorflow-probability 0.22.0 requires typing-extensions<4.6.0, but you have typing-extensions 4.9.0 which is incompatible.[0m[31m
    [0mSuccessfully installed dataclasses-json-0.6.3 docarray-0.40.0 h11-0.14.0 httpcore-1.0.2 httpx-0.26.0 jsonpatch-1.33 jsonpointer-2.4 langchain-0.1.2 langchain-community-0.0.14 langchain-core-0.1.14 langchain-openai-0.0.3 langsmith-0.0.83 marshmallow-3.20.2 mypy-extensions-1.0.0 openai-1.9.0 orjson-3.9.12 tiktoken-0.5.2 types-requests-2.31.0.20240106 typing-extensions-4.9.0 typing-inspect-0.9.0 wikipedia-1.4.0 xmltodict-0.13.0
    


```python
import os
from openai import OpenAI
import langchain
from IPython.display import display, Markdown as display_markdown
import json
import pandas as pd
```


```python
os.environ["OPENAI_API_KEY"] = "sua-chave"
```


```python
model = "gpt-3.5-turbo"
```


```python
from langchain.output_parsers import ResponseSchema
from langchain.output_parsers import StructuredOutputParser
from langchain.chat_models import ChatOpenAI
from langchain.prompts import ChatPromptTemplate
```


```python
!git clone https://github.com/michelpf/dataset-customer-evaluations
```

    Cloning into 'dataset-customer-evaluations'...
    remote: Enumerating objects: 11, done.[K
    remote: Counting objects: 100% (11/11), done.[K
    remote: Compressing objects: 100% (10/10), done.[K
    remote: Total 11 (delta 1), reused 7 (delta 0), pack-reused 0[K
    Receiving objects: 100% (11/11), 2.52 MiB | 4.86 MiB/s, done.
    Resolving deltas: 100% (1/1), done.
    


```python
df = pd.read_csv("dataset-customer-evaluations/dataset/am_scrape_final.csv")
df.head()
```





  <div id="df-57f02d49-3e6f-41ae-9cf2-0c2cbfbc642e" class="colab-df-container">
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
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-57f02d49-3e6f-41ae-9cf2-0c2cbfbc642e')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-57f02d49-3e6f-41ae-9cf2-0c2cbfbc642e button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-57f02d49-3e6f-41ae-9cf2-0c2cbfbc642e');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


<div id="df-f61957b9-3703-4edc-a074-30a079da7a88">
  <button class="colab-df-quickchart" onclick="quickchart('df-f61957b9-3703-4edc-a074-30a079da7a88')"
            title="Suggest charts"
            style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
  </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

  <script>
    async function quickchart(key) {
      const quickchartButtonEl =
        document.querySelector('#' + key + ' button');
      quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
      quickchartButtonEl.classList.add('colab-df-spinner');
      try {
        const charts = await google.colab.kernel.invokeFunction(
            'suggestCharts', [key], {});
      } catch (error) {
        console.error('Error during call to suggestCharts:', error);
      }
      quickchartButtonEl.classList.remove('colab-df-spinner');
      quickchartButtonEl.classList.add('colab-df-quickchart-complete');
    }
    (() => {
      let quickchartButtonEl =
        document.querySelector('#df-f61957b9-3703-4edc-a074-30a079da7a88 button');
      quickchartButtonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';
    })();
  </script>
</div>

    </div>
  </div>





```python
openai_chat = ChatOpenAI(temperature=0.0, model=model)
```

    /usr/local/lib/python3.10/dist-packages/langchain_core/_api/deprecation.py:117: LangChainDeprecationWarning: The class `langchain_community.chat_models.openai.ChatOpenAI` was deprecated in langchain-community 0.0.10 and will be removed in 0.2.0. An updated version of the class exists in the langchain-openai package and should be used instead. To use it run `pip install -U langchain-openai` and import as `from langchain_openai import ChatOpenAI`.
      warn_deprecated(
    


```python
sentiment_score_schema = ResponseSchema(name="score",
                             description="Pontue de -10 a +10 conforme análise sentimental de um dado texto.")
sentiment_schema = ResponseSchema(name="sentiment",
                                      description="Defina como positivo, neutro ou negativo para sintetizar\
                                      a análise sentimental do referido texto.")

response_schemas = [sentiment_score_schema, sentiment_schema]
```


```python
output_parser = StructuredOutputParser.from_response_schemas(response_schemas)
format_instructions = output_parser.get_format_instructions()
format_instructions
```




    'The output should be a markdown code snippet formatted in the following schema, including the leading and trailing "```json" and "```":\n\n```json\n{\n\t"score": string  // Pontue de -10 a +10 conforme análise sentimental de um dado texto.\n\t"sentiment": string  // Defina como positivo, neutro ou negativo para sintetizar                                      a análise sentimental do referido texto.\n}\n```'




```python
user_review = df.iloc[6]["Review"]
user_review
```




    'Entrega: Comprei no dia 28 de abril e chegou 2 de mais (só não chegou antes por conta do feriado), estou com ele a 2 dias e posso afirmar que é um telefone incrívelBateria: bateria dura literalmente o dia todo, ontem sai de casa 7:30 da manhã e só fui carregar umas 00:00, isso que joguei por umas 2 horas ( call os duty e wild Rift ) e também assisti por horas, pra quem não joga dura mais de 1 dia tranquiloTela: dele é maravilhosa pra quem quer assistir, MT linda, grande e com ótimo brilho e cores, sem contar taxa de atualização de 120hz q deixa super fluídoDesemprenho: fenomenal, para quem joga e tem medo de lags e travamento, fica tranquilo, já joguei ( pubg, call of duty, wild Rift e outros ) todos rodaram perfeitamente sem nenhum travamento, mas acredito que se jogar um game extremamente pesado e com TD no máximo deve dar alguma travada, mas pra jogos básicos roda brincandoResumo: super compensa, já tive iPhone e outro xiomi ( poco x3 pro ) e msm tendo esses excelentes telefones, afirmo que esse me atende igualmente a esses outros'




```python
review_template_sentiment = """\
Para uma revisão de clientes sobre um produto, extraia as seguintes informações:

score: Pontue de -10 a +10 conforme análise sentimental de um dado texto

sentiment: Defina como positivo, neutro ou negativo para sintetizar a
análise sentimental do referido texto

Texto da revisão do produto: {user_review}

{format_instructions}
"""

prompt = ChatPromptTemplate.from_template(template=review_template_sentiment)

customer_response_review = prompt.format_messages(user_review=user_review, format_instructions=format_instructions)

customer_response_review
```




    [HumanMessage(content='Para uma revisão de clientes sobre um produto, extraia as seguintes informações:\n\nscore: Pontue de -10 a +10 conforme análise sentimental de um dado texto\n\nsentiment: Defina como positivo, neutro ou negativo para sintetizar a\nanálise sentimental do referido texto\n\nTexto da revisão do produto: Entrega: Comprei no dia 28 de abril e chegou 2 de mais (só não chegou antes por conta do feriado), estou com ele a 2 dias e posso afirmar que é um telefone incrívelBateria: bateria dura literalmente o dia todo, ontem sai de casa 7:30 da manhã e só fui carregar umas 00:00, isso que joguei por umas 2 horas ( call os duty e wild Rift ) e também assisti por horas, pra quem não joga dura mais de 1 dia tranquiloTela: dele é maravilhosa pra quem quer assistir, MT linda, grande e com ótimo brilho e cores, sem contar taxa de atualização de 120hz q deixa super fluídoDesemprenho: fenomenal, para quem joga e tem medo de lags e travamento, fica tranquilo, já joguei ( pubg, call of duty, wild Rift e outros ) todos rodaram perfeitamente sem nenhum travamento, mas acredito que se jogar um game extremamente pesado e com TD no máximo deve dar alguma travada, mas pra jogos básicos roda brincandoResumo: super compensa, já tive iPhone e outro xiomi ( poco x3 pro ) e msm tendo esses excelentes telefones, afirmo que esse me atende igualmente a esses outros\n\nThe output should be a markdown code snippet formatted in the following schema, including the leading and trailing "```json" and "```":\n\n```json\n{\n\t"score": string  // Pontue de -10 a +10 conforme análise sentimental de um dado texto.\n\t"sentiment": string  // Defina como positivo, neutro ou negativo para sintetizar                                      a análise sentimental do referido texto.\n}\n```\n')]




```python
response = openai_chat.invoke(customer_response_review)
response
```




    AIMessage(content='```json\n{\n\t"score": "9",\n\t"sentiment": "positivo"\n}\n```')




```python
result_dict = output_parser.parse(response.content)
result_dict
```




    {'score': '9', 'sentiment': 'positivo'}


