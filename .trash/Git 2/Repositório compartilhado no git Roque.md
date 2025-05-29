---
tags:
  - git
  - repositório
---
##### Resumo desse artigo
Está sendo criando um repositório compartilhado para que nós, equipe de TI da Roque possamos usar o git daqui pra frente para fazer algumas alterações em nossos novos sistemas e teste de versionamento para implementação de coisas novas no nosso servidor Ubuntu. A maneira que estou fazendo vai ser a seguinte:
##### 1. Repositório compartilhada
Crie uma pasta para o repositório
```bash
sudo mkdir -p /srv/git/repositorio-roque.git
```
Navegue até pasta criada
```bash
cd /srv/git/repositorio-roque.git
```
Compartilhe o repositório
```bash
sudo git init --bare
```
##### 2. Usuário e permissões
Adicionamos o usuário para ter acesso ao repositório.
```bash
sudo chowd -R guilherme:guilherme /srv/git/repositorio-roque.git
```
Conceder as permissões necessárias para o mesmo acessar o repositório.
```bash
sudo chmod -R 755 /srv/git/repositorio-roque.git
```
##### 3. Clonagem do repositório
No servidor ou em qualquer máquina que tenha acesso ao servidor, você pode clonar o repositório com o comando. Porém, você precisa primeiro passar o comando para que ele identifique o repositório como confiável e seguro.
```bash
git config --global --add safe.directory /srv/git/repositorio-roque.git
```
Comando de clonagem do repositório:
```bash
git clone guilherme@192.168.2.94:/srv/git/repositorio-roque.git
```
##### 4. Etapas do fluxo de trabalho:
Fazer alterações: Editar arquivos localmente.

Adicionar arquivos ao índice:
```bash
 git add arquivo_modificado.txt
```
Commit das alterações:
```bash
git commit -m "Descrição das alterações"
```
Push para o repositório central:
```bash
git push origin main
 ```
Puxar alterações de outros desenvolvedores:
```bash
 git pull origin main
```
