---
tags:
  - roteador
  - hardware
  - aprendizado
  - redes
  - switch
---
Roteador trabalha na camada 3 do modelo OSI, camada rede, então ele consegue analisar o que tem dentro dessa camada. O roteador consegue ler os campos de endereçamento lógicos. Os switchs conseguem ler os campos de endereçamentos físicos.

Switchs camadas três (L3) - é um tipo especial de switch, ele opera de forma similar ao roteador, conecta só a rede maior, não tem conexão com o rede externa, as duas portas são LANs. Normalmente usado com a técnica de redes virtuais (VLANs). Pegaremos um exemplo do switch camada 2: fisicamente, há um único switch, mas logicamente, há dois switches e são acessados por equipamentos diferentes. Não há comunicação entre estas redes ! Vão operar como se fossem dois switches separados. Com o switch de camada 3 e tiver 24 portas, 12 portas para uma rede e 12 portas para outra, podem se comunicar, funciona como um roteador.