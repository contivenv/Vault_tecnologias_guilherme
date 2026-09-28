---
tags:
  - linux
  - permissões
  - usuários
  - arquivos
  - diretórios
  - pastas
  - sudo
  - debian
---
# Configurando Permissões de Sudo no Debian (Terminal)

Em instalações mínimas do Debian, caso uma senha de `root` tenha sido definida durante o setup, o pacote `sudo` frequentemente não é instalado por padrão e o usuário principal não é adicionado ao grupo de administradores. 

Siga o procedimento abaixo para conceder privilégios administrativos ao usuário, aplicando as melhores práticas de administração de sistemas.

## 1. Autenticar como Root
Como o usuário padrão ainda não possui privilégios, é necessário escalar para o superusuário. O traço (`-`) é crucial, pois garante que o perfil e as variáveis de ambiente corretas do root (como o `$PATH` para o `usermod`) sejam carregadas.

```bash
su -
````

_(Insira a senha de root definida durante a instalação)_

## 2. Instalar o Pacote Sudo

Garante que o pacote responsável pelo gerenciamento de privilégios está presente no sistema.

```bash
apt update && apt install sudo -y
```

## 3. Adicionar o Usuário ao Grupo Sudo

No Debian, o grupo padrão com permissões de execução administrativa é o `sudo` (diferente do `wheel` em sistemas baseados em Red Hat/Fedora). Use o `usermod` com as flags `-a` (append) e `-G` (groups) para não sobrescrever a filiação a outros grupos existentes.

```bash
usermod -aG sudo nome_do_usuario
```

> [!warning] Boa Prática !
>  Edite o arquivo `/etc/sudoers` diretamente com editores de texto comuns se precisar alterar regras de permissão no futuro. Utilize sempre o comando `visudo`. Ele valida a sintaxe antes de salvar e previne que um erro de digitação bloqueie permanentemente a escalação de privilégios no sistema.

## 4. Aplicar as Alterações

Para que a nova atribuição de grupo entre em vigor, a sessão do usuário precisa ser recarregada. Saia do usuário root e simule um novo login na conta `guilherme`.

```bash
exit
su - guilherme
```

_(Você também pode simplesmente encerrar a sessão atual e logar novamente)_

## 5. Validação

Teste o acesso executando um comando que exige elevação.

```bash
sudo whoami
```

_Resultado esperado: O sistema pedirá a senha do usuário `guilherme` e retornará a palavra `root`._