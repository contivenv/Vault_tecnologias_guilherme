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

Abaixo está a documentação técnica e o script refinado.

---

# Documentação Técnica: Autenticação Estruturada GitHub (SSH + CLI)

Esta documentação descreve o procedimento de provisionamento de credenciais Git em ambientes Linux baseados em múltiplas distribuições, utilizando o protocolo **Ed25519** para criptografia de chave pública e a **GitHub CLI (`gh`)** para gerenciamento de identidade.

## 1. Requisitos de Sistema

A ferramenta `gh` é essencial para evitar a interação manual com a interface web do GitHub.

### Instalação por Gerenciador de Pacotes

|**Distribuição**|**Comando**|
|---|---|
|**Debian/Ubuntu/Pop!_OS**|`sudo apt update && sudo apt install gh git -y`|
|**Fedora/RHEL**|`sudo dnf install gh git -y`|
|**Arch Linux**|`sudo pacman -S github-cli git --noconfirm`|
|**openSUSE**|`sudo zypper install github-cli git`|

---

## 2. Script de Automação de Ambiente (`setup-env.sh`)

Este script realiza a configuração do escopo global do Git, geração de par de chaves assíncronas e vinculação automática ao perfil do GitHub via socket seguro.

Bash

```bash
# --- CONFIGURAÇÕES ---
# Definição de variáveis globais para identidade e caminhos de sistema
EMAIL="guilhermect@tutanota.com"
NOME="Guilherme Conti Teixeira"
KEY_PATH="$HOME/.ssh/id_ed25519"

echo " iniciando configuração do ambiente Git..."

# 1. Configurar Identidade Global do Git
# Define quem é o autor dos commits realizados nesta máquina
git config --global user.name "$NOME"
git config --global user.email "$EMAIL"

# 2. Gerar chave SSH (se não existir)
# Utiliza o algoritmo Ed25519 (Curva Elíptica), superior ao RSA em segurança e performance
if [ ! -f "$KEY_PATH" ]; then
    echo "Gerando nova chave SSH..."
    # -N "" define uma passphrase vazia para evitar prompts manuais durante operações git
    ssh-keygen -t ed25519 -C "$EMAIL" -f "$KEY_PATH" -N "" 
else
    echo "Chave SSH já existe."
fi

# 3. Iniciar SSH Agent e adicionar chave à memória
# O eval garante que o agente SSH esteja rodando no shell atual
eval "$(ssh-agent -s)"
ssh-add "$KEY_PATH"

# 4. Integração com GitHub via CLI
# Se a CLI 'gh' estiver presente, o upload da chave pública é automatizado via API
if command -v gh &> /dev/null; then
    echo "GitHub CLI detectada. Autenticando..."
    # -p ssh força o protocolo SSH; -w abre o navegador para aprovação de token
    gh auth login -w -p ssh
else
    echo "GitHub CLI (gh) não instalada."
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

echo "Processo finalizado"
```

Obs: para acessar a primeira versão, clique [[aqui]]

---

## 3. Fluxo de Operação Técnico

Ao executar o script em uma nova distribuição, o processo segue a seguinte lógica:

1. **Idempotência:** O script verifica a existência de chaves prévias para evitar sobrescrita de acessos antigos.
    
2. **Segurança de Algoritmo:** Utiliza-se **Ed25519** em detrimento do RSA legatário, oferecendo chaves menores e maior resistência a ataques de força bruta.
    
3. **Abstração de API:** O comando `gh auth login -p ssh` executa internamente o `POST /user/keys` na API do GitHub, enviando o conteúdo de `id_ed25519.pub` sem intervenção manual de "copiar e colar".
    
4. **Persistência de Sessão:** O agente SSH é invocado para manter a chave em memória, evitando prompts de senha durante operações de `git push` ou `git pull` no Vault do Obsidian.
    

---

## 4. Troubleshooting e Permissões

Caso ocorram erros de permissão após a migração de arquivos de backup de chaves, aplique o endurecimento de diretório:

Bash

```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

[[Edição Void Linux para teste]] 