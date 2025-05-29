---
tags:
  - hardware
  - switch
  - pontos_positivos_e_negativos
  - redes
  - cibersegurança
  - aprendizado
  - VLAN
  - rede_virtual
---
O switch opera na camada 2, por conta disso, ele pode ler os quadros que estão sendo trafegados na rede, ele pode fazer a analise dos dados. Hub a camada física não tem essa capacidade, pois o Hub replica isso para todas as suas portas, já o switch tem a capacidade de realizar o processo apenas em uma porta. Cada conexão feita pelo switch terá o seu próprio domínio de colisão, porém, todas as portas pertencem ao mesmo domínio de broadcast. Switch tem uma fase de aprendizado, se chama inundação (flooding). Quando você liga um switch, ele não sabe o endereço físico conectados a sua porta. Replicar para todas as portas esse quadro até um responder ele, quando responde, qual respondeu é aquela que tem o endereço ligado a ela.

Em empresas de grande porte e em data centers são usados o que chamamos de Rede Virtual (VLAN). Configurar por software qual porta vai fazer parte de qual rede. Tecnicamente, criamos dois domínios de broadcast, duas redes separadas. Isolar redes no mesmo switch, por exemplo um switch de 24 portas, 12 em uma rede e 12 em outra, não precisa ser portas contínuas, podem ser as que escolhemos. Isso nos dá flexibilidade e economia de dinheiro.

As pontes também operam na camada 2 do modelo OSI. Hoje não se é mais usado a ponte, pois o mesmo era usado para cabo coaxial.
Para complemento dessa aula, anexarei um link aqui de um artigo explicando o que é uma [switch]([O que é um switch de rede? | Juniper Networks](https://www.juniper.net/br/pt/research-topics/what-is-a-network-switch.html)).
![[Pasted image 20250123134047.png]]