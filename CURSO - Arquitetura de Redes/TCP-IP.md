---
tags:
  - aprendizado
  - redes
  - protocolo_de_redes
  - modelo_OSI
  - tcp_ip
---

## Parte 1

A principal diferença entre os modelos OSI e TCP/IP está na agregação das camadas. No TCP/IP, as camadas 7 (Aplicação), 6 (Apresentação) e 5 (Sessão) do modelo OSI são combinadas em uma única camada chamada **Aplicação**. Além disso, as camadas 2 (Enlace) e 1 (Física) do OSI são agrupadas em uma camada chamada **Interface de Rede** ou **Link** no TCP/IP. Vale lembrar que o modelo TCP/IP é composto por 4 camadas segundo as RFCs 1122 e 1123: Aplicação, Transporte, Rede e Interface de Rede.

Na prática, a arquitetura pode variar de acordo com a implementação. No padrão Ethernet, por exemplo, a camada de enlace é subdividida em três partes: **LLC** (Controle de Enlace Lógico), **MAC** (Controle de Acesso ao Meio) e **Físico**. Já no Wi-Fi, essa subdivisão se amplia para incluir **PLCP** (Procedimento de Convergência da Camada Física) e **PMD** (Dependente do Meio Físico), além de LLC e MAC.

## Parte 2

Conforme subimos na pilha de protocolos, os conceitos se tornam mais abstratos e mais fáceis de serem compreendidos. Na **camada de Aplicação**, é comum interagir com dados legíveis, como "mensagem" ou "armazenamento", ao passo que camadas mais baixas, como **Transporte** ou **Rede**, lidam com a fragmentação e roteamento dos dados.

- **Aplicação → Dados (mensagem)**: Requer o número da porta de origem e destino para identificar o protocolo de aplicação, como **TCP** ou **UDP**. O **TCP** garante a entrega confiável dos pacotes (com confirmações de recebimento), enquanto o **UDP** é mais leve, mas sem garantias de entrega.
- **Transporte → Segmento ou Datagrama**: No TCP, o dado é dividido em **segmentos**, enquanto no UDP é chamado de **datagrama**. A camada de Transporte gerencia a comunicação entre aplicações de origem e destino.
- **Rede → Pacote**: Envolve a encapsulação do segmento ou datagrama dentro de um **pacote IP** para roteamento, com endereços IP de origem e destino.
- **Link de Dados → Quadro (Frame)**: Aqui, são adicionados os endereços físicos (MAC) e o quadro é preparado para ser transmitido pelo meio físico.

## Parte 3

O campo **EtherType** nos quadros Ethernet identifica qual protocolo de rede está sendo utilizado (por exemplo, **IPv4**, **IPv6** ou **ICMP**). Depois de identificado o protocolo, os dados passam pela camada de Rede, onde o endereço IP de destino é comparado com o IP local. Se corresponder, o pacote é encaminhado para a camada de Transporte, onde o cabeçalho do **segmento** ou **datagrama** será lido para determinar qual aplicação receberá os dados. Na camada de Aplicação, a mensagem é finalmente processada pela aplicação de destino, conforme o protocolo utilizado. Em seguida, é entregue ao aplicativo ou programa, como um navegador de internet.

![[TCP-IP.pdf]]

Resumo do arquivo

## Modelo TCP/IP

O arquivo apresenta duas representações do modelo TCP/IP:

- Representação moderna (A. Tanenbaum, D. Comer): Divide o modelo em cinco camadas.
- Representação tradicional (RFC 1122 e 1123): Divide o modelo em quatro camadas.

## Camadas do modelo TCP/IP

As camadas do modelo TCP/IP, de cima para baixo, são:

1. Aplicação
2. Transporte
3. Rede (também chamada de Internet)
4. Link de dados (também chamada de Enlace ou Enlace de dados)
5. Física (na representação moderna)

## Implementações específicas

O arquivo detalha duas implementações específicas da camada de link de dados e física:

### Ethernet

- LLC (Logical Link Control, IEEE 802.2): Controle do link lógico
- MAC (Medium Access Control, IEEE 802.3): Controle de acesso ao meio (CSMA/CD)

### Wi-Fi

- LLC (Logical Link Control, IEEE 802.2): Controle do link lógico
- MAC (Medium Access Control, IEEE 802.11): Controle de acesso ao meio (CSMA/CA)
- PLCP (Physical Layer Convergence Procedure): Procedimento de convergência da camada física
- PMD (Physical Medium Dependent): Dependente do meio físico

O arquivo também menciona o conceito de encapsulamento, mas não fornece detalhes adicionais sobre esse processo.