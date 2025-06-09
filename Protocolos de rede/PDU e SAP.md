---
tags:
  - PDU
  - SAP
  - redes
  - modelo_OSI
  - protocolo_de_redes
---
## PDU (Protocol Data Unit)

O PDU (Protocol Data Unit) é uma unidade de dados que cada protocolo adiciona em diferentes camadas de uma rede. À medida que os dados descem pela pilha de protocolos, cada camada acrescenta suas próprias informações de controle ao pacote. O termo exato para essa unidade de dados varia conforme a camada: pode ser chamado de segmento, datagrama, quadro, célula, etc. "Pacote de dados" é o termo genérico utilizado. Cada camada adiciona um cabeçalho ao dado recebido da camada acima, formando um encapsulamento: a camada 7 (aplicação) envia dados para a camada 6 (apresentação), que adiciona seu cabeçalho e encaminha para a camada 5, e assim por diante até atingir a camada física.

## SAP (Service Access Point)

SAP (Service Access Point) identifica qual protocolo está sendo utilizado em uma determinada camada da comunicação. Quando uma máquina de origem utiliza diferentes protocolos — como para acessar a web, enviar e-mails ou transferir arquivos — é necessário um sistema que identifique o protocolo associado a cada PDU. Isso garante que a máquina de destino saiba para qual protocolo ela deve entregar os dados. O SAP, representado por um campo no cabeçalho da PDU, informa qual protocolo da camada superior gerou os dados que estão sendo transportados. Na prática, a comunicação entre as camadas 1 e 2 é feita através do EtherType, associado ao endereço MAC da máquina. Na camada 3, o SAP é o número do protocolo, como o IP, enquanto na camada 4 o SAP utilizado é chamado de "porta".

![[PDU e SAP.pdf]]

Resumo do arquivo "PDU e SAP.pdf":

1. PDU (Packet Data Unit):

- É um pacote de dados criado em cada camada do modelo OSI.
- Recebe nomes diferentes dependendo do contexto: segmento, datagrama, quadro, célula, etc.
- "Pacote de dados" é o termo genérico.
- O PDU da camada superior é encapsulado na área de dados do PDU da camada atual.

1. SAP (Service Access Point):

- Faz a interface entre as camadas do modelo OSI.
- É um sistema de endereçamento que identifica qual protocolo da camada superior gerou os dados.
- Garante que os dados sejam entregues ao protocolo correto na camada superior no destino.

1. Exemplos práticos de SAP:

- Camada 2: endereço físico (MAC) e EtherType
- Camada 3: endereço lógico (IP) e número do protocolo
- Camada 4: porta