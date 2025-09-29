---
tags:
  - implementação
  - ubuntu_server
  - rustdesk
  - Guilherme
---

### Primeiro passo

Antes de qualquer procedimento verifique as atualizações nos repositórios e as instale com esse comando:

```bash
sudo apt update 
sudp apt upgrade 
```

Configurar servidor

1. Abra o VSCode, clique no botão no canto esquerdo e selecione SSH
    
    ![https://rustdesk.com/docs/en/self-host/rustdesk-server-oss/ubuntu-server/docker/images/setup/open_vscode.png](https://rustdesk.com/docs/en/self-host/rustdesk-server-oss/ubuntu-server/docker/images/setup/open_vscode.png)
    
2. Digitar`username@IP`, por exemplo`demouser@192.168.2.98`, e então`Enter`
    
    ![https://rustdesk.com/docs/en/self-host/rustdesk-server-oss/ubuntu-server/docker/images/setup/connect_server_through_vscode.png](https://rustdesk.com/docs/en/self-host/rustdesk-server-oss/ubuntu-server/docker/images/setup/connect_server_through_vscode.png)
    
3. Selecione seu sistema`Linux`
    
4. Confirme a impressão digital do servidor
    
5. Digite a senha do seu usuário
    
6. Abra sua pasta pessoal
    
    ![https://rustdesk.com/docs/en/self-host/rustdesk-server-oss/ubuntu-server/docker/images/setup/vscode_open_folder.png](https://rustdesk.com/docs/en/self-host/rustdesk-server-oss/ubuntu-server/docker/images/setup/vscode_open_folder.png)
    
7. Clique`Yes, I trust the authors`
    
8. Terminal aberto
    
    ![https://rustdesk.com/docs/en/self-host/rustdesk-server-oss/ubuntu-server/docker/images/setup/vscode_open_terminal.png](https://rustdesk.com/docs/en/self-host/rustdesk-server-oss/ubuntu-server/docker/images/setup/vscode_open_terminal.png)
    
9. Instalar pacotes
    
10. Instalar pacotes
    

```lua
sudo apt install docker.io docker-compose python3-pip curl git vim nano zram-config -y

```

1. Desabilitar troca de disco

Verifique se o arquivo de swap existe

```bash
sudo vim /etc/fstab
```

Se você encontrar algo semelhante a:

```bash
/swap.img       none    swap    sw      0       0
```

Se não: Digite `:qa!`then `Enter`para sair. E pule para a etapa 11

Se sim: Pressione `i`para ativar o modo de edição, comente essa linha `#`assim:

```bash
#/swap.img       none    swap    sw      0       0

```

Pressione `Esc`e digite `:wq`para `Enter`salvar as alterações.

1. Ajustar tamanho da ZRAM

ZRAM significa “comprimir memória RAM”, é mais eficiente e não ocupa espaço em disco.

```bash
sudo vim /usr/bin/init-zram-swapping
```

Encontre a linha com

```bash
mem=$((totalmem / 2 * 1024))

```

E ajuste-o para:

```bash
mem=$((totalmem * 2 * 1024))

```

Salvar e sair

1. Defina seu fuso horário

Encontre seu fuso horário em [Wikipédia](https://translate.google.com/website?sl=auto&tl=pt&hl=pt-BR&client=webapp&u=https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)

```bash
sudo timedatectl set-timezone "Asia/Taipei"

```

1. Reinício

```bash
sudo reboot

```

Após a reinicialização, reconecte-se ao seu VSCode e abra o terminal.

1. Excluir`swap.img`

(Pule se você não tiver.)

Substituímos o arquivo de swap por ZRAM, agora podemos excluir`swap.img`agora, substitua`swap.img`com outros se seu nome for diferente.

```bash
sudo rm /swap.img

```

### 3. Configurar o servidor RustDesk

1. Execute este comando para criar as pastas necessárias uma vez:

```bash
cd ~ && mkdir -p docker/rustdesk-server/data

```

1. Criar`compose.yml`

Clique com o botão direito`rustdesk-server`pasta, crie um novo arquivo chamado`compose.yml`.

Cole isso em`compose.yml`.

Você pode modificar a linha com `hbbs`o IP LAN do seu servidor temporariamente (se estiver implantando na sua LAN) para garantir que esteja funcionando. Depois de verificar se o seu servidor está funcionando, você **deve** alterar de volta.

Está com problemas depois de mudar o IP da LAN para o domínio? Você deve verificar[este artigo](https://rustdesk-com.translate.goog/docs/en/self-host/nat-loopback-issues/?_x_tr_sl=auto&_x_tr_tl=pt&_x_tr_hl=pt-BR&_x_tr_pto=wapp).

`services: hbbs: container_name: hbbsimage: rustdesk/rustdesk-server:latestcommand: hbbsvolumes: - ./data:/rootnetwork_mode: hostdepends_on: - hbbrrestart: alwayshbbr: container_name: hbbrimage: rustdesk/rustdesk-server:latestcommand: hbbrvolumes: - ./data:/rootnetwork_mode: hostrestart: always# Because using docker host mode# Just in case you forgot the ports:# 21114 TCP for web console, only available in Pro version# 21115 TCP for NAT type test# 21116 TCP TCP hole punching# 21116 UDP heartbeat/ID server# 21117 TCP relay# 21118/21119 TCP for web socket if you want to run web client`

Verificar [aqui](https://rustdesk-com.translate.goog/docs/en/client?_x_tr_sl=auto&_x_tr_tl=pt&_x_tr_hl=pt-BR&_x_tr_pto=wapp) para configurar seu cliente. Apenas`ID server`e `Key`é necessário.`Relay server`não é necessário porque o configuramos em `hbbs`, o hbbs fornecerá essas informações automaticamente.

1. Inicie o servidor

```bash
cd ~/docker/rustdesk-server
sudo docker-compose up -d
```

1. Verifique se está funcionando

No seu VSCode, você deve ver`id_ed25519`,`id_ed25519.pub`no seu`docker/rustdesk-server/data`pasta. Você pode clicar`id_ed25519.pub`, esta é a chave pública que você precisa para seu cliente RustDesk.

A chave pública ficará assim:

![https://rustdesk.com/docs/en/self-host/rustdesk-server-oss/ubuntu-server/docker/images/setup/vscode_see_public_key.png](https://rustdesk.com/docs/en/self-host/rustdesk-server-oss/ubuntu-server/docker/images/setup/vscode_see_public_key.png)

### 4. Defina o encaminhamento de porta no seu roteador/VPS

Acesse a página de administração do seu roteador e encontre qualquer coisa relacionada a`Port forwarding`, deve aparecer em `WAN`ou`Firewall`configurações.

Se você ainda não conseguir encontrar a configuração, pesquise no Google`{Router brand} + port forwarding`ou`{Router model} + port forwarding`. Se este dispositivo for do seu ISP, pergunte a eles.

Se você estiver usando VPS, pesquise no Google`{VPS vendor name} + firewall port`para encontrar o procedimento específico para seu VPS.

Abra estas portas necessárias:`21114`

TCP para console web, disponível apenas na versão Pro`21115`

TCP para teste de tipo NAT`21116`

TCP TCP perfuração`21116`

Servidor de pulsação/ID UDP`21117`

Relé TCP`21118/21119`

TCP para web socket se você quiser executar o cliente web

### 5. Alguns princípios básicos

1. Como aplicar as configurações após a modificação`compose.yml`?

Execute isto novamente:

```
sudo docker-compose up -d

```

1. Como parar e excluir o contêiner?

(Isso não limpará seus dados)

```
sudo docker-compose down

```

1. Como fazer backup do servidor?

Primeiro, corra`sudo docker-compose down`, então faça o download.

![https://rustdesk.com/docs/en/self-host/rustdesk-server-oss/ubuntu-server/docker/images/setup/vscode_download_folder.png](https://rustdesk.com/docs/en/self-host/rustdesk-server-oss/ubuntu-server/docker/images/setup/vscode_download_folder.png)

Arraste e solte-os no VSCode Explorer se quiser carregá-los.

1. Como atualizar o contêiner automaticamente?

Usar [Torre de vigia](https://translate.google.com/website?sl=auto&tl=pt&hl=pt-BR&client=webapp&u=https://containrrr.dev/watchtower/).

Crie uma pasta e coloque o`compose.yml`nele.

```jsx
mkdir ~/docker/watchtower

```

Altere seu fuso horário com o seu em `TZ`.

**Se você não especificou nenhum nome de contêiner, todos** os seus contêineres serão atualizados .

No comando a seguir, ele será executado todos os dias às 3 da manhã, para mais detalhes, verifique o[documentação](https://translate.google.com/website?sl=auto&tl=pt&hl=pt-BR&client=webapp&u=https://containrrr.dev/watchtower/arguments/%23scheduling).

`version: "3"services: watchtower: image: containrrr/watchtower:latestcontainer_name: watchtowernetwork_mode: bridgevolumes: - /var/run/docker.sock:/var/run/docker.sockenvironment: TZ: Asia/Taipeicommand: --cleanup --schedule "0 0 3 * * *" hbbr hbbsrestart: always`

1. Como atualizar o sistema Ubuntu automaticamente?

Por padrão, o Ubuntu instalará atualizações de segurança automaticamente, pesquise no Google:`ubuntu unattended-upgrades`ou verifique o arquivo em`/etc/apt/apt.conf.d/50unattended-upgrades`para mais detalhes.