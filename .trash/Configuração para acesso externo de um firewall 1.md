---
tags:
  - redes
  - firewall
  - pfsense
  - tcc
---
Eu e Murilo conseguimos configurar o pfSense para conectar de forma externa, criando regras de passagem permitidas na rede WAN. Ficou faltando uma configuração que nós não tínhamos colocado na aba de:

*Sistemas > Avançado > Acessos de administrador*  e navegar até o campo *Porta TCP*, colocando a do nosso firewall que no caso é 6030.![[Pasted image 20250321163759.png]]
- Após isso, desabilitamos a regra *Anti-lockout* na mesma aba.![[Pasted image 20250321163843.png]]

*Agora em Firewall > NAT > Encaminhamento de Portas* seguindo as seguintes configurações de instruções: 

- Interface - WAN
- Protocolo - TCP
- Endereço de Destino - WAN address
- Porta de Destino - 6030
- IP NAT - 192.168.1.1
- Portas NAT - 6030
- Descrição - pfsense - Trieng
![[Pasted image 20250321163929.png]]![[Pasted image 20250321164055.png]]![[Pasted image 20250321164132.png]]
Em seguida seguiremos em *Firewall > Regras > WAN*
![[Pasted image 20250321164549.png]]
![[Pasted image 20250321164629.png]]
![[Pasted image 20250321164720.png]]
Agora seguiremos para *Firewall > Regras > LAN*
![[Pasted image 20250321164823.png]]
![[Pasted image 20250321164857.png]]
![[Pasted image 20250321164915.png]]

Teste as configurações que foram realizadas nas portas que foram colocadas nas regras de liberação do nosso firewall. Mas atente-seao detalhe de onde está vindo a sua conexão para porta WAN, porque nesse caso, teremos que pegar o IP do link que temos que pode ser verificado no site https://meuip.com.br/.  No caso nesse cenário, colocamos a 6030 para este firewall. Acesse o site https://www.testeportas.com.br/ e realize o teste. No resultado, espera-se que obtenha o resultado *"aberto"*.![[Pasted image 20250321165434.png]]
Tente acessar o pfSense de uma rede externa agora e verifique se tem o acesso.![[Pasted image 20250321165727.png]]