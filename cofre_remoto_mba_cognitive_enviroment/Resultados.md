🚀 EXECUTANDO ANÁLISES COMPLETAS DO TRABALHO
============================================================

1️⃣ EXTRAÇÃO DE NOMES
🔍 EXTRAINDO NOMES DOS DOCUMENTOS
========================================
📄 Texto completo extraído:
------------------------------
FIAP MBA+
FACE & TEXT EXTRACTION
o setor de fraudes apontou que existem clientes que se queixaram de não contratar serviços
específicos, como o crédito pessoal. Entretanto após o indicador de Detecção de vivacidade
(liveness), desenvolvido na disciplina de Computer Vision, ter apresentado um percentual de
vivacidade menor que 90% apontou a necessidade de uma nova validação do self da pessoa com o
documento.
REPUBLICA FEDERATIVA 00 BRASIL
BR
SECRETARIA NACIONA TRANSITO
CARTERA NACIONAL DE HABULTACIO DRIVER LICENSE PERMISO DE CONDUCCION
see
det
NOME SOCIAL TEST CENTOR DEZ
16090389 SAO PALLOSS
-
Miss referencia
Date
- business
26060022
85 3691-7258
JULHO 2022
12/07/2022
R$
101.48
503584349
000000
1195
-
10
PA/DO TERTE HCM 5
1. Extração da Face, Nome e CPF
2. Comparação de faces (precisam ter
3. Extração do Nome e Endereço. Nome
mais do 90% de semelhança)
precisa ser o mesmo da CNH.
------------------------------
📄 Nome Social CNH: TEST CENTOR DEZ
📄 Nome CNH: e CPF
📄 Nome CNH: e Endereço. Nome
🏦 Nome comprovante: NOME SOCIAL TEST CENTOR DEZ
✅ Nome principal CNH definido: TEST CENTOR DEZ

2️⃣ COMPARAÇÃO FACIAL

🔍 COMPARAÇÃO FACIAL DETALHADA
==================================================
🎯 Análise de similaridade entre faces:

👥 Comparação 1:
   🎯 Similaridade: 100.00%
   🔍 Confiança: 100.00%
   ✅ Atende 90%: SIM
   ✅ Atende 80%: SIM

📊 ANÁLISE POR POSIÇÃO DAS FACES:
   Face 1: Posição (0.498, 0.543) - Confiança: 100.00%
   Face 2: Posição (0.381, 0.583) - Confiança: 99.99%
   Face 3: Posição (0.121, 0.665) - Confiança: 99.98%

3️⃣ COMPARAÇÃO DE NOMES - MÉTODO PADRÃO

📋 COMPARAÇÃO DE NOMES - MÉTODO PADRÃO
========================================
📄 Nome CNH (padrão): e Endereço. Nome
🏦 Nome Comprovante: NOME SOCIAL TEST CENTOR DEZ

🔍 Análise de similaridade (método padrão):
   📄 CNH normalizado: E ENDEREO NOME
   🏦 Comprovante normalizado: NOME SOCIAL TEST CENTOR DEZ
   🎯 Similaridade: 14.29%
   ✅ Nomes DIFERENTES

4️⃣ COMPARAÇÃO DE NOMES - MÉTODO APRIMORADO

📋 COMPARAÇÃO DE NOMES - MÉTODO APRIMORADO
========================================
💡 CONSIDERANDO NOME e NOME SOCIAL como equivalentes
📄 Nome CNH (principal): TEST CENTOR DEZ
🏦 Nome Comprovante: NOME SOCIAL TEST CENTOR DEZ
   ℹ️  Nome Social detectado: TEST CENTOR DEZ
   ℹ️  Nome Civil detectado: e Endereço. Nome

🔍 Análise de similaridade (método aprimorado):
   📄 CNH normalizado: TEST CENTOR DEZ
   🏦 Comprovante normalizado: NOME SOCIAL TEST CENTOR DEZ
   🎯 Similaridade: 60.00%
   ✅ Nomes COMPATÍVEIS
   🎯 Correspondência de palavras principais: SIM

============================================================
📋 RELATÓRIO FINAL COMPARATIVO
============================================================
📸 Imagem processada: processed_Pasted image 20250608090919.png
🕐 Data/Hora: 10/06/2025 às 16:33:02

👥 DETECÇÃO FACIAL:
   • Faces detectadas: 3
   • Face 1: 100.00% confiança
   • Face 2: 99.99% confiança
   • Face 3: 99.98% confiança

🔍 COMPARAÇÃO FACIAL:
   • Face comparação 1: 100.00% similaridade
     ✅ Atende 90%: SIM

📄 EXTRAÇÃO DE DADOS:
   • Nome Civil CNH: e Endereço. Nome
   • Nome Social CNH: TEST CENTOR DEZ
   • Nome Principal CNH: TEST CENTOR DEZ
   • CPF: None
   • Nome Comprovante: NOME SOCIAL TEST CENTOR DEZ

📋 COMPARAÇÃO DE NOMES - RESULTADOS:

   🔹 MÉTODO PADRÃO (apenas Nome Civil):
     • Similaridade: 14.29%
     • Status: DIFERENTES

   🔹 MÉTODO APRIMORADO (Nome Civil + Nome Social):
     • Similaridade: 60.00%
     • Status básico: COMPATÍVEIS
     • Correspondência principal: SIM
     • Status aprimorado: COMPATÍVEIS

✅ CONCLUSÕES FINAIS:
   • Detecção facial: ✅ OK
   • Comparação facial 90%: ✅ OK
   • Compatibilidade nomes (padrão): ❌ FALHA
   • Compatibilidade nomes (aprimorado): ✅ OK

🎯 STATUS FINAL:
   • Método Padrão: APROVADO
   • Método Aprimorado: APROVADO
============================================================