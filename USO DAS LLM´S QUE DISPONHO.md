



|Fase|Ação|LLM Sugerido|
|---|---|---|
|**1. Ambiente e Credenciais**|Criar repositório Git, configurar IAM (AWS/OpenAI), definir variáveis de ambiente e instalar dependências.|o4-mini-high|
|**2. EDA e Pré‑processamento**|Desenvolver funções de leitura, limpeza de dados e visualizações iniciais; gerar logs textuais.|o4-mini-high|
|**3. Core – Rekognition & Textract**|Implementar detecção de faces, extração de texto de imagens e aplicar regras de fraude.|GPT-4.5|
|**4. Relatórios Automáticos**|Gerar trechos em Markdown com insights (ex.: “imagem sem oclusão” ou “texto extraído válido”).|GPT-4.5|
|**5. Aplicativo Streamlit**|Estruturar front‑end interativo, integrar chamadas ao backend (Lambda/API), tratar erros e UX.|GPT-4o|
|**6. Testes & CI/CD**|Criar unit tests, simular AWS local (LocalStack) e configurar pipeline de CI (GitHub Actions).|GPT-4.1|
|**7. Documentação Final**|Redigir README, guia de deploy e manual do usuário em Markdown.|GPT-4.1 / o3|
|**8. Pontos Extras (Langchain, PubMed)**|Explorar agentes Langchain e integração com PubMed conforme desafios das aulas.|GPT-4.5|

## Razões das Mudanças

- **Aproveitamento Máximo de LLMs**: Usa GPT-4.5 e GPT-4o para automação avançada de relatórios e análise multimodal.
    
- **Custo vs. Desempenho**: Emprega modelos menores (o4-mini-high, o3) em iterações rápidas de EDA e ajustes de código.
    
- **Robustez no Desenvolvimento**: Inclui testes automatizados e LocalStack para reduzir custos e riscos de chamadas diretas à AWS.
    
- **Clareza e Usabilidade**: Relatórios gerados por LLMs garantem entregáveis consistentes e de fácil leitura.
    

---

_Este documento pode ser colado diretamente no Obsidian para acompanhamento do projeto._

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