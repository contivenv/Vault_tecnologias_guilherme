#!/bin/bash

# --- CONFIGURAÇÕES FIXAS ---
EMAIL="guilhermect@tutanota.com"
NOME="Guilherme Conti Teixeira"
KEY_PATH="$HOME/.ssh/id_ed25519"

echo "[INFO] Iniciando provisionamento do ambiente (Debian 13)..."

# 1. Verificação e Instalação de Dependências
if ! command -v apt &> /dev/null; then
    echo "[ERROR] Gerenciador de pacotes APT não encontrado."
    echo "[ERROR] Este script foi configurado para rodar exclusivamente no Debian."
    exit 1
fi

echo "[INFO] Atualizando repositórios e instalando dependências (git e gh)..."
sudo apt update
sudo apt install -y git gh

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

cd "$TARGET_PATH" || { echo "[ERROR] Falha ao acessar o diretório $TARGET_PATH"; exit 1; }

echo "[INFO] Clonando repositório em: $(pwd)"
git clone git@github.com:contivenv/CompTIA-Security-.git .

echo "[SUCCESS] Ambiente provisionado com sucesso."
