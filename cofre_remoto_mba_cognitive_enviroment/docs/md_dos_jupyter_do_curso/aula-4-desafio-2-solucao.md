# Desafio 1

Implemente o agente do PubMed e utilize o prompt para trazer informações e/ou artigos médicos sobre uma doença rara conhecida como “Febre Familiar do Mediterrâneo”.



```python
!pip install openai langchain docarray xmltodict langchain-openai langchainhub
```

    Collecting openai
      Downloading openai-1.9.0-py3-none-any.whl (223 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m223.4/223.4 kB[0m [31m2.7 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting langchain
      Downloading langchain-0.1.2-py3-none-any.whl (803 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m803.6/803.6 kB[0m [31m10.5 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting docarray
      Downloading docarray-0.40.0-py3-none-any.whl (270 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m270.2/270.2 kB[0m [31m8.0 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting xmltodict
      Downloading xmltodict-0.13.0-py2.py3-none-any.whl (10.0 kB)
    Collecting langchain-openai
      Downloading langchain_openai-0.0.3-py3-none-any.whl (28 kB)
    Collecting langchainhub
      Downloading langchainhub-0.1.14-py3-none-any.whl (3.4 kB)
    Requirement already satisfied: anyio<5,>=3.5.0 in /usr/local/lib/python3.10/dist-packages (from openai) (3.7.1)
    Requirement already satisfied: distro<2,>=1.7.0 in /usr/lib/python3/dist-packages (from openai) (1.7.0)
    Collecting httpx<1,>=0.23.0 (from openai)
      Downloading httpx-0.26.0-py3-none-any.whl (75 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m75.9/75.9 kB[0m [31m4.4 MB/s[0m eta [36m0:00:00[0m
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
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m1.6/1.6 MB[0m [31m11.4 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting langchain-core<0.2,>=0.1.14 (from langchain)
      Downloading langchain_core-0.1.14-py3-none-any.whl (229 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m229.5/229.5 kB[0m [31m14.6 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting langsmith<0.0.84,>=0.0.83 (from langchain)
      Downloading langsmith-0.0.83-py3-none-any.whl (49 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m49.3/49.3 kB[0m [31m2.2 MB/s[0m eta [36m0:00:00[0m
    [?25hRequirement already satisfied: numpy<2,>=1 in /usr/local/lib/python3.10/dist-packages (from langchain) (1.23.5)
    Requirement already satisfied: requests<3,>=2 in /usr/local/lib/python3.10/dist-packages (from langchain) (2.31.0)
    Requirement already satisfied: tenacity<9.0.0,>=8.1.0 in /usr/local/lib/python3.10/dist-packages (from langchain) (8.2.3)
    Collecting orjson>=3.8.2 (from docarray)
      Downloading orjson-3.9.12-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (139 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m139.8/139.8 kB[0m [31m1.2 MB/s[0m eta [36m0:00:00[0m
    [?25hRequirement already satisfied: rich>=13.1.0 in /usr/local/lib/python3.10/dist-packages (from docarray) (13.7.0)
    Collecting types-requests>=2.28.11.6 (from docarray)
      Downloading types_requests-2.31.0.20240106-py3-none-any.whl (14 kB)
    Collecting typing-inspect>=0.8.0 (from docarray)
      Downloading typing_inspect-0.9.0-py3-none-any.whl (8.8 kB)
    Collecting tiktoken<0.6.0,>=0.5.2 (from langchain-openai)
      Downloading tiktoken-0.5.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (2.0 MB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m2.0/2.0 MB[0m [31m19.1 MB/s[0m eta [36m0:00:00[0m
    [?25hRequirement already satisfied: attrs>=17.3.0 in /usr/local/lib/python3.10/dist-packages (from aiohttp<4.0.0,>=3.8.3->langchain) (23.2.0)
    Requirement already satisfied: multidict<7.0,>=4.5 in /usr/local/lib/python3.10/dist-packages (from aiohttp<4.0.0,>=3.8.3->langchain) (6.0.4)
    Requirement already satisfied: yarl<2.0,>=1.0 in /usr/local/lib/python3.10/dist-packages (from aiohttp<4.0.0,>=3.8.3->langchain) (1.9.4)
    Requirement already satisfied: frozenlist>=1.1.1 in /usr/local/lib/python3.10/dist-packages (from aiohttp<4.0.0,>=3.8.3->langchain) (1.4.1)
    Requirement already satisfied: aiosignal>=1.1.2 in /usr/local/lib/python3.10/dist-packages (from aiohttp<4.0.0,>=3.8.3->langchain) (1.3.1)
    Requirement already satisfied: idna>=2.8 in /usr/local/lib/python3.10/dist-packages (from anyio<5,>=3.5.0->openai) (3.6)
    Requirement already satisfied: exceptiongroup in /usr/local/lib/python3.10/dist-packages (from anyio<5,>=3.5.0->openai) (1.2.0)
    Collecting marshmallow<4.0.0,>=3.18.0 (from dataclasses-json<0.7,>=0.5.7->langchain)
      Downloading marshmallow-3.20.2-py3-none-any.whl (49 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m49.4/49.4 kB[0m [31m4.0 MB/s[0m eta [36m0:00:00[0m
    [?25hRequirement already satisfied: certifi in /usr/local/lib/python3.10/dist-packages (from httpx<1,>=0.23.0->openai) (2023.11.17)
    Collecting httpcore==1.* (from httpx<1,>=0.23.0->openai)
      Downloading httpcore-1.0.2-py3-none-any.whl (76 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m76.9/76.9 kB[0m [31m5.0 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting h11<0.15,>=0.13 (from httpcore==1.*->httpx<1,>=0.23.0->openai)
      Downloading h11-0.14.0-py3-none-any.whl (58 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m58.3/58.3 kB[0m [31m6.2 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting jsonpointer>=1.9 (from jsonpatch<2.0,>=1.33->langchain)
      Downloading jsonpointer-2.4-py2.py3-none-any.whl (7.8 kB)
    Requirement already satisfied: packaging<24.0,>=23.2 in /usr/local/lib/python3.10/dist-packages (from langchain-core<0.2,>=0.1.14->langchain) (23.2)
    Requirement already satisfied: charset-normalizer<4,>=2 in /usr/local/lib/python3.10/dist-packages (from requests<3,>=2->langchain) (3.3.2)
    Requirement already satisfied: urllib3<3,>=1.21.1 in /usr/local/lib/python3.10/dist-packages (from requests<3,>=2->langchain) (2.0.7)
    Requirement already satisfied: markdown-it-py>=2.2.0 in /usr/local/lib/python3.10/dist-packages (from rich>=13.1.0->docarray) (3.0.0)
    Requirement already satisfied: pygments<3.0.0,>=2.13.0 in /usr/local/lib/python3.10/dist-packages (from rich>=13.1.0->docarray) (2.16.1)
    Requirement already satisfied: greenlet!=0.4.17 in /usr/local/lib/python3.10/dist-packages (from SQLAlchemy<3,>=1.4->langchain) (3.0.3)
    Requirement already satisfied: regex>=2022.1.18 in /usr/local/lib/python3.10/dist-packages (from tiktoken<0.6.0,>=0.5.2->langchain-openai) (2023.6.3)
    Collecting mypy-extensions>=0.3.0 (from typing-inspect>=0.8.0->docarray)
      Downloading mypy_extensions-1.0.0-py3-none-any.whl (4.7 kB)
    Requirement already satisfied: mdurl~=0.1 in /usr/local/lib/python3.10/dist-packages (from markdown-it-py>=2.2.0->rich>=13.1.0->docarray) (0.1.2)
    Installing collected packages: xmltodict, typing-extensions, types-requests, orjson, mypy-extensions, marshmallow, jsonpointer, h11, typing-inspect, tiktoken, langchainhub, jsonpatch, httpcore, langsmith, httpx, docarray, dataclasses-json, openai, langchain-core, langchain-openai, langchain-community, langchain
      Attempting uninstall: typing-extensions
        Found existing installation: typing_extensions 4.5.0
        Uninstalling typing_extensions-4.5.0:
          Successfully uninstalled typing_extensions-4.5.0
    [31mERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
    llmx 0.0.15a0 requires cohere, which is not installed.
    tensorflow-probability 0.22.0 requires typing-extensions<4.6.0, but you have typing-extensions 4.9.0 which is incompatible.[0m[31m
    [0mSuccessfully installed dataclasses-json-0.6.3 docarray-0.40.0 h11-0.14.0 httpcore-1.0.2 httpx-0.26.0 jsonpatch-1.33 jsonpointer-2.4 langchain-0.1.2 langchain-community-0.0.14 langchain-core-0.1.14 langchain-openai-0.0.3 langchainhub-0.1.14 langsmith-0.0.83 marshmallow-3.20.2 mypy-extensions-1.0.0 openai-1.9.0 orjson-3.9.12 tiktoken-0.5.2 types-requests-2.31.0.20240106 typing-extensions-4.9.0 typing-inspect-0.9.0 xmltodict-0.13.0
    


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
from langchain.tools import PubmedQueryRun
from langchain.agents import load_tools
from langchain.agents import AgentType, create_react_agent, AgentExecutor
from langchain_openai import ChatOpenAI
```


```python
client = OpenAI(
    api_key=os.environ.get("OPENAI_API_KEY"),
)
```


```python
chat_openai = ChatOpenAI(temperature=0, model=model)
```


```python
tool = PubmedQueryRun()

tool.run("Febre Familiar do Mediterrâneo")
```




    'Published: 2017-03-23\nTitle: Clinical outcomes and survival in AA amyloidosis patients.\nCopyright Information: Copyright © 2017. Published by Elsevier Editora Ltda.\nSummary::\nAIM: Amyloid A amyloidosis is a rare complication of chronic inflammatory conditions. Most patients with amyloid A amyloidosis present with nephropathy and it leads to renal failure and death. We studied clinical characteristics and survival in patients with amyloid A amyloidosis.\nMETHODS: A total of 81 patients (51 males, 30 females) with renal biopsy proven amyloid A amyloidosis were analyzed retrospectively. The patients were divided into good and poor outcomes groups according to survival results.\nRESULTS: Most of the patients (55.6%) had nephrotic range proteinuria at diagnosis. Most frequent underlying disorders were familial Mediterranean fever (21.2%) and rheumatoid arthritis (10.6%) in the good outcome group and malignancy (20%) in the poor outcome group. Only diastolic blood pressure in the good outcome group and phosphorus level in the poor outcome group was higher. Serum creatinine levels increased after treatment in both groups, while proteinuria in the good outcome group decreased. Increase in serum creatinine and decrease in estimated glomerular filtration rate of the poor outcome group were more significant in the good outcome group. At the time of diagnosis 18.5% and 27.2% of all patients had advanced chronic kidney disease (stage 4 and 5, respectively). Median duration of renal survival was 65±3.54 months. Among all patients, 27.1% were started dialysis treatment during the follow-up period and 7.4% of all patients underwent kidney transplantation. Higher levels of systolic blood pressure [hazard ratios 1.03, 95% confidence interval: 1-1.06, p=0.036], serum creatinine (hazard ratios 1.25, 95% confidence interval: 1.07-1.46, p=0.006) and urinary protein excretion (hazard ratios 1.08, 95% confidence interval: 1.01-1.16, p=0.027) were predictors of end-stage renal disease. Median'




```python
tools = load_tools(
    ["pubmed"]
)
```


```python
from langchain import hub

prompt = hub.pull("hwchase17/react")
```


```python
agent = create_react_agent(
    chat_openai,
    [PubmedQueryRun()],
    prompt)
```


```python
agent_executor = AgentExecutor(
    agent=agent, tools=tools, verbose=True, handle_parsing_errors=True
)
```


```python
agent_executor.invoke({"input": "Qual o tratamento mais comum para a Febre Familiar do Mediterrâneo."})
```

    
    
    [1m> Entering new AgentExecutor chain...[0m
    [32;1m[1;3mI need to find information on the most common treatment for Familial Mediterranean Fever.
    Action: PubMed
    Action Input: "Familial Mediterranean Fever treatment"[0m[36;1m[1;3mPublished: 2024-01-09
    Title: Inhibition of CYP1A1 Alleviates Colchicine-Induced Hepatotoxicity.
    Copyright Information: 
    Summary::
    Colchicine, a natural compound extracted from Colchicum autumnale, is a phytotoxin, but interestingly, it also has multiple pharmacological activities. Clinically, colchicine is widely used for the treatment of gouty arthritis, familial Mediterranean fever, cardiovascular dysfunction and new coronary pneumonia. However, overdose intake of colchicine could cause lethal liver damage, which is a limitation of its application. Therefore, exploring the potential mechanism of colchicine-induced hepatotoxicity is meaningful. Interestingly, it was found that CYP1A1 played an important role in the hepatotoxicity of colchicine, while it might also participate in its metabolism. Inhibition of CYP1A1 could alleviate oxidative stress and pyroptosis in the liver upon colchicine treatment. By regulating CYP1A1 through the CASPASE-1-GSDMD pathway, colchicine-induced liver injury was effectively relieved in a mouse model. In summary, we concluded that CYP1A1 may be a potential target, and the inhibition of CYP1A1 alleviates colchicine-induced liver injury through pyroptosis regulated by the CASPASE-1-GSDMD pathway.
    
    Published: 2024-01-18
    Title: Colchicine-intolerant familial mediterranean fever patients: A comparative study between different colchicine doses and IL-1 inhibitor monotherapy.
    Copyright Information: Copyright © 2024 Elsevier B.V. All rights reserved.
    Summary::
    OBJECTIVE: Familial Mediterranean fever (FMF) is the most common monogenic autoinflammatory disease. Colchicine is the primary treatment for FMF, although some patients do not respond well or are unable to tolerate it. For these patients, the addition of interleukin-1 (IL-1) antagonists is the preferred option. However, the impact of colchicine treatment alongside the use of IL-1 antagonists remains unclear.
    METHODS: We recruited adult FMF patients who satisfied the Eurofever and Pediat[0m[32;1m[1;3mBased on the information from PubMed, the most common treatment for Familial Mediterranean Fever is colchicine. However, some patients may not respond well or tolerate colchicine, in which case the addition of interleukin-1 (IL-1) antagonists may be considered. 
    
    Final Answer: The most common treatment for Familial Mediterranean Fever is colchicine, with the option of adding IL-1 antagonists for patients who do not respond well or tolerate colchicine.[0m
    
    [1m> Finished chain.[0m
    




    {'input': 'Qual o tratamento mais comum para a Febre Familiar do Mediterrâneo.',
     'output': 'The most common treatment for Familial Mediterranean Fever is colchicine, with the option of adding IL-1 antagonists for patients who do not respond well or tolerate colchicine.'}




```python

```
