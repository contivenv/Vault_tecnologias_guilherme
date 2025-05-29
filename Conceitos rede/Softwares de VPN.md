---
tags:
  - aprendizado
  - VPN
  - redes
  - criptografia
  - cibersegurança
---

## VPN TLS

É um plugin que você instalar no seu navegador de internet que cria um túnel de acesso a um servidor VPN. Ele é usado somente em sites específicos, por exemplo algum site que deseja acessar de filmes ou algo assim. Ele não cria um túnel seguro para todos os sites que você for acessar, isso é porque é em relação ao trafego ele é limitado já que ele vai estar amarrado ao navegador web, logo sem VPN em email, transferência de arquivos, terminal, etc. Ele fica na camada 6 do protocolo OSI.

## VPN IPsec

Pode ser usado tanto para site-to-site quanto para a sua máquina em geral, para acessar servidores por exemplo. Esse sim pode ser usado para todos os tráfegos possíveis na rede, ao contrário do VPN TLS que temos limitações amarradas ao navegador. Por ele operar na camada 3 do modelo OSI que é a diretamente de rede, ele pode fazer tudo isso sem nenhuma limitação. Consegue transportar todo o trafego da sua máquina.