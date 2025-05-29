---
tags:
  - redes
  - NIC
---
Interface de rede, seus sinônimos são: porta de rede, placa de rede, NIC (Network Interface Card ou Controller). Permite a conexão do equipamento e a rede. No caso sem fio, vai ser por rádio frequência. Em cabo, um conecto no equipamento da rede. Esse equipamento varia dependendo da arquitetura. O mais usado hoje em dia em rede local é o 8P8C (RJ-45). Em fibra óptica, existem vários tipos.

A interface de rede pode receber um endereço físico. Endereço MAC é definido pelo fabricante do equipamento. Antigamente era feito por uma placa de rede avulsa. Caso precise de mais conexões de portas de redes, precisa adicionar portas de expansão. Se a porta de rede é ethernet e sua internet tem capacidade de transferir dados até 1 Gbit/s, então o padrão menor da sua porta é que vai valer.

Placa de redes wifi podem ser como PCIe ou M.2 para notebooks que são embutidos direto na placa mãe.

Portas de um hubs ou switch não recebem endereço MAC. Exceções switicher gerenciáveis (têm ao menos uma porta com endereço MAC para o acesso remoto) e switches camada 3 (L3).

Switch - portas lan do roteador não disponibilizam endereços MAC, mas a porta WAN recebe um endereço MAC. Tudo vai depender do equipamento que você está usando.![[Interface de rede (NIC).pdf]]