---
tags:
  - redes
  - aprendizado
  - IP
  - protocolo_de_redes
---
Tem esse nome porque é realmente um tradutor, mas não de línguas estrangeiras, mas sim de destino e origem de pacotes de redes. ==Pega o endereço e porta local para traduzir para um endereço e uma porta GLOBAL==. 

Vamos supor que eu queira enviar uma mensagem para a um servidor que se encontra na internet, sempre vamos ter um "de" e um "para". Essa mensagem vai para o roteador NAT, apaga o nosso endereço de IP do notebook e manda para o servidor com o endereço de IP que está no roteador NAT. Ele faz isso para que a mensagem seja enviada e retorne para o roteador, porque se não for feito dessa maneira, o pacote e descartado e a conexão com o envio da mensagem se encerra ao não identificar quem requisitou a mensagem (no caso, o roteador NAT).
![[Pasted image 20250128132420.png]]
Agora o servidor vai retornar a mensagem destinado ao roteador NAT que vai "de": 195.48.2.1 "para": 200.158.47.2. Quando o roteador NAT receber essa mensagem do servidor, automaticamente ele vai traduzir essa mensagem pro endereço de IP do notebook e só aí ele vai enviar para o notebook que requisitou o envio dessa mensagem.
![[Pasted image 20250128132504.png]]
Todos os dispositivos na rede local compartilham o mesmo IP válido na internet. Todos compartilham o endereço IP do roteador, assim gastando apenas um IP.

Após a requisição da informação que foi enviado do nosso notebook para internet e foi transmitida pelo roteador NAT, o servidor irá nos responder com o destino (200.1.8.20) e origem (171.1.8.5). O roteador vai olhar na tabela de NAT dele e ver a quem esse pacote pertence, que seria para o 192.168.0.17, ele então vai pegar, fazer a tradução e mandar para o nosso notebook de novo.
![[Pasted image 20250128102725.png]]
![[Pasted image 20250128102754.png]]
**Observação:** percebe que isso foi gerado todo um processo no roteador de uso de memória e processamento ? Pois então, quanto mais máquinas estiverem atrás desse equipamento, mais vai ser o uso do roteador nesses quesitos.
##### Vantagens
- **Economia de IPs válidos:** usando um IP válido somente para várias máquinas de uma vez, literalemnte mascarando a rede.
- **Segurança:** um hacker tenta invadir a minha rede mandando um pacote na minha rede (destino 200.1.8.20) é o único endereço que ele consegue enxergar (porta 18336). Porém, o IP de origem é um IP que o roteador nunca viu, então quando esse pacote chegar no roteador, ele vai olhar na tabela de NAT e vai chegar a conclusão de que ninguém acessou ou requereu nada desse endereço de IP, então logo após essa verificação, o roteador vai descartar esse pacote. O hacker não vai ter acesso a nenhuma máquina real efetiva dentro da rede. Isso só vai ser válido se alguém de dentro pediu uma requisição, mesmo o hacker crie um pacote com o endereço de origem correspondente ao da máquina (171.1.8.5), ele não teria sucesso, pois até chegaria na máquina, só que na hora da máquina retornar, ela retornaria ao servidor e não ao hacker. O servidor não entendendo essa requisição, ele descarta isso. Dessa forma criamos uma proteção no sentido de que se ninguém requisitou nada a internet, logo não vai ser entregue esse pacote.
##### Desvantagens
- **VPN:** Se não for um protocolo de VPN que entenda o que é NAT, quando o notebook for fazer a conexão, ele vai dizer: "Eu sou o notebook Dell; sou o usuário Guilherme; minha senha é XXXXX e meu IP é 192.168.0.17". Quando passar pelo roteador e ele fizer o NAT, o servidor de VPN não vai entender porque ao fazer o NAT, o IP que foi entregue para o servidor de VPN foi o 200.1.8.20, sendo assim, ele não entende esse IP e fecha a conexão. Precisa ter um protocolo de VPN que seja amigável ao NAT, porque senão esse tipo de processo não vai funcionar.
![[Pasted image 20250128110515.png]]
- **Sites seguros:** Se nós como administradores do roteador configurarmos para tudo que chegar na minha porta de destino 80 vamos mandar para o 192.168.0.11:80. Então vemos que não existe um IP de definição de origem, qualquer origem que for da porta 80 desse IP, o roteador vai fazer o NAT e trocar, vai de 200.1.8.20 para 192.168.0.11 e mandar para o servidor e quando tiver a resposta, vai desfazer o NAT e mandar adiante.![[Pasted image 20250128113900.png]]
**Observações relevantes:**
- Se não fosse o NAT, desde a época de 90 já teriam acabado.
- Na Ásia eles existem 4 níveis de NAT, isso é bem complexo.
- O NAT impede que o hacker acesse a máquina diretamente pelo IP. Ele querendo ou não, é uma proteção indireta.



Vídeos para consulta do assunto sobre NAT
[O que é NAT (Network Address Translation)](https://www.youtube.com/watch?v=BSe7EgvDB6Q&t=982s)
[Redes de computador - O que é NAT? (Network Address Translator)](https://www.youtube.com/watch?v=z26pX50T168)

Artigos sobre NAT
[Como a NAT funciona? - Cisco](https://www.cisco.com/c/pt_br/support/docs/ip/network-address-translation-nat/26704-nat-faq-00.html#:~:text=Basicamente%2C%20a%20NAT%20permite%20que,qualquer%20situa%C3%A7%C3%A3o%20fora%20da%20rede.)
[O que é tradução de endereços de rede (NAT)](https://www.checkpoint.com/pt/cyber-hub/network-security/what-is-network-address-translation-nat/)?
[O que é NAT e como funciona este protocolo?](https://falati.com.br/o-que-e-nat/)