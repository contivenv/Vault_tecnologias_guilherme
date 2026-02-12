# Criptografia: o que é e para que serve essa tecnologia?

![Imagem de relógio para tempo de leitura.](https://www.totvs.com/wp-content/themes/totvs-theme/dist/images/clock-blog_930f2f05.svg)

Tempo de leitura: 17 minutos

![](https://www.totvs.com/wp-content/themes/totvs-theme/dist/images/totvs-logo-simbolo_80aeb43a.png)

Escrito por [Equipe TOTVS](https://www.totvs.com/blog/author/digital.totvs/)

Última atualização em 25 July, 2024

A criptografia é parte do mundo dos negócios e da rotina das pessoas atualmente — e tudo indica que se tornará ainda mais importante com o tempo. Esse processo elimina os riscos de terceiros acessarem dados e informações digitais, codificando-os.

Não à toa, canais como WhatsApp e Telegram mantém as conversas de seus usuários protegidas sob uma forte camada criptografada.

A partir desses eventos, a Lei Geral de Proteção de Dados foi criada, visando proteger os dados pessoais dos usuários e exigindo que as empresas tenham esse cuidado.

Por isso, neste artigo, vamos falar sobre o que é esse método, como funciona e os motivos pelos quais é importante que seu negócio considere utilizá-lo. Vamos lá? É só seguir a leitura conosco!

[![Nova call to action](https://no-cache.hubspot.com/cta/default/2287241/3642489e-32ab-4ffe-993e-23e4a18c4da2.png)](https://cta-redirect.hubspot.com/cta/redirect/2287241/3642489e-32ab-4ffe-993e-23e4a18c4da2)

## O que é criptografia?

A criptografia baseia-se em técnicas para proteger dados por meio de códigos, de modo que apenas o emissor e receptor os compreendam. Com o método, as informações tornam-se indecifráveis a quem não tem a devida autorização para acessá-las. 

O princípio fundamental é permitir a troca segura de mensagens entre dois indivíduos, impedindo o acesso de terceiros.

Essa técnica é utilizada em incontáveis aplicações, como na comunicação digital para envio de mensagens ou pagamentos online.

## Como funciona a criptografia?

De maneira geral, o processo permite converter dados legíveis em um formato codificado para torná-los inacessíveis. Para isso, normalmente os sistemas criptográficos usam um texto cifrado (ciphertext) baseado em chave para encobrir o texto simples (plaintext).

Pegamos um exemplo da [Tensumo](https://tensumo.com/cryptography/) para ilustrar exatamente como funciona:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcrBhPvjttWK2SWaOMdMol3yz5b2Uf6q0GFzpTSYjYHapKRSwdRZYbN4ijZwuSSdiLeZ0nla88iIbJC8p3NeUbHIUUerjagsCtuB-u3DTIYnpM1mrEUhRanKWn9g2KEB4ZiDndZV--d3nnqi6-2mWlLj6Oq?key=lxcA5Jxqchg9CUglCxi1cg)

Neste caso, trata-se de uma criptografia assimétrica (ou de chave única), em que as chaves para descriptografar são as mesmas para os dois extremos de um canal de comunicação.

Existe também o modelo assimétrico (chave pública), em que cada extremidade conta com suas chaves – logo mais explicaremos em detalhes como funciona!

Essa tecnologia pode parecer complexa, mas considere-a como um cofre para informações vitais.

Para funcionar, ela depende de um algoritmo que codifica dados – e somente aqueles com a chave correta podem decodificá-los.

### Chaves e protocolos

Atualmente, a base dos modelos simétricos e assimétricos são as chaves, que podem ser utilizadas para criptografar e também para descriptografar informações. 

Como explicamos, <mark style="background:#fff88f">quando a chave é simétrica, pode ser usada nas duas pontas da transmissão. Já quando é assimétrica, as chaves utilizadas para criptografar e descriptografar são diferentes.</mark>

Alguns exemplos de protocolos são: DES, 3DES, AES, IDEA, RC4, TLS e SSL.

Existem também protocolos que não utilizam chaves, chamados de algoritmos de HASH.

Eles transformam um texto, de qualquer tamanho, em uma sequência de caracteres, de tamanho fixo, única para identificar o texto original. 

<mark style="background:#fff88f">Pode ser utilizada como uma espécie de dígito verificador, mas que não permite a reversão desse código para o texto original. </mark>

Eles são usados a todo tempo por nós, quando os sistemas armazenam as nossas senhas, ou comparam o que digitamos com a senha armazenada. Alguns exemplos de protocolos são: MD5 e SHA-256.

## Quais são os tipos de criptografia?

A [confidencialidade](https://www.totvs.com/blog/gestao-para-assinatura-de-documentos/confidencialidade/) de dados é o maior princípio do método, que usa a encriptação de informações para garantir esse sigilo.

<mark style="background:#fff88f">Trata-se do embaralhamento de informações para que elas sejam inelegíveis para qualquer um que não possua a chave correta.</mark>

Nesse sentido, quando falamos do tema, normalmente nos referimos a dois tipos já citados neste conteúdo: simétrico e assimétrico. Que tal conhecê-los melhor?

### Criptografia simétrica

A simétrica é o tipo mais tradicional e provavelmente o sistema que as pessoas estão mais familiarizadas.

Nele, a criptografia é realizada com base em uma única chave — que é utilizada para criptografar e também descriptografar uma mensagem.

O exemplo da imagem que postamos anteriormente ilustra bem a maneira simétrica de criptografar:

A chave é 3 e a mensagem original, “_HELLO_“, foi criptografada como “_KHOOR_“.

Ou seja, para descriptografar, basta aplicar a mesma chave/lógica, que é a troca de cada letra para a terceira letra posterior a ela no alfabeto. E as duas partes precisam ter acesso a essa chave.

Sua principal aplicação é na proteção de dados em repouso, como em bancos de dados ou discos rígidos – isso porque é necessário contar com um canal seguro para transmitir a mensagem.

<mark style="background:#fff88f">Esse tipo de encriptação se destaca por ser mais rápido e por ser ideal para proteger dados que vão ficar em único local.</mark>

<mark style="background:#fff88f">Porém, como desvantagem, destaca-se a dificuldade de distribuição segura de chaves.</mark>

<mark style="background:#fff88f">A lógica é: se há um canal seguro para passar as chaves, porque não passar a mensagem de uma vez?</mark>

<mark style="background:#fff88f">Assim, qualquer pessoa que interceptar e ler a chave, poderá tranquilamente descriptografar a mensagem.</mark>

### Criptografia assimétrica

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcDVdWifLpt851NZYsQuilodczOG_82sx3HvCGfN1_b9ZKYL_h7oe0bWL8MNOeE15yPoofo94iCAEmEciqU-XHOiyJ9x3vxmxC2grP8xr55I3iJCM7CTMZcm25S9aU8KqAI4vXDTH1MCZXHO6rtslyzKhS7?key=lxcA5Jxqchg9CUglCxi1cg)

A assimétrica — ou criptografia de chave pública — utiliza duas chaves diferentes para criptografar e descriptografar um dado.

Vamos direto ao ponto:

<mark style="background:#fff88f">A primeira chave é uma chave pública usada para criptografar uma mensagem e a segunda é uma chave privada utilizada para descriptografá-la.</mark>

O principal a se entender é que apenas uma chave privada pode descriptografar as mensagens criptografadas por uma chave pública.

Esse modelo assimétrico é aplicado em várias operações do dia a dia, como [assinatura eletrônica](https://www.totvs.com/assinatura-eletronica/), envio de e-mails ou mesmo realizar uma conexão remota a um sistema privado.

O próprio protocolo de segurança do seu navegador (repare no  “_https://_” antes da URL) é um exemplo de proteção assimétrica.

<mark style="background:#fff88f">O principal benefício é que as pessoas não precisam de nenhum esquema de segurança específico para trocar mensagens com confidencialidade.</mark>

Digamos que Gabriel queira se comunicar com Jorge usando o método assimétrico. Ele vai utilizar a chave pública de Jorge para criptografar a mensagem.

Assim, depois de receber a mensagem, Jorge usa sua chave privada para descriptografar as informações.

Dessa forma, ninguém pode interceptar e desvendar a mensagem entre os dois – e não é necessário que haja um canal seguro para a troca de chaves.

#### Qual é a criptografia mais usada?

Dados os vários métodos disponíveis, você talvez se pergunte qual é o mais usado. Bom, é fácil apontar um em termos de popularidade: **o modelo assimétrico**.

É a base para proteger muitas das interações digitais do dia a dia, como e-mails, assinaturas eletrônicas e conexões remotas seguras com sistemas privados, além do protocolo “https://”, como vimos anteriormente.

No entanto, é essencial reconhecer que o mais usado não equivale ao _mais adequado para todos os cenários._

Dependendo do caso de uso específico, como dados em repouso ou troca segura de chaves, o modelo simétrico ou o hashing podem ser mais apropriados.

Cada tipo tem seus pontos fortes e aplicações ideais, que contribuem para um ecossistema digital seguro.

## Exemplos de criptografia

Hoje, criptografar dados é muito mais comum do que se imagina. Os [pagamentos digitais](https://www.totvs.com/blog/servicos-financeiros/pagamento-digital/) e [por aproximação](https://www.totvs.com/blog/servicos-financeiros/pagamento-por-aproximacao/) utilizam o método para concretizar transações.

Cartões ou dispositivos mobile que utilizam a [tecnologia NFC](https://www.totvs.com/blog/inovacoes/nfc/), por exemplo, podem trocar informações com terminais de pagamentos para que o dinheiro saia de uma conta e vá para outra.

Essa troca de dados precisa ser protegida para que nenhum terceiro próximo o suficiente para interceptar a operação descubra as informações sigilosas (como a conta, senha, valores, etc).

Na verdade, uma operação financeira criptografada, como essa, esconde totalmente a identidade do consumidor.

Outro exemplo está nos e-mails. A união da tecnologia de proteção de dados garante que o conteúdo dos e-mails fique protegido de qualquer um que não faça parte da conversa.

Se alguém de fora, um terceiro, procura ler o conteúdo de um e-mail que não faz parte, ele apenas vai acessar uma forma criptografada do mesmo – que não é legível por um humano.

Apenas quem faz parte da conversa (ou possui acesso aos e-mails envolvidos nela) pode ler o conteúdo em sua totalidade.

Além disso, como mencionamos, redes sociais como o WhatsApp e o Telegram (específicos para troca de mensagens), bem como o Instagram, utilizam protocolos para criptografar os dados.

## Quais são os algoritmos de criptografia mais conhecidos?

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdqF93Vlb2gBsMDkZMf56iK12jG2lFcjPMgk7Y3BLWhFYLAk_Qu_DZhCme5XIyqLf_S9_EK4Usr8y73HiPvByPpluiat_FBQDLQc9wb5sZsy6O4NNS-EAzJGMUN8kamGOiRN_MdDG0NEkaiD2N9mJd2Hvfl?key=lxcA5Jxqchg9CUglCxi1cg)

Apesar de ser resumida em poucos tipos, essa tecnologia possui vários [algoritmos](https://www.totvs.com/blog/inovacoes/o-que-sao-algoritmos/): tanto simétricos, quanto assimétricos.

Talvez você se pergunte: “_e porque existem tantos algoritmos diferentes?_“

Bom, os motivos são variados, como para o que o algoritmo é aplicado. Além disso, alguns deles são “evoluções” de outros, corrigindo falhas ou brechas encontradas.

Que tal conferir os principais?

### Criptografia RC4

RC4, sigla para Rivest Cipher 4, é uma cifra de fluxo (stream cipher) criada no fim dos anos 1980, um algoritmo simétrico.

Essa cifra opera nos dados um byte por vez, de modo a criptografar esses dados.

O RC4 é uma das cifras de fluxo mais usadas, tendo sido usado nos protocolos Secure Socket Layer (SSL) – hoje conhecido como Transport Layer Security (TLS).

Hoje, esse algoritmo não é tão utilizado, pois apresentou algumas vulnerabilidades, que permitiram que usuários quebrassem a chave em questão de um minuto.

### Criptografia Twofish

Outro tipo simétrico é a Twofish, uma evolução da Blowfish – sendo assim, apenas uma chave de 256 bit é necessária.

É bastante útil e segura, sendo finalista de uma competição do Instituto de Tecnologia e Ciências Nacional americano, que buscava um método para substituir a DES.

### Criptografia DES

DES, sigla para Data Encryption Standard, é também um tipo de chave simétrica – um dos primeiros que foi criado, datando do começo da década de 1970, por um time de desenvolvedores da IBM.

O algoritmo converte texto simples em blocos de 64 bits em texto cifrado, com chaves 48 bits.

Por conta do tamanho pequeno da chave, ele é considerado inseguro para várias aplicações atualmente.

Hoje, o DES foi substituído pelo AES.

### Criptografia 3DES

Derivada do DES, a 3DES (ou Triplo DES) se tornou popular nos anos 1990 – muito embora hoje já não seja uma unanimidade.

Além disso, vale mencionar, ele se tornará obsoleto a partir de 2023.

Sua diferença para o antecessor é que utiliza 3 chaves de 64 bits.

### Criptografia RSA

<mark style="background:#fff88f">Já a RSA é um tipo assimétrico</mark>. A sigla diz respeito ao nome de seus criadores, Rivest-Shamir-Adleman.

Ele é muito utilizado hoje em dia e seu funcionamento tem a mesma explicação da criptografia assimétrica.

Ou seja, é baseado na utilização de uma chave pública para criptografar dados e em uma chave privada para descriptografá-los.

### Criptografia AES

A AES ou Advanced Encryption Standard é um tipo de cifra que protege a transferência de dados online.

É um dos melhores e mais seguros protocolos para criptografar e é utilizado em incontáveis aplicações.

Na prática, é uma chave simétrica, pois utiliza a mesma chave para criptografar e descriptografar o conteúdo.

<mark style="background:#fff88f">Ele também usa o algoritmo SPN (rede de permutação de substituição), aplicando várias rodadas para criptografar dados.</mark>

<mark style="background:#fff88f">Essas rodadas são a razão do alto nível de proteção do AES: se alguém quiser quebrar a proteção, precisará fazê-lo por várias “rodadas”.</mark>

Além disso, a AES conta com 3 tamanhos diferentes de chaves, partindo de 128 bits, 192 bits e 256 bits.

## Quais são os benefícios da criptografia nas empresas?

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXco6_0tf-j8PJ2j_LM9SQaGYrZbfwAoDtHXcgdIXKPKmzEvZWVnaV7oU7vw6w1xsX9UzspmyTaQr0-pmlGRBaE5MKwixq-8DtrNnNh4xfNjjIiHfACW6sfXeiDR9Vac1-yHFCDa9FkNmwpS_C_5tAqlk8c?key=lxcA5Jxqchg9CUglCxi1cg)

A criptografia é uma boa alternativa – entre outras medidas que devem ser tomadas – para manter a segurança dos dados, além de preservar a imagem da empresa, que é tão importante para a conquista de novos negócios.

Depois de escândalos de utilização indevida de dados (como o caso da [Cambridge Analytica](https://g1.globo.com/economia/tecnologia/noticia/2019/01/09/cambridge-analytica-se-declara-culpada-por-uso-de-dados-do-facebook.ghtml), que utilizou dados do Facebook) ficou clara a necessidade de criação de legislações e técnicas que protegessem os interesses dos titulares de dados pessoais.

Por esse motivo, foi criada a [Lei Geral de Proteção de Dados](https://www.totvs.com/blog/adequacao-a-legislacao/lgpd/), que visa garantir a segurança das informações coletadas pelas organizações através de medidas que precisam ser adotadas.

Além de manter a conformidade legal, veja outros benefícios de proteger as informações da sua empresa.

### Processos sigilosos mais seguros

Com essa proteção, é possível garantir que todos os processos sigilosos da empresa (transações bancárias, dados de clientes, informações de colaboradores) sejam feitos com mais segurança.

Caso esse tipo de informação vaze, pode acarretar grandes prejuízos financeiros para a companhia. Por isso, o ideal é contar com meios seguros para efetuar essas transações.

Assim, ao realizar ou receber pagamentos, o processo poderá ser feito sem a preocupação de que pessoas não autorizadas tenham acesso a essas informações.

### Proteção no envio e no recebimento de dados

Uma das formas mais comuns de vazamento de informações é por meio do trânsito de dados.

Quando uma empresa envia dados estratégicos em código criptografado, apenas quem está recebendo-os e possui a chave para codificar a mensagem poderá ter acesso a essas informações. 

O processo também protege o titular do dado e a empresa, uma vez que a exposição e a utilização indevida desses dados pode acarretar em danos à imagem da empresa, multas e processos judiciais.

Por esse motivo, proteger o tráfego de dados é essencial para que a empresa possa garantir a integridade deles, e o código criptografado pode aumentar esse nível de proteção.

### Maior segurança de informações

Desde informações de clientes a dados sobre estratégias, a maioria das empresas possui dados que não devem ser divulgados. 

No caso de dados pessoais de clientes, isso representaria um processo judicial para a companhia.

Para respeitar a legislação e manter a empresa protegida contra ataques, é preciso utilizar uma tecnologia que garanta essa segurança.

Isso pode ser feito por meio da encriptação de dados e outros métodos combinados.

### Integridade dos dados

Uma maneira de proteger a integridade dos seus dados é através da criptografia, que assegura o conteúdo de uma mensagem ou arquivo através de algoritmos avançados.

Assim, é possível manter o sigilo das informações, bem como garantir que nenhum dado seja acessado por um terceiro não autorizado.

### Garantia de conformidade

Manter-se à par das diretrizes de compliance digital – especialmente no que diz respeito à segurança de dados dos seus clientes – envolve o uso de métodos para criptografar.

Através deles, é possível alinhar-se aos padrões exigidos em lei, pelo mercado e fornecedores, de modo a evoluir a governança de informações.

### Segurança na nuvem 

Com cada vez mais usuários e empresas utilizando os recursos de [cloud computing](https://www.totvs.com/blog/inovacoes/cloud-computing/), é preciso cercar-se de maneiras de proteger os dados.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfXAha392eIqqw0hMEjx1I5fi1yBmrw1iVS3xyW-zc4yY9ihFon5b75VlHhmLBAz_d8I7HMTIlOLmkMdnQAYBLB9GUbYW5UeZaBKKxVvPQCpUXTvBSMlahz2YK2MNq3AMrwnGhMfybCaz9eeKOY_DJcdeVm?key=lxcA5Jxqchg9CUglCxi1cg)

Afinal, por serem acessados de qualquer dispositivo, é essencial criptografá-los para garantir sua integridade.

### Assegura a propriedade intelectual

Dados em repouso, especialmente arquivos que sejam parte de sua propriedade intelectual (seja de pessoa física ou da sua empresa), ganham um fator a mais de segurança ao serem criptografados.

Assim, é possível protegê-los de ações criminosas, como roubos com intenção de uso ou comercialização não autorizada.

## O que é criptografia de ponta a ponta?

O método de ponta a ponta _(end-to-end encryption_ ou E2EE) é um tipo de proteção à informações durante uma troca de mensagens – ela acontece no WhatsApp, por exemplo.

Dessa forma, permite que apenas o remetente e o destinatário das mensagens possam acessá-las (e ninguém mais, nem mesmo a própria dona do aplicativo).

## Qual a diferença entre criptografia em trânsito e em repouso?

Ter dados em repouso criptografados significa que a proteção se aplica apenas a informações “paradas” em algum banco de dados, HD ou drive externo.

Já os dados em trânsito criptografados são aquelas informações protegidas _‘em movimento_’ – como mensagens trocadas no WhatsApp ou quando você acessa um site na Internet.

## Como a tecnologia pode manter seus dados seguros?

Tecnologias como [inteligência artificial](https://www.totvs.com/blog/inovacoes/o-que-e-inteligencia-artificial/), machine learning e sistemas de gestão podem manter a segurança de dados de uma empresa.

A computação em nuvem, por exemplo, é uma forma de armazenar informações importantes com um design de soluções criado para isso.

O principal é entender que, hoje, no mercado, existem plataformas e soluções que possuem recursos de proteção de altíssimo nível – que garantem total integridade e confidencialidade dos dados.

Muitas vezes, é claro, com apoio da tecnologia para criptografar dados e informações vitais.

Um sistema de gestão capacitado, por exemplo, pode ser responsável por integrar todos os dados do seu negócio, protegendo-os sob camadas criptografadas intrincadas.

Assim, torna-os acessíveis apenas àqueles com acesso, bem como hierarquia para tal.

## TOTVS Assinatura Eletrônica

No mundo digital interconectado, qualquer passo em falso é um risco desnecessário. Por isso, garantir a proteção das informações mais vitais de sua organização não é um luxo, mas uma **necessidade**.

No entanto, somente criptografar dados por si só pode não ser suficiente para realmente proteger seus documentos.

O que você precisa é de uma solução robusta, segura e eficiente para assinar e guardar seus documentos. E é aí que entra o TOTVS Assinatura Eletrônica.

Imagine uma ferramenta que, além de garantir total validade jurídica de seus documentos, também simplifica todo o processo de assinaturas eletrônicas, reduzindo em impressionantes 80% o “vai e vem” de contratos em sua empresa.

Parece um sonho? Não é. 

**É o que o TOTVS Assinatura Eletrônica pode fazer.**

Nosso sistema é compatível com LGPD, com dispositivos móveis e ainda oferece integrações nativas à soluções TOTVS, bem como a softwares de terceiros.

Com o TOTVS Assinatura Eletrônica, você assina e gerencia suas assinaturas de qualquer lugar, acompanha o andamento de seus contratos e localiza documentos rapidamente.

E aí, que tal elevar a segurança de seus dados e otimizar o gerenciamento de documentos?

Conheça hoje mesmo as funcionalidades do [TOTVS Assinatura Eletrônica](https://www.totvs.com/assinatura-eletronica/)!

[![Nova call to action](https://no-cache.hubspot.com/cta/default/2287241/3642489e-32ab-4ffe-993e-23e4a18c4da2.png)](https://cta-redirect.hubspot.com/cta/redirect/2287241/3642489e-32ab-4ffe-993e-23e4a18c4da2)

## Conclusão

Ao desenvolver uma maior compreensão dos métodos comuns de criptografia e seus algoritmos, você estará alinhado com um dos principais métodos para garantir a segurança dos seus dados.

Você, como pessoa física  e jurídica, precisa entender o que é e como esse recurso pode proteger as informações mais sensíveis da sua rotina.

Assim, é possível prevenir possíveis ataques cibernéticos e violações aos seus dados.

E hoje, com a transformação digital e a evolução tecnológica, esse tópico é mais importante do que nunca.

Gostou de aprender mais sobre o assunto em nosso guia completo? Esperamos que seja útil e que esse conhecimento agregue valor ao seu dia a dia!

Aproveite para ler nosso artigo sobre [anonimização](https://www.totvs.com/blog/negocios/anonimizacao/) e conheça mais sobre o assunto. 

Continue acompanhando o blog da TOTVS e não deixe de assinar a newsletter!
