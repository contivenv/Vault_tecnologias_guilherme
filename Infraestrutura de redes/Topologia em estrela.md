---
tags:
  - redes
  - aprendizado
  - modelo_teórico
  - topologia
---

Nessa topologia, sempre irá existir um “dispositivo concentrador”, onde a função dele e distribuir os dados pela rede de maneira que a máquina de origem (remetente) consiga enviar a mensagem para a máquina que ela quer (destinatário). Dessa maneira, existe o dispositivo concentrador HUB onde ele opera na camada 1 do modelo OSI com conexões diretas de cabos de rede distribuindo o pacote de destino para todos, não somente para a máquina que ele quer. Existe também a melhor solução que seria o Switch onde ele opera na camada 2 do modelo OSI. O primeiro motivo para utiliza-lo seria que ele faz a distribuição de maneira que ele lê o quadro, analisa-o e manda somente para o computador de destino. Ele opera dessa maneira como um “distribuidor geral” onde todos os pacotes de dados são recebidos por ele e ele é quem faz essa distribuição. O segundo motivo é que ele usa conexão full-duplex (que significa que recebe e manda dados ao mesmo tempo) gerando assim um desempenho superior ao hub que é somente half-duplex, outro motivo para eles não serem mais utilizados.