---
tags:
  - pfsense
  - funcionalidades
  - firewall
  - cibersegurança
  - aprendizado
  - redes
---
##### 1. Roteador
Por que fazer um roteador?

Os roteadores que muitas vezes vem “de graça” junto com a assinatura do seu plano de internet podem não ser tão eficientes, porque eles têm objetivos audaciosos, querem ser no mínimo 3 coisas em uma só, um roteador, um switch e também o AP (wireless access point), somado a isso, são feitos com componentes modestos, focados em consumir o mínimo de energia e serem o mais baratos possível.

Quem tenta fazer várias coisas ao mesmo tempo, geralmente não alcança resultados excelentes em nada, ainda mais quando não é muito capacitado, é exatamente essa a situação da maioria dos roteadores domésticos comuns. Se você tem muito tráfego e diversos dispositivos conectados em sua rede, converter um computador em roteador fará a diferença no desempenho da conexão com a web, principalmente em empresas.

Qual o material necessário para fazer um roteador?

Para montar o seu próprio roteador, você precisa de basicamente duas coisas, um computador qualquer e um sistema operacional de roteador. Quanto ao sistema operacional, existem diversas boas opções, a maior parte delas gratuitas e com código aberto, como o OpenWRT, IPFire, OPNSense e o que a gente vai usar hoje, o pfSense, um sistema para roteador e firewall baseado no FreeBSD, com nível empresarial, que você pode ter também na sua casa, sem pagar nada.

Já a parte do hardware requer um pouco mais de explicação, porque, embora qualquer computador tenha potência para fazer o trabalho de um roteador, alguns detalhes precisam ser levados em consideração. Realmente não é preciso se preocupar quanto à capacidade de processamento e memória do computador que será utilizado, afinal, por exemplo, um bom roteador, como o [ER605](https://www.amazon.com.br/s?k=er605&i=computers&rh=n%3A16339926011&dc&language=pt_BR&ds=v1%3ABD%2BOY28tdVgQzbclXJNQL6zaoUN%2BoLVd%2B0AKjpcxyvQ&crid=2X043TVKT5PZO&linkCode=sl2&linkId=75e44c8eb6535469ebe1c930edabd365&qid=1679402652&rnid=15137643011&sprefix=ER605%2Caps%2C211&tag=diolinux0a-20&ref=as_li_ss_tl), tem 16MB de armazenamento, 128MB de RAM e aparentemente o processador é um MediaTek dual core de 880Mhz.

Porém, não só de CPU, RAM e armazenamento um bom roteador é feito, apesar de qualquer máquina antiga ter o poder necessário para trabalhar para você como um roteador, esse computador precisa ter ao menos duas conexões de rede, uma para você conectar a internet que vem do seu provedor, e outra para criar a sua rede doméstica.

Existem vários pequenos computadores com aspecto mais empresarial, que podem servir para essa finalidade sendo [vendidos no AliExpress](https://pt.aliexpress.com/category/201001502/Mini-PC.html?category_redirect=1&spm=a2g0o.productlist.breadcrumb_navigation.3.708a5ba9DSy4ca&cv=85279&dp=p12815831017815123630&utm_campaign=affiliate&utm_content=p12815831017815123630&utm_medium=referral&utm_source=zbanx&utm_term=85279&aff_fcid=379ce0121b524cf9b0827b7c5a4a6d10-1680541058344-09661-_EHUumgr&aff_fsk=_EHUumgr&aff_platform=api-new-link-generate&sk=_EHUumgr&aff_trace_key=379ce0121b524cf9b0827b7c5a4a6d10-1680541058344-09661-_EHUumgr&terminal_id=86932e43d35a4de09b7b3af738e69877), mas nada impede que você coloque uma placa de rede no seu computador tradicional.

Idealmente você vai querer um computador que não gaste muita energia, afinal, ele vai ter que ficar ligado 24 horas por dia, dessa forma, até um [Raspberry Pi](https://diolinux.com.br/tutoriais/o-que-e-o-raspberry-pi.html) pode servir para o trabalho, mas existem bem mais opções de sistemas para arquitetura x86 ou x64, então não faça nenhuma compra sem pesquisar antes.

Quanta energia é muita? Isso depende um pouco do quanto você está disposto a gastar em prol de ter um equipamento melhor. Para o nosso projeto aqui no Diolinux, usamos a [Zimaboard](https://youtu.be/tWf-s452CcQ), que já tem duas conexões de rede, o que elimina a necessidade de comprar alguma placa de rede extra.

Caso você precise de mais de uma conexão LAN, existem opções, você pode usar o seu roteador atual com as suas funções de switch para isso, ou comprar um específico para este fim, como o [switch TP-LINK de 8 portas](https://www.amazon.com.br/Switch-Gigabit-Mesa-Portas-Ls1008G/dp/B07VC41CXJ?__mk_pt_BR=%C3%85M%C3%85%C5%BD%C3%95%C3%91&keywords=ls1008g&qid=1679079049&sr=8-1&ufe=app_do:amzn1.fos.6a09f7ec-d911-4889-ad70-de8dd83c8a7&linkCode=sl1&tag=diolinux0a-20&linkId=67018740896dbccf627dd0f48419be50&language=pt_BR&ref_=as_li_ss_tl), assim você terá várias portas.

Como fazer um roteador com o com pfSense

O primeiro passo aqui é instalar o sistema operacional. Baixe o pfSense diretamente do [site oficial](https://www.pfsense.org/download/), em arquitetura, escolha AMD64, se for utilizar algum computador comum e no tipo do instalador, selecione o formato ISO.

Com uma ISO baixada, a gente prepara um pendrive bootável com algum programa como o [Ventoy](https://youtu.be/11CkqZQ3scE), como faríamos para formatar um computador. Para fazer a instalação, você precisará de um monitor, além de um teclado ligado ao seu computador, isso só vai ser necessário agora.

Instalando o pfSense

O pfSense é um sistema operacional de roteador e firewall, então seu instalador é simples. A gente aceita os termos e basicamente precisamos clicar em “next” algumas vezes. Você pode mudar o esquema do teclado se quiser, o que, neste caso, faz pouca diferença.

Auto ZFS é uma boa opção na hora de configurar o armazenamento, mas lembre que isso apagará todos os dados na unidade de disco que estiver no computador. Na próxima tela, para o nosso uso, a primeira opção, stripe, basta, mas em ambiente empresarial, onde você quer redundância até no roteador, tem alguns raids interessantes para escolher.

Na hora de selecionar a unidade de armazenamento para a instalação, fique atento para não escolher o pendrive, mas sim, o drive interno da sua máquina. Depois disso, a instalação deve começar, por ser um sistema simples, será um processo muito rápido.

Configurando o pfSense

Depois que a instalação terminar, você reinicia o computador, ou melhor, o roteador, e a gente pode começar a fazer as primeiras configurações. No primeiro boot você vai precisar escolher qual das suas entradas de rede vai se comportar como WAN, ou seja, que vai receber os dados da internet do seu provedor, nessa hora você precisa conectar o cabo de internet nessa porta. Ao mesmo tempo, a outra porta será designada para LAN, ou seja, para distribuir os dados da internet dentro da sua rede.

Uma vez definido isso, você verá que o pfSense mostrará um endereço de IP externo, e também um endereço de IP para a LAN, que será o gateway de todos os devices que estiverem se conectando à internet através do seu roteador.

Agora é a última vez que a gente vai precisar de monitor e teclado ligados no nosso roteador, o restante das configurações, será feito através de uma interface web. Como todo roteador, a gente só precisa conectar um computador na porta lan e acessar o endereço de IP dele na rede.

A configuração do roteador pode mudar para cada caso de uso, mas acompanhando ao vídeo presente no início desse artigo, você verá uma forma simples de configurar, que funcionará na maioria das ocasiões.

Se você achou que algumas siglas e palavras foram um pouco confusas, [conheça sobre o funcionamento básico de uma rede e os protocolos mais comuns](https://diolinux.com.br/video/como-funcionam-as-redes-domesticas.html)!
##### 2. Inspeção de pacotes
A inspeção de pacotes é uma ==técnica que analisa os dados que passam por uma rede de computadores==. É usada para garantir a segurança e a integridade das informações. 

A inspeção de pacotes pode ser feita de diferentes formas, como a inspeção profunda de pacotes (DPI) e a inspeção de pacotes com estado (SPI). 

Inspeção profunda de pacotes (DPI) 

- Analisa o conteúdo dos pacotes de dados
- Identifica ameaças como vírus, spams e intrusões
- Pode descobrir de onde veio a ameaça
- Pode ser configurada para redirecionar o tráfego de rede
- É usada por empresas e provedores de serviços de internet (ISPs)

Inspeção de pacotes com estado (SPI) 

- Examina o conteúdo de um pacote de dados e compara-o com os dados de pacotes anteriores
- Identifica tráfego anômalo
- É usada por firewalls com estado
##### 3. Captive portal
Um captive portal ==é uma página web que o usuário deve visualizar antes de acessar a internet em uma rede Wi-Fi pública ou privada==. Ele é usado para autenticar usuários, coletar informações e gerir o acesso à rede. 

O captive portal é usado em locais como aeroportos, hotéis, cafés, centros de negócios e outros locais públicos que oferecem Wi-Fi gratuito. 

O captive portal pode ser usado para: 

- Autenticar usuários
- Exibir anúncios

- Coletar informações de contato

- Personalizar a experiência do usuário

- Segmentar os usuários em grupos com diferentes permissões de acesso

- Proteger o negócio de crimes virtuais

O captive portal pode ser personalizado com a imagem da empresa, promoções ou anúncios.
[Autenticando usuários na nuvem com Pfsense + Captive Portal + FreeRadius](https://www.youtube.com/watch?v=4lyHTirW3eY)
##### 4. Traffic shapping - QoS (limitar banda dos usuários e priorizar trafego de alguns usuários)
QoS (Quality of Service) ==é uma tecnologia que controla o tráfego de uma rede para garantir o desempenho de aplicativos importantes==. 

A QoS é útil quando a largura de banda é limitada ou a rede está congestionada. Ela permite que os aplicativos de alta prioridade continuem a funcionar de forma eficaz. 

A QoS funciona monitorando e priorizando o tráfego de dados. Ela também altera a largura de banda e a rota dos pacotes para evitar atrasos. 

A QoS pode ser usada em vários contextos, como: Cloud Computing e SaaS, Backup e recuperação de desastres, IoT e dispositivos conectados, VoIP (Voz sobre IP). 

Alguns requisitos básicos de QoS são: Confiabilidade, Retardo, Flutuação do tempo de transmissão, Vazão (Largura de Banda).
[Aula 13 - Traffic Shapping - Limite de banda e Priorização de tráfego](https://www.youtube.com/watch?v=-iPDdjxCDbk)
##### 5. Servidor PPPoE
Um servidor PPPoE ==é um servidor que utiliza o protocolo PPPoE (Point-to-Point Protocol over Ethernet) para gerenciar e autenticar conexões de clientes==. 

O PPPoE é um protocolo de rede que permite a conexão entre um cliente e um servidor, de forma que a conexão possa ser compartilhada com vários usuários. É muito utilizado por provedores de internet (ISPs) para gerenciar conexões de banda larga, como DSL e FTTH. 

O PPPoE funciona da seguinte forma: 

- O roteador exige uma senha para que o usuário possa se conectar

- O provedor recebe a informação do usuário e da senha

- Se os dados estiverem corretos, o login é autenticado

A autenticação PPPoE garante que apenas usuários autorizados possam acessar a rede.
[PfSense - Servidor pppoe configuração basica](https://www.youtube.com/watch?v=9CPnnkLaNP8)
##### 6. Appliance VPN IPsec / SSL
Um **Appliance VPN** é um dispositivo de hardware ou software projetado especificamente para fornecer **conexões seguras de rede via VPN (Virtual Private Network)**. Ele facilita o estabelecimento de túneis criptografados que protegem a comunicação de dados entre redes ou entre usuários remotos e uma rede corporativa.

Características principais:

1. **Hardware dedicado**: Alguns appliances VPN são dispositivos físicos desenvolvidos para operar exclusivamente como servidores de VPN. Eles têm hardware otimizado para desempenho e segurança.
2. **Software especializado**: Outros podem ser soluções baseadas em software que são instaladas em servidores gerais ou máquinas virtuais.
3. **Gerenciamento centralizado**: Muitos appliances VPN permitem o gerenciamento e a configuração centralizada de conexões, autenticação de usuários e políticas de segurança.
4. **Criptografia robusta**: Oferecem suporte a protocolos como **IPSec**, **OpenVPN**, **SSL/TLS**, entre outros, para garantir a segurança das comunicações.
5. **Firewall integrado**: Em alguns casos, também incluem funcionalidades adicionais, como **firewalls**, para aumentar a proteção da rede.

Benefícios:

- **Segurança aprimorada**: Conexões seguras, criptografadas e autenticadas.
- **Fácil gerenciamento**: Interfaces simplificadas para configurar políticas de acesso e monitoramento de tráfego.
- **Desempenho otimizado**: No caso de appliances físicos, o hardware dedicado oferece alta performance, especialmente em redes com muitos usuários.
- **Escalabilidade**: Muitas soluções permitem a adição de novos usuários ou redes de forma simples.

Exemplos de uso:

- Empresas que desejam conectar filiais via VPN.
- Provedores de acesso remoto seguro para funcionários trabalhando em home office.
- Acesso seguro a sistemas internos por parceiros ou fornecedores.

Exemplos de soluções populares:

- **Fortinet FortiGate**
- **Cisco ASA**
- **Palo Alto Networks GlobalProtect**
- **Sophos XG Firewall**
- **OpenVPN Access Server** (software).

Se sua empresa utiliza o PFSense (como mencionado antes), ele também pode ser configurado como um appliance VPN, oferecendo recursos como IPSec, OpenVPN, e L2TP/IPSec.![[0.jpg]]
##### 7. Proxy
Um proxy ==é um servidor que atua como intermediário entre o usuário e a internet==. Ele recebe e repassa as solicitações do usuário ao site que ele está acessando. 

O proxy é útil para proteger a privacidade do usuário, pois o endereço IP registrado nos sites acessados é o do proxy, e não o do usuário. 

O proxy também pode ser usado para aplicar políticas corporativas de navegação na internet. 

Para encontrar o endereço do servidor proxy no Windows, é possível: 

1. Abrir o menu do Windows

- Clicar no ícone de engrenagem para abrir as configurações

- Clicar em Rede e internet

- Selecionar Proxy na barra lateral esquerda
Existem diferentes tipos de proxies, que são usados para atender a necessidades específicas.
![[Proxy-Server-Overview.png]]


##### 8. Servidor de rede
Um servidor de rede ==é um computador ou sistema que processa solicitações de dados e comandos de outros computadores conectados à mesma rede==. Ele é o coração de uma rede de computadores, permitindo o compartilhamento de recursos e a execução de aplicações. 

Os servidores de rede são fundamentais para o armazenamento e gestão de dados de uma empresa, pois centralizam os arquivos e recursos, tornando o acesso a eles mais fácil e seguro. 

Existem diversos tipos de servidores de rede, cada um com suas características e finalidades específicas. Alguns exemplos são: Servidor de arquivos, Servidor de aplicações, Servidor de banco de dados, Servidor de correio eletrônico, Servidor de mídia. 

A escolha do tipo de servidor de rede depende do volume de dados a serem processados, da velocidade de resposta necessária e da criticidade dos dados.
##### 9. Proxy reverso (Notícia do X no Brasil, podemos pegar de exemplo esse caso para entender proxy reverso:
    
A manobra do proxy reverso do Elon Musk para o X ==foi uma atualização no aplicativo da rede social que mudou a forma como os servidores se conectam à internet==. Essa mudança permitiu que o X contornasse o bloqueio imposto pelo Supremo Tribunal Federal (STF) no Brasil. O proxy reverso é um serviço que atua como intermediário entre o servidor de um site e o usuário. Ele é usado para melhorar a performance do site e aumentar a segurança. A manobra do proxy reverso do Elon Musk para o X envolveu:

- A utilização da Cloudflare, uma empresa de hospedagem de conteúdo e cibersegurança 
- A alteração dos endereços de internet (IPs) usados para acessar o X, tornando-os dinâmicos e frequentemente alterados 
- A "mascaragem" do IP real do servidor 

O X conseguiu driblar o bloqueio do STF porque os IPs são dinâmicos e constantemente alterados.)
![[Pasted image 20250204131503.png]]
##### 10. Load Balance
(Um load balancer, ou balanceador de carga, ==é um dispositivo que distribui o tráfego entre vários servidores==. Ele evita que um servidor fique sobrecarregado, mantendo o funcionamento de um sistema, mesmo em momentos de alta demanda. O load balancer é um ponto de contato único para os clientes, e atua como um facilitador entre o usuário e os servidores. O load balancer pode ser usado para: Aumentar a disponibilidade do aplicativo, Aumentar a tolerância a falhas dos aplicativos, Descarregar o trabalho de criptografia e descriptografia, Melhorar o desempenho do sistema, Estabilizar a navegação. Existem diferentes tipos de load balancer, como o Network Load Balancer e o Application Load Balancer.)![[Load Balancer.png]]