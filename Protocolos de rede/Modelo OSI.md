---
tags:
  - aprendizado
  - redes
  - modelo_OSI
  - hardware
  - protocolo_de_redes
---
Esse modelo divide um pouco melhor as camadas de rede e é bem mais teórico/didático do que o modelo TCP/IP para entender o funcionamento da rede.
##### Camadas:

1. Física - tudo o que conseguimos chutar por assim dizer. Exemplo: cabos de rede, máquina, switch, processador, tudo o que é físico
2. Enlace - frames (menor unidade possível da informação antes de cair na camada física, para cabo, laca de rede, switch). Exemplo: protocolo ARP, MAC, VLAN, domínios de brodcast
3. Internet - endereços lógicos (Coloca IP na origem, destino, como uso de CEP’s. Roteado)
4. Transporte - transporte lógico de dados (e é aqui que se usa o protocolo TCP, confiança grande, diferente do UDP que não se preocupa se faltar um pedaço dele como por exemplo: VOIP, transmissão de televisão. O UDP é mais rápido que o TCP). Exemplo: endereçamento de portas, diversas comunicações
5. Sessão - manter estado de conexão lógica (nessa camada é possível ser rastreado pelo uso de [cookies de navegação](https://support.google.com/chrome/answer/95647?hl=pt-BR&co=GENIE.Platform%3DDesktop))
6. Apresentação - preparar dados para a visualização
7. Aplicação - exemplo: o navegador Firefox processa o protocolo HTTP, FTP, SMTP, SSH
![[Modelo OSI.pdf]]