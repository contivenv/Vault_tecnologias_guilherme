---
tags:
  - git
  - github
  - programação
---
Para criar uma chave SSH para usar com o GitHub, você precisa gerar um par de chaves (uma pública e uma privada) no seu computador, adicionar a chave pública à sua conta do GitHub e configurar o agente SSH na sua máquina.
### 1. Gerar uma Nova Chave SSH 🔑

Abra o seu terminal (ou Git Bash no Windows) e execute o comando abaixo. Substitua `"seu_email@exemplo.com"` pelo e-mail associado à sua conta do GitHub.

O método **Ed25519** é o mais recomendado atualmente por ser mais seguro e performático.

```bash
ssh-keygen -t ed25519 -C "seu_email@exemplo.com"
```

Durante o processo, você verá algumas perguntas:

1. **`Enter a file in which to save the key`**: Pressione **Enter** para aceitar o local padrão (`/home/seu_usuario/.ssh/id_ed25519` ou similar).
2. **`Enter passphrase (empty for no passphrase)`**: Digite uma senha segura e pressione **Enter**. Essa senha (passphrase) protege sua chave privada. Você precisará digitá-la sempre que usar a chave, a menos que a adicione ao agente SSH (próximo passo). Recomendo fortemente que você crie uma.

---

### 2. Adicionar a Chave ao Agente SSH 💻

O agente SSH gerencia suas chaves e evita que você precise digitar sua senha (passphrase) a todo momento.

1. Inicie o agente SSH em segundo plano:
    
    
```bash
eval "$(ssh-agent -s)"
```
    
2. Adicione sua nova chave privada ao agente. Se você criou a chave com um nome ou local diferente, ajuste o caminho do arquivo.
    
```bash
ssh-add ~/.ssh/id_ed25519
```
    
⚠️ Você precisará digitar a senha (passphrase) que criou no passo anterior.
    

---

###  3. Copiar a Chave Pública

Agora, você precisa copiar o conteúdo da sua **chave pública** (o arquivo com final `.pub`) para a área de transferência.

Use o comando correspondente ao seu sistema operacional:

- **macOS:**
    
    
    ```bash
    pbcopy < ~/.ssh/id_ed25519.pub
    ```
    
- **Windows (Git Bash):**
    
    
    ```bash
    cat ~/.ssh/id_ed25519.pub | clip
    ```
    
- **Linux (requer `xclip`):**
    
    
    ```bash
    sudo apt-get install xclip -y # Se não tiver o xclip
    xclip -selection clipboard < ~/.ssh/id_ed25519.pub
    ```
    

Se nenhum desses comandos funcionar, você pode abrir o arquivo manualmente com `cat ~/.ssh/id_ed25519.pub`, selecionar todo o texto (de `ssh-ed25519` até o seu e-mail) e copiar (Ctrl+C ou Cmd+C).

---

###  4. Adicionar a Chave Pública ao GitHub 🚀

1. Acesse o [GitHub](https://github.com/) e faça login.
2. Clique na sua foto de perfil no canto superior direito e vá para **Settings** (Configurações).
3. No menu esquerdo, clique em **SSH and GPG keys**.
4. Clique no botão verde **New SSH key**.
5. Dê um **Title** (Título) para a sua chave, como "Meu Notebook Pessoal" ou "PC do Trabalho".
6. Cole a chave pública que você copiou no campo **Key**.
7. Clique em **Add SSH key**.

---

### 5. Testar a Conexão ✔️

Para verificar se tudo funcionou, execute o seguinte comando no seu terminal:

Bash

```bash
ssh -T git@github.com
```

Você pode ver um aviso como este na primeira vez:

```bash
The authenticity of host 'github.com (IP ADDRESS)' can't be established.
...
Are you sure you want to continue connecting (yes/no)?
```

Digite **yes** e pressione **Enter**.

Se tudo estiver correto, você receberá uma mensagem de sucesso:

> Hi _seu-username_! You've successfully authenticated, but GitHub does not provide shell access.

Pronto! Sua máquina agora está configurada para se comunicar de forma segura com o GitHub usando SSH.