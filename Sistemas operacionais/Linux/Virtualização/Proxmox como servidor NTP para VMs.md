---
tags:
  - chrony
  - NTP
  - configuração
  - IP
  - MAC
  - adaptadores_de_rede
---
Mudamos os IP do Proxmox de 192.168.3.21 para 192.168.1.197 que está setado com a interface `eno2` e da VM para Gaia 192.168.1.211 para 192.168.1.202 utilizando o adaptador de rede virtual `vmbr0`.

##### 1. Mapeamento completo das interfaces do Proxmox

```
eno1
	MAC (24:b6:fd:f8:2a:d6)
		IP (192.168.1.198)
```

```
eno2
	MAC (24:b6:fd:f8:2a:d8)
		IP (192.168.1.197)
```

Organizei e coloquei as descrições desses adaptadores em nosso firewall para deixarmos marcado.

Reiniciei o Proxmox via interface web agora para aplicar as alterações de rede. O `eno2` já estava setado, reiniciei apenas para aplica do adaptador de rede `eno1`.

O servidor de virtualização Proxmox está UP de novo com os endereços de IP ajustados corretamente para as duas placas de rede agora com sucesso.![[Pasted image 20251105092319.png|725]]
Somente a `eno2` está como UP pois deixamos uma só "ativa" para puxar a rede. Antes de realizarmos as configurações no firewall pfSense, o `eno1` não estava aparecendo.

##### 2. Proxmox como servidor NTP
Iremos agora apontar para o Proxmox que está com o NTP do Brasil configurado.

1. Iniciar a VM do Debian novamente após a reinicialização do Proxmox.
2. Entrar no VM do Debian via ssh com o usuário de `root`.![[Pasted image 20251105092857.png]]
3. Editar o arquivo do chrony com o comando abaixo exatamente dessa maneira:
```bash
nano /etc/chrony/chrony.conf
```

```bash
# Welcome to the chrony configuration file. See chrony.conf(5) for more
# information about usable directives.

# Use Debian vendor zone.
server a.st1.ntp.br iburst nts
server b.st1.ntp.br iburst nts
server c.st1.ntp.br iburst nts
server d.st1.ntp.br iburst nts
server gps.ntp.br iburst nts

#VMS on
allow 192.168.0.0/22

#Força o Proxmox para se indentificar como servidor
local stratum 10

# Use time sources from DHCP.
sourcedir /run/chrony-dhcp

# Use NTP sources found in /etc/chrony/sources.d.
sourcedir /etc/chrony/sources.d

# This directive specifies the location of the file containing ID/key pairs for
# NTP authentication.
keyfile /etc/chrony/chrony.keys

# This directive specifies the file into which chronyd will store the rate
# information.
driftfile /var/lib/chrony/chrony.drift

# Save NTS keys and cookies.
ntsdumpdir /var/lib/chrony

# Uncomment the following line to turn logging on.
#log tracking measurements statistics

# Log files location.
logdir /var/log/chrony

# Stop bad estimates upsetting machine clock.
maxupdateskew 100.0

# This directive enables kernel synchronisation (every 11 minutes) of the
# real-time clock. Note that it can't be used along with the 'rtcfile' directive.
rtcsync

# Step the system clock instead of slewing it if the adjustment is larger than
# one second, but only in the first three clock updates.
makestep 1 3

# Get TAI-UTC offset and leap seconds from the system tz database.
# This directive must be commented out when using time sources serving
# leap-smeared time.
leapseclist /usr/share/zoneinfo/leap-seconds.list

# Include configuration files found in /etc/chrony/conf.d.
confdir /etc/chrony/conf.d
```
5. Após Configurar o arquivo de configurações do chrony no Proxmox, verifique se o o sistema do NTP está sendo sincronizado em `System clock synchronized`. A descrição a frente tem que estar como `yes`.
```bash
timedatectl
```
![[Pasted image 20251105093556.png]]
##### 3. Apontando a VM para enxergar o Proxmox como servidor NTP.
1. Após realizar a verificação do NTP no servidor do Proxmox, vamos agora editar o arquivo do chrony na VM do Debian dessa forma para que a VM possa enxergar o Proxmox como servidor NTP. Para isso abra o arquivo de configurações do chrony em `nano /etc/chrony/chrony.conf` e configure apontando para o servidor dessa maneira:
```bash
# Welcome to the chrony configuration file. See chrony.conf(5) for more
# information about usable directives.

# Use Debian vendor zone.
server 192.168.1.197

# Use time sources from DHCP.
sourcedir /run/chrony-dhcp

# Use NTP sources found in /etc/chrony/sources.d.
sourcedir /etc/chrony/sources.d

# This directive specifies the location of the file containing ID/key pairs for
# NTP authentication.
keyfile /etc/chrony/chrony.keys

# This directive specifies the file into which chronyd will store the rate
# information.
driftfile /var/lib/chrony/chrony.drift

# Save NTS keys and cookies.
ntsdumpdir /var/lib/chrony

# Uncomment the following line to turn logging on.
#log tracking measurements statistics

# Log files location.
logdir /var/log/chrony

# Stop bad estimates upsetting machine clock.
maxupdateskew 100.0

# This directive enables kernel synchronisation (every 11 minutes) of the
# real-time clock. Note that it can't be used along with the 'rtcfile' directive.
rtcsync

# Step the system clock instead of slewing it if the adjustment is larger than
# one second, but only in the first three clock updates.
makestep 1 3

# Get TAI-UTC offset and leap seconds from the system tz database.
# This directive must be commented out when using time sources serving
# leap-smeared time.
leapseclist /usr/share/zoneinfo/leap-seconds.list

# Include configuration files found in /etc/chrony/conf.d.
confdir /etc/chrony/conf.d
```
2. Reinicie o serviço do chrony depois de salvar as configurações do arquivo `chrony.conf`
```bash
systemctl restart chrony
```
3. Force uma primeira transação de pacote para comunicação da VM com o Proxmox.
```bash
chronyc burst 4/4
```
4. Verifique agora a saída do comando `chronyc sources -v` onde o servidor em que o IP aponta tem que estar marcado com \* antes do IP. Isso significa sucesso, que está puxando as informações desse IP que foi setado nele.
```bash
chronyc sources -v
```

```bash

  .-- Source mode  '^' = server, '=' = peer, '#' = local clock.
 / .- Source state '*' = current best, '+' = combined, '-' = not combined,
| /             'x' = may be in error, '~' = too variable, '?' = unusable.
||                                                 .- xxxx [ yyyy ] +/- zzzz
||      Reachability register (octal) -.           |  xxxx = adjusted offset,
||      Log2(Polling interval) --.      |          |  yyyy = measured offset,
||                                \     |          |  zzzz = estimated error.
||                                 |    |           \
MS Name/IP address         Stratum Poll Reach LastRx Last sample               
===============================================================================
^* 192.168.1.197                 2   7   377    87    +15us[  +36us] +/- 5251us
```

5. Verifique também usando o comando `timedatectl`.![[Captura de tela de 2025-11-05 11-41-01.png]]