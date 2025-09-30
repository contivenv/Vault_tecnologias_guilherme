# Instalação do Debian

- Baixar o debian: https://www.debian.org/index.pt.html

- Fazer a instalação com interface gráfica
 - selecionar a linguagem 
 - selecionar layout do teclado

- dar um nome a máquina
- dar um nome ao domínio 

- configurar a senha do usuário root
- criar um novo usuário com senha

- configurar o relógio
- escolher o Estado ( São Paulo)

- selecionar o particionamento do disco
	- Assistido - usar disco inteiro e configurar LVM
- selecionar o disco e selecionar as partições
	- Partições /home, /var e /tmp separadas

- finalizar e escrever mudanças no disco
- escrever as mudanças nos discos? Sim
- participar do concurso de utilização de pacotes? Não

- Seleção de software
	- servidor ssh
	- utilitários de sistema padrão
- Instalar o carregador de inicialização GRUB? Sim
	- dispositivo onde será instalado:
	- /dev/das
Finalizar instalação do debian

## Configurações e instalação do Samba

- atualizar o debian:

```bash
apt-get -y update && apt-get -y dist-upgrade
```
## configuração de interfaces

```bash
vim /etc/network/interfaces
```

```bash
static (se necessário)
address 192.168.x.x
netmask 255.255.255.0
gateway 192.168.y.y
```

Reiniciar servidor: reboot

# Instalação e configuração Samba

```bash
apt-get -y install samba
```

```bash
apt-get -y install winbind
```

## Parando serviços do samba

```bash
systemctl stop smbd nmbd
```

## Desabilitando start dos serviços ao iniciar a máquina

```bash
systemctl disable smbd nmbd
```
## Parar resolução de nomes automática e endereçamentos

```bash
systemctl stop systemd-networkd
```

```bash
systemctl disable systemd-nertworkd
```

## Remoção do arquivo de configuração padrão do samba

```bash
rm /etc/samba/smb.conf
```
## Provisionar um domínio samba

```bash
samba-tool domain provision --use-rfc2307 --interactive
```

- Realm = nome completo do domínio (TESTE.EMPRESA.LOCAL)
- Domain = domínio (TESTE)
- Server Role = Que tipo de servidor vai ser
- DNS backend = dns que será usado (Por padrão é utilizado o do próprio samba)
- DNS forwarder IP address = Qual ip o samba vai usar caso ele não consiga resolver nomes de domínio ( pode-se usar o dns do google 8.8.8.8)

## Habilitação do samba como controlador de domínio 

```bash
systemctl unmask samba-ad-dc
```

```bash
systemctl enable samba-ad-dc
```

```bash
systemctl start samba-ad-dc
```

## Configuração do arquivo resolv

 ```bash
 vim /etc/resolv.conf
```

- remover todas as linhas
 - nameserver 127.0.0.1 
- Essa configuração é feita para usar o próprio dns do samba (localhost)

## Verificação Winbind

```bash
systemctl -a | grep -iE winb
```
## Parar serviços do Winbind e samba

```bash
systemctl stop winbind
```

```bash
systemctl stop samba
```

## Iniciar apenas o samba como controlador de domínio

```bash
systemctl start samba-ad-dc
```

## Verificar se o samba está rodando 

```bash
systemctl status samba-ad-dc
```

## Teste de DNS

```bash
nslookup (nome completo do dominio)
```
## Acesso ao Samba

```bash
apt-get -y install smbclient
```

```bash
smbclient -L localhost -N 
```

- executa o smbclient em modo browser para verificar compartilhamentos do samba
- SMB1 desabilitado por questões de segurança

## Acesso como cliente a pasta logon (apenas para verificação)

```bash
smbclient //localhost/netlogon -UAdministrator -c 'ls'
```

- Irá listar duas pastas vazias

## Consulta dns especifica de localização de servidor ldap na rede

```bash
host -t SRV _ldap._tcp.nome.completo.do.dominio
```
	
- É esperado que a saída mostre o servidor ad

## Instalação do Kerberos

```bash
 apt-get -y install krb5-user
```

```bash
rm /etc/krb5.conf
```

```bash
cp /var/lib/samba/private/krb5.conf /etc/krb5.conf
```

#### para ver o conteúdo do krb5.conf

```bash
less /etc/krb5.conf
```

## Emissão de ticket kerberos para o user Administrator

```bash
kinit Administrator
```

 ```bash
 klist
```

- Exibe mais informações do ticket gerado
# Gerenciamento pela interface WEB

## instalação e configuração do WEBMIN

https://webmin.com/download/

## Instalação do script

```bash
- curl -o webmin-setup-repo.sh https://raw.githubusercontent.com/webmin/webmin/master/webmin-setup-repo.sh
sh webmin-setup-repo.sh
```
### instalação do webmin

```bash
 apt-get install webmin --install-recommends
```

### Dependências do webmin

```bash
apt-get -y install perl libnet-ssleay-perl openssl libauthen-pam-perl
```

```bash
apt-get -y install libpam-runtime libio-pty-perl apt-show-versions python3 unzip shared-mine-info
```

## Verificar a porta padrão do webmin

```bash
ss -tnlp | grep 10000
```
