---
tags:
  - fastfetch
  - inicialização_de_programas
  - bash
  - terminal
  - personalização
  - configurações
---
### 1. O que é o Fastfetch ?

O Fastfetch é uma ferramenta semelhante ao [neofetch](https://github.com/dylanaraps/neofetch) para buscar informações do sistema e exibi-las de maneira visualmente atraente. É escrito principalmente em C, com foco no desempenho e na customização. Atualmente, ele suporta Linux, macOS, Windows 7+, Android, FreeBSD, OpenBSD, NetBSD, DragonFly, Haiku e SunOS.

### 2. Instalação

Eu poderia descrever todo o processo aqui para cada distribuição, mas seria trabalho demais sendo que a documentação oficial é muito rica, detalhada e de fácil interpretação. Então deixarei o link [oficial](https://github.com/fastfetch-cli/fastfetch) deles aqui para mais consulta se for necessário.

### 3. Inicialização automática no termianal do Fastfetch

1. Para saber qual shell você usa, primeiro acesse o terminal com o atalho `Ctrl + Alt + T` ou apanas `Terminal` na barra de pesquisa e digite `echo $SHELL`. Ele ira exibir na linha seguinte algo como:

```bash
echo $SHELL
/usr/bin/bash
```

2. Use o comando `nano ${HOME}/.bashrc` para editar o arquivo do seu shell. No meu caso eu estou usando bash, então pra mim é .bash.
3. Após abrir, adicione na última linha de comando o nome `fastfetch`.
4. Salve o arquivo que acabou de edita com o comando `Ctrl + O` se estiver usando `nano`.
5. A inicialização automática irá ocorrer agora.![[Pasted image 20251125095415.png]]

### **Bônus**

Em abientes somente com terminal (CLI), execute os seguintes comandos para que ele seja exibido a cada limpeza de terminal com o coamndo Ctrl + L:

**1.Instalar o fastfetch: Pule este passo se o pacote já estiver instalado.


```bash
sudo apt update && sudo apt install fastfetch -y
```

**2.Editar o arquivo de configuração do Bash:**

Abra o arquivo `.bashrc` usando o editor de texto `nano`:

```bash
nano ~/.bashrc
```

**3.Adicionar os comandos de automação:**

Navegue até a última linha do arquivo (usando a seta para baixo) e adicione o seguinte bloco:

```bash
# (Opcional) Executa o fastfetch assim que você fizer login na máquina
fastfetch

# Executa o fastfetch ao digitar o comando 'clear'
alias clear='command clear && fastfetch'

# Executa o fastfetch ao usar o atalho do teclado Ctrl+L
bind -x '"\C-l": command clear; fastfetch'
```

**4.Salvar o arquivo e fechar o editor:**

Pressione `Ctrl+O` e depois `Enter` para salvar as alterações. Em seguida, pressione `Ctrl+X` para sair do `nano`.

**5.Aplicar as alterações:**

Recarregue as configurações do Bash para que os atalhos e comandos funcionem imediatamente, sem precisar reiniciar a máquina virtual:

```bash
source ~/.bashrc
```

### **Fontes**

**Fastfetch**; acessado na data de 25/11/2025 em https://github.com/fastfetch-cli/fastfetch

**Como fazer o fastfetch iniciar automaticamente?**; acessado na data de 25/11/2025 em https://plus.diolinux.com.br/t/como-fazer-o-fastfetch-iniciar-automaticamente/68993



