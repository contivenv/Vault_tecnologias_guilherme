---
tags:
  - samba
  - smb
  - linux
  - servidores
  - arquivos
  - acesso
  - credenciais
  - smb1
  - smb2
  - smb3
  - thunar
  - xfce
  - bash
  - terminal
  - GVFS
data criação: 2025-12-05T08:40:00
---
Basicamente, eu estava tendo problemas para me conectar ao servidor do trabalho onde o IP de acesso do servidor de arquivos é 192.168.2.3. Com os números do servidor isso não funcionava de jeito nenhum, então resolvi testar com o nome dele, uma alternativa que funcionou comigo em diversas vezes quando estava tentando acessar um servidor na rede com o Windows. Funcionou da maneira que eu esperava, uma tela de autenticação aparece falando para que você preencha nome, domínio e sua senha para acessar ele. Eu fiquei curioso pra saber como é feita essa negociação do GVFS no linux com o thunar no xfce. Eu pedi uma explicação no ChatGPT e essa foi a resposta que ele me deu. Eu busquei explicações e fóruns falando sobre o GVFS e o que mais fez sentido pra mim em termos de explicação, foi o fórum do [Unix & Linux falando sobre o GVFS](https://unix.stackexchange.com/questions/38109/what-is-gvfs-and-why-should-i-want-it-on-my-system).

---
# 🧠 **Por que `smb://thor` funciona, mas `smb://192.168.2.3/` falha?**

A mágica está em **como o GVFS (o backend de SMB do Linux Mint Xfce)** negocia autenticação e protocolos com o servidor.

Vamos por partes, igual um Snowden desmontando um pacote SMB:

---

# 🟥 1. Quando você usa **o IP**

`gvfs-smb` pensa:

> “Ah, é só um servidor SMB qualquer… deixa eu tentar listar tudo SEM autenticar.”

E ele manda esse pedido:

```
SMB: session setup anônimo
SMB: tree connect anônimo
SMB: list shares
```

Só que seu servidor (provavelmente um Windows Server) responde:

```
Access Denied, anonymous boy!
```

E o GVFS Xfce **não tenta autenticar de novo**, ele simplesmente morre com:

**"Falha ao recuperar a lista do servidor: Argumento inválido."**

Tradução:  
“Não sei lidar com a negativa do servidor. Boa sorte aí.”

---

# 🟩 2. Quando você usa **o nome `smb://thor`**

Agora o GVFS pensa:

> “Opa, isso aqui parece um servidor real do domínio…”

E aí muda **todo o comportamento**:

### ✔ Ele tenta resolução DNS (ou WINS/NetBIOS)

Isso ativa uma flag interna dizendo que é um _host SMB corporativo_.

### ✔ Ele tenta autenticar _antes_ de listar

Ele automaticamente usa:

- Kerberos (se houver ticket)
    
- NTLMSSP (se não houver ticket)
    
- Seu nome de usuário já logado na sessão de domínio
    

Ou seja, ele faz:

```
SMB: session setup com credencial do usuário
SMB: tree connect com credencial
SMB: list shares autenticado
```

O servidor responde:

```
Tá logado? Beleza, toma a lista.
```

E é por isso que aparece tudo na GUI.

---

# 🔥 3. A cereja do bolo: quando usa IP, **SMB1 não entra no jogo**

Listar compartilhamentos via IP **exigia SMB1 por 20 anos**.

As distros modernas:

- Desativaram SMB1 (segurança)
    
- Mas o GVFS ainda depende dele pra listar via IP
    
- Então… ele simplesmente se lasca
    

Mas quando você usa **o nome do servidor**, o GVFS negocia SMB2/SMB3 corretamente.

---

# 🧨 4. Conclusão nerd definitiva

|Forma|O que acontece|Resultado|
|---|---|---|
|`smb://192.168.2.3/`|Tentativa anônima → listagem negada → GVFS pifa|❌ Falha|
|`smb://thor/`|Autenticação automática → listagem liberada|✔️ Funciona|

Você basicamente enganou o GVFS e fez ele se comportar como um adulto responsável.

---