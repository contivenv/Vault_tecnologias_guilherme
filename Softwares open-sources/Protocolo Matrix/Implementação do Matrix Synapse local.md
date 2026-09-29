---
tags: [cibersegurança, protocolos, comunicacao, matrix, descentralizacao]
---
### Requisitos básicos para o funcionamento do Synapse

Iremos precisar de:

- Uma máquina virtual ou física para a instalação do sistema operacional
- Instalação do Docker e seus plugins para funcionar
- Configurações de arquivos via terminal dentro do nosso sistema.

### Local da instalação

Em primeiro lugar, precisamos antes de tudo ter um sistema operacional instalado para poder alocar esse sistema. Sempre optamos por uma solução mais robusta e sólida quando se trata de aplicações críticas e de baixa mutabilidade: Debian.

Nesse caso, estaremos utilizando uma máquina virtual no ***Boxes*** que é o software de virtualização que já vem direto na instalação do Fedora Workstation como padrão. Você pode usar qualquer outro programa de virtualização como VirtualBox, o próprio KVM do Linux que é nativo (mas vai precisar do virt-manager para facilitar as coisas caso nunca tenha tido a experiência de mexer e outra interface), VMWare, entre outros. E claro, existe a possibilidade de querer rodar isso em uma máquina dedicada para fazer esse tutorial, esse é somente nosso laboratório de testes para mostrarmos a vocês.

![[Pasted image 20260928114159.png|Sistema Debian 13 já instalado e pronto para uso no Boxes]]
![[ssh para server virtualizado.png]]

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