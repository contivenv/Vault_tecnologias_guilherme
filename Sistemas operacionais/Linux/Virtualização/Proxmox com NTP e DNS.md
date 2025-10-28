---
tags:
  - virtualização
  - proxmox
  - linux
  - debian
  - aprendizado
---
Uma lição importante que nunca vou esquecer é sobre os sistemas de virtualização, especialmente o [Virtual Environment (VE)](https://www.proxmox.com/en/products/proxmox-virtual-environment/overview) e também outros softwares como, por exemplo, [VirtualBox](https://www.oracle.com/br/virtualization/virtualbox/) e [KVM](https://www.redhat.com/pt-br/topics/virtualization/what-is-KVM). Essa experiência fez com que eu entendesse melhor como esses sistemas funcionam e sua importância na virtualização.

Ao aplicar regras específicas de rede, percebi que o ideal é que a árvore de decisões esteja no sistema VE e não dentro das máquinas virtuais. Isso me lembrou o funcionamento do [Active Directory](https://learn.microsoft.com/pt-br/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview), que possui uma árvore hierárquica de permissões. O Proxmox segue essa lógica, distribuindo as configurações de rede para as VMs a partir do próprio VE.

Nesse cenário, passei por um problema simples, mas que me deu bastante trabalho: sincronizar o servidor [NTP](https://ntp.br/conteudo/ntp/) em uma VM com [Debian](https://www.debian.org/intro/about.pt.html), que seria o novo domínio da empresa. Para conseguir isso, foi necessário garantir dois pontos essenciais: manter o NTP sincronizado e o [DNS](https://www.cloudflare.com/pt-br/learning/dns/what-is-dns/) configurado corretamente. Sem isso, métodos de autenticação como [Winbind](https://docs.redhat.com/pt-br/documentation/red_hat_enterprise_linux/8/html/integrating_rhel_systems_directly_with_windows_active_directory/connecting-rhel-systems-directly-to-ad-using-samba-winbind_integrating-rhel-systems-directly-with-active-directory#overview-of-direct-integration-using-samba-winbind_connecting-rhel-systems-directly-to-ad-using-samba-winbind), [Kerberus](https://www.fortinet.com/br/resources/cyberglossary/kerberos-authentication) e outros simplesmente não funcionariam.

Comecei tentando configurar o NTP dentro da própria VM no Proxmox, mas nada dava certo. Foi quando usei o tutorial do site [ntp.br](https://ntp.br/guia/linux/) para configurar o serviço [chrony](https://chrony-project.org/) no Linux, e tive sucesso! A partir daí, consegui replicar essa configuração para todas as máquinas dentro do meu ambiente Proxmox automaticamente. Agora, todas as VMs sincronizam o horário diretamente pelo servidor Proxmox, eliminando a necessidade de configurar uma por uma.