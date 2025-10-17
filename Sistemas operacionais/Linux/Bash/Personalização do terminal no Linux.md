---
tags:
  - terminal
  - bash
  - linux
  - personalização
  - validado
  - distros
---
## Retirar nome do usuário e nome da máquina do terminal
### 🐧 **Fedora**

```bash
# Adicionar prompt no final do .bashrc
echo "PS1='\W\$ '" >> ~/.bashrc
source ~/.bashrc
```

---

### 🟠 **Ubuntu / Debian / Mint**

```bash
# Adicionar prompt no final do .bashrc (igual ao Fedora)
echo "PS1='\W\$ '" >> ~/.bashrc
source ~/.bashrc
```

_(Ubuntu usa o mesmo esquema do Fedora: /etc/bash.bashrc é carregado antes do ~/.bashrc, então basta sobrescrever no final.)_

---

### 🟢 **OpenSUSE**

```bash
# Adicionar prompt no final do .bashrc do usuário
echo "PS1='\W\$ '" >> ~/.bashrc
source ~/.bashrc
```

_(OpenSUSE também carrega /etc/bash.bashrc, mas o ~/.bashrc do usuário tem prioridade no final.)_

---

### 🔵 **Arch Linux / Manjaro**

```bash
# Adicionar prompt no final do .bashrc
echo "PS1='\W\$ '" >> ~/.bashrc
source ~/.bashrc
```

---

### ⚙️ **Zsh (qualquer distro que use zsh como padrão)**

```bash
echo "PROMPT='%1~%# '" >> ~/.zshrc
source ~/.zshrc
```

---

### ✨ **Exemplo de prompt minimalista colorido (opcional)**

Pra deixar o diretório atual verde:

```bash
echo "PS1='\[\e[1;32m\]\W\[\e[0m\]\$ '" >> ~/.bashrc
source ~/.bashrc
```

---

🧠 Em resumo:  
Todos os sistemas baseados em Linux permitem sobrescrever o prompt adicionando `PS1='...'` no final do **~/.bashrc** (ou `PROMPT='...'` no **~/.zshrc**).

## Colocar hora no terminal, mostrar diretório atual e cor

### 💡 Resultado final:

Exemplo de como vai aparecer:

```
[14:52] ~/projetos$
```

---

### 🐧 **Para qualquer distro (Fedora, Ubuntu, openSUSE, Arch etc.)**

Execute:

```bash
echo "PS1='[\[\e[1;34m\]\A\[\e[0m\]] \[\e[1;32m\]\W\[\e[0m\]\$ '" >> ~/.bashrc
source ~/.bashrc
```

👉 Explicando rapidamente:

- `\A` → mostra **hora e minuto**
    
- `\W` → mostra **nome da pasta atual**
    
- `\e[1;34m` → **azul forte** pra hora
    
- `\e[1;32m` → **verde forte** pro diretório
    
- `\[\e[0m\]` → reseta a cor
    
- `$` → símbolo final do prompt
    

---

### ⚙️ **Se estiver usando Zsh**

```bash
echo "PROMPT='[%F{blue}%*%f] %F{green}%1~%f %# '" >> ~/.zshrc
source ~/.zshrc
```

Mesmo estilo, mas usando a sintaxe do Zsh.