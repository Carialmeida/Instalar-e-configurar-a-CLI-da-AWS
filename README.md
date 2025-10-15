# 🌩️ Instalar e Configurar a AWS CLI

Repositório criado como parte de um **laboratório AWS**, demonstrando:

- 🚀 Acesso a uma **instância EC2 (Amazon Linux 2)** via **PuTTY**  
- 🧩 Validação e uso da **AWS CLI v2**  
- 🛡️ Execução do **desafio IAM (`lab_policy`)** — usando apenas linha de comando!  

---

## 🎯 Objetivo

Aprender a:

- Conectar-se a uma instância EC2 com chave privada (`.ppk`)  
- Configurar e validar a AWS CLI  
- Executar comandos IAM diretamente no terminal  
- Baixar o documento JSON da política **`lab_policy`**

---

## 🖥️ Acesso à Instância EC2 (via PuTTY)

1. **Baixe a chave `.ppk`** no painel do laboratório.  
2. **Copie o IP público** (exemplo: `35.161.200.130`).  
3. **Abra o PuTTY** e configure:

Host Name: ec2-user@<SEU_IP>
Port: 22
Connection type: SSH

css
Copiar código
4. Vá em:
Connection → SSH → Auth → Credentials

yaml
Copiar código
Selecione sua chave `.ppk` em **Private key file for authentication**.  
5. Clique em **Open** e aceite o aviso de segurança.  
6. Você verá o prompt de boas-vindas do Amazon Linux 🎉  

> 💡 **Dica:** use o usuário `ec2-user` (Amazon Linux) ou `ubuntu` (para AMI Ubuntu).

---

## ⚙️ Validando a AWS CLI

Verifique se a CLI está instalada corretamente:

```bash
aws --version
Saída esperada:

bash
Copiar código
aws-cli/2.x Python/3.x Linux/...
🧠 Desafio: Baixar o JSON da Política lab_policy
O objetivo é baixar o documento da política IAM lab_policy sem usar o Console da AWS.

🔹 Passo a passo
1️⃣ Listar políticas locais:

bash
Copiar código
aws iam list-policies --scope Local
2️⃣ Copiar o ARN da política lab_policy.

3️⃣ Obter a versão padrão:

bash
Copiar código
aws iam get-policy --policy-arn <ARN>
4️⃣ Baixar o documento JSON:

bash
Copiar código
aws iam get-policy-version \
  --policy-arn <ARN> \
  --version-id <VERSAO> \
  --query "PolicyVersion.Document" \
  --output json > lab_policy.json
5️⃣ Verificar o conteúdo:

bash
Copiar código
cat lab_policy.json
⚡ Script Automatizado (opcional)
Crie um arquivo chamado get_lab_policy.sh dentro da pasta scripts/ e adicione:

bash
Copiar código
#!/usr/bin/env bash
set -euo pipefail

echo "🔎 Buscando ARN da política 'lab_policy'..."
POLICY_ARN=$(aws iam list-policies --scope Local \
  --query "Policies[?PolicyName=='lab_policy'].Arn" --output text)

if [[ -z "${POLICY_ARN}" || "${POLICY_ARN}" == "None" ]]; then
  echo "❌ Política 'lab_policy' não encontrada."
  exit 1
fi
echo "✅ POLICY_ARN: $POLICY_ARN"

VERSION_ID=$(aws iam get-policy --policy-arn "$POLICY_ARN" \
  --query "Policy.DefaultVersionId" --output text)
echo "✅ VERSION_ID: $VERSION_ID"

aws iam get-policy-version --policy-arn "$POLICY_ARN" --version-id "$VERSION_ID" \
  --query "PolicyVersion.Document" --output json > lab_policy.json

echo "📄 Salvo em $(pwd)/lab_policy.json"
Execute com:

bash
Copiar código
bash scripts/get_lab_policy.sh


## 🧰 Solução de Problemas

| 💬 **Erro** | ⚙️ **Causa Provável** | 💡 **Solução** |
|--------------|-----------------------|----------------|
| ❌ **AccessDenied** | Permissão IAM insuficiente | Use o perfil do lab ou peça permissões `iam:ListPolicies` e `iam:GetPolicy*`. |
| ⚠️ **Unable to locate credentials** | Credenciais ausentes | Execute `aws configure` e insira suas chaves ou perfil do lab. |
| 🚫 **Access denied (publickey)** | Usuário incorreto | Use `ec2-user` (Amazon Linux) ou `ubuntu` (Ubuntu). |
| ⏳ **Connection timed out** | Instância ainda iniciando | Aguarde alguns segundos e tente novamente. |
