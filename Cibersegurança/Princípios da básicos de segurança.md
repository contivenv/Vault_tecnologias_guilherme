---
tags:
  - cibersegurança
  - redes
  - Andrew_S_Tanenbaum
  - POLA
  - privilégio
---
Pensar de uma maneira sólida logo de cara o que são os conceitos básicos de segurança, onde em uma era é possível se ter acesso de diversas formas, com camadas da pilha da rede que é difícil saber se todas elas foram devidamente tratadas. Em outras palavras, é difícil garantir a segurança no geral, são diversas camadas, portas, pontos a se explorar. Foi daí que surgiu os melhores conceitos sólidos para um estudo baseado em segurança pelos autores **Jerome Saltzer** e **Michael Schroeder** em 1975 com o Paper intitulado de *The Protection of Information in Computer Systems* (A Proteção da Informação em Sistemas Informáticos). No Paper, eles colocaram os princípios clássicos de segurança:

**Economia de mecanismo**: pelo que entendi, seria mais um esquema de poupar recursos de coisas que não precisam ser complexas. Quanto mais complexo sem necessidade é seu esquema de infraestrutura, mas recursos de tempo será necessário gastar em algo que poderia ser simples para concertar. Um exemplo que é dado sobre esse tópico é o PGP.

> ‘Por exemplo, PGP (Pretty Good Privacy, consulte a Seção 8.11) oferece uma proteção poderosa para o e-mail. No entanto, muitos usuários o consideram complicado na prática e, até agora, ele ainda não foi amplamente adotado. ’
> —Andrew Tanenbaum, Nick Feamster e David Wetherall, “Redes de Computadores 6ª Edição”

**Principio do default seguro**: é muito mais fácil deixar as todas permissões todas bloqueadas do que adivinhas qual a pessoa precisa ou não. Siga da linha de liberação parcial.

> ‘ Digamos que seja preciso organizar o acesso a um recurso. É melhor estabelecer regras explícitas sobre quando alguém pode acessar o recurso do que tentar identificar a condição sob a qual o acesso a ele deve ser negado. Em outras palavras: um default de falta de permissão é mais seguro.’
> —Andrew Tanenbaum, Nick Feamster e David Wetherall, “Redes de Computadores 6ª Edição”



