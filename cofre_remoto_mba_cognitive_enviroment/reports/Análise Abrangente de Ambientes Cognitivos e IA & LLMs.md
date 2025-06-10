
1. Visão Geral e Contexto de IA/LLMs

Este briefing detalha os principais conceitos e ferramentas para o desenvolvimento e implantação de soluções de Inteligência Artificial e Modelos de Linguagem de Grande Escala (LLMs), com foco nas plataformas da AWS e OpenAI. Aborda desde a base de dados e infraestrutura até modelos avançados e frameworks como Langchain.

1.1. Crescimento e Acessibilidade da Tecnologia

O avanço da IA é impulsionado por fatores como a proliferação de livros digitais, bibliotecas de fotos, dispositivos e IoT, e o custo decrescente de armazenamento e processamento. Tecnologias como SoC, arquitetura ARM e SSDs mais acessíveis contribuem para essa democratização. As Big Techs (Google, Meta) investem pesadamente em pesquisa e iniciativas Open Source (e.g., TensorFlow, Llama 3), enquanto provedores de cloud oferecem eficiências de escala e experiência de usuário.

1.2. Definição e Tipos de Modelos

• Inteligência Artificial (IA): Ampla área da ciência da computação dedicada à criação de máquinas que podem pensar, aprender e agir de forma inteligente.

• Aprendizado de Máquina (ML): Subcampo da IA que permite que os sistemas aprendam com dados sem serem explicitamente programados.

• Deep Learning (DL): Subcampo do ML que utiliza redes neurais profundas para modelar padrões complexos em grandes conjuntos de dados.

1.3. Modelos de Linguagem de Grande Escala (LLMs)

LLMs como GPT (OpenAI), Bard (Google), Claude (Anthropic), Titan (AWS) e Llama (Meta) são modelos de linguagem pré-treinados em vastos conjuntos de dados da internet, incluindo Wikipedia e livros. Eles se destacam pela capacidade de processar linguagem natural e gerar texto coerente e relevante.

• Tokens: A "granularidade mínima de contagem para determinar tanto a entrada de dados quanto a saída." Um token não é necessariamente uma palavra inteira e pode variar em diferentes posicionamentos em uma frase.

◦"1 token ~= 4 chars in English"

◦"1 token ~= ¾ words"

• Janela de Contexto (Tokens): "Quantidade de tokens que o modelo é capaz de processar na entrada." Modelos com maiores janelas de contexto são cruciais para técnicas como RAG (Retrieval-Augmented Generation), que enviam "partes de conhecimento para o GPT utilizar como base."

• Parâmetros: "Parâmetros utilizados durante o treinamento de um determinado modelo. Quanto mais parâmetros, mais robusto o modelo tende a ser." No entanto, "Para tarefas específicas não podemos deduzir que o número de parâmetros crescente em performance. Por isso a avaliação humana se faz necessária."

2. Plataformas de Cloud para IA e ML

As plataformas de cloud são essenciais para o desenvolvimento de IA devido à "Complexidade e Custo" e "Tempo de Desenvolvimento" envolvidos. Elas oferecem frameworks (TensorFlow, PyTorch, Scikit-Learn), APIs de cloud (Azure, AWS, IBM Cloud, Google Cloud, Hugging Face), e modelos pré-treinados, simplificando o processo.

2.1. AWS: Um Ecossistema Abrangente de Serviços de IA

A AWS oferece uma vasta gama de serviços de IA e ML, com foco em escalabilidade e facilidade de uso. Recomenda-se utilizar a região us-east-1 por ser "a região mais barata dos serviços da AWS". Para acesso aos serviços, é crucial criar um usuário dedicado e obter chaves de acesso (ACCESS_ID e ACCESS_KEY), que "Nunca versione ou disponibilize... em repositórios, somente (em último caso quando somente você tem acesso) em repositórios privados."

2.1.1. Amazon SageMaker Canvas

Uma plataforma para "Treinamento e Implantação de Modelo no Sagemaker Studio". Permite o uso de "tecnologia AutoML da AWS" para construir modelos.

• Fluxo de Trabalho:

1. Carregar Dataset: Suporta formatos tabular (CSV), imagens (PNG) e documentos (PDF).

2.  Revisão e Transformação de Dados: Tipagem, tratamento de dados inválidos.

3. Feature Engineering: Exemplos incluem "Conversão datas com horas e minutos para dia da semana, dia, hora e quarter," "Remoção de atributos identificadores," e "Definição de intervalos para ser aplicado em idade."

4. Treinamento do Modelo: Ex: XGBoost para classificação logística binária. O "Standardt Build" busca "Precisão".

5. Implantação (Inferência Serverless): O SageMaker Canvas "somente permite a implantação de modelos via instâncias dedicadas de inferência" (em contraste com o SageMaker Studio, que suporta serverless).

2.1.2. Amazon Rekognition

Serviço de visão computacional para análise de imagens e vídeos.

•

DetectLabels: Detecta objetos e cenas em imagens, retornando rótulos com "Confiança mínima necessária para considerar um rótulo."

•

DetectFaces: Identifica faces e extrai atributos como "Idade, Gênero, Pelos faciais, Sentimento, Abertura e fechamento dos olhos, Abertura e fechamento da boca, Posicionamento da face e elementos da face."

◦ Importante: Para cenários sensíveis, como aplicação da lei, é crucial "utilizar limiares de confiança de 99% ou mais e empregar a revisão humana das previsões de comparação facial para garantir que os direitos civis de uma pessoa não sejam violados."

◦ Desafio 1 (Aula 1): Criar um algoritmo para comparar duas faces, exigindo similaridade "acima de 90%" e proibindo "Face com oclusão (face parcialmente oculta), Óculos de sol, Olhos fechados."

• Moderation Labels: Ajuda a "identificar conteúdo potencialmente inadequado em imagens" (e.g., nudez, violência, drogas). Os rótulos são hierárquicos (TaxonomyLevel).

• Custom Labels: "Uma ferramenta para treinar modelos de visão computacional personalizados" para "reconhecer objetos ou padrões específicos relevantes para o seu caso de uso."

◦ Classificação de Imagens: Treinamento para classificar imagens em categorias (e.g., "truck," "car," "bus").

◦ Classificação de Objetos: Treinamento para detectar e delimitar objetos em imagens, utilizando o padrão YOLO para anotações.

2.1.3. Amazon Textract

"Serviço especilizado em lidar com OCR (Optical Character Recognition), ou seja, é capaz de identificar texto em imagens." Suporta "JPG e PNG" e "PDF" (até 5MB e 11 páginas). Permite filtrar textos detectados com base na confiança.

2.1.4. Amazon Comprehend

Serviço de Processamento de Linguagem Natural (NLP) para análise de texto.

• Detecção de Entidades: Identifica entidades nomeadas (PESSOA, ORGANIZAÇÃO, etc.) em textos.

• Modelo Customizado: Permite "criar um modelo customizado treinamento textos correspondentes a uma determinada classe, por exemplo artigos e sua determinada categoria." Os dados devem estar em formato CSV, com a classe na primeira coluna e o texto na segunda. O Comprehend "separa os dados de treinamento e teste automaticamente."

◦ Requisitos de métricas: "dados de precisão acima de 80% (isso varia com a especificação de cada projeto)." Métricas como Accuracy, Precision, Recall e F1 Score (Macro e Micro) e Hamming Loss são avaliadas.

2.1.5. Amazon Polly

"Serviço para converter texto em áudio." Suporta diversos idiomas e a linguagem de marcação SSML, que permite "adicionar uma pausa, enfatizar palavras, especificar outro idioma para palavras específicas, usar pronúncia fonética" e controlar "volume, taxa de fala e tom," além de efeitos especiais da Amazon como "sussurrando."

2.1.6. Amazon Transcribe

"Serviço para transcrever em texto a partir de gravações de voz. É capaz de separar as pessoas que são identificadas na gravação."

2.1.7. Amazon Bedrock

"Serviço da AWS que consolida diversos modelos de base de LLM de diferentes empresas, como por exemplo Meta, Anthropic, etc. além dos modelos da própria AWS." Oferece modelos generativos de texto/chat e imagem.

• Playgrounds: Ambientes para testar modelos, configurações e payloads.

• Modelos de Imagem: Permitem configurar "Prompt negativo, Imagem de referência, Tamanho, Força do prompt, Propagação (ou seed)."

• Geração de Texto: Permite controlar a geração com parâmetros como max_gen_len, temperature, e top_p (Top K define o número de candidatos mais prováveis, Top P define a porcentagem dos candidatos mais prováveis).

2.1.8. AWS Lambda

"Um tipo de serviço de computação serverless" que "permite que você execute código sem precisar provisionar ou gerenciar servidores."

• Características: "Serverless," "Escalabilidade Automática," "Pagamento por Uso," "Integração com Serviços AWS" (S3, DynamoDB, API Gateway, SNS, SQS, EventBridge).

• Acionadores: Pode ser iniciado por "eventos, como alterações em dados no Amazon S3, atualizações em tabelas do Amazon DynamoDB, chamadas de API no Amazon API Gateway."

• Deploy: Pode ser feito localmente (empacotando dependências com zip) ou via AWS CLI.

2.2. OpenAI

Pioneira em LLMs, oferece acesso à série GPT de modelos.

• Chave de API: "Novos usuários possuem 30 dias de acesso gratuito."

• Modelos:

◦ GPT-4o: "Modelo “omni” com capacidades nativas de entrada e saída de texto, imagem e áudio." (128.000 tokens)

◦ GPT-4 Turbo: "Versão otimizada do GPT-4, com maior janela de contexto e custo reduzido." (128.000 tokens)

◦ GPT-4: "Mais capaz do que qualquer modelo GPT-3.5, capaz de realizar tarefas mais complexas e otimizado para conversação." (8.192 tokens)

◦ GPT-3.5 Turbo: "Modelo GPT-3.5 mais capaz e otimizado para conversação a 1/10 do custo do text-davinci-003." (4.097 tokens, 16k versão com 16.385 tokens)

• DALL-E: Modelo para geração de imagens a partir de texto.

• Whisper: Modelo para transcrição de áudio.

• APIs: Acesso aos modelos via API, usando a biblioteca openai e definindo messages com diferentes "papéis" (usuário, sistema, assistente) para engenharia de prompt.

3. Frameworks e Ferramentas para Desenvolvimento de Aplicações de IA

3.1. Boto3: AWS SDK Python

A biblioteca "oficial Python para todos os serviços da AWS, incluindo os de Machine Learning." Permite usar credenciais armazenadas no AWS CLI para desenvolvimento local ou chaves diretamente no código para serviços externos como Google Colab. "Sempre utilize a documentação oficial."

3.2. Streamlit

"Uma biblioteca de código aberto em Python que permite a criação rápida e fácil de aplicativos da web interativos para ciência de dados e aprendizado de máquina." Permite transformar scripts Python em apps web "com apenas algumas linhas de código."

• Interface: Suporta Markdown para renderizar texto na aplicação.

• Interatividade: Componentes como spinner (processamento pendente) e success/error (exibição de estados).

• Secrets: Armazenamento de "dados que não precise ser exposto publicamente como chaves e endpoints" em arquivos secrets.toml dentro da pasta .streamlit.

• Dependências: Utiliza bibliotecas como Pillow (recorte de imagem) e Requests (acesso a APIs externas). "Cravar o versionamento evita atualizações mais recentes que podem quebrar a compatibilidade do nosso projeto."

3.3. Langchain: LLM Framework

"Um framework relativamente novo que propõe a abstrair o uso de diferentes modelos de base de LLM ao integrar com cada uma das APIs e permitir que sejam construídos aplicações não dependentes de APIs próprias das plataformas e acrescentar novas camadas neste tipo de uso, tais como memória, encadeamento e agentes."

• Prompt Template: Permite padronizar prompts, substituindo partes dinâmicas por variáveis. Exemplo: analisar avaliações de clientes.

• Memória e Contexto: Adiciona contexto às interações com LLMs, similar ao contexto de conversa do ChatGPT.

◦ Buffer de Conversa: Mantém o histórico da conversa para contextualização.

◦ Buffer de Conversa com Janela: Limita o contexto a uma janela de interações.

◦ Buffer de Conversa com Tokens: Limita o contexto por número de tokens.

◦ Sumarização: Utiliza o próprio LLM para resumir a memória, otimizando o uso de tokens.

• Dados Externos (Retrieval-Augmented Generation - RAG): Permite adicionar "conhecimento especializado da qual não foi originalmente treinado" aos LLMs.

◦ Fine-tuning: Treinar o modelo com mais exemplos de prompt e resposta (custoso).

◦ Embeddings: Mapear documentos para representações vetoriais que permitem uma "pesquisa inicial para então levar aos LLMs contexto relevante para uma resposta ser elaborada." * Loader: Ferramentas para carregar diferentes tipos de documentos (CSVLoader, PDFLoader). * Vector Store (Ex: FAISS): Armazenamento de embeddings com índices para busca por similaridade. * Permite "citar as fontes dos documentos utilizados" para garantir a origem da resposta.

• Agentes: Permite "criação de agentes utilizando os LLM como formas de criar uma planejamento, escolha de ação e refino das respostas obtidas." Agentes podem suprir deficiências dos LLMs ou conectar com outros sistemas (e.g., Agente de Matemática, Wikipedia). Permite criar ferramentas customizadas que consomem APIs externas.

4. Exemplos e Desafios Práticos

Os materiais fornecem diversos exemplos práticos e desafios, ilustrando a aplicação dos serviços e frameworks.

• Detecção de Face para Documentos (Streamlit): Validações de face como ausência de oclusão, óculos de sol, olhos fechados, e similaridade acima de 90%.

• Classificação de Plantações (Rekognition Custom Labels): Desafio para "Criar um modelo para classificar diferentes tipos de plantações (considere no mínimo 3 classes). Busque alcançar valores de precisão, recall e average precision maiores do que 90%."

• Análise de Avaliações de Clientes (Langchain + LLM):

◦ Desafio 1 (Aula 4): "Revise as avaliações da Amazon utilizando LLM e extraia as seguintes informações: resumo em até 300 caracteres em tom calmo, análise de sentimento (positivo, negativou ou neutro) e uma pontuação de -10 até +10 em relação ao sentimento detectado."

◦ Exemplo de sintetização de reclamações para padronizar análises e remover palavras irrelevantes.

• Interação com Conhecimento Externo (Langchain): Exemplos de uso de embeddings para buscar informações em datasets CSV (avaliações do Mercado Livre) e PDF, e usar o LLM para responder com base nesse contexto.

5. Recomendações e Melhores Práticas

• Segurança de Credenciais: "Nunca versione ou disponibilize as chaves de acesso em repositórios, somente (em último caso quando somente você tem acesso) em repositórios privados." Utilizar variáveis de ambiente ou serviços de secrets (como no Streamlit) é fundamental.

• Documentação Oficial: "Sempre utilize a documentação oficial" para SDKs e APIs, pois os modelos e serviços podem ser atualizados ou descontinuados.

• Versionamento de Dependências: "Cravar o versionamento evita atualizações mais recentes que podem quebrar a compatibilidade do nosso projeto."

• Revisão Humana: Para cenários críticos que impactam direitos civis, "a decisão de agir deve ser tomada por uma pessoa devidamente treinada com base em sua análise independente da evidência de identificação."

• Otimização de Custos:

◦ Utilizar a região us-east-1 na AWS.

◦ A execução de detecção por Job no Amazon Comprehend "tem custo muito menor que a em tempo real."

◦ Avaliar os limites máximos de gastos para aplicações em produção com serviços de LLM.

◦ Técnicas como sumarização e embeddings em Langchain ajudam a diminuir o número de tokens enviados aos LLMs, reduzindo custos.

6. Considerações Finais

O campo da IA e LLMs está em constante evolução, com o lançamento de novos modelos e técnicas a cada mês. A compreensão dos conceitos fundamentais, a familiaridade com as principais plataformas de cloud (AWS, OpenAI) e o domínio de frameworks como Langchain são cruciais para o desenvolvimento de soluções inteligentes e escaláveis. A ênfase na acessibilidade, na otimização de custos e na segurança é igualmente importante para a implantação bem-sucedida dessas tecnologias.