```bash
#!/bin/bash

# --- CONFIGURAÇÕES ---
EMAIL="guilhermect@tutanota.com"
NOME="Guilherme Conti Teixeira"
KEY_PATH="$HOME/.ssh/id_ed25519"

echo " iniciando configuração do ambiente Git..."

# 1. Configurar Git Global
git config --global user.name "$NOME"
git config --global user.email "$EMAIL"

# 2. Gerar chave SSH (se não existir)
if [ ! -f "$KEY_PATH" ]; then
    echo "Gerando nova chave SSH..."
    ssh-keygen -t ed25519 -C "$EMAIL" -f "$KEY_PATH" -N "" 
else
    echo "Chave SSH já existe."
fi

# 3. Iniciar SSH Agent e adicionar chave
eval "$(ssh-agent -s)"
ssh-add "$KEY_PATH"

# 4. Upload para o GitHub via CLI (automação)
if command -v gh &> /dev/null; then
    echo "GitHub CLI detectada. Autenticando..."
    gh auth login -w -p ssh
else
    echo "GitHub CLI (gh) não instalada."
    echo "Copie sua chave pública abaixo e cole em: https://github.com/settings/keys"
    cat "${KEY_PATH}.pub"
fi

# 5. Clonar o Vault (Opcional)
read -p "Deseja clonar o Vault agora? (s/n): " choice
if [ "$choice" == "s" ]; then
    mkdir -p ~/Documents/Obsidian
    cd ~/Documents/Obsidian
    git@github.com:contivenv/CompTIA-Security-.git
fi

echo "Processo finalizado"
```