---
tags:
  - git
---
Você pode aprender mais na prática testando seu conhecimento em git na nota com os questionários do [[Guia de estudo Pro Git]]

Substitua `"nome_do_editor"` pelo nome do seu editor, como `vim`, `nano` ou `atom`.

- Para criar aliases (atalhos) para comandos Git longos, use o comando: Substitua `"atalho"` pelo nome curto que você quer usar e `"comando_git"` pelo comando completo. Por exemplo: criará o alias `cb` para o comando `checkout -b`.
```bash
git config --global alias.atalho "comando_git"
git config --global alias.cb "checkout -b"    
```
## `git clone`

Este comando é usado para copiar um repositório Git existente, seja de uma rede ou de outro lugar.

- Para clonar um repositório, use:
```bash
git clone <url_do_repositório>
```
- Você pode especificar um nome de diretório diferente para o repositório clonado usando:
```bash
git clone <url_do_repositório> <nome_do_diretório>
```
## `git add`

Este comando adiciona conteúdo de arquivos da sua área de trabalho para o _staging area_ (área de preparação), para serem incluídos no próximo _commit_.
- Para adicionar um único arquivo:
```bash
git add <arquivo> 
```
- Para adicionar todos os arquivos modificados:
```bash
git add . 
```
## `git status`

Este comando mostra o estado dos arquivos no seu diretório de trabalho, incluindo quais arquivos foram modificados, adicionados ou excluídos, e quais estão no _staging area_.
## `git commit`

Este comando tira um _snapshot_ de todos os arquivos no _staging area_ e o salva permanentemente no histórico do repositório.
- Para fazer um commit com uma mensagem, use:
```bash
git commit -m "Mensagem do commit"
```
- Para fazer o commit de todos os arquivos modificados sem adicioná-los ao _staging area_, use:
```bash
git commit -a -m "Mensagem do commit" 
```
## `git log`

Este comando mostra o histórico de _commits_ do repositório, incluindo mensagens, autores e datas.
- Para ver o histórico completo:
```bash
git log
```
- Para ver o histórico resumido:
```bash
git log --oneline
```
- Para ver o histórico com informações detalhadas sobre cada commit:
```bash
git log -p
```
## `git branch`
Este comando lista, cria e exclui _branches_ (ramos).
- Para listar todas as branches:
```bash
git branch
```
- Para criar uma nova branch:
```bash
git branch <nome_da_branch>
```
- Para excluir uma branch:
```bash
git branch -d <nome_da_branch>
```
## `git checkout`

Este comando é usado para alternar entre _branches_.
- Para alternar para uma branch existente:
```bash
git checkout <nome_da_branch>
```
- Para criar e alternar para uma nova branch:
```bash
git checkout -b <nome_da_branch>
```
## `git merge`

Este comando combina o histórico de duas _branches_.
- Para mesclar uma branch na branch atual:
```bash
git merge <nome_da_branch>
```
## `git push`
Este comando envia seus _commits_ locais para um repositório remoto.
- Para enviar a branch atual para o repositório remoto chamado "origin":
```bash
git push origin <nome_da_branch> 
```
## `git pull`
Este comando busca e mescla alterações de um repositório remoto para o seu repositório local.
- Para buscar e mesclar alterações do repositório remoto chamado "origin" para a branch atual:
```bash
git pull origin <nome_da_branch> 
```
## `git remote`

Este comando é uma ferramenta de gerenciamento para seu registro de repositórios remotos. Ele permite salvar URLs longas como apelidos curtos, como "origin", para que você não precise digitá-las o tempo todo.

- Para listar os repositórios remotos:
```bash
git remote -v
```
- Para adicionar um repositório remoto:
```bash
git remote add <nome> <url> 
```
## `git diff`

Este comando mostra as diferenças entre os arquivos no seu diretório de trabalho, _staging area_ e _commits_ anteriores.

- Para ver as diferenças entre o diretório de trabalho e o _staging area_:
```bash
git diff
```
- Para ver as diferenças entre o _staging area_ e o último commit:
```bash
git diff --staged
```
- Para ver as diferenças entre dois commits:
```bash
git diff <commit_1> <commit_2>
```