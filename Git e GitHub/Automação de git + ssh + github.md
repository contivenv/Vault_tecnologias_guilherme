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