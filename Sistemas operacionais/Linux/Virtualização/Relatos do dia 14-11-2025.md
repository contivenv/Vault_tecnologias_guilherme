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