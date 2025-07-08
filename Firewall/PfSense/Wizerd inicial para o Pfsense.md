---
tags:
  - pfsense
  - aprendizado
  - firewall
---
## Step 1

Agradecimentos por usar o PfSense e tela de apresentação apresentando também a informação de suporte deles.

## Step 2

Deixar a primeira opção marcada sobre o DNS e as demais caixas de DNS vazias por enquanto.

## Step 3

O Time Server é sempre bom deixar o mesmo, mas se quisermos deixar um do Brasil, podemos usar o [a.ntp.br](http://a.mtp.br) e [b.ntp.br](http://b.ntp.br). Usar sempre do estado que você está, por exemplo São Paulo para correlacionar a hora.

## Step 4

Deixar como DHCP mesmo na primeira opção e não precisamos preencher mais nada por enquanto ali. No final temos duas opções importantes: a primeira caixa marcada ele bloqueia 192.168 / 172.16 e 10.0 na WAN. Neste momento, no comunicamos através na internet modem ou cabo com essas configurações de IP, então desmarcamos. A segunda opção é redes que nem deveriam estar passado na rede, como por exemplo redes de testes e de documentação, deixamos marcado.

## Step 5

Subnet Mask deixamos como 24 mesmo que seria igual a 255.255.255.0. O IP por enquanto deixamos como está mesmo 192.168.1.1.

## Step 6

TROCAR A SENHA do firewall, isso é muito importante.

## Step 7

Reload das configurações.

## Step 8

Aguardar as configurações de reload serem concluídas com êxito.

## Step 9

Clicar em finish para ir para a tela final do firewall. O Wizard inicial foi completo com sucesso.