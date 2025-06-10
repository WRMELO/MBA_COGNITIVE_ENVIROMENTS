
1. Colocado os pds e md do curso em avalição no LM [NotebookLM](https://notebooklm.google.com/notebook/cbcd8576-675e-4461-9040-dfd2c898d1bb)  [Resumo esperto aqui][Análise Abrangente de Ambientes Cognitivos e IA & LLMs]
2. Colocado o [[EXERCICIO PARA ENTREGA]] no NotebookLM, foi recebida uma [[PROPOSIÇÃO PARA SOLUÇÃO]] 
3. Essa [[PROPOSIÇÃO PARA SOLUÇÃO]] foi alimentada e trabalhada na OPENAI, com o seguinte resultado [[PLANO DE ATAQUE DETALHADO]]
4. **Decidi pela criação de um container no worker da docker Swarm que estou rodando, para conter todas as dependências sem gerar conflito com os demais que estão rodando. Desenvolvido em** ==DEU RUIM! PERDI DOIS DIAS COM ISSO E NÃO FUNCIONOU==
	1. Abandonei a ideia de SWARM, vou usar apenas o Zembook como local de desenvolvimento. Quando precisar de GPU, vejo o que faço.
	2. Vou criar o container para isso. [[README_container_plano_ataque]] 
5. Foram obtidas as senhas para a [[README_aws_credenciais_corrigido | AWS]]  e [[OPEN AI - CHAVE |OPENAI]]
6. Foi criado um notebook que testou e validou o acesso a AWS e OPENAI [[00_configuração_aws_openai_s3]] 
7. Foi criado um notebook que trabalhou as imagens e texto na AWS [[RELATÓRIO TÉCNICO - NOTEBOOK 1-  PROCESSAMENTO E ANÁLISE DE IMAGENS | 01.AWS-Rekognition]] 
8. Foi criado um notebook voltado para atender a demanda do exercício proposto [[RELATÓRIO TÉCNICO - NOTEBOOK 2 - VISUALIZAÇÃO E ANÁLISE AVANÇADA| 02.comparacao_facial_extração_de_dados]]
9. Com a primeira rodada, percebi que o erro na extração do nome era causada pela expressão NOME SOCIAL na conta e então criei um método alternativo em que NOME = NOME SOCIAL
10. [[Resultados |RESULTADOS EXPRESSIVOS]]
11. [[annotated_image.jpg | RESULTADOS VISUAL]]
12. Fiz o [[FECHAMENTO DOS SERVIÇOS]] na conta AWS, deletando os objetos em S3.
