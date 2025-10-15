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


## 💼 Solução de Problemas

| 💬 **Erro** | ⚙️ **Causa Provável** | 💡 **Solução** |
|--------------|------------------------|----------------|
| ❌ **AccessDenied** | Permissão IAM insuficiente | Use o perfil do lab ou peça permissões `iam:ListPolicies` e `iam:GetPolicy*`. |
| ⚠️ **Unable to locate credentials** | Credenciais ausentes | Execute `aws configure` e insira suas chaves ou o perfil do lab. |
| 🚫 **Access denied (publickey)** | Usuário incorreto | Use `ec2-user` (Amazon Linux) ou `ubuntu` (Ubuntu). |
| ⏳ **Connection timed out** | Instância ainda iniciando | Aguarde alguns segundos e tente novamente. |
