🔍 VERIFICAÇÃO DE RECURSOS AWS
==================================================
📦 VERIFICANDO BUCKET S3:
   📁 Bucket 'staging-face-text' contém 2 objetos:
      📄 ocr_results_20250610_160913.json - 0.04 MB
      📄 processed_Pasted image 20250608090919.png - 0.13 MB
   📊 Tamanho total: 0.17 MB

🗑️ DELETANDO ARQUIVOS...
   ✅ Deletado: ocr_results_20250610_160913.json
   ✅ Deletado: processed_Pasted image 20250608090919.png
✅ Todos os arquivos foram deletados!

🔍 VERIFICANDO OUTROS SERVIÇOS AWS:
   👁️ Rekognition: Serviço pay-per-use (sem recursos persistentes)
   📄 Textract: Serviço pay-per-use (sem recursos persistentes)
   ⚠️ CloudWatch: Não foi possível verificar (An error occurred (AccessDeniedException) when calling the DescribeLogGroups operation: User: arn:aws:iam::210867938716:user/WRMELO is not authorized to perform: logs:DescribeLogGroups on resource: arn:aws:logs:us-east-1:210867938716:log-group::log-stream: because no identity-based policy allows the logs:DescribeLogGroups action)

💰 RESUMO DE CUSTOS:
   📦 S3: ~$0.023/GB/mês (dados armazenados)
   👁️ Rekognition: ~$0.001 por imagem processada
   📄 Textract: ~$0.0015 por página processada
   📊 Total estimado desta sessão: < $0.10

🤖 VERIFICAÇÃO OPENAI:
==============================
ℹ️ Você está usando Claude (Anthropic), não OpenAI
ℹ️ Não há recursos OpenAI para verificar/fechar
✅ Sem custos OpenAI nesta sessão

💡 DICAS DE OTIMIZAÇÃO DE CUSTOS:
========================================
🔧 AWS:
   • Delete arquivos S3 não utilizados regularmente
   • Use lifecycle policies para arquivos temporários
   • Configure alertas de billing no CloudWatch
   • Monitore uso através do AWS Cost Explorer

🤖 IA (Claude/Anthropic):
   • Esta conversa é cobrada por tokens utilizados
   • Evite conversas muito longas desnecessárias
   • Use prompts concisos e diretos
   • Considere pausar quando não estiver usando