🌩️ Instalar e Configurar a AWS CLI

Repositório criado como parte do laboratório de AWS para demonstrar:

✅ Acesso a uma instância EC2 (Amazon Linux 2) via PuTTY
✅ Verificação e uso da AWS CLI v2
✅ Execução do desafio de política IAM (lab_policy) — tudo pela linha de comando!

🧩 Objetivo

Aprender na prática como:

Conectar-se a uma instância EC2 de forma segura usando chave privada (.ppk);

Configurar e validar a AWS CLI;

Executar comandos IAM via terminal, sem usar o Console da AWS;

Baixar o documento JSON da política lab_policy.

🖥️ Acesso à Instância EC2

Baixe a chave .ppk no painel do laboratório.

Copie o IP público (exemplo: 35.161.200.130).

Abra o PuTTY e configure:

Host Name: ec2-user@<SEU_IP>

Port: 22

Connection type: SSH

Vá em Connection → SSH → Auth → Credentials

Em Private key file for authentication, selecione sua chave .ppk.

Clique em Open → aceite o aviso de segurança.

Pronto! Você verá a tela de boas-vindas do Amazon Linux 🎉

💡 Dica: use o usuário ec2-user (Amazon Linux) ou ubuntu (caso use AMI Ubuntu).

⚙️ Validando a AWS CLI

Para confirmar que a CLI está instalada:

aws --version


Saída esperada:

aws-cli/2.x Python/3.x Linux/...

🧠 Desafio: Baixar o JSON da Política lab_policy

O desafio consiste em encontrar e baixar o documento da política IAM lab_policy usando apenas comandos AWS CLI.

Passos resumidos

1️⃣ Listar políticas gerenciadas localmente:

aws iam list-policies --scope Local


2️⃣ Pegar o ARN da política lab_policy.

3️⃣ Obter a versão padrão:

aws iam get-policy --policy-arn <ARN>


4️⃣ Baixar o JSON da política:

aws iam get-policy-version \
  --policy-arn <ARN> \
  --version-id <VERSAO> \
  --query "PolicyVersion.Document" \
  --output json > lab_policy.json


5️⃣ Verificar o arquivo:

cat lab_policy.json

🪄 Script Automatizado (opcional)

Você pode salvar esse script em scripts/get_lab_policy.sh e executá-lo:

bash scripts/get_lab_policy.sh


Ele faz tudo automaticamente e gera o arquivo lab_policy.json.

🧰 Solução de Problemas
Erro	Causa provável	Solução
❌ AccessDenied	Sem permissão no IAM	Use as credenciais do lab
⚠️ Unable to locate credentials	CLI não configurada	Execute aws configure
🚫 Access denied (publickey)	Usuário incorreto	Use ec2-user ou ubuntu
⏳ Connection timed out	Instância iniciando	Aguarde o boot completo
🗂️ Estrutura do Projeto
Instalar-e-configurar-a-CLI-da-AWS/
├── README.md
├── LICENSE
├── .gitignore
├── docs/
│   └── comandos_basicos.md
├── scripts/
│   └── get_lab_policy.sh
└── imagens/
    ├── ec2-ssh-bemvindo.png
    ├── ec2-console-instancias.png
    └── lab-painel.png

☁️ Publicação no GitHub

Crie o repositório Instalar-e-configurar-a-CLI-da-AWS no seu GitHub.

Faça upload destes arquivos manualmente (via “Add file → Upload files”).

Clique em Commit changes e pronto 🎉
