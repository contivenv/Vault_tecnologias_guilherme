---
tags:
  - Andrew_S_Tanenbaum
  - cibersegurança
  - redes
  - autenticação
  - não_repúdio
---
Para começarmos a falar do que é a segurança de rede, primeiro precisamos falar sobre os três principais pilares dela. A primeira coisa que me veio a mente é Pai, filho e espirito santo, a divina trindade. São elas conhecidas como a sigla CIA (Confidentiality, Integrity, Availability). Tenebaum até brinca na escrita desse capítulo sobre o acronimo dessa sigla que não cumpriu com seu papel no passodo, fazendo a clara referência ao serviço secreto dos EUA.

> ‘talvez não seja muito boa, uma vez que o outro significado comum desse acrônimo não tenha sido tão cuidadoso na violação dessas propriedades no passado.’
> —Andrew Tanenbaum, Nick Feamster e David Wetherall, “Redes de Computadores 6ª Edição”

**Confidencialidade**: isso costuma ser tratado com base nas informações que queremos manter longe do usuário, ou seja, tudo aquilo que ele não pode ver, ter acesso ou compartilhar para fora da rede. Isso é o que costuma vir na nossa mente em primeiro lugar quando falamos de segurança de redes, diz Tenenbaum.

**Integridade**: saber que você realmente recebeu a informação do ponto A ao ponto B sem ser modificada.

**Disponibilidade**: se trata de tornar serviços ou erros acontecendo, quebras de sistemas ou na infraestrutura e situações de sobrecarga ou erros deliberados na configuração.

Além dessa santa trindade (ou triunvirato como o autor fala no livro, achei essa palavra com um significado bem interessante) que me veio a cabeça logo de cara quando li esse capítulo do livro, existem outros tipos de segurança uma pouco mais específicas, mas que o mercado já usa a muito tempo para reforçar essas medidas, como por exemplo, a forma de autenticação de múltiplos fatores e os termos de não repúdio. E o que cada um deles é ?

**Autenticação**: cuida do processo de determinar com quem você está se comunicando antes de revelar informações sigilosas ou entrar em uma transação comercial.

**Não repúdio**: trata de assinaturas como seu ponto principal: como provar que seu cliente realmente fez um pedido eletrônico de dez milhões de unidades de um produto com preço unitário de 89 centavos quando mais tarde ele afirmar que o preço era 69 centavos? Ou talvez ele afirme que nunca efetuou nenhum pedido, depois de ver que uma empresa chinesa está inundando o mercado com esses mesmos produtos por 49 centavos.

Esses fatos que acompanhamos hoje pode ser facilmente adotados em outros sistemas foras da computação também. Tenebaum dá um exemplo de desconto de cheques. Pediu para nos compara com um desconto de cheque original na segunda-feira e uma fotocópia de um na terça-feira.