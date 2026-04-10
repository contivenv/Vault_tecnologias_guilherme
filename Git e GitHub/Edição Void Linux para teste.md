```shell
#!/bin/bash

# --- CONFIGURAÇÕES ---
# Definição de variáveis globais para identidade e caminhos de sistema
EMAIL="guilhermect@tutanota.com"
NOME="Guilherme Conti Teixeira"
KEY_PATH="$HOME/.ssh/id_ed25519"

echo "[INFO] Iniciando configuração do ambiente Git no Void Linux..."

# 0. Instalar dependências via XBPS (Específico para Void Linux)
if ! command -v git &> /dev/null || ! command -v gh &> /dev/null; then
    echo "[INFO] Dependências ausentes. Instalando git e github-cli via xbps..."
    # Atualiza os repositórios (-S) e instala sem perguntar (-y)
    sudo xbps-install -Sy git github-cli
else
    echo "[INFO] Dependências (git e gh) já estão instaladas."
fi

# 1. Configurar Identidade Global do Git
# Define quem é o autor dos commits realizados nesta máquina
git config --global user.name "$NOME"
git config --global user.email "$EMAIL"

# 2. Gerar chave SSH (se não existir)
# Utiliza o algoritmo Ed25519 (Curva Elíptica), superior ao RSA em segurança e performance
if [ ! -f "$KEY_PATH" ]; then
    echo "[INFO] Gerando nova chave SSH..."
    # -N "" define uma passphrase vazia para evitar prompts manuais durante operações git
    ssh-keygen -t ed25519 -C "$EMAIL" -f "$KEY_PATH" -N "" 
else
    echo "[WARN] Chave SSH já existe em $KEY_PATH."
fi

# 3. Iniciar SSH Agent e adicionar chave à memória
# O eval garante que o agente SSH esteja rodando no shell atual
eval "$(ssh-agent -s)"
ssh-add "$KEY_PATH"

# 4. Integração com GitHub via CLI
# Se a CLI 'gh' estiver presente, o upload da chave pública é automatizado via API
if command -v gh &> /dev/null; then
    echo "[INFO] GitHub CLI detectada. Autenticando..."
    # -p ssh força o protocolo SSH; -w abre o navegador para aprovação de token
    gh auth login -w -p ssh
else
    echo "[ERROR] GitHub CLI (gh) não instalada."
    echo "Copie sua chave pública abaixo e cole em: https://github.com/settings/keys"
    cat "${KEY_PATH}.pub"
fi

# 5. Clonagem de Repositório (Vault de Estudos CompTIA Security+)
read -p "Deseja clonar o Vault agora? (s/n): " choice
if [ "$choice" == "s" ]; then
    mkdir -p ~/Documents/Obsidian
    cd ~/Documents/Obsidian
    # Clonagem via SSH para evitar solicitações de usuário/senha
    git clone git@github.com:contivenv/CompTIA-Security-.git
fi

echo "[SUCCESS] Processo finalizado"
```