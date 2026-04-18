---
tags:
  - validado
  - IdM
  - RHEL
  - Red_Hat
  - servidores
  - domínio
  - DNS
  - Ansible
---

IdM com Ansible como configurar no RHEL 9 ? Me faça um tutorial. Eu estou uma máquina virtualizada para rodar o RHEL 9. Ela está dentro de um Proxmox e quero que ela seja um domínio na minha empresa, quero fazer um teste primeiro. O nome do domínio vai ser "teste", o IP do servidor RHEL é 192.168.2.255 e estou fazendo a conexão via ssh por ele.

  

guilherme.teixeira@guilherme-ti:~$ ssh gteixeira@192.168.2.255

gteixeira@192.168.2.255's password: 

Activate the web console with: systemctl enable --now cockpit.socket

  

Register this system with Red Hat Insights: rhc connect

  

Example:

# rhc connect --activation-key <key> --organization <org>

  

The rhc client and Red Hat Insights will enable analytics and additional

management capabilities on your system.

View your connected systems at https://console.redhat.com/insights

  

You can learn more about how to register your system 

using rhc at https://red.ht/registration

Last login: Sat Apr 18 09:41:55 2026

[gteixeira@localhost ~]$ ip a

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000

    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00

    inet 127.0.0.1/8 scope host lo

       valid_lft forever preferred_lft forever

    inet6 ::1/128 scope host 

       valid_lft forever preferred_lft forever

2: enp6s18: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000

    link/ether bc:24:11:f5:aa:3e brd ff:ff:ff:ff:ff:ff

    inet 192.168.2.255/22 brd 192.168.3.255 scope global dynamic noprefixroute enp6s18

       valid_lft 7121sec preferred_lft 7121sec

    inet6 fe80::be24:11ff:fef5:aa3e/64 scope link noprefixroute 

       valid_lft forever preferred_lft forever

Olá, Guilherme! Excelente iniciativa usar o Proxmox para criar esse laboratório com o RHEL 9. Automatizar a instalação do Red Hat Identity Management (IdM) com o Ansible é a melhor forma de garantir que o ambiente seja replicável no futuro.

Antes de colocarmos a mão na massa, preciso fazer dois pequenos alinhamentos técnicos sobre a sua configuração:

1. **Nome do Domínio (FQDN):** O IdM exige um _Fully Qualified Domain Name_ (FQDN) válido. O nome isolado "teste" não vai funcionar. Para o nosso laboratório, vamos usar **`teste.local`** como domínio e **`idm.teste.local`** como o nome da máquina (hostname).
    
2. **IP Dinâmico vs Estático:** Analisando a saída do seu comando `ip a`, vejo que o IP `192.168.2.255/22` está marcado como `dynamic` (entregue por DHCP). Em uma rede `/22`, o final `.255` é um IP de host perfeitamente válido, mas um servidor de domínio/DNS **precisa ter um IP estático**. Recomendo que você fixe esse IP no seu servidor DHCP/roteador ou configure-o estaticamente no RHEL usando o `nmcli` antes de seguir para produção.
    

Como este é um ambiente de teste e você já está acessando a máquina via SSH, vamos configurar o Ansible para rodar **localmente** no próprio servidor RHEL 9.

Aqui está o seu tutorial passo a passo.

---

### Passo 1: Preparar o Hostname e o Arquivo Hosts

O IdM é extremamente sensível à resolução de nomes. O próprio servidor precisa saber quem ele é.

**1. Defina o hostname correto:**

Bash

```
sudo hostnamectl set-hostname idm.teste.local
```

**2. Edite o arquivo `/etc/hosts`:** Abra o arquivo com seu editor favorito (ex: `sudo nano /etc/hosts` ou `sudo vi /etc/hosts`) e adicione o IP e o FQDN. Ele deve ficar parecido com isso:

Plaintext

```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

# Adicione esta linha:
192.168.2.255   idm.teste.local idm
```

### Passo 2: Instalar os Pacotes Necessários

No RHEL 9, o Ansible e as _roles_ oficiais do IdM (FreeIPA) estão disponíveis nos repositórios padrão (AppStream).

Execute o comando abaixo para instalar o motor do Ansible e a coleção do FreeIPA:

Bash

```
sudo dnf install ansible-core ansible-freeipa -y
```

### Passo 3: Criar os Arquivos do Ansible

Crie um diretório para organizar seus arquivos de configuração:

Bash

```
mkdir ~/idm-ansible && cd ~/idm-ansible
```

**1. Crie o arquivo de inventário (`inventory.ini`):** Crie um arquivo chamado `inventory.ini` e adicione o conteúdo abaixo. A instrução `ansible_connection=local` avisa ao Ansible para rodar os comandos na própria máquina, sem tentar abrir uma nova conexão SSH.

Ini, TOML

```
[ipaserver]
idm.teste.local ansible_connection=local
```

**2. Crie o Playbook (`install-idm.yml`):** Crie um arquivo chamado `install-idm.yml`. É aqui que declaramos como o domínio deve ser construído.

_Atenção: Substitua as senhas por senhas seguras, pois o IdM falhará na instalação se você usar senhas muito fracas (como "123456")._

YAML

```
---
- name: Playbook para configurar o Servidor IdM
  hosts: ipaserver
  become: true # Roda com privilégios de root (sudo)

  vars:
    # Configurações do Domínio
    ipaserver_domain: teste.local
    ipaserver_realm: TESTE.LOCAL
    ipaserver_setup_dns: yes
    ipaserver_auto_forwarders: yes
    
    # Senhas (Atenção: Use senhas fortes, ex: no mínimo 8 caracteres com letras e números)
    ipaadmin_password: SenhaForteAdmin123!
    ipadm_password: SenhaForteDirectory123!
    
    # O IP do servidor
    ipaserver_ip_addresses:
      - 192.168.2.255

  roles:
    - role: ipaserver
```

### Passo 4: Executar a Instalação

Agora que tudo está preparado, basta mandar o Ansible executar o playbook. Como ele precisa instalar pacotes pesados, configurar o Directory Server, Certificate Authority (CA) e o DNS (Bind), **esse processo pode demorar alguns minutos**.

Execute:

Bash

```
ansible-playbook -i inventory.ini install-idm.yml -K
```

_(O parâmetro `-K` vai pedir a sua senha sudo para que o Ansible possa realizar as configurações como root)._

---

### O que acontece depois?

Assim que o Ansible terminar e a tela mostrar tudo com status verde e amarelo (ok/changed), seu domínio estará rodando!

- **Acesso via Web:** Você poderá acessar a interface gráfica do IdM no seu navegador, digitando `https://192.168.2.255`. Faça login com o usuário `admin` e a senha que você definiu em `ipaadmin_password`.
    
- **Acesso no Terminal:** Para começar a administrar o domínio pelo terminal do RHEL, você precisa obter um ticket Kerberos. Basta rodar: `kinit admin` (e inserir a senha).
    

Porque ele deu esse erro no final ?

  

```bash
[root@idm idm-ansible]# ansible-playbook -i inventory.ini install-idm.yml -K

BECOME password: 

  

PLAY [Playbook para configurar o Servidor IdM] **************************************************************************************************************************

  

TASK [Gathering Facts] **************************************************************************************************************************************************

ok: [idm.teste.local]

  

TASK [ipaserver : Import variables specific to distribution] ************************************************************************************************************

ok: [idm.teste.local] => (item=/usr/share/ansible/roles/ipaserver/vars/default.yml)

  

TASK [ipaserver : Install IPA server] ***********************************************************************************************************************************

included: /usr/share/ansible/roles/ipaserver/tasks/install.yml for idm.teste.local

  

TASK [ipaserver : Install - Set ipaserver__dns_over_lts] ****************************************************************************************************************

ok: [idm.teste.local]

  

TASK [ipaserver : Install - Set packages for installation] **************************************************************************************************************

ok: [idm.teste.local]

  

TASK [ipaserver : Install - Set packages for installlation, add DNS] ****************************************************************************************************

ok: [idm.teste.local]

  

TASK [ipaserver : Install - Set packages for installlation, add DOT] ****************************************************************************************************

skipping: [idm.teste.local]

  

TASK [ipaserver : Install - Set packages for installlation, add adtrust] ************************************************************************************************

skipping: [idm.teste.local]

  

TASK [ipaserver : Install - Set packages for installlation, add firewalld] **********************************************************************************************

ok: [idm.teste.local]

  

TASK [ipaserver : Install - Ensure that packages are installed] *********************************************************************************************************

changed: [idm.teste.local]

  

TASK [ipaserver : Firewalld service - Ensure that firewalld is running] *************************************************************************************************

ok: [idm.teste.local]

  

TASK [ipaserver : Firewalld - Verify runtime zone "{{ ipaserver_firewalld_zone }}"] *************************************************************************************

skipping: [idm.teste.local]

  

TASK [ipaserver : Firewalld - Verify permanent zone "{{ ipaserver_firewalld_zone }}"] ***********************************************************************************

skipping: [idm.teste.local]

  

TASK [ipaserver : Copy external certs] **********************************************************************************************************************************

skipping: [idm.teste.local]

  

TASK [ipaserver : Install - Server installation test] *******************************************************************************************************************

ok: [idm.teste.local]

  

TASK [ipaserver : Install - Master password creation] *******************************************************************************************************************

changed: [idm.teste.local]

  

TASK [ipaserver : Install - Use new master password] ********************************************************************************************************************

ok: [idm.teste.local]

  

TASK [ipaserver : Use user defined master password, if provided] ********************************************************************************************************

skipping: [idm.teste.local]

  

TASK [ipaserver : Install - Server preparation] *************************************************************************************************************************

fatal: [idm.teste.local]: FAILED! => {"changed": false, "msg": "DNS server 192.168.2.55: consulta \". SOA\": All nameservers failed to answer the query . IN SOA: Server Do53:192.168.2.55@53 answered FORMERR"}

  

TASK [ipaserver : Install - Cleanup root IPA cache] *********************************************************************************************************************

ok: [idm.teste.local]

  

TASK [ipaserver : Cleanup temporary files] ******************************************************************************************************************************

ok: [idm.teste.local] => (item=/etc/ipa/.tmp_pkcs12_dirsrv)

ok: [idm.teste.local] => (item=/etc/ipa/.tmp_pkcs12_http)

ok: [idm.teste.local] => (item=/etc/ipa/.tmp_pkcs12_pkinit)

  

PLAY RECAP **************************************************************************************************************************************************************

idm.teste.local            : ok=14   changed=2    unreachable=0    failed=1    skipped=6    rescued=0    ignored=0 
```

Esse é um erro clássico (e um pouco frustrante) ao configurar o IdM em ambientes de laboratório ou redes domésticas.

O problema está no servidor DNS que a sua máquina virtual está usando atualmente (o IP **`192.168.2.55`**, que provavelmente é o seu roteador de internet, o gateway do Proxmox ou um servidor DHCP local).

Quando colocamos `ipaserver_auto_forwarders: yes` no playbook, o instalador lê o arquivo `/etc/resolv.conf` do Linux, pega o DNS atual e faz um teste rigoroso consultando o registro _SOA (Start of Authority) raiz_. O problema é que a maioria dos roteadores domésticos ou modems de operadora não suportam esse tipo específico de consulta e respondem com um erro de formato (`FORMERR`). O IdM, sendo extremamente exigente com DNS, aborta a instalação para evitar problemas no futuro.

Para resolver, vamos forçar o IdM a ignorar o DNS do seu roteador e usar resolvedores públicos confiáveis (como Google e Cloudflare).

Aqui está o passo a passo para consertar e tentar novamente:

### Passo 1: Limpar a instalação interrompida

Como o Ansible falhou no meio do processo, o RHEL ficou em um estado "meio instalado". Se tentarmos rodar o playbook agora, ele vai dar erro dizendo que o IdM já existe. Precisamos resetar isso.

Rode o comando de desinstalação manual direto no terminal do servidor:

Bash

```
sudo ipa-server-install --uninstall -U
```

_(Ele vai limpar os diretórios, remover os certificados gerados na primeira tentativa e voltar a máquina ao estado original)._

### Passo 2: Atualizar o Playbook

Abra o seu arquivo `install-idm.yml` e modifique a seção `vars`. Vamos remover o `ipaserver_auto_forwarders: yes` e colocar endereços manuais, além de desligar a validação de DNSSEC (que também costuma dar dor de cabeça em laboratórios atrás de NAT).

Deixe a seção de variáveis do seu playbook exatamente assim:

```yaml
  vars:
    # Configurações do Domínio
    ipaserver_domain: teste.local
    ipaserver_realm: TESTE.LOCAL
    ipaserver_setup_dns: yes
    
    # NOVAS CONFIGURAÇÕES DE DNS:
    ipaserver_auto_forwarders: no
    ipaserver_forwarders:
      - 8.8.8.8
      - 1.1.1.1
    ipaserver_no_dnssec_validation: yes
    
    # Senhas 
    ipaadmin_password: SenhaForteAdmin123!
    ipadm_password: SenhaForteDirectory123!
    
    # O IP do servidor
    ipaserver_ip_addresses:
      - 192.168.2.255
```

### Passo 3: Executar novamente

Agora, com o ambiente limpo e o playbook apontando para os DNS do Google/Cloudflare para resolver endereços externos, rode novamente a instalação:

Bash

```
ansible-playbook -i inventory.ini install-idm.yml -K
```

Meu resolv.conf está assim:

  

# Generated by NetworkManager

search teste.local

nameserver 192.168.2.55

nameserver 192.168.2.10

Exatamente! Esse arquivo confirma o nosso diagnóstico.

Como a sua máquina está recebendo as configurações de rede via DHCP, o `NetworkManager` preencheu o `/etc/resolv.conf` automaticamente com os servidores DNS que o seu roteador (ou Proxmox) enviou (`192.168.2.55` e `192.168.2.10`). Foram eles que não conseguiram responder à consulta rigorosa do instalador do IdM.

A boa notícia é que **as alterações que fizemos no playbook no passo anterior já resolvem isso**. Ao colocar `ipaserver_auto_forwarders: no` e definir os IPs do Google/Cloudflare manualmente, o instalador do IdM vai simplesmente ignorar esses servidores do seu `resolv.conf` na hora de configurar o serviço de DNS do domínio.

Você já pode rodar o playbook atualizado e a instalação deverá passar sem aquele erro.

Porém, há um detalhe importante para o futuro: como o seu IP é dinâmico (DHCP), toda vez que você reiniciar o servidor RHEL, o `NetworkManager` vai sobrescrever o `/etc/resolv.conf` com esses mesmos IPs antigos. Um servidor de domínio (IdM/Active Directory) precisa obrigatoriamente olhar para **ele mesmo** (127.0.0.1) para resolver o DNS da rede.

Você prefere que eu te mostre como fixar esse IP e o DNS permanentemente no RHEL usando o `nmcli` antes de rodarmos o playbook novamente, ou prefere rodar a instalação do Ansible primeiro e ajustar a rede depois?

Agora deu certo.

Excelente notícia, Guilherme! Ver esse `failed=0` no log do Ansible é sempre uma satisfação e confirma que a instalação ocorreu perfeitamente.

Subir esse ambiente de Identity Management do zero no seu Proxmox é um laboratório fantástico. Inclusive, colocar a mão na massa com LDAP, Kerberos, certificados e controle de acesso centralizado vai te dar uma base prática excelente para gabaritar os conceitos de IAM na sua prova do Security+ em junho. Nada supera ver a teoria rodando na prática.

Para garantir que o seu servidor não "quebre" caso a máquina virtual seja reiniciada, precisamos fazer aquele ajuste de fixar o IP e apontar o DNS para o próprio servidor (localhost).

### Ajustando a Rede com o `nmcli`

Como vimos no seu comando `ip a` lá no início, sua interface de rede se chama `enp6s18`. Execute os comandos abaixo diretamente no terminal do RHEL:

**1. Configure o IP como estático (manual):**

Bash

```
sudo nmcli con modify "enp6s18" ipv4.method manual ipv4.addresses 192.168.2.255/22
```

_(Nota: Se o seu RHEL precisar de acesso à internet e o roteador da sua rede for, por exemplo, o 192.168.2.55, você pode adicionar `ipv4.gateway 192.168.2.55` ao final do comando acima)._

**2. Defina o próprio servidor como DNS primário:** Isso garante que o RHEL sempre pergunte ao serviço de DNS interno do IdM na hora de resolver nomes.

Bash

```
sudo nmcli con modify "enp6s18" ipv4.dns "127.0.0.1" ipv4.dns-search "teste.local"
```

**3. Reinicie a conexão de rede para aplicar:**

Bash

```
sudo nmcli con up "enp6s18"
```

### Acessando o seu Domínio

Agora o ambiente está sólido e pronto para uso! Você tem duas formas principais de interagir com ele:

- **Pelo Navegador (Web UI):** Abra **`https://192.168.2.255`** no navegador da sua máquina física. Vai aparecer um aviso de certificado inválido (pois o certificado foi gerado pela sua própria CA agora mesmo), mas você pode prosseguir com segurança. O usuário é `admin` e a senha é a `ipaadmin_password` que você definiu no playbook.
    
- **Pelo Terminal:** Digite `kinit admin` e insira sua senha. Isso vai gerar um ticket Kerberos e liberar os comandos administrativos da ferramenta (como o comando `ipa user-add`).
    

Qual vai ser o próximo passo do nosso laboratório: quer explorar a criação de usuários e políticas de senha, ou prefere já tentar ingressar uma outra máquina cliente nesse novo domínio?

Consegui criar um usuário pelo terminal !

  

```bash
[root@idm etc]# ipa user-add

First name: Guilherme

Last name: Conti Teixeira

User login [gconti teixeira]: guilhermect

------------------------

Added user "guilhermect"

------------------------

  User login: guilhermect

  First name: Guilherme

  Last name: Conti Teixeira

  Full name: Guilherme Conti Teixeira

  Display name: Guilherme Conti Teixeira

  Initials: GC

  Home directory: /home/guilhermect

  GECOS: Guilherme Conti Teixeira

  Login shell: /bin/sh

  Principal name: guilhermect@TESTE.LOCAL

  Principal alias: guilhermect@TESTE.LOCAL

  Email address: guilhermect@teste.local

  UID: 562400003

  GID: 562400003

  Password: False

  Member of groups: ipausers

  Kerberos keys available: False

Boa, Guilherme! Mandou muito bem. A criação foi um sucesso.

Se você observar com atenção a saída do comando, vai notar dois detalhes importantes no final:

- `Password: False`
    
- `Kerberos keys available: False`
```
    

Isso significa que a conta existe, mas o usuário ainda não pode fazer login em lugar nenhum porque não tem uma credencial válida. Esse comportamento de criar a conta desativada/sem chaves por padrão é um ótimo exemplo prático do princípio de _Secure by Default_, um conceito de Identity and Access Management (IAM) que é essencial dominar para a prova do Security+.

### O Próximo Passo: Definir a Senha

Para ativar a sua conta recém-criada e gerar as chaves do Kerberos, você precisa definir uma senha inicial.

Ainda no terminal, execute o comando abaixo:

Bash

```
ipa passwd guilhermect
```

O sistema vai pedir para você digitar a nova senha duas vezes. A partir desse momento, a conta estará ativa e pronta para uso.

Para testar se a autenticação está funcionando perfeitamente de forma local, você pode tentar emitir um ticket Kerberos para esse novo usuário:


```bash
kinit guilhermect
```

_(Ele vai pedir a senha que você acabou de criar. Se não retornar nenhum erro, a autenticação foi um sucesso!)_

Como o nosso servidor já está de pé e com um usuário criado, qual é a sua ideia agora: quer aprender a ingressar o seu notebook com Linux Mint neste domínio para testar o login de forma remota, ou prefere criar alguns grupos de acesso dentro do próprio servidor?

Agora eu posso pegar um roteador velho só pra fazer a distribuição DHCP desse meu domínio de teste que acabei de criar em uma rede local mesmo ? Por exemplo, conectar a máquina que tem o Proxmox instalado no roteador com o DHCP habilitado e do outro lado do roteador, conectar um cabo de rede para pegar esse DHCP ? Atenção, não vai existir mais nenhum equipamento na rede, tudo vai ser feito com cabos de redes plugados, literalmente fazendo uma ponte de informações. Servidor com Proxmox instalado, máquina RHEL dentro dele rodando todas as configurações que fizemos, um roteador e um outra máquina desktop com entrado para cabo de rede.

Com certeza, Guilherme! Essa topologia que você desenhou é perfeitamente viável e é a essência de um laboratório de redes físico. Construir redes isoladas assim (frequentemente chamadas de redes _air-gapped_ quando não têm saída para a internet) é uma prática excelente e super segura para testar serviços de diretório.

Você vai usar o roteador velho literalmente como um **Switch** (para conectar os cabos) e como **Servidor DHCP** (para entregar os IPs).

Para que tudo converse perfeitamente com o seu novo domínio `teste.local`, você só precisa ter atenção a três detalhes técnicos críticos na hora de configurar esse roteador:

### 1. Conexões Físicas (Use apenas as portas LAN)

Conecte o cabo do servidor Proxmox e o cabo da máquina desktop nas **portas LAN** do roteador velho (geralmente são as portas amarelas numeradas de 1 a 4). **Deixe a porta WAN/Internet vazia.** Assim, o roteador funcionará apenas como um switch de rede local, criando uma ponte direta entre as máquinas.

### 2. A Máscara de Subrede (O Desafio do /22)

Lembra que configuramos o RHEL com o IP `192.168.2.255/22`? O `/22` equivale à máscara **`255.255.252.0`**. Você precisará entrar na interface de configuração do roteador velho e definir:

- **IP da LAN do Roteador:** Pode ser algo como `192.168.0.1` ou `192.168.2.1`.
    
- **Máscara de Subrede:** Obrigatoriamente `255.255.252.0`.
    
- _Atenção:_ Alguns roteadores residenciais muito básicos "travam" a máscara em `/24` (`255.255.255.0`). Se o seu roteador não permitir alterar a máscara, a máquina desktop pode ter dificuldades de comunicação com o servidor Proxmox/RHEL.
    

### 3. O "Pulo do Gato" do DNS (Fundamental para o IdM)

Esse é o ponto onde muitos laboratórios falham. Quando o roteador entregar o endereço IP via DHCP para a máquina desktop, ele **deve** entregar o IP do RHEL (`192.168.2.255`) como sendo o Servidor DNS primário. Se o roteador velho entregar o próprio IP dele como DNS, a máquina desktop não vai conseguir resolver o nome `teste.local` e não conseguirá ingressar no domínio. Verifique nas configurações de DHCP do roteador se existe a opção de especificar um **DNS Manual** para os clientes.

---

**Uma Alternativa 100% Virtualizada** Como você já tem o Proxmox rodando e domina conceitos de infraestrutura de redes, caso esse roteador físico seja muito limitado e não permita mudar a máscara ou o DNS, uma solução extremamente elegante seria subir uma máquina virtual leve com o **pfSense** dentro do próprio Proxmox. O pfSense assumiria o papel de firewall e servidor DHCP da sua rede de laboratório com controle absoluto, eliminando a dependência do roteador físico (que ficaria atuando apenas como um "switch burro" para conectar os cabos).

Você quer ajuda para planejar o escopo de IPs (DHCP Pool) que vamos configurar nesse roteador físico, ou prefere dar uma olhada em como subir um pfSense no Proxmox para gerenciar essa rede?