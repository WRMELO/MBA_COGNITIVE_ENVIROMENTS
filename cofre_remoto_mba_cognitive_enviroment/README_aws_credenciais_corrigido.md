# 🔐 Obtenção das Credenciais AWS (Access Key e Secret Access Key)

Este documento descreve detalhadamente o processo utilizado para criar um **usuário IAM** na AWS com **acesso programático** e gerar a **Access Key ID** e **Secret Access Key**, usadas para autenticação via boto3 ou outras SDKs.

---

## ✅ Etapas Realizadas

### 1. Acesso como Usuário Root
- URL acessada: [https://signin.aws.amazon.com](https://signin.aws.amazon.com)
- Tipo de login: **Usuário raiz (root user)**
- Login feito com o e-mail principal da conta AWS

---

### 2. Acesso ao IAM (Gerenciamento de Identidade e Acesso)
- No console AWS, acessamos o serviço **IAM** diretamente via atalho ou busca

---

### 3. Criação do Usuário IAM
- Menu: **Usuários > Adicionar usuário**
- Nome do usuário: `WRMELO`
- Tipo de acesso: ☑️ **Acesso programático** (somente)
- NÃO foi concedido acesso ao Console AWS
- Políticas atribuídas:
  - AmazonRekognitionFullAccess
  - AmazonTextractFullAccess
  - AmazonS3FullAccess
  - AmazonPollyFullAccess (opcional)
  - ComprehendFullAccess (opcional)

---

### 4. Geração das Chaves de Acesso
- Acessamos a aba: **Credenciais de segurança**
- Seção: **Chaves de acesso**
- Ação: **Criar chave de acesso**
  - Uso: Aplicação fora da AWS
  - Após clicar em "Próximo", a chave foi nomeada (`projeto-verificacao`)
  - A chave foi criada com sucesso e os dados foram exibidos:

#### Exemplo (não real):

- Access Key ID: `AKIAIOSFODNN7EXAMPLE`
- Secret Access Key: `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`

⚠️ A Secret Access Key só é exibida uma vez.  
Deve ser salva imediatamente em um local seguro.

---

## 📁 Armazenamento local
As chaves foram copiadas e salvas no arquivo:
```
WRMELO_accessKeys.csv
```

Este arquivo foi movido para a raiz do projeto local:
```
C:/Users/wilso/MBA_COGNITIVE_ENVIROMENTS/
```

E está protegendo por `.gitignore`, garantindo que não será versionado no Git.
![[Pasted image 20250610081551.png]]
---

## 📌 Observações de Segurança
- Nunca envie chaves para repositórios públicos
- Use variáveis de ambiente (.env) ou boto3.Session() para carregamento seguro
- Revogue chaves não utilizadas e monitore logs de uso via CloudTrail

---

**Status final**:  
✅ Credenciais criadas, salvas localmente e prontas para uso seguro com boto3.
