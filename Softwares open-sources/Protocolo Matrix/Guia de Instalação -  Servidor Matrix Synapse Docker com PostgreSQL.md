---
tags:
  - cibersegurança
  - self-hosted
  - matrix
  - synapse
  - docker
  - redes
  - protocolos
  - comunicação
  - descentralização
---
### Requisitos básicos para o funcionamento do Synapse

Iremos precisar de:

- Uma máquina virtual ou física para a instalação do sistema operacional
- Instalação do [Docker](https://www.docker.com/) e seus plugins para funcionar.
- Um banco de dados (de preferência [PostgreSQL](https://www.postgresql.org/))
- Configurações de arquivos via terminal dentro do nosso sistema.

### Local da instalação

Em primeiro lugar, precisamos antes de tudo ter um sistema operacional instalado para poder alocar esse sistema. Sempre optamos por uma solução mais robusta e sólida quando se trata de aplicações críticas e de baixa mutabilidade: Debian.

Nesse caso, estaremos utilizando uma máquina virtual no ***Boxes*** que é o software de virtualização que já vem direto na instalação do Fedora Workstation como padrão. Você pode usar qualquer outro programa de virtualização como VirtualBox, o próprio KVM do Linux que é nativo (mas vai precisar do virt-manager para facilitar as coisas caso nunca tenha tido a experiência de mexer e outra interface), VMWare, entre outros. E claro, existe a possibilidade de querer rodar isso em uma máquina dedicada para fazer esse tutorial, esse é somente nosso laboratório de testes para mostrarmos a vocês.

![[Pasted image 20260928114159.png|Sistema Debian 13 já instalado e pronto para uso no Boxes]]
![[ssh para server virtualizado 1.png|ssh para servidor local virtualizado]]

Para mais informações de forma detalhada e ajuda para usar o Boxes, consulte a [documentação oficial](https://help.gnome.org/gnome-boxes/index.html.pt_BR).

---

### [Instalar usando o `apt` repositório](https://docs.docker.com/engine/install/debian/#install-using-the-repository)

Antes de instalar o Docker Engine pela primeira vez em uma nova máquina host, você precisa configurar o repositório `apt` do Docker. Depois, você poderá instalar e atualizar o Docker diretamente deste repositório.

### 1. Configurar o repositório APT do Docker

Execute o bloco de comandos abaixo para adicionar a chave GPG oficial do Docker e configurar as fontes do `apt`:

```bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL [https://download.docker.com/linux/debian/gpg](https://download.docker.com/linux/debian/gpg) -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF

Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

````

#### Nota para distribuições derivadas

> Se você usar o Debian Testing ou uma distribuição derivada, como o Kali Linux, você pode precisar substituir a parte deste comando que é executada para imprimir o codinome da versão:
> `(. /etc/os-release && echo "$VERSION_CODENAME")`
> Substitua esta parte pelo codinome da versão Debian correspondente, como, por exemplo, `trixie`.
## 2. Instalar os pacotes do Docker

Para instalar a versão mais recente, execute:

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 3. Verificar a instalação

> [!info] Status do Serviço Após a instalação, verifique se o Docker está sendo executado através do comando:
> 
> Bash
> 
> ```
> sudo systemctl status docker
> ```
> 
> Se o Docker não estiver em execução, inicie-o manualmente:
> 
> Bash
> 
> ```
> sudo systemctl start docker
> ```

Verifique se a instalação foi bem-sucedida executando a imagem `hello-world`:

```bash
sudo docker run hello-world
```

Este comando baixa uma imagem de teste e a executa em um contêiner. Quando o contêiner é executado, ele imprime uma mensagem de confirmação e sai. Se isso ocorrer, você já instalou e iniciou com sucesso o Docker Engine.

> [!tip] Dica: Recebendo erros ao tentar rodar sem root? O grupo de usuários `docker` existe, mas inicialmente não contém usuários, e é por isso que é necessário usar `sudo` para executar comandos do Docker.
> 
> Continue para as etapas de pós-instalação do Linux (_Linux postinstall_) para permitir que usuários não privilegiados executem comandos do Docker e para realizar outras etapas de configuração opcionais.

## Atualizar o Docker Engine

Para atualizar o Docker Engine, basta rodar o `sudo apt update` e seguir o **Passo 2** das instruções de instalação, escolhendo a nova versão que deseja instalar

---
### Colocando a mão na massa

Agora com os pré requisitos feitos, vamos agora começar a passar os comandos ao nosso terminal para começar a realizar as configurações iniciais do Synapse no nosso servidor local.

**Contexto do ambiente até agora:**

- **Sistema Operacional:** Debian 13 (Trixie) virtualizado via Fedora Boxes
    
- **IP da Máquina Virtual:** `192.168.122.138`
    
- **Dependências:** Docker instalado, acesso via SSH
    

## Passo 1: Criar o diretório de persistência de dados

O Synapse precisa de um diretório persistente no host para armazenar o arquivo de configuração, chaves e arquivos de mídia.

```bash
mkdir -p ~/synapse/data
```

## Passo 2: Gerar os arquivos de configuração e chaves

Antes de subir o serviço, é necessário gerar o `homeserver.yaml` e as chaves de assinatura do servidor. _Nota: O nome do servidor (`SYNAPSE_SERVER_NAME`) deve ser o IP ou domínio definitivo, pois não pode ser alterado posteriormente e define a estrutura dos usuários (ex: `@usuario:192.168.122.138`)_.

```bash
sudo docker run -it --rm \
    -v ~/synapse/data:/data \
    -e SYNAPSE_SERVER_NAME=192.168.122.138 \
    -e SYNAPSE_REPORT_STATS=no \
    matrixdotorg/synapse:latest generate
```

## Passo 3: Iniciar o Banco de Dados (PostgreSQL)

O Synapse utiliza SQLite por padrão, mas ele tem sérios problemas de performance em salas grandes. O recomendado é usar o PostgreSQL.

Inicie o container do Postgres:

```bash
sudo docker run -d --name synapse-postgres \
    -e POSTGRES_PASSWORD=mysecretpassword \
    -e POSTGRES_USER=postgres \
    -e POSTGRES_DB=postgres \
    -p 5432:5432 \
    postgres:14
```

## Passo 4: Configurar o Synapse para usar o PostgreSQL

Edite o arquivo de configuração recém-gerado:

```bash
nano ~/synapse/data/homeserver.yaml
```

Encontre a seção `database:` (que estará preenchida com configurações do `sqlite3`) e substitua **todo o bloco** pelo conteúdo abaixo, respeitando a indentação.

> **Importante:** A flag `allow_unsafe_locale: true` é obrigatória nesse ambiente de testes. O container padrão do Postgres inicia com o locale do sistema (`en_US.utf8`), mas o Synapse exige o locale `C` por segurança de codificação UTF-8. Essa flag desabilita a trava que impede a inicialização do Synapse.

```yaml
database:
  name: psycopg2
  allow_unsafe_locale: true
  args:
    user: postgres
    password: mysecretpassword
    dbname: postgres
    host: 192.168.122.138
    port: 5432
    cp_min: 5
    cp_max: 10
```

Salve o arquivo (`Ctrl+O`, `Enter`) e saia (`Ctrl+X`).

## Passo 5: Iniciar o servidor Synapse

Inicie o container principal do Synapse mapeando a porta HTTP padrão `8008`:

```bash
sudo docker run -d --name synapse \
    -p 8008:8008 \
    -v ~/synapse/data:/data \
    matrixdotorg/synapse:latest
```

_Nota: Aguarde de 15 a 30 segundos após executar este comando. O Synapse estará criando dezenas de tabelas vazias no PostgreSQL antes de liberar o acesso à rede._

### Teste de Conexão

Verifique se a API subiu corretamente:

```bash
curl http://localhost:8008/_matrix/client/versions
```

_(O retorno deve ser um objeto JSON detalhando as versões da API Matrix suportadas)._

## Passo 6: Registrar o primeiro usuário administrador

Com o servidor rodando, crie a sua conta administrativa utilizando o script interno do Synapse (`register_new_matrix_user`).

Execute dentro do container:


```bash
sudo docker exec -it synapse register_new_matrix_user -c /data/homeserver.yaml http://localhost:8008
```

O assistente no terminal solicitará:

1. **New user localpart:** (seu nome de usuário, ex: `admin`)
    
2. **Password:** (sua senha)
    
3. **Confirm password:** (repita a senha)
    
4. **Make admin [no]:** Digite `yes` e pressione Enter.
    

## Passo 7: Acessando via Cliente (Element)

1. Baixe e abra um cliente Matrix (como o aplicativo Element Web ou Desktop).
    
2. Na tela de login, selecione a opção para **Editar** ou **Mudar de provedor** (Homeserver).
    
3. Altere o endereço padrão (`matrix.org`) para o IP local do seu servidor: `[http://192.168.122.138:8008](http://192.168.122.138:8008)`
    
4. Faça login usando as credenciais criadas no Passo 6.

![[Captura de tela de 2026-10-01 00-12-21.png|acessando servidor local e usando nossas credenciais]]
![[Pasted image 20261001001253.png|tela inicial do client Element no Fedora com nosso servidor local]]

### Fontes

[Synapse documentação oficial](https://element-hq.github.io/synapse/latest/welcome_and_overview.html)

[Docker documentação oficial para intalação usando pacote apt](https://docs.docker.com/engine/install/debian/#install-using-the-repository)

[Installing Element Server Suite](https://docs.element.io/latest/element-server-suite-classic/introduction-to-element-server-suite/)