# Desafio 1

Construa um algoritmo para comparar 2 faces afim de determinar se elas são iguais. Para ambas as faces garanta que nenhuma das condições abaixo seja permitido em nenhuma das faces de comparação:

* Face com oclusão (face parcialmente oculta)
* Óculos de sol
* Olhos fechados

Além disso, o algoritmo deve considerar o valor de similaridade como score e somente valores acima de 90% deverão ser aceitos.



```python
# Instalação da biblioteca Boto3, sempre obter a última versão
!pip install boto3
```

    Collecting boto3
      Downloading boto3-1.28.63-py3-none-any.whl (135 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m135.8/135.8 kB[0m [31m3.7 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting botocore<1.32.0,>=1.31.63 (from boto3)
      Downloading botocore-1.31.63-py3-none-any.whl (11.3 MB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m11.3/11.3 MB[0m [31m38.7 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting jmespath<2.0.0,>=0.7.1 (from boto3)
      Downloading jmespath-1.0.1-py3-none-any.whl (20 kB)
    Collecting s3transfer<0.8.0,>=0.7.0 (from boto3)
      Downloading s3transfer-0.7.0-py3-none-any.whl (79 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m79.8/79.8 kB[0m [31m10.0 MB/s[0m eta [36m0:00:00[0m
    [?25hRequirement already satisfied: python-dateutil<3.0.0,>=2.1 in /usr/local/lib/python3.10/dist-packages (from botocore<1.32.0,>=1.31.63->boto3) (2.8.2)
    Requirement already satisfied: urllib3<2.1,>=1.25.4 in /usr/local/lib/python3.10/dist-packages (from botocore<1.32.0,>=1.31.63->boto3) (2.0.6)
    Requirement already satisfied: six>=1.5 in /usr/local/lib/python3.10/dist-packages (from python-dateutil<3.0.0,>=2.1->botocore<1.32.0,>=1.31.63->boto3) (1.16.0)
    Installing collected packages: jmespath, botocore, s3transfer, boto3
    Successfully installed boto3-1.28.63 botocore-1.31.63 jmespath-1.0.1 s3transfer-0.7.0
    


```python
import boto3
import matplotlib.pyplot as plt
import matplotlib.image as mpimg
from PIL import Image, ImageDraw
```


```python
ACCESS_ID = "ACCESS-ID"
ACCESS_KEY = "ACCESS-KEY"
region = "us-east-1"
```


```python
imagem = Image.open("imagens/tom-cruise-closed-eyes.jpg")
imagem
```




    
![png](aula-1-desafio-1-solucao_files/aula-1-desafio-1-solucao_4_0.png)
    




```python
# nome do arquivo
file_name = "imagens/tom-cruise-closed-eyes.jpg"

# conversão para binário
with open(file_name, "rb") as file:
  img_file = file.read()
  bytes_file = bytearray(img_file)

# abrindo a sessão
session = boto3.Session(aws_access_key_id=ACCESS_ID, aws_secret_access_key= ACCESS_KEY)

# criando o cliente
client = session.client("rekognition", region_name=region)

# criando a requisição
response = client.detect_faces(
    Image={'Bytes': bytes_file},
    Attributes=["ALL"]
)
```


```python
def get_face_details(file_name):
  # conversão para binário
  with open(file_name, "rb") as file:
    img_file = file.read()
    bytes_file = bytearray(img_file)

  # abrindo a sessão
  session = boto3.Session(aws_access_key_id=ACCESS_ID, aws_secret_access_key= ACCESS_KEY)

  # criando o cliente
  client = session.client("rekognition", region_name=region)

  # criando a requisição
  response = client.detect_faces(
      Image={'Bytes': bytes_file},
      Attributes=["EYES_OPEN", "FACE_OCCLUDED", "SUNGLASSES"]
  )

  return response
```


```python
response
```




    {'FaceDetails': [{'BoundingBox': {'Width': 0.27534058690071106,
        'Height': 0.5361295342445374,
        'Left': 0.47012874484062195,
        'Top': 0.27049511671066284},
       'AgeRange': {'Low': 47, 'High': 53},
       'Smile': {'Value': True, 'Confidence': 96.4332275390625},
       'Eyeglasses': {'Value': False, 'Confidence': 97.72664642333984},
       'Sunglasses': {'Value': False, 'Confidence': 99.99673461914062},
       'Gender': {'Value': 'Male', 'Confidence': 99.99757385253906},
       'Beard': {'Value': False, 'Confidence': 81.54521179199219},
       'Mustache': {'Value': False, 'Confidence': 98.11453247070312},
       'EyesOpen': {'Value': False, 'Confidence': 52.076072692871094},
       'MouthOpen': {'Value': True, 'Confidence': 95.58202362060547},
       'Emotions': [{'Type': 'HAPPY', 'Confidence': 99.04776763916016},
        {'Type': 'SURPRISED', 'Confidence': 6.263737201690674},
        {'Type': 'FEAR', 'Confidence': 5.895318031311035},
        {'Type': 'SAD', 'Confidence': 2.160299301147461},
        {'Type': 'CONFUSED', 'Confidence': 0.5519374012947083},
        {'Type': 'DISGUSTED', 'Confidence': 0.1261473447084427},
        {'Type': 'ANGRY', 'Confidence': 0.12597715854644775},
        {'Type': 'CALM', 'Confidence': 0.024291789159178734}],
       'Landmarks': [{'Type': 'eyeLeft',
         'X': 0.5343376994132996,
         'Y': 0.479768842458725},
        {'Type': 'eyeRight', 'X': 0.6591022610664368, 'Y': 0.46340101957321167},
        {'Type': 'mouthLeft', 'X': 0.5578840970993042, 'Y': 0.6584852337837219},
        {'Type': 'mouthRight', 'X': 0.6624504923820496, 'Y': 0.6450290083885193},
        {'Type': 'nose', 'X': 0.6003144383430481, 'Y': 0.5740194320678711},
        {'Type': 'leftEyeBrowLeft',
         'X': 0.48634129762649536,
         'Y': 0.4415777325630188},
        {'Type': 'leftEyeBrowRight',
         'X': 0.5554004907608032,
         'Y': 0.42392244935035706},
        {'Type': 'leftEyeBrowUp',
         'X': 0.5194430947303772,
         'Y': 0.41810277104377747},
        {'Type': 'rightEyeBrowLeft',
         'X': 0.6262542605400085,
         'Y': 0.41428226232528687},
        {'Type': 'rightEyeBrowRight',
         'X': 0.7019156813621521,
         'Y': 0.41251373291015625},
        {'Type': 'rightEyeBrowUp',
         'X': 0.6620921492576599,
         'Y': 0.3987189829349518},
        {'Type': 'leftEyeLeft', 'X': 0.5126071572303772, 'Y': 0.4810470938682556},
        {'Type': 'leftEyeRight',
         'X': 0.5590682625770569,
         'Y': 0.47812598943710327},
        {'Type': 'leftEyeUp', 'X': 0.5331725478172302, 'Y': 0.47090575098991394},
        {'Type': 'leftEyeDown', 'X': 0.5354377031326294, 'Y': 0.48771941661834717},
        {'Type': 'rightEyeLeft',
         'X': 0.6342713236808777,
         'Y': 0.46816980838775635},
        {'Type': 'rightEyeRight',
         'X': 0.6812176704406738,
         'Y': 0.4587289094924927},
        {'Type': 'rightEyeUp', 'X': 0.6581871509552002, 'Y': 0.4543592035770416},
        {'Type': 'rightEyeDown',
         'X': 0.6587864756584167,
         'Y': 0.47143271565437317},
        {'Type': 'noseLeft', 'X': 0.5803433060646057, 'Y': 0.5938774347305298},
        {'Type': 'noseRight', 'X': 0.6260779500007629, 'Y': 0.5877040028572083},
        {'Type': 'mouthUp', 'X': 0.606110692024231, 'Y': 0.6343348026275635},
        {'Type': 'mouthDown', 'X': 0.6105177402496338, 'Y': 0.6884245276451111},
        {'Type': 'leftPupil', 'X': 0.5343376994132996, 'Y': 0.479768842458725},
        {'Type': 'rightPupil', 'X': 0.6591022610664368, 'Y': 0.46340101957321167},
        {'Type': 'upperJawlineLeft',
         'X': 0.46675676107406616,
         'Y': 0.4828830361366272},
        {'Type': 'midJawlineLeft',
         'X': 0.506222128868103,
         'Y': 0.6779934763908386},
        {'Type': 'chinBottom', 'X': 0.6188875436782837, 'Y': 0.7804732918739319},
        {'Type': 'midJawlineRight',
         'X': 0.7241610288619995,
         'Y': 0.6481039524078369},
        {'Type': 'upperJawlineRight',
         'X': 0.7361435294151306,
         'Y': 0.44609472155570984}],
       'Pose': {'Roll': -6.262796401977539,
        'Yaw': -1.6711121797561646,
        'Pitch': 2.4845173358917236},
       'Quality': {'Brightness': 67.92255401611328,
        'Sharpness': 95.51618957519531},
       'Confidence': 99.99996185302734,
       'FaceOccluded': {'Value': False, 'Confidence': 99.85398864746094},
       'EyeDirection': {'Yaw': -15.527738571166992,
        'Pitch': 1.491986870765686,
        'Confidence': 99.67452239990234}}],
     'ResponseMetadata': {'RequestId': '81915710-cb76-4102-8a7e-b1ef79852362',
      'HTTPStatusCode': 200,
      'HTTPHeaders': {'x-amzn-requestid': '81915710-cb76-4102-8a7e-b1ef79852362',
       'content-type': 'application/x-amz-json-1.1',
       'content-length': '3508',
       'date': 'Fri, 13 Oct 2023 10:44:29 GMT'},
      'RetryAttempts': 0}}




```python
response["FaceDetails"][0]["EyesOpen"]

```




    {'Value': False, 'Confidence': 52.076072692871094}




```python
response["FaceDetails"][0]["Sunglasses"]
```




    {'Value': False, 'Confidence': 99.99673461914062}




```python
response["FaceDetails"][0]["FaceOccluded"]
```




    {'Value': False, 'Confidence': 99.85398864746094}




```python
def verificacao_face(face_details):
  if len(face_details["FaceDetails"]) > 1:
    print("Imagem com 2 faces.")
    return None
  face = face_details["FaceDetails"][0]
  eyes_open = face["EyesOpen"]
  sunglasses = face["Sunglasses"]
  occlusion = face["FaceOccluded"]
  if eyes_open["Value"] == False:
    print("Face com olhos fechados, confiança " + str(eyes_open["Confidence"]) + "%.")
  if sunglasses["Value"] == True:
    print("Face com óculos escuros, confiança " + str(sunglasses["Confidence"]) + "%.")
  if occlusion["Value"] == True:
    print("Face com oclusão, confiança " + str(occlusion["Confidence"]) + "%.")
  if eyes_open["Value"] == True and sunglasses["Value"] == False and occlusion["Value"] == False:
    print("Face sem problemas para processar")
```


```python
verificacao_face(response)
```

    Face com olhos fechados, confiança 52.076072692871094%.
    


```python
file_path = "imagens/tom-cruise-closed-eyes.jpg"
face_details = get_face_details(file_path)
verificacao_face(face_details)
imagem = Image.open(file_path)
imagem
```

    Face com olhos fechados, confiança 52.076072692871094%.
    




    
![png](aula-1-desafio-1-solucao_files/aula-1-desafio-1-solucao_13_1.png)
    




```python
file_path = "imagens/tom-cruise-occlusion.png"
face_details = get_face_details(file_path)
verificacao_face(face_details)
imagem = Image.open(file_path)
imagem
```

    Face com oclusão, confiança 85.25176239013672%.
    




    
![png](aula-1-desafio-1-solucao_files/aula-1-desafio-1-solucao_14_1.png)
    




```python
file_path = "imagens/tom-cruise-sunglasses.jpg"
face_details = get_face_details(file_path)
verificacao_face(face_details)
imagem = Image.open(file_path)
imagem
```

    Face com óculos escuros, confiança 99.68962860107422%.
    Face com oclusão, confiança 99.99376678466797%.
    




    
![png](aula-1-desafio-1-solucao_files/aula-1-desafio-1-solucao_15_1.png)
    




```python
file_path = "imagens/tom-cruise-2-people.jpg"
face_details = get_face_details(file_path)
verificacao_face(face_details)
imagem = Image.open(file_path)
imagem
```

    Imagem com 2 faces.
    




    
![png](aula-1-desafio-1-solucao_files/aula-1-desafio-1-solucao_16_1.png)
    




```python
file_path = "imagens/tom-cruise-1.jpg"
face_details = get_face_details(file_path)
verificacao_face(face_details)
imagem = Image.open(file_path)
imagem
```

    Face sem problemas para processar
    




    
![png](aula-1-desafio-1-solucao_files/aula-1-desafio-1-solucao_17_1.png)
    




```python
file_path = "imagens/tom-cruise-2.jpg"
face_details = get_face_details(file_path)
verificacao_face(face_details)
imagem = Image.open(file_path)
imagem
```

    Face sem problemas para processar
    




    
![png](aula-1-desafio-1-solucao_files/aula-1-desafio-1-solucao_18_1.png)
    




```python
# imagem de comparação 1 (origem)
file_name_source = "imagens/tom-cruise-1.jpg"

# imagem de comparação 2 (alvo)
file_name_target = "imagens/tom-cruise-2.jpg"

# convertendo a imagem de origem no formato binário
with open(file_name_source, "rb") as file:
  img_file = file.read()
  bytes_file_source = bytearray(img_file)

# convertendo a imagem alvo no formato binário
with open(file_name_target, "rb") as file:
  img_file = file.read()
  bytes_file_target = bytearray(img_file)

# abrindo a sessão
session = boto3.Session(aws_access_key_id=ACCESS_ID, aws_secret_access_key= ACCESS_KEY)

# criando o cliente
client = session.client("rekognition", region_name=region)

# realizando a requisição
response = client.compare_faces(
    SourceImage={'Bytes': bytes_file_source},
    TargetImage={'Bytes': bytes_file_target},
)
```


```python
response
```




    {'SourceImageFace': {'BoundingBox': {'Width': 0.2823902368545532,
       'Height': 0.3734133243560791,
       'Left': 0.3723376393318176,
       'Top': 0.17630769312381744},
      'Confidence': 99.99870300292969},
     'FaceMatches': [{'Similarity': 99.99909973144531,
       'Face': {'BoundingBox': {'Width': 0.21921908855438232,
         'Height': 0.459481418132782,
         'Left': 0.39702609181404114,
         'Top': 0.16736984252929688},
        'Confidence': 99.99957275390625,
        'Landmarks': [{'Type': 'eyeLeft',
          'X': 0.4558055102825165,
          'Y': 0.32461464405059814},
         {'Type': 'eyeRight', 'X': 0.5539187788963318, 'Y': 0.3309805393218994},
         {'Type': 'mouthLeft', 'X': 0.4597553312778473, 'Y': 0.4785703122615814},
         {'Type': 'mouthRight', 'X': 0.5416693687438965, 'Y': 0.48399412631988525},
         {'Type': 'nose', 'X': 0.5028138160705566, 'Y': 0.4017588496208191}],
        'Pose': {'Roll': 1.4602535963058472,
         'Yaw': -0.6464588046073914,
         'Pitch': 9.44393539428711},
        'Quality': {'Brightness': 81.76628112792969,
         'Sharpness': 95.51618957519531}}}],
     'UnmatchedFaces': [],
     'ResponseMetadata': {'RequestId': '555fadc7-80df-42d9-9475-4f03ae994bec',
      'HTTPStatusCode': 200,
      'HTTPHeaders': {'x-amzn-requestid': '555fadc7-80df-42d9-9475-4f03ae994bec',
       'content-type': 'application/x-amz-json-1.1',
       'content-length': '911',
       'date': 'Fri, 13 Oct 2023 11:24:16 GMT'},
      'RetryAttempts': 0}}




```python
response["FaceMatches"][0]["Similarity"]
```




    99.99909973144531




```python

```
