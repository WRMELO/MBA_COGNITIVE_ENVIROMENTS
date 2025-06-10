Com base na sua consulta e nos materiais fornecidos, apresento um roteiro detalhado para a solução da entrega do exercício, focando na **extração de face e texto para prevenção de fraudes**, conforme indicado no material, e considerando as restrições e sugestões das aulas.

---

### Roteiro Detalhado para a Solução do Exercício

O objetivo principal do exercício é desenvolver um projeto baseado nas especificações dos materiais, priorizando a utilização de **serviços em nuvem** (AWS e OpenAI são recomendados) e **não utilizando modelos criados sem a nuvem** ou modelos reutilizados da disciplina de Computer Vision. A entrega mínima inclui Jupyter Notebook(s) e artefatos, com a possibilidade de um ponto adicional para um aplicativo Streamlit funcional e publicado no StreamlitCloud.

O cenário central de fraude descrito () indica a necessidade de **validação de identidade através da comparação de uma "selfie" com um documento, complementada pela detecção de vivacidade e análise facial para garantir uma foto neutra**.

#### **Fase 1: Configuração do Ambiente e Requisitos Iniciais**

1. **Criação do Repositório GitHub**:
    
    - Inicie um novo repositório no GitHub para hospedar seu projeto. Pode ser público, mas **chaves de acesso nunca devem ser versionadas ou disponibilizadas publicamente**.
    - Crie uma estrutura de pastas organizada para os notebooks, artefatos (imagens, datasets) e, se aplicável, o código do aplicativo Streamlit.
2. **Configuração de Credenciais e Ferramentas**:
    
    - **Ambiente de Desenvolvimento**: Utilize o Google Colab ou um ambiente Python local com as bibliotecas necessárias.
    - **Credenciais AWS**:
        - Crie um **usuário dedicado para a aula** no Amazon IAM, sem acesso ao console, com foco em uso programático.
        - Obtenha a **chave de identificação (`ACCESS_ID`)** e a **chave de acesso (`ACCESS_KEY`)**. **Armazene-as em um local seguro** (variáveis de ambiente, AWS CLI credenciais ou `.streamlit/secrets.toml` para o app) e **nunca as inclua diretamente no código ou em repositórios públicos**.
        - Defina a **região da AWS como `us-east-1`**, pois é a região mais barata para os serviços da AWS.
        - Conceda as seguintes políticas de permissão ao usuário criado: `AmazonS3FullAccess`, `AmazonTextractFullAccess`, `AmazonPollyFullAccess`, `AmazonRekognitionFullAccess`, `ComprehendFullAccess`, e `AssumeRoleComprehend`.
    - **Chave de API OpenAI**:
        - Inscreva-se na plataforma OpenAI para obter sua chave de API. Novos usuários geralmente têm 30 dias de acesso gratuito.
        - Armazene esta chave de forma segura, similar às credenciais da AWS.
    - **Instalação de Bibliotecas**: Garanta que as seguintes bibliotecas Python estejam instaladas: `boto3` (AWS SDK), `matplotlib`, `pandas`, `requests`, `Pillow` (PIL), `json`, `langchain`, `openai`, `docarray`, `wikipedia`, `xmltodict`. **Cravar o versionamento** é uma boa prática para evitar quebras de compatibilidade.

#### **Fase 2: Desenvolvimento do Core do Projeto (Verificação de Identidade)**

O foco aqui é abordar o problema de fraude através da **validação do self com o documento**, incluindo a análise de vivacidade implicitamente através da verificação de expressões faciais.

1. **Análise Facial e Comparação com Amazon Rekognition**:
    
    - **Detecção de Faces (`detect_faces`)**:
        - Utilize o `Amazon Rekognition` para detectar faces na imagem da selfie e na imagem do documento.
        - Extraia atributos faciais importantes: `AgeRange`, `Gender`, `Eyeglasses`, `Sunglasses`, `EyesOpen`, `MouthOpen`, `Emotions`.
        - **Validações Essenciais (conforme Desafio 1 da Aula 1 e requisitos de fraude)**:
            - Verifique se há apenas uma face detectada na selfie.
            - **Evite oclusão parcial da face**: Analise os `BoundingBox` e a presença de acessórios.
            - Verifique que **não há óculos de sol** (`Sunglasses` com `Value=False` e alta `Confidence`).
            - Verifique que **os olhos estão abertos** (`EyesOpen` com `Value=True` e alta `Confidence`).
            - **Análise de Emoções para "Liveness" / Foto Neutra**: Use a função para obter a emoção com maior confiança (`obter_maior_confianca`). A foto deve ter uma **emoção predominante "CALM"**, ou a confiança de outras emoções deve ser baixa (ex: abaixo de 70%) para ser considerada neutra.
    - **Comparação de Faces (`compare_faces`)**:
        - Compare a face da selfie com a face detectada no documento.
        - O algoritmo deve aceitar apenas comparações com **similaridade acima de 90%**.
2. **Extração de Texto do Documento com Amazon Textract**:
    
    - Utilize o `Amazon Textract` para converter texto de imagens de documentos (ex: CNH, RG) para texto editável (`analyze_document` com `FeatureTypes=['FORMS']` ou `detect_document_text`).
    - Filtre os resultados para obter apenas palavras (`BlockType == "WORD"`) e com um alto grau de confiança (ex: >50%).
    - Implemente lógica para extrair informações relevantes do documento, como nome completo, número do documento, data de nascimento, etc. (se aplicável para o cenário de fraude específico).
3. **Lógica de Negócio para Prevenção de Fraude**:
    
    - Combine os resultados do Rekognition (validação facial, similaridade, emoção) e do Textract (dados do documento) para tomar uma decisão sobre a validação da identidade.
    - Defina as regras claras para aprovação ou reprovação da identidade, considerando todos os critérios de segurança.

#### **Fase 3: Desenvolvimento do Aplicativo Streamlit (Entrega Adicional Opcional)**

A criação de um aplicativo Streamlit interativo demonstrará a funcionalidade do seu modelo em um ambiente real, concedendo até 1 ponto adicional.

1. **Backend com AWS Lambda**:
    
    - Crie uma **função Lambda** que encapsule toda a lógica de Rekognition e Textract desenvolvida na Fase 2.
    - Configure um **Lambda URL** como endpoint externo para a função, permitindo que o Streamlit se comunique com ela via HTTP POST.
    - Dentro da função Lambda, **decodifique a imagem recebida em Base64** para bytes antes de passá-la aos clientes Rekognition/Textract.
    - A função Lambda deve retornar um JSON indicando sucesso/erro e os resultados da análise (ex: `statusCode`, `body` com bounding box da face, mensagens de erro).
    - As permissões IAM para o Lambda devem ser configuradas via **role de execução**, **não é necessário incluir as chaves de acesso AWS diretamente no código Lambda**.
2. **Frontend com Streamlit**:
    
    - **Captura de Imagem**: Utilize `st.camera_input` para a selfie e `st.file_uploader` (ou outro `camera_input`) para a imagem do documento.
    - **Envio para o Backend**: Codifique as imagens capturadas para Base64 e envie-as para o endpoint do Lambda via biblioteca `requests`.
    - **Feedback ao Usuário**:
        - Utilize `st.spinner` para indicar processamento em andamento.
        - Exiba `st.success` para fotos validadas e `st.error` para reprovações, com mensagens claras (ex: "Rosto com emoção predominante", "Não foram encontrados detalhes faciais").
        - Se a foto for validada, **exiba o rosto recortado** utilizando as coordenadas do `BoundingBox` retornado pelo Rekognition e a biblioteca Pillow.
    - **Gerenciamento de Segredos**: Crie um arquivo `.streamlit/secrets.toml` dentro da pasta `.streamlit` para armazenar a URL do endpoint Lambda, garantindo que não seja exposta publicamente no repositório.
    - **Design Procedural**: Siga a filosofia procedural do Streamlit, com fluxo sequencial e validações após cada etapa de entrada/processamento.
    - **Markdown**: Utilize Markdown (`st.markdown`) para enriquecer a interface do usuário com títulos, descrições e instruções.
3. **Publicação no StreamlitCloud**:
    
    - Após o desenvolvimento local, prepare seu aplicativo Streamlit para publicação no StreamlitCloud. Siga as instruções da plataforma para o deploy do seu repositório GitHub.
    - **Certifique-se de que o aplicativo seja funcional e interativo**.

#### **Fase 4: Documentação e Entrega**

1. **Jupyter Notebook(s)**:
    
    - Crie um ou mais notebooks Jupyter que demonstrem **claramente todo o processo de exploração, análise e treinamento do modelo** (no caso, a lógica de validação via Rekognition/Textract).
    - Inclua código limpo, comentários explicativos e saídas claras dos resultados.
    - Explique como as APIs da AWS (Rekognition, Textract) foram utilizadas para resolver o problema de fraude, detalhando os parâmetros e as respostas.
    - **Importante**: Mencione explicitamente que os modelos de Computer Vision de disciplinas anteriores não foram reutilizados, conforme a restrição.
2. **Artefatos**:
    
    - Inclua no seu repositório quaisquer imagens de exemplo (selfie, documento, imagens de teste para diferentes cenários de fraude) utilizadas para demonstrar e testar a solução.
    - Se você utilizou algum dataset específico (ex: para treinamento de Rekognition Custom Labels, se o escopo for expandido), inclua-o ou referencie sua fonte.
3. **Arquivo de Texto com URLs**:
    
    - Crie um arquivo de texto (`README.md` é ideal) na raiz do seu repositório GitHub.
    - Inclua a **URL pública do seu repositório GitHub**.
    - Se você desenvolveu o aplicativo Streamlit, inclua também a **URL pública da aplicação no StreamlitCloud**.
    - Forneça instruções claras sobre como rodar os notebooks e, se aplicável, como acessar e interagir com o aplicativo Streamlit.

#### **Fase 5: Considerações para Pontuação Adicional (Opcional)**

Embora o cenário de fraude seja o foco principal, as aulas apresentaram outros desafios que podem ser explorados em notebooks separados para demonstrar proficiência em mais áreas.

- **Análise de Avaliações de Clientes (LLMs com Langchain)**:
    - Implemente o **Desafio 1 da Aula 4**: Use Langchain com um modelo LLM (ex: OpenAI GPT-3.5 Turbo ou AWS Bedrock Llama/Titan) para resumir avaliações de clientes (dataset `dataset-customer-evaluations`), analisar o sentimento (positivo, negativo, neutro) e atribuir uma pontuação de -10 a +10.
    - Isto demonstraria o uso de `Prompt Template` e a capacidade dos LLMs de processar texto.
- **Agente PubMed (Langchain)**:
    - Implemente o **Desafio 2 da Aula 4**: Crie um agente Langchain que utilize a ferramenta PubMed para buscar informações e artigos médicos sobre a "Febre Familiar do Mediterrâneo".
    - Isto demonstraria o uso de `Agentes` com ferramentas externas.
- **Classificação de Plantações (Rekognition Custom Labels)**:
    - Implemente o **Desafio 1 da Aula 2**: Crie um modelo de classificação de imagens no Amazon Rekognition Custom Labels para classificar tipos de plantações, buscando precisão, recall e average precision acima de 90%. Utilize o dataset `dataset-agricultural-crops`.
    - Isto demonstraria o treinamento e inferência de modelos de Visão Computacional personalizados em nuvem.

Lembre-se de que a **clareza e organização dos notebooks e artefatos** serão avaliadas, além da **funcionalidade e usabilidade** do aplicativo Streamlit.