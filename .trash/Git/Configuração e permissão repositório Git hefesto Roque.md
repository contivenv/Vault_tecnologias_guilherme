---
tags:
  - git
  - servidor
  - linux
  - ubuntu_server
---
##### Configuração do Servidor Git - resumo
O servidor que hospedará o repositório Git chamado **hefestoroqueimoveis** e tem o IP **192.168.2.94**. Ele está rodando em uma máquina virtual (VM) no servidor físico com o IP **192.168.2.2** e usa o **Ubuntu Server** como sistema operacional. Para criar um repositório Git público em um servidor Ubuntu, você pode seguir estes passos. Esse repositório será acessível para qualquer usuário que tenha acesso ao servidor e permissões de leitura no diretório do repositório.

Antes de tudo, use para instalar o Git no Ubuntu Server, use o seguinte comando:
```bash
sudo apt-get update
sudo apt-get install git
```
##### 1. Acesse o servidor e crie uma pasta para o repositório
```bash
ssh usuario@192.168.2.94
sudo mkdir -p /srv/git/meu-repositorio.git
```
##### 2. Inicialize o repositório como um “bare repository”
Repositórios bare são usados para compartilhamento, pois eles não contêm arquivos de trabalho (workspace), apenas o conteúdo do repositório.
```bash
cd /srv/git/meu-repositorio.git
sudo git init --bare
```
##### 3. Ajuste as permissões:
Se você quer permitir que outros usuários no servidor tenham acesso de leitura e escrita, ajuste as permissões adequadamente.
```bash
sudo chown -R gituser:gitgroup /srv/git/meu-repositorio.git
sudo chmod -R 755 /srv/git/meu-repositorio.git
```
**Observação:** substitua gituser e gitgroup pelo usuário e grupo que os outros usuários fazem parte. Isso pode facilitar o controle de acesso.

Para acessar o repositório Git no servidor a partir da sua máquina, você pode usar o SSH, assumindo que você já tem as credenciais de acesso ao servidor e a chave SSH configurada (ou usando autenticação com senha). Vou detalhar os passos:
##### 4. Clone o repositório no servidor localmente para usá-lo
No servidor ou em qualquer máquina que tenha acesso ao servidor, você pode clonar o repositório com o comando:
```bash
git clone usuario@192.168.2.94:/srv/git/meu-repositorio.git
```