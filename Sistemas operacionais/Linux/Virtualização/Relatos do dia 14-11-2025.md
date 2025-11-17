---
tags:
  - AD
  - samba
  - firewall
  - IP
  - linux
  - configurações
  - domínio
  - DNS
---
Na empresa, fizemos as configurações servidor de controlador com os direcionamentos de que o Proxmox é o servidor de virtualização principal, onde dentro dele existe uma VM que vai hospedar o domínio AC DC. Qual foi o problema em tudo isso partindo do ponto que já estava configurado ?

### 1. Configurações de nomes do DC

Em nossos arquivos de configurações, colocamos o mesmo nome do controlador de domínio do que o anterior que já tínhamos rodando. Estávamos usando **roqueimoveis.intra** e colocamos exatamente o mesmo nome no domínio atual que fomos subir. Chegamos a conclusão que seria melhor dar um nome diferente para nos orientarmos melhor e realizar a configuração desse novo DC, por que logo fazendo esse tipo de direcionamento de nomes 
, isso pode causar conflitos se não desativarmos o outro. Fiz uma pesquisa sobre esse problema e cheguei a esse conteúdo sobre [Problemas de Replica no Active Directory](https://www.youtube.com/watch?v=yBqLxkJXQrI).

### 2. Configurações inválidas de IP na rede

Estávamos configurando o servidor Debian com dois endereços de IP, causando confusão no direcionamento de informações e procuras no mesmo. Estamos utilizando o `192.168.1.202` via DHCP (que estava sendo bloqueado no firewall) e o `192.168.2.56` como serviço de resolução de nome. Isso impactou em erros graves de resolução de nome e procura sobre nosso servidor DC. O modo correto de se seria apenas realizar a configuração para o endereço de IP `192.168.2.56` desde o início.

### 3. Mais de um cabo de rede conectado no servidor Proxmox

Deixamos por algum motivo que não lembro dois cabos de rede no conectar Proxmox onde essas duas interfaces de rede física dele estavam liberadas no firewall pfsense. Após tirarmos uma delas, o VM do servidor Debian conseguiu ser fixada com o IP `192.168.2.56` com sucesso.

### 4. Configurações de arquivos dentro do servidor Debian

Depois de realizarmos essas mudanças listadas no tópico acima, precisaríamos mudar todos os arquivos de configurações que fizermos anteriormente para que dessa maneira, nosso servidor Debian seja o sistema DC com sucesso, sendo reconhecido na rede e para as máquinas com Windows que tentarem ingressar no domínio `roque.intra`. 

Fontes


Aqui estão **links para referência** que sustentam a questão de usar o mesmo nome de domínio ou controlador de domínio, ou ter conflitos de nomes em ambientes de AD. Você pode usá-los como bibliografia:

---

**Bibliografia**

**1. Configurações de nomes do DC**

- Problemas de Replica no Active Directory https://www.youtube.com/watch?v=yBqLxkJXQrI ([youtube.com](https://www.youtube.com/))

- “Active Directory Naming FAQ” — SambaWiki. Disponível em: [https://wiki.samba.org/index.php/Active_Directory_Naming_FAQ](https://wiki.samba.org/index.php/Active_Directory_Naming_FAQ) ([wiki.samba.org](https://wiki.samba.org/index.php/Active_Directory_Naming_FAQ?utm_source=chatgpt.com "Active Directory Naming FAQ - SambaWiki"))
    
- “Windows DNS registers duplicate SRV records for a DC if its computer name has uppercase letters” — Microsoft Learn. Disponível em: [https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/dns-registers-duplicate-srv-records-for-dc](https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/dns-registers-duplicate-srv-records-for-dc) ([Microsoft Learn](https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/dns-registers-duplicate-srv-records-for-dc?utm_source=chatgpt.com "Windows DNS registers duplicate SRV records for a DC if ..."))
    
- “How to prevent same host name to join domain” — Microsoft Answers. Disponível em: [https://learn.microsoft.com/en-ie/answers/questions/2150697/how-to-prevent-same-host-name-to-join-in-doamin](https://learn.microsoft.com/en-ie/answers/questions/2150697/how-to-prevent-same-host-name-to-join-in-doamin) ([Microsoft Learn](https://learn.microsoft.com/en-ie/answers/questions/2150697/how-to-prevent-same-host-name-to-join-in-doamin?utm_source=chatgpt.com "How to prevent same host name to join in doamin."))
    
- “Major domain controller issues – duplicate SPN …” — Spiceworks Community. Disponível em: [https://community.spiceworks.com/t/major-domain-controller-issues-possible-duplicate-spn-records/308709](https://community.spiceworks.com/t/major-domain-controller-issues-possible-duplicate-spn-records/308709) ([community.spiceworks.com](https://community.spiceworks.com/t/major-domain-controller-issues-possible-duplicate-spn-records/308709?utm_source=chatgpt.com "Major domain controller issues - possible duplicate SPN ..."))

**2. Configurações inválidas de IP na rede**

- “Registering multiple server interfaces in DNS” — Spiceworks Community. Disponível em: [https://community.spiceworks.com/t/registering-multiple-server-interfaces-in-dns/717987](https://community.spiceworks.com/t/registering-multiple-server-interfaces-in-dns/717987) ([community.spiceworks.com](https://community.spiceworks.com/t/registering-multiple-server-interfaces-in-dns/717987?utm_source=chatgpt.com "Registering multiple server interfaces in DNS. - Networking"))
    
- “Dual nic same subnet – r/Proxmox” — Reddit. Disponível em: [https://www.reddit.com/r/Proxmox/comments/1i9qcj0/dual_nic_same_subnet/?utm_source=chatgpt.com](https://www.reddit.com/r/Proxmox/comments/1i9qcj0/dual_nic_same_subnet/?utm_source=chatgpt.com) ([Reddit](https://www.reddit.com/r/Proxmox/comments/1i9qcj0/dual_nic_same_subnet/?utm_source=chatgpt.com "Dual NIC SAME SUBNET : r/Proxmox"))
    
- “Assign multiple IPs to 1 Entry in hosts file” — ServerFault. Disponível em: [https://serverfault.com/questions/429839/assign-multiple-ips-to-1-entry-in-hosts-file](https://serverfault.com/questions/429839/assign-multiple-ips-to-1-entry-in-hosts-file) ([Server Fault](https://serverfault.com/questions/429839/assign-multiple-ips-to-1-entry-in-hosts-file?utm_source=chatgpt.com "Assign multiple IPs to 1 Entry in hosts file"))
    

**3. Mais de um cabo de rede conectado no servidor Proxmox**

- “2 NICs on same subnet – 1 for management 1 for VMs?” — fórum Proxmox. Disponível em: [https://forum.proxmox.com/threads/2-nics-on-same-subnet-1-for-management-1-for-vms.149762/](https://forum.proxmox.com/threads/2-nics-on-same-subnet-1-for-management-1-for-vms.149762/) ([Proxmox Support Forum](https://forum.proxmox.com/threads/2-nics-on-same-subnet-1-for-management-1-for-vms.149762/?utm_source=chatgpt.com "2 NICs on same subnet - 1 for management 1 for VMs?"))
    
- “dual nic issue” — fórum Proxmox. Disponível em: [https://forum.proxmox.com/threads/dual-nic-issue.108647/](https://forum.proxmox.com/threads/dual-nic-issue.108647/) ([Proxmox Support Forum](https://forum.proxmox.com/threads/dual-nic-issue.108647/?utm_source=chatgpt.com "dual nic issue"))
    

**4. Configurações de arquivos dentro do servidor Debian**

- “resolv.conf” — Debian Wiki. Disponível em: [https://wiki.debian.org/resolv.conf](https://wiki.debian.org/resolv.conf) ([wiki.debian.org](https://wiki.debian.org/resolv.conf?utm_source=chatgpt.com "resolv.conf"))
    
- “NetworkConfiguration” — Debian Wiki (pt-BR). Disponível em: [https://wiki.debian.org/pt_BR/NetworkConfiguration](https://wiki.debian.org/pt_BR/NetworkConfiguration) ([wiki.debian.org](https://wiki.debian.org/pt_BR/NetworkConfiguration?utm_source=chatgpt.com "pt_BR/NetworkConfiguration"))
    
- “How can I configure my DNS settings on Debian 12?” — ServerFault. Disponível em: [https://serverfault.com/questions/1145358/how-can-i-configure-my-dns-settings-on-debian-12](https://serverfault.com/questions/1145358/how-can-i-configure-my-dns-settings-on-debian-12) ([Server Fault](https://serverfault.com/questions/1145358/how-can-i-configure-my-dns-settings-on-debian-12?utm_source=chatgpt.com "How can I configure my DNS settings on Debian 12?"))