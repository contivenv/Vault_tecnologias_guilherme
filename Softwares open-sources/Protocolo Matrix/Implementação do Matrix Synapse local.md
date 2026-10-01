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
- Instalação do Docker e seus plugins para funcionar.
- Configurações de arquivos via terminal dentro do nosso sistema.

### Local da instalação

Em primeiro lugar, precisamos antes de tudo ter um sistema operacional instalado para poder alocar esse sistema. Sempre optamos por uma solução mais robusta e sólida quando se trata de aplicações críticas e de baixa mutabilidade: Debian.

Nesse caso, estaremos utilizando uma máquina virtual no ***Boxes*** que é o software de virtualização que já vem direto na instalação do Fedora Workstation como padrão. Você pode usar qualquer outro programa de virtualização como VirtualBox, o próprio KVM do Linux que é nativo (mas vai precisar do virt-manager para facilitar as coisas caso nunca tenha tido a experiência de mexer e outra interface), VMWare, entre outros. E claro, existe a possibilidade de querer rodar isso em uma máquina dedicada para fazer esse tutorial, esse é somente nosso laboratório de testes para mostrarmos a vocês.

![[Pasted image 20260928114159.png|Sistema Debian 13 já instalado e pronto para uso no Boxes]]
![[ssh para server virtualizado.png|ssh para servidor local virtualizado]]

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

Markdown

````
---
tags:
  - self-hosted
  - matrix
  - synapse
  - docker
  - redes
---

# Como auto-hospedar um servidor Matrix local

Para auto-hospedar um servidor Matrix local, o método mais prático é utilizar o Docker e o Matrix Synapse (o servidor de referência oficial) junto com o Docker Compose.

## O que você vai precisar

- Um computador ou mini PC (como um Raspberry Pi ou servidor dedicado) rodando Linux.
- O **Docker** e o **Docker Compose** instalados.
- Um domínio ou subdomínio (ex: `https://seudominio.com`) apontando para o seu IP (ou um IP local, se for apenas para a rede interna).

---

## Passo a passo para a instalação

### 1. Criar a pasta do projeto
Abra o terminal do seu servidor e crie um diretório para organizar os arquivos do Matrix:

```bash
mkdir -p ~/matrix-synapse
cd ~/matrix-synapse
````

### 2. Criar o arquivo de configuração inicial

Gere o arquivo de configuração padrão do Synapse rodando o container em modo de geração:

```bash
docker run -it --rm \
  -v ~/matrix-synapse:/data \
  -e SYNAPSE_SERVER_NAME=seudominio.com \
  -e SYNAPSE_REPORT_STATS=no \
  matrixdotorg/synapse:latest generate
```

> [!info] Dica de Configuração Substitua `seudominio.com` pelo seu domínio configurado ou endereço de IP local.

### 3. Criar o arquivo docker-compose.yml

Crie um arquivo chamado `docker-compose.yml` na mesma pasta com o seguinte conteúdo básico:

```yaml
version: '3'
services:
  synapse:
    image: matrixdotorg/synapse:latest
    container_name: matrix-synapse
    restart: unless-stopped
    ports:
      - 8008:8008
    volumes:
      - ~/matrix-synapse:/data
    environment:
      - UID=1000
      - GID=1000
```

### 4. Iniciar o servidor

Execute o container em segundo plano:

Bash

```
docker compose up -d
```

### 5. Criar um usuário administrador

Com o servidor rodando, crie a sua conta de administrador executando:

Bash

```
docker exec -it matrix-synapse register_new_matrix_user \
  -c /data/homeserver.yaml \
  http://localhost:8008
```

> [!tip] Próximos Passos Siga as instruções no terminal para definir o nome de usuário, a senha e confirmar se a conta terá privilégios de administrador.

### 6. Conectar um cliente

Baixe um aplicativo cliente compatível com o Matrix, como o **Element** (disponível para celular, computador ou navegador). Na tela de login, mude o campo do servidor (_homeserver_) para o endereço do seu servidor local (ex: `http://192.168.X.X:8008` ou o seu domínio configurado com HTTPS via proxy reverso) e entre com a sua nova conta.