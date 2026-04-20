---
tags:
  - git
  - github
  - automação
  - script
data criação: 2026-03-27T21:58:00
situação: validado
---
Para um ambiente técnico e focado em automação de infraestrutura (IaC), a melhor abordagem é utilizar scripts idempotentes (que podem ser executados várias vezes sem causar erros) e a CLI oficial do GitHub para manipulação de chaves via API.

---

# Documentação Técnica: Autenticação Estruturada GitHub (SSH + CLI)

Esta documentação descreve o procedimento de provisionamento de credenciais Git em ambientes Linux baseados em múltiplas distribuições, utilizando o protocolo **Ed25519** para criptografia de chave pública e a **GitHub CLI (`gh`)** para gerenciamento de identidade.
## Script Universal (Debian/Mint e Fedora)

```bash
#!/bin/bash

# --- CONFIGURAÇÕES FIXAS ---
EMAIL="guilhermect@tutanota.com"
NOME="Guilherme Conti Teixeira"
KEY_PATH="$HOME/.ssh/id_ed25519"

echo "[INFO] Iniciando provisionamento do ambiente (Debian/Fedora)..."

# 1. Detecção de SO e Instalação de Dependências
if command -v apt &> /dev/null; then
    echo "[INFO] Gerenciador de pacotes APT detectado (Base Debian/Mint/Ubuntu)."
    echo "[INFO] Atualizando repositórios e instalando dependências..."
    sudo apt update
    sudo apt install -y git gh
elif command -v dnf &> /dev/null; then
    echo "[INFO] Gerenciador de pacotes DNF detectado (Base Fedora/RHEL)."
    echo "[INFO] Instalando dependências..."
    sudo dnf install -y git gh
else
    echo "[ERROR] Sistema operacional não suportado por este script (apt/dnf não encontrados)."
    echo "[ERROR] Instale o git e o gh manualmente e execute o script novamente."
    exit 1
fi

# 2. Configuração de Identidade Global
echo "[INFO] Configurando credenciais locais do Git..."
git config --global user.name "$NOME"
git config --global user.email "$EMAIL"

# 3. Geração de Par de Chaves SSH Ed25519
if [ ! -f "$KEY_PATH" ]; then
    echo "[INFO] Gerando chave SSH Ed25519..."
    ssh-keygen -t ed25519 -C "$EMAIL" -f "$KEY_PATH" -N "" 
else
    echo "[WARN] Chave SSH já detectada no sistema."
fi

# 4. Agente SSH e Persistência
echo "[INFO] Inicializando agente SSH..."
eval "$(ssh-agent -s)"
ssh-add "$KEY_PATH"

# 5. Autenticação GitHub CLI (Fluxo de Terminal)
echo "[INFO] Iniciando autenticação."
echo "[INSTRUÇÃO] Siga os passos no terminal e utilize o Device Code gerado."
echo "------------------------------------------------------"
if command -v gh &> /dev/null; then
    gh auth login -p ssh
else
    echo "[ERROR] Falha ao detectar a GitHub CLI pós-instalação."
    exit 1
fi

# 6. Definição Manual de Caminho e Clonagem
echo "------------------------------------------------------"
read -p "[PROMPT] Insira o caminho absoluto para salvar o Vault (ex: /home/usuario/Estudos): " TARGET_PATH

# Expande o til (~) para o caminho absoluto da home do usuário atual
TARGET_PATH="${TARGET_PATH/#\~/$HOME}"

if [ -d "$TARGET_PATH" ]; then
    echo "[INFO] Diretório existente detectado. Acessando..."
else
    echo "[INFO] Criando novo diretório: $TARGET_PATH"
    mkdir -p "$TARGET_PATH"
fi

cd "$TARGET_PATH" || exit
echo "[INFO] Clonando repositório em: $(pwd)"
git clone git@github.com:contivenv/CompTIA-Security-.git .

echo "[SUCCESS] Ambiente provisionado com sucesso."
```

Obs: para acessar a primeira versão, clique [[aqui]]

---
### Detalhes da Estrutura de Detecção

A lógica utilizada no Bloco 1 (`command -v apt` e `command -v dnf`) é a abordagem mais segura em scripts Bash. Em vez de ler arquivos de texto do sistema (como o `/etc/os-release`, que pode ter variações de sintaxe dependendo da distro derivada), o script verifica diretamente se o binário do gerenciador de pacotes existe na variável de ambiente `$PATH`.

Isso garante que, independentemente da variação específica da distribuição que você estiver testando, desde que ela utilize os repositórios padrões APT ou DNF, as ferramentas essenciais de versionamento serão instaladas sem interromper a execução da automação.