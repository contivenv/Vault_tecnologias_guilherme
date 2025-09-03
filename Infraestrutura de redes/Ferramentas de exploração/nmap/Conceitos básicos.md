---
tags:
  - nmap
  - redes
  - cibersegurança
  - exploração_de_vulnerabilidades
  - escaneamento_de_rede
---
Existem alguns tipos de retorno de pacotes que recebemos quando fazemos o escaneamento comum na rede usando o nmap com comando nmap -a -T4 para obter informações.

A saída do Nmap é uma lista de alvos escaneados, com informações adicionais de cada um dependendo das opções utilizadas. Uma informação chave é a “tabela de portas interessantes”. Essa tabela lista o número da porta e o protocolo, o nome do serviço e o estado. ==O estado pode ser `aberto (open)`, `filtrado (filtered)`, `fechado (closed)`, ou `não-filtrado (unfilterd)`.== 

## Open
Aberto (open) significa que uma aplicação na máquina-alvo está escutando as conexões/pacotes naquela porta. 

## Filtered
`Filtrado (filtered)` significa que o firewall, filtro ou outro obstáculo de rede está bloqueando a porta de forma que o Nmap não consegue dizer se ela está `aberta (open)` ou `fechada (closed)`. 

## Closed
Portas `fechadas (closed)`não possuem uma aplicação escutando nelas, embora possam abrir a qualquer instante. Portas são classificadas como `não filtradas (unfiltered)`quando elas respondem às sondagens do Nmap, mas o Nmap não consegue determinar se as portas estão abertas ou fechadas. 

O Nmap reporta as combinações `aberta|filtrada (open|filtered)`e `fechada|filtrada (closed|filtered)`quando não consegue determinar qual dos dois estados descrevem melhor a porta. A tabela de portas também pode incluir detalhes de versão de software quando a detecção de versão for solicitada. Quando um scan do protocolo IP é solicitado (`-sO`), o Nmap fornece informações dos protocolos IP suportados ao invés de portas que estejam abertas.