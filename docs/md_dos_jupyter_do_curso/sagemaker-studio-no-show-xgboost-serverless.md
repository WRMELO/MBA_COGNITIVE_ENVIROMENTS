# Treinamento e Implantação de Modelo no Sagemaker Studio

Notebook dedicado a utilizar um exemplo de treinamento de modelo e implantação do mesmo via inferência baseado em serverless.
O Sagemaker permite apenas este tipo de inferência utilizando este método.
O Canvas, recém lançado por exemplo, somente permite a implantação de modelos via instâncias dedicadas de inferência.

## Instalação das bibliotecas necessárias

Vamos utilizar o AWS SDK e ferramentas do Sagemaker.


```python
! pip install sagemaker botocore boto3 awscli --upgrade
```

    Requirement already satisfied: sagemaker in /opt/conda/lib/python3.10/site-packages (2.219.0)
    Collecting sagemaker
      Using cached sagemaker-2.221.1-py3-none-any.whl.metadata (14 kB)
    Requirement already satisfied: botocore in /opt/conda/lib/python3.10/site-packages (1.34.51)
    Collecting botocore
      Using cached botocore-1.34.117-py3-none-any.whl.metadata (5.7 kB)
    Requirement already satisfied: boto3 in /opt/conda/lib/python3.10/site-packages (1.34.51)
    Collecting boto3
      Using cached boto3-1.34.117-py3-none-any.whl.metadata (6.6 kB)
    Collecting awscli
      Using cached awscli-1.32.117-py3-none-any.whl.metadata (11 kB)
    Requirement already satisfied: attrs<24,>=23.1.0 in /opt/conda/lib/python3.10/site-packages (from sagemaker) (23.2.0)
    Requirement already satisfied: cloudpickle==2.2.1 in /opt/conda/lib/python3.10/site-packages (from sagemaker) (2.2.1)
    Requirement already satisfied: google-pasta in /opt/conda/lib/python3.10/site-packages (from sagemaker) (0.2.0)
    Requirement already satisfied: numpy<2.0,>=1.9.0 in /opt/conda/lib/python3.10/site-packages (from sagemaker) (1.26.4)
    Requirement already satisfied: protobuf<5.0,>=3.12 in /opt/conda/lib/python3.10/site-packages (from sagemaker) (4.24.4)
    Requirement already satisfied: smdebug-rulesconfig==1.0.1 in /opt/conda/lib/python3.10/site-packages (from sagemaker) (1.0.1)
    Requirement already satisfied: importlib-metadata<7.0,>=1.4.0 in /opt/conda/lib/python3.10/site-packages (from sagemaker) (6.10.0)
    Requirement already satisfied: packaging>=20.0 in /opt/conda/lib/python3.10/site-packages (from sagemaker) (23.2)
    Requirement already satisfied: pandas in /opt/conda/lib/python3.10/site-packages (from sagemaker) (2.1.4)
    Requirement already satisfied: pathos in /opt/conda/lib/python3.10/site-packages (from sagemaker) (0.3.2)
    Requirement already satisfied: schema in /opt/conda/lib/python3.10/site-packages (from sagemaker) (0.7.7)
    Requirement already satisfied: PyYAML~=6.0 in /opt/conda/lib/python3.10/site-packages (from sagemaker) (6.0.1)
    Requirement already satisfied: jsonschema in /opt/conda/lib/python3.10/site-packages (from sagemaker) (4.17.3)
    Requirement already satisfied: platformdirs in /opt/conda/lib/python3.10/site-packages (from sagemaker) (4.2.1)
    Requirement already satisfied: tblib<4,>=1.7.0 in /opt/conda/lib/python3.10/site-packages (from sagemaker) (2.0.0)
    Requirement already satisfied: urllib3<3.0.0,>=1.26.8 in /opt/conda/lib/python3.10/site-packages (from sagemaker) (1.26.18)
    Requirement already satisfied: requests in /opt/conda/lib/python3.10/site-packages (from sagemaker) (2.31.0)
    Requirement already satisfied: docker in /opt/conda/lib/python3.10/site-packages (from sagemaker) (7.0.0)
    Requirement already satisfied: tqdm in /opt/conda/lib/python3.10/site-packages (from sagemaker) (4.66.4)
    Requirement already satisfied: psutil in /opt/conda/lib/python3.10/site-packages (from sagemaker) (5.9.8)
    Requirement already satisfied: jmespath<2.0.0,>=0.7.1 in /opt/conda/lib/python3.10/site-packages (from botocore) (1.0.1)
    Requirement already satisfied: python-dateutil<3.0.0,>=2.1 in /opt/conda/lib/python3.10/site-packages (from botocore) (2.9.0)
    Requirement already satisfied: s3transfer<0.11.0,>=0.10.0 in /opt/conda/lib/python3.10/site-packages (from boto3) (0.10.1)
    Collecting docutils<0.17,>=0.10 (from awscli)
      Using cached docutils-0.16-py2.py3-none-any.whl.metadata (2.7 kB)
    Requirement already satisfied: colorama<0.4.7,>=0.2.5 in /opt/conda/lib/python3.10/site-packages (from awscli) (0.4.6)
    Collecting rsa<4.8,>=3.1.2 (from awscli)
      Using cached rsa-4.7.2-py3-none-any.whl.metadata (3.6 kB)
    Requirement already satisfied: zipp>=0.5 in /opt/conda/lib/python3.10/site-packages (from importlib-metadata<7.0,>=1.4.0->sagemaker) (3.17.0)
    Requirement already satisfied: six>=1.5 in /opt/conda/lib/python3.10/site-packages (from python-dateutil<3.0.0,>=2.1->botocore) (1.16.0)
    Requirement already satisfied: pyasn1>=0.1.3 in /opt/conda/lib/python3.10/site-packages (from rsa<4.8,>=3.1.2->awscli) (0.6.0)
    Requirement already satisfied: charset-normalizer<4,>=2 in /opt/conda/lib/python3.10/site-packages (from requests->sagemaker) (3.3.2)
    Requirement already satisfied: idna<4,>=2.5 in /opt/conda/lib/python3.10/site-packages (from requests->sagemaker) (3.7)
    Requirement already satisfied: certifi>=2017.4.17 in /opt/conda/lib/python3.10/site-packages (from requests->sagemaker) (2024.2.2)
    Requirement already satisfied: pyrsistent!=0.17.0,!=0.17.1,!=0.17.2,>=0.14.0 in /opt/conda/lib/python3.10/site-packages (from jsonschema->sagemaker) (0.20.0)
    Requirement already satisfied: pytz>=2020.1 in /opt/conda/lib/python3.10/site-packages (from pandas->sagemaker) (2023.3)
    Requirement already satisfied: tzdata>=2022.1 in /opt/conda/lib/python3.10/site-packages (from pandas->sagemaker) (2024.1)
    Requirement already satisfied: ppft>=1.7.6.8 in /opt/conda/lib/python3.10/site-packages (from pathos->sagemaker) (1.7.6.8)
    Requirement already satisfied: dill>=0.3.8 in /opt/conda/lib/python3.10/site-packages (from pathos->sagemaker) (0.3.8)
    Requirement already satisfied: pox>=0.3.4 in /opt/conda/lib/python3.10/site-packages (from pathos->sagemaker) (0.3.4)
    Requirement already satisfied: multiprocess>=0.70.16 in /opt/conda/lib/python3.10/site-packages (from pathos->sagemaker) (0.70.16)
    Using cached sagemaker-2.221.1-py3-none-any.whl (1.5 MB)
    Using cached botocore-1.34.117-py3-none-any.whl (12.3 MB)
    Using cached boto3-1.34.117-py3-none-any.whl (139 kB)
    Using cached awscli-1.32.117-py3-none-any.whl (4.5 MB)
    Using cached docutils-0.16-py2.py3-none-any.whl (548 kB)
    Using cached rsa-4.7.2-py3-none-any.whl (34 kB)
    Installing collected packages: rsa, docutils, botocore, boto3, awscli, sagemaker
      Attempting uninstall: rsa
        Found existing installation: rsa 4.9
        Uninstalling rsa-4.9:
          Successfully uninstalled rsa-4.9
      Attempting uninstall: botocore
        Found existing installation: botocore 1.34.51
        Uninstalling botocore-1.34.51:
          Successfully uninstalled botocore-1.34.51
      Attempting uninstall: boto3
        Found existing installation: boto3 1.34.51
        Uninstalling boto3-1.34.51:
          Successfully uninstalled boto3-1.34.51
      Attempting uninstall: sagemaker
        Found existing installation: sagemaker 2.219.0
        Uninstalling sagemaker-2.219.0:
          Successfully uninstalled sagemaker-2.219.0
    [31mERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
    aiobotocore 2.12.2 requires botocore<1.34.52,>=1.34.41, but you have botocore 1.34.117 which is incompatible.
    autogluon-common 0.8.3 requires pandas<1.6,>=1.4.1, but you have pandas 2.1.4 which is incompatible.
    autogluon-core 0.8.3 requires pandas<1.6,>=1.4.1, but you have pandas 2.1.4 which is incompatible.
    autogluon-core 0.8.3 requires scikit-learn<1.4.1,>=1.1, but you have scikit-learn 1.4.2 which is incompatible.
    autogluon-features 0.8.3 requires pandas<1.6,>=1.4.1, but you have pandas 2.1.4 which is incompatible.
    autogluon-features 0.8.3 requires scikit-learn<1.4.1,>=1.1, but you have scikit-learn 1.4.2 which is incompatible.
    autogluon-multimodal 0.8.3 requires pandas<1.6,>=1.4.1, but you have pandas 2.1.4 which is incompatible.
    autogluon-multimodal 0.8.3 requires pytorch-lightning<1.10.0,>=1.9.0, but you have pytorch-lightning 2.0.9 which is incompatible.
    autogluon-multimodal 0.8.3 requires scikit-learn<1.4.1,>=1.1, but you have scikit-learn 1.4.2 which is incompatible.
    autogluon-multimodal 0.8.3 requires torch<1.14,>=1.9, but you have torch 2.0.0.post104 which is incompatible.
    autogluon-multimodal 0.8.3 requires torchmetrics<0.12.0,>=0.11.0, but you have torchmetrics 1.0.3 which is incompatible.
    autogluon-multimodal 0.8.3 requires torchvision<0.15.0, but you have torchvision 0.15.2a0+ab7b3e6 which is incompatible.
    autogluon-tabular 0.8.3 requires pandas<1.6,>=1.4.1, but you have pandas 2.1.4 which is incompatible.
    autogluon-tabular 0.8.3 requires scikit-learn<1.4.1,>=1.1, but you have scikit-learn 1.4.2 which is incompatible.
    autogluon-timeseries 0.8.3 requires pandas<1.6,>=1.4.1, but you have pandas 2.1.4 which is incompatible.
    autogluon-timeseries 0.8.3 requires pytorch-lightning<1.10.0,>=1.7.4, but you have pytorch-lightning 2.0.9 which is incompatible.
    autogluon-timeseries 0.8.3 requires torch<1.14,>=1.9, but you have torch 2.0.0.post104 which is incompatible.[0m[31m
    [0mSuccessfully installed awscli-1.32.117 boto3-1.34.117 botocore-1.34.117 docutils-0.16 rsa-4.7.2 sagemaker-2.221.1
    

Importação das bibliotecas e criando os clientes do Sagemaker para o treinamento e outro para inferência.


```python
import boto3
import sagemaker
from sagemaker.estimator import Estimator
import pandas as pd
from sklearn.preprocessing import LabelEncoder
from sagemaker.inputs import TrainingInput
from time import gmtime, strftime
```


```python
client = boto3.client(service_name="sagemaker")
runtime = boto3.client(service_name="sagemaker-runtime")
```

Configurando prefixos e definindo instância de treinamento.
Obtendo a região padrão de onde o domínio do Sagemaker foi criado.


```python
boto_session = boto3.session.Session()
region = boto_session.region_name
print(region)

sagemaker_session = sagemaker.Session()
base_job_prefix = "xgboost-no-show"
role = sagemaker.get_execution_role()
print(role)

default_bucket = sagemaker_session.default_bucket()
s3_prefix = base_job_prefix

training_instance_type = "ml.m5.large"
```

    us-east-1
    arn:aws:iam::989944764342:role/service-role/AmazonSageMaker-ExecutionRole-20240601T211333
    

## Obtendo dataset

Este dataset é composto uma lista de consultas médicas das quais foram confirmadas a presentaça ou não. Detalhes como bairro, envio de SMS prévio, dia da marcação e dia da consulta, por exemplo, fornecem indícios sobre como podemos predizer a presença das pessoas.


```python
!git clone https://github.com/michelpf/fiap-ds-cloud-cognitive-environments
```


```python
df = pd.read_csv("fiap-ds-cloud-cognitive-environments/aula-1-introducao/dataset/brazil_noshowappointments_cleaned_may2016.csv")
```

## Feature engineering

Transformando e preparando os dados para amplificar o poder de predição.
A estratégia é tornar os dados mais generalistas, e com isso poder convergir melhor. 

* Conversão datas com horas e minutos para dia da semana, dia, hora e quarter.
* Remoção de atributos identificadores
* Definição de intervalos para ser aplicado em idade.


```python
df.drop(columns=['patientid'], inplace=True)

df['scheduledday'] = pd.to_datetime(df['scheduledday'])
df['scheduled_month'] = df['scheduledday'].dt.month
df['scheduled_day'] = df['scheduledday'].dt.day
df['scheduled_hour'] = df['scheduledday'].dt.hour
df['scheduled_day_of_week'] = df['scheduledday'].dt.weekday  # 0=Monday, 6=Sunday
df['scheduled_quarter'] = df['scheduledday'].dt.quarter
df['scheduled_week_of_year'] = df['scheduledday'].dt.isocalendar().week

df['appointmentday'] = pd.to_datetime(df['appointmentday'])
df['appointment_month'] = df['appointmentday'].dt.month
df['appointment_day'] = df['appointmentday'].dt.day
df['appointment_hour'] = df['appointmentday'].dt.hour
df['appointment_day_of_week'] = df['appointmentday'].dt.weekday  # 0=Monday, 6=Sunday
df['appointment_quarter'] = df['appointmentday'].dt.quarter
df['appointment_week_of_year'] = df['appointmentday'].dt.isocalendar().week

df.drop(columns=['appointmentday'], inplace=True)
df.drop(columns=['scheduledday'], inplace=True)
```


```python
# Definir os intervalos de idade
bins = [0, 12, 17, 35, 50, 65, float('inf')]

# Definir os rótulos correspondentes a cada faixa etária
labels = ['Criança', 'Adolescente', 'Jovem Adulto', 'Adulto', 'Meia-idade', 'Idoso']

# Usar pd.cut para criar uma nova coluna com as faixas etárias
df['age_type'] = pd.cut(df['age'], bins=bins, labels=labels, right=False)

df.drop(columns=['age'], inplace=True)
```

Utilização de codificação de valor string para número, por meio da utilização de um label encoder.


```python
label_encoder = LabelEncoder()

df['gender'] = label_encoder.fit_transform(df['gender'])
df['neighbourhood'] = label_encoder.fit_transform(df['neighbourhood'])
df['scholarship'] = label_encoder.fit_transform(df['scholarship'])
df['sms_received'] = label_encoder.fit_transform(df['sms_received'])
df['appointment'] = label_encoder.fit_transform(df['appointment'])
df['ailment'] = label_encoder.fit_transform(df['ailment'])
df['age_type'] = label_encoder.fit_transform(df['age_type'])

```


```python
df
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
      <th>gender</th>
      <th>neighbourhood</th>
      <th>scholarship</th>
      <th>sms_received</th>
      <th>appointment</th>
      <th>ailment</th>
      <th>scheduled_month</th>
      <th>scheduled_day</th>
      <th>scheduled_hour</th>
      <th>scheduled_day_of_week</th>
      <th>scheduled_quarter</th>
      <th>scheduled_week_of_year</th>
      <th>appointment_month</th>
      <th>appointment_day</th>
      <th>appointment_hour</th>
      <th>appointment_day_of_week</th>
      <th>appointment_quarter</th>
      <th>appointment_week_of_year</th>
      <th>age_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>39</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>7</td>
      <td>4</td>
      <td>29</td>
      <td>18</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>4</td>
      <td>29</td>
      <td>0</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>39</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>15</td>
      <td>4</td>
      <td>29</td>
      <td>16</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>4</td>
      <td>29</td>
      <td>0</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>45</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>15</td>
      <td>4</td>
      <td>29</td>
      <td>16</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>4</td>
      <td>29</td>
      <td>0</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>54</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>15</td>
      <td>4</td>
      <td>29</td>
      <td>17</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>4</td>
      <td>29</td>
      <td>0</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>39</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>10</td>
      <td>4</td>
      <td>29</td>
      <td>16</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>4</td>
      <td>29</td>
      <td>0</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>5</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>109904</th>
      <td>0</td>
      <td>43</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>15</td>
      <td>5</td>
      <td>3</td>
      <td>9</td>
      <td>1</td>
      <td>2</td>
      <td>18</td>
      <td>6</td>
      <td>7</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>23</td>
      <td>5</td>
    </tr>
    <tr>
      <th>109905</th>
      <td>0</td>
      <td>43</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>15</td>
      <td>5</td>
      <td>3</td>
      <td>7</td>
      <td>1</td>
      <td>2</td>
      <td>18</td>
      <td>6</td>
      <td>7</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>23</td>
      <td>5</td>
    </tr>
    <tr>
      <th>109906</th>
      <td>0</td>
      <td>43</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>15</td>
      <td>4</td>
      <td>27</td>
      <td>16</td>
      <td>2</td>
      <td>2</td>
      <td>17</td>
      <td>6</td>
      <td>7</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>23</td>
      <td>4</td>
    </tr>
    <tr>
      <th>109907</th>
      <td>0</td>
      <td>43</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>15</td>
      <td>4</td>
      <td>27</td>
      <td>15</td>
      <td>2</td>
      <td>2</td>
      <td>17</td>
      <td>6</td>
      <td>7</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>23</td>
      <td>1</td>
    </tr>
    <tr>
      <th>109908</th>
      <td>0</td>
      <td>43</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>15</td>
      <td>4</td>
      <td>27</td>
      <td>13</td>
      <td>2</td>
      <td>2</td>
      <td>17</td>
      <td>6</td>
      <td>7</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>23</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
<p>109909 rows × 19 columns</p>
</div>




```python
df = df.rename(columns={'appointment': 'Target'})
```


```python
other_columns = [col for col in df.columns if col != 'Target']

# Renomear as outras colunas para 'Feature_0', 'Feature_1', etc.
new_column_names = ['Feature_' + str(i) for i in range(len(other_columns))]
rename_dict = dict(zip(other_columns, new_column_names))

df = df.rename(columns=rename_dict)

# Reorganizar para que a coluna 'Target' seja a primeira
df = df[['Target'] + new_column_names]

```


```python
df
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
      <th>Target</th>
      <th>Feature_0</th>
      <th>Feature_1</th>
      <th>Feature_2</th>
      <th>Feature_3</th>
      <th>Feature_4</th>
      <th>Feature_5</th>
      <th>Feature_6</th>
      <th>Feature_7</th>
      <th>Feature_8</th>
      <th>Feature_9</th>
      <th>Feature_10</th>
      <th>Feature_11</th>
      <th>Feature_12</th>
      <th>Feature_13</th>
      <th>Feature_14</th>
      <th>Feature_15</th>
      <th>Feature_16</th>
      <th>Feature_17</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>39</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
      <td>4</td>
      <td>29</td>
      <td>18</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>4</td>
      <td>29</td>
      <td>0</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>39</td>
      <td>0</td>
      <td>0</td>
      <td>15</td>
      <td>4</td>
      <td>29</td>
      <td>16</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>4</td>
      <td>29</td>
      <td>0</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>0</td>
      <td>45</td>
      <td>0</td>
      <td>0</td>
      <td>15</td>
      <td>4</td>
      <td>29</td>
      <td>16</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>4</td>
      <td>29</td>
      <td>0</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>0</td>
      <td>54</td>
      <td>0</td>
      <td>0</td>
      <td>15</td>
      <td>4</td>
      <td>29</td>
      <td>17</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>4</td>
      <td>29</td>
      <td>0</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>0</td>
      <td>39</td>
      <td>0</td>
      <td>0</td>
      <td>10</td>
      <td>4</td>
      <td>29</td>
      <td>16</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>4</td>
      <td>29</td>
      <td>0</td>
      <td>4</td>
      <td>2</td>
      <td>17</td>
      <td>5</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>109904</th>
      <td>1</td>
      <td>0</td>
      <td>43</td>
      <td>0</td>
      <td>1</td>
      <td>15</td>
      <td>5</td>
      <td>3</td>
      <td>9</td>
      <td>1</td>
      <td>2</td>
      <td>18</td>
      <td>6</td>
      <td>7</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>23</td>
      <td>5</td>
    </tr>
    <tr>
      <th>109905</th>
      <td>1</td>
      <td>0</td>
      <td>43</td>
      <td>0</td>
      <td>1</td>
      <td>15</td>
      <td>5</td>
      <td>3</td>
      <td>7</td>
      <td>1</td>
      <td>2</td>
      <td>18</td>
      <td>6</td>
      <td>7</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>23</td>
      <td>5</td>
    </tr>
    <tr>
      <th>109906</th>
      <td>1</td>
      <td>0</td>
      <td>43</td>
      <td>0</td>
      <td>1</td>
      <td>15</td>
      <td>4</td>
      <td>27</td>
      <td>16</td>
      <td>2</td>
      <td>2</td>
      <td>17</td>
      <td>6</td>
      <td>7</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>23</td>
      <td>4</td>
    </tr>
    <tr>
      <th>109907</th>
      <td>1</td>
      <td>0</td>
      <td>43</td>
      <td>0</td>
      <td>1</td>
      <td>15</td>
      <td>4</td>
      <td>27</td>
      <td>15</td>
      <td>2</td>
      <td>2</td>
      <td>17</td>
      <td>6</td>
      <td>7</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>23</td>
      <td>1</td>
    </tr>
    <tr>
      <th>109908</th>
      <td>1</td>
      <td>0</td>
      <td>43</td>
      <td>0</td>
      <td>1</td>
      <td>15</td>
      <td>4</td>
      <td>27</td>
      <td>13</td>
      <td>2</td>
      <td>2</td>
      <td>17</td>
      <td>6</td>
      <td>7</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>23</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
<p>109909 rows × 19 columns</p>
</div>




```python
df['Target'].unique()
```




    array([1, 0])



## Separação de dados

Separando dados de treinamento e teste.


```python
train_data = df.sample(frac=0.8, random_state=200)
test_data = df.drop(train_data.index)
```

Exportando para o formato que o Sagemaker necessita para o treinamento.


```python
# Exportando o DataFrame para um arquivo CSV
train_data.to_csv('train.csv', index=False)
```

Enviando o arquivo de treinamento para o S3. Local de onde os modelos a serem treinados podem acessá-lo.


```python
# upload data to S3
!aws s3 cp train.csv s3://{default_bucket}/xgboost-classification/train.csv
```

    upload: ./train.csv to s3://sagemaker-us-east-1-989944764342/xgboost-classification/train.csv
    

## Treinamento do modelo

Temos diversos modelos disponíveis para utilizar no treinamento, desde os internos da Amazon quanto externos.
O Amazon JumpStart oferece uma forma amigável para acessar diferentes modelos para diferentes propósitos. Ainda é possível obter conteiners de diferentes outros modelos disponíveis no Sagemaker.
O Sagemaker possui alguns modelos com biblioteca dedicada, sendo este outra forma de acessar os modelos padrão.

Para mais detalhes sobre os modelos disponíveis, acesse esta [referência](https://docs.aws.amazon.com/sagemaker/latest/dg/train-model.html).

Neste projeto, vamos utilizar o XGBoost como modelo a ser treinado.


```python
training_path = f"s3://{default_bucket}/xgboost-classification/train.csv"
train_input = TrainingInput(training_path, content_type="text/csv")
model_path = f"s3://{default_bucket}/{s3_prefix}/xgb_model"
```

Configuramos o XGBoost para classificação logística binária. Ou seja, será responsável por prever entre 0 e 1, sendo que o valor de corte, comumente utilizado como 0.5 pode ser ajustado conforme a necessidade do modelo.
Por exemplo, para predições mais precisas de "presença" podemos optar o corte acima de 0.7. E assim por diante.


```python
# Obter a imagem do XGBoost
image_uri = sagemaker.image_uris.retrieve(
    framework="xgboost",
    region=region,
    version="1.0-1",
    py_version="py3",
    instance_type=training_instance_type,
)

# Configurar Estimador
xgb_train = Estimator(
    image_uri=image_uri,
    instance_type=training_instance_type,
    instance_count=1,
    output_path=model_path,
    sagemaker_session=sagemaker_session,
    role=role,
)

# Configurar Hiperparâmetros
xgb_train.set_hyperparameters(
    objective="binary:logistic",
    num_round=50,
    max_depth=5,
    eta=0.2,
    gamma=4,
    min_child_weight=6,
    subsample=0.7,
    silent=0,
)
```


```python
# Fit model
xgb_train.fit({"train": train_input})
```

    INFO:sagemaker:Creating training-job with name: sagemaker-xgboost-2024-06-02-19-05-24-202
    

    2024-06-02 19:05:24 Starting - Starting the training job...
    2024-06-02 19:05:42 Starting - Preparing the instances for training...
    2024-06-02 19:06:09 Downloading - Downloading input data...
    2024-06-02 19:06:49 Downloading - Downloading the training image......
    2024-06-02 19:07:50 Training - Training image download completed. Training in progress.
    2024-06-02 19:07:50 Uploading - Uploading generated training model[34m[2024-06-02 19:07:39.781 ip-10-0-85-128.ec2.internal:7 INFO utils.py:27] RULE_JOB_STOP_SIGNAL_FILENAME: None[0m
    [34mINFO:sagemaker-containers:Imported framework sagemaker_xgboost_container.training[0m
    [34mINFO:sagemaker-containers:Failed to parse hyperparameter objective value binary:logistic to Json.[0m
    [34mReturning the value itself[0m
    [34mINFO:sagemaker-containers:No GPUs detected (normal if no gpus installed)[0m
    [34mINFO:sagemaker_xgboost_container.training:Running XGBoost Sagemaker in algorithm mode[0m
    [34mINFO:root:Determined delimiter of CSV input is ','[0m
    [34mINFO:root:Determined delimiter of CSV input is ','[0m
    [34mINFO:root:Single node training.[0m
    [34m[19:07:39] 87928x18 matrix with 1582704 entries loaded from /opt/ml/input/data/train?format=csv&label_column=0&delimiter=,[0m
    [34m[2024-06-02 19:07:39.939 ip-10-0-85-128.ec2.internal:7 INFO json_config.py:91] Creating hook from json_config at /opt/ml/input/config/debughookconfig.json.[0m
    [34m[2024-06-02 19:07:39.940 ip-10-0-85-128.ec2.internal:7 INFO hook.py:201] tensorboard_dir has not been set for the hook. SMDebug will not be exporting tensorboard summaries.[0m
    [34m[2024-06-02 19:07:39.940 ip-10-0-85-128.ec2.internal:7 INFO profiler_config_parser.py:102] User has disabled profiler.[0m
    [34m[2024-06-02 19:07:39.941 ip-10-0-85-128.ec2.internal:7 INFO hook.py:255] Saving to /opt/ml/output/tensors[0m
    [34m[2024-06-02 19:07:39.941 ip-10-0-85-128.ec2.internal:7 INFO state_store.py:77] The checkpoint config file /opt/ml/input/config/checkpointconfig.json does not exist.[0m
    [34mINFO:root:Debug hook created from config[0m
    [34mINFO:root:Train matrix has 87928 rows[0m
    [34m[19:07:39] WARNING: /workspace/src/learner.cc:328: [0m
    [34mParameters: { num_round, silent } might not be used.
      This may not be accurate due to some parameters are only used in language bindings but
      passed down to XGBoost core.  Or some parameters are not used but slip through this
      verification. Please open an issue if you find above cases.[0m
    [34m[0]#011train-error:0.20102[0m
    [34m[2024-06-02 19:07:40.118 ip-10-0-85-128.ec2.internal:7 INFO hook.py:423] Monitoring the collections: metrics[0m
    [34m[2024-06-02 19:07:40.120 ip-10-0-85-128.ec2.internal:7 INFO hook.py:486] Hook is writing from the hook with pid: 7[0m
    [34m[1]#011train-error:0.20086[0m
    [34m[2]#011train-error:0.20097[0m
    [34m[3]#011train-error:0.20097[0m
    [34m[4]#011train-error:0.20097[0m
    [34m[5]#011train-error:0.20095[0m
    [34m[6]#011train-error:0.20088[0m
    [34m[7]#011train-error:0.20091[0m
    [34m[8]#011train-error:0.20087[0m
    [34m[9]#011train-error:0.20080[0m
    [34m[10]#011train-error:0.20080[0m
    [34m[11]#011train-error:0.20071[0m
    [34m[12]#011train-error:0.20077[0m
    [34m[13]#011train-error:0.20068[0m
    [34m[14]#011train-error:0.20070[0m
    [34m[15]#011train-error:0.20068[0m
    [34m[16]#011train-error:0.20060[0m
    [34m[17]#011train-error:0.20050[0m
    [34m[18]#011train-error:0.20039[0m
    [34m[19]#011train-error:0.20044[0m
    [34m[20]#011train-error:0.20029[0m
    [34m[21]#011train-error:0.20007[0m
    [34m[22]#011train-error:0.19997[0m
    [34m[23]#011train-error:0.19980[0m
    [34m[24]#011train-error:0.19971[0m
    [34m[25]#011train-error:0.19948[0m
    [34m[26]#011train-error:0.19948[0m
    [34m[27]#011train-error:0.19956[0m
    [34m[28]#011train-error:0.19941[0m
    [34m[29]#011train-error:0.19945[0m
    [34m[30]#011train-error:0.19916[0m
    [34m[31]#011train-error:0.19923[0m
    [34m[32]#011train-error:0.19895[0m
    [34m[33]#011train-error:0.19897[0m
    [34m[34]#011train-error:0.19869[0m
    [34m[35]#011train-error:0.19853[0m
    [34m[36]#011train-error:0.19870[0m
    [34m[37]#011train-error:0.19851[0m
    [34m[38]#011train-error:0.19849[0m
    [34m[39]#011train-error:0.19837[0m
    [34m[40]#011train-error:0.19808[0m
    [34m[41]#011train-error:0.19796[0m
    [34m[42]#011train-error:0.19793[0m
    [34m[43]#011train-error:0.19789[0m
    [34m[44]#011train-error:0.19788[0m
    [34m[45]#011train-error:0.19776[0m
    [34m[46]#011train-error:0.19771[0m
    [34m[47]#011train-error:0.19780[0m
    [34m[48]#011train-error:0.19767[0m
    [34m[49]#011train-error:0.19765[0m
    
    2024-06-02 19:08:03 Completed - Training job completed
    Training seconds: 114
    Billable seconds: 114
    

## Criando inferência serverless

Nesta etapa, após o modelo ter sido treinado, vamos criar um endpoint para inferências baseado em serverless, ou seja, não utiliza uma instância dedicada.


```python
# Retrieve model data from training job
model_artifacts = xgb_train.model_data
model_artifacts
```




    's3://sagemaker-us-east-1-989944764342/xgboost-no-show/xgb_model/sagemaker-xgboost-2024-06-02-19-05-24-202/output/model.tar.gz'




```python
model_name = "xgboost-serverless" + strftime("%Y-%m-%d-%H-%M-%S", gmtime())
print("Model name: " + model_name)

# dummy environment variables
byo_container_env_vars = {"SAGEMAKER_CONTAINER_LOG_LEVEL": "20", "SOME_ENV_VAR": "myEnvVar"}

create_model_response = client.create_model(
    ModelName=model_name,
    Containers=[
        {
            "Image": image_uri,
            "Mode": "SingleModel",
            "ModelDataUrl": model_artifacts,
            "Environment": byo_container_env_vars,
        }
    ],
    ExecutionRoleArn=role,
)

print("Model Arn: " + create_model_response["ModelArn"])
```

    Model name: xgboost-serverless2024-06-02-19-08-40
    Model Arn: arn:aws:sagemaker:us-east-1:989944764342:model/xgboost-serverless2024-06-02-19-08-40
    


```python
xgboost_epc_name = "xgboost-serverless-epc" + strftime("%Y-%m-%d-%H-%M-%S", gmtime())

endpoint_config_response = client.create_endpoint_config(
    EndpointConfigName=xgboost_epc_name,
    ProductionVariants=[
        {
            "VariantName": "byoVariant",
            "ModelName": model_name,
            "ServerlessConfig": {
                "MemorySizeInMB": 1024,
                "MaxConcurrency": 1,
            },
        },
    ],
)

print("Endpoint Configuration Arn: " + endpoint_config_response["EndpointConfigArn"])
```

    Endpoint Configuration Arn: arn:aws:sagemaker:us-east-1:989944764342:endpoint-config/xgboost-serverless-epc2024-06-02-19-08-43
    


```python
endpoint_name = "xgboost-serverless-noshow" + strftime("%Y-%m-%d-%H-%M-%S", gmtime())

create_endpoint_response = client.create_endpoint(
    EndpointName=endpoint_name,
    EndpointConfigName=xgboost_epc_name,
)

print("Endpoint Arn: " + create_endpoint_response["EndpointArn"])
```

    Endpoint Arn: arn:aws:sagemaker:us-east-1:989944764342:endpoint/xgboost-serverless-noshow2024-06-02-19-09-03
    


```python
# wait for endpoint to reach a terminal state (InService) using describe endpoint
import time

describe_endpoint_response = client.describe_endpoint(EndpointName=endpoint_name)

while describe_endpoint_response["EndpointStatus"] == "Creating":
    describe_endpoint_response = client.describe_endpoint(EndpointName=endpoint_name)
    print(describe_endpoint_response["EndpointStatus"])
    time.sleep(15)

describe_endpoint_response
```

    Creating
    Creating
    Creating
    Creating
    Creating
    Creating
    Creating
    Creating
    Creating
    Creating
    Creating
    InService
    




    {'EndpointName': 'xgboost-serverless-noshow2024-06-02-19-09-03',
     'EndpointArn': 'arn:aws:sagemaker:us-east-1:989944764342:endpoint/xgboost-serverless-noshow2024-06-02-19-09-03',
     'EndpointConfigName': 'xgboost-serverless-epc2024-06-02-19-08-43',
     'ProductionVariants': [{'VariantName': 'byoVariant',
       'DeployedImages': [{'SpecifiedImage': '683313688378.dkr.ecr.us-east-1.amazonaws.com/sagemaker-xgboost:1.0-1-cpu-py3',
         'ResolvedImage': '683313688378.dkr.ecr.us-east-1.amazonaws.com/sagemaker-xgboost@sha256:b517ba0196d2579c5393750335ed104da74b11ad2a6f0ae5da6907a7221ff2e0',
         'ResolutionTime': datetime.datetime(2024, 6, 2, 19, 9, 4, 361000, tzinfo=tzlocal())}],
       'CurrentWeight': 1.0,
       'DesiredWeight': 1.0,
       'CurrentInstanceCount': 0,
       'CurrentServerlessConfig': {'MemorySizeInMB': 1024, 'MaxConcurrency': 1}}],
     'EndpointStatus': 'InService',
     'CreationTime': datetime.datetime(2024, 6, 2, 19, 9, 3, 603000, tzinfo=tzlocal()),
     'LastModifiedTime': datetime.datetime(2024, 6, 2, 19, 11, 48, 944000, tzinfo=tzlocal()),
     'ResponseMetadata': {'RequestId': 'c458aea2-893f-41b1-9003-56417ead324a',
      'HTTPStatusCode': 200,
      'HTTPHeaders': {'x-amzn-requestid': 'c458aea2-893f-41b1-9003-56417ead324a',
       'content-type': 'application/x-amz-json-1.1',
       'content-length': '826',
       'date': 'Sun, 02 Jun 2024 19:11:49 GMT'},
      'RetryAttempts': 0}}




```python
linha_0 = test_data.iloc[0]
linha_0
```




    Target         1
    Feature_0      0
    Feature_1     54
    Feature_2      0
    Feature_3      0
    Feature_4     15
    Feature_5      4
    Feature_6     29
    Feature_7     17
    Feature_8      4
    Feature_9      2
    Feature_10    17
    Feature_11     4
    Feature_12    29
    Feature_13     0
    Feature_14     4
    Feature_15     2
    Feature_16    17
    Feature_17     2
    Name: 3, dtype: Int64




```python
linha_bytes = ','.join(map(str, df.iloc[0, 1:].values)).encode()
linha_bytes
```




    b'0,39,0,0,7,4,29,18,4,2,17,4,29,0,4,2,17,5'




```python
response = runtime.invoke_endpoint(
    EndpointName=endpoint_name,
    Body=linha_bytes,
    ContentType="text/csv",
)

print(response["Body"].read())
```

    b'0.9499961733818054'
    


```python
label_test = test_data['Target'].tolist()
prediction_test = []
```


```python
len(test_data)
```




    21982




```python
indice_divisao = len(test_data) // 2

# Dividindo o DataFrame em duas partes
df_parte1 = df.iloc[:indice_divisao]
df_parte2 = df.iloc[indice_divisao:]
```


```python
import io
from io import StringIO
csv_file = io.StringIO()
# by default sagemaker expects comma separated
df_sem_primeira_coluna = test_data.iloc[:, 1:]

df_sem_primeira_coluna.to_csv(csv_file, sep=",", header=False, index=False)
my_payload_as_csv = csv_file.getvalue()
```


```python
response = runtime.invoke_endpoint(
    EndpointName=endpoint_name,
    Body=my_payload_as_csv,
    ContentType="text/csv")
```


```python
response = response["Body"].read().decode('utf-8')
```


```python
predictions = response.split(",")
```


```python
prediction_test = []
for item in predictions:
    prediction_float = float(item)
    #prediction_rounded = round(prediction_float)
    if prediction_float > 0.5:
        prediction_rounded = 1
    else:
        prediction_rounded = 0
    prediction_test.append(prediction_rounded)
```


```python
len(label_test), len(prediction_test)
```




    (21982, 21982)




```python
from sklearn.metrics import accuracy_score, precision_score, f1_score
from math import sqrt

accuracy = accuracy_score(label_test, prediction_test)
precision = precision_score(label_test, prediction_test)
f1 = f1_score(label_test, prediction_test)

print("Accuracy: {0}\nPrecision: {1}\nF1: {2}".format(accuracy, precision, f1))
```

    Accuracy: 0.7996997543444636
    Precision: 0.801994301994302
    F1: 0.8879900277290188
    


```python
client.delete_model(ModelName=model_name)
client.delete_endpoint_config(EndpointConfigName=xgboost_epc_name)
client.delete_endpoint(EndpointName=endpoint_name)
```




    {'ResponseMetadata': {'RequestId': 'e15647c4-cb82-4741-b7d3-cfe93598a299',
      'HTTPStatusCode': 200,
      'HTTPHeaders': {'x-amzn-requestid': 'e15647c4-cb82-4741-b7d3-cfe93598a299',
       'content-type': 'application/x-amz-json-1.1',
       'date': 'Sun, 02 Jun 2024 19:47:04 GMT',
       'content-length': '0'},
      'RetryAttempts': 0}}




```python

```
