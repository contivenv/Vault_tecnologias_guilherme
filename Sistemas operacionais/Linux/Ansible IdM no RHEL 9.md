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

**2. Edite o arquivo `/etc/hosts`:**

Abra o arquivo com seu editor favorito (ex: `sudo nano /etc/hosts` ou `sudo vi /etc/hosts`) e adicione o IP e o FQDN. Ele deve ficar parecido com isso:

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

**1. Crie o arquivo de inventário (`inventory.ini`):**

Crie um arquivo chamado `inventory.ini` e adicione o conteúdo abaixo. A instrução `ansible_connection=local` avisa ao Ansible para rodar os comandos na própria máquina, sem tentar abrir uma nova conexão SSH.

Ini, TOML

```
[ipaserver]
idm.teste.local ansible_connection=local
```

**2. Crie o Playbook (`install-idm.yml`):**

Crie um arquivo chamado `install-idm.yml`. É aqui que declaramos como o domínio deve ser construído.

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