---
tags:
  - aprendizado
  - VPN
  - cibersegurança
  - criptografia
  - redes
---

A criptografia da VPN só é 100% garantida se usarmos ele dentro do túnel de conexão com outra o outro lado também usando uma VPN. Fora desse túnel, sem garantia de criptografia. Um exemplo é: se utilizarmos somente o servidor VPN para utilizamos para determinado site ou serviço, ele vai estar protegido somente nessas duas ocasiões hipotéticas. O restante dos outros sites que acessarmos será de forma direta, por fora do túnel. Se vai ser criptografado ou não, só vai depender do servidor da outra ponta. Como podemos saber se a criptografia está ativa ou não no site que estamos acessando ? Vendo se ela possuí um “cadeado” do lado esquerdo da URL no site.

Se conectarmos, por exemplo, em um rede de um cafeteria onde o Wi-Fi é liberado e não precisa de senha, isso quer dizer que essa rede interna não possuí nenhum tipo de criptografia. Ou seja,

Seu computador → roteador → internet

Se acessamos um servidor VPN, o servidor VPN acessa outro servidor sem criptografia ativa, então somente metade do caminho está 100% seguro. Mas isso é melhor que nada.

Nossa máquina → Servidor VPN → servidor sem criptografia