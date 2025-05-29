---
tags:
  - git
  - aprendizado
---
##### Resumo
A intenção aqui é aprendermos como usar git registrando o que estamos fazendo desde o começo até o final do processo. O que cada comando significa e por que estamos usando cada um deles.

##### 1. Criação da pasta
Crie uma pasta na sua máquina local para armazenar os arquivos de teste que vamos usar daqui pra frente.
![[Pasted image 20250122111645.png]]
##### 2. Baixe o Git
Acesse o site oficial do [git](https://git-scm.com/downloads), baixe o arquivo e faça a instalação na sua máquina.
##### 3. Acessando o Git Bash
Após a instalação do Git, acesse o aplicativo que é chamado de Git Bash na sua máquina para fazer a listagem do comando dos mesmos que precisamos logo a seguir.
![[Pasted image 20250122112125.png]]

##### 4. Acessando a pasta dentro do Git Bash
Dentro do Git Bash vamos adicionar os seguintes comando para acessar a pasta dentro da nossa máquina:
```bash
cd Documents\teste_git
```
**Observação:** esse caminho que colocamos aqui na documentação foi o caminho que criei na minha própria máquina, então se você criou sua pasta em um caminho diferente, foque atento na hora de localizar para acessar o mesmo.
##### 5. Inicie o projeto
Toda vez que você precisar trabalhar com git e precisar fazer algo do zero, não se esqueça de iniciar seu projeto usando o seguinte comando:
```bash
git init
```
**Observação:** a validação de que o comando foi executado com sucesso é a pasta oculta com o nome de .git que fica dentro da pasta que você inicializou seu repositório.
![[Pasted image 20250122114722.png]]
##### 5. Arquivo de teste
Crie um arquivo de teste para enviar para nosso repositório remoto depois, como esse, por exemplo ou qualquer outro:
![[Pasted image 20250122114759.png]]
##### 6. Comando status
Dentro do git, ele não sabe que tudo o que criamos dentro da pasta vai ser versionado, precisamos falar isso pra ele, avisar. Uma maneira de checarmos isso é em qual "pé" está nosso arquivo digitando o comando:
```bash
git status
```
O que recebemos de retorno é o seguinte na tela do Git Bash:
![[Pasted image 20250122115418.png]]
Basicamente essa mensagem nos diz que esse arquivo ainda não está no fluxo de projeto do Git, ou seja, isso não é nada bom.
##### 7. Adicionando os arquivos ao fluxo do Git
Usaremos o comando git add . para listar vários arquivos no fluxo do git de uma única vez. Se quisermos colocar apenas um, devemos colocar o comando git add (caminho_com_o_nome_do_arquivo).
```bash
git add .
```
Agora checando o status do processo, podemos ver que agora foi alterado como sucesso e agora está no fluxo do git.
![[Pasted image 20250122120026.png]]
##### 8. Informando o git
Enviamos para o fluxo, mas o git ainda não sabe da alteração que fizemos. Para deixamos claro que foi feito, precisamos fazer o "commit". Seguiremos com o comando abaixo:
```bash
git commit -a -m
```
**-a:** adicionar todo tipo de arquivo a esse commit
**-m:** adicionar uma mensagem
![[Pasted image 20250122134416.png]]
Finalização do processo
![[Pasted image 20250122134500.png]]
##### 9. Disponibilização do seu repositório
Esses passos que fizemos estão somente registrados na nossa máquina local. Para que agora outras pessoas possam puxar nossos arquivos para dar continuidade ou alteração neles, temos que compartilhar eles conectando de forma remoto o repositório da nossa máquina em um servidor da empresa ou disponibilizando no GitHub (nuvem).

Para mais detalhes de como conectar seu repositório de forma remota, siga o artigo [[Configuração e permissão repositório Git hefesto Roque]]
