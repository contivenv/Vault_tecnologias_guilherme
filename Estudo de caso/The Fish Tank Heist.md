---
aliases: [Fish Tank Hack, Casino Fish Tank, Ataque ao Termômetro IoT]
tags: [cibersegurança, iot, vulnerabilidade, estudo-de-caso, hacking]
---
# The Fish Tank Heist: Como um aquário comprometeu um cassino

O **"Fish Tank Heist"** (O Assalto do Aquário) é um dos estudos de caso mais famosos e curiosos do mundo da Segurança da Informação, ocorrido em 2017. Ele ilustra perfeitamente os perigos ocultos trazidos pela Internet das Coisas (IoT) em redes corporativas.![[Pasted image 20261007143523.png]]

## Resumo do Incidente
Cibercriminosos conseguiram invadir a rede corporativa de um cassino norte-americano (não identificado na época, mas reportado pela empresa de segurança Darktrace) utilizando um vetor de ataque extremamente inusitado: um **termômetro inteligente** instalado em um aquário no saguão do estabelecimento. 

## Como o ataque funcionou ?
1. **O Vetor de Entrada:** O cassino instalou um aquário de alta tecnologia que possuía sensores conectados à internet (via Wi-Fi) para monitorar a temperatura, salinidade e alimentação dos peixes.
2. **A Exploração:** Os atacantes encontraram uma vulnerabilidade no termômetro inteligente. Como dispositivos IoT frequentemente carecem de protocolos de segurança robustos, ele serviu como uma "porta dos fundos" (*backdoor*) para a rede.
3. **Movimentação Lateral:** Uma vez dentro da rede através do termômetro, os hackers conseguiram se mover lateralmente até alcançar o banco de dados principal do cassino.
4. **Exfiltração de Dados:** Os criminosos roubaram cerca de 10 gigabytes de dados, focando no banco de dados de grandes apostadores (*high-rollers*). Ironicamente, os dados foram exfiltrados da rede passando de volta pelo termômetro e enviados para um dispositivo localizado na Finlândia.

## Por que dispositivos IoT são vulneráveis ?
De acordo com especialistas, incluindo pesquisadores da Lancaster University, dispositivos inteligentes falham em segurança por alguns motivos centrais:
- **Falta de Atualizações:** O software de dispositivos IoT raramente é atualizado pelos usuários (ou é difícil de atualizar).
- **Baixo Poder de Processamento:** Seu tamanho reduzido significa que possuem pouca memória e processamento, inviabilizando a instalação de recursos de segurança avançados (como criptografia pesada ou antivírus).
- **Falhas de Firmware:** Muitos dispositivos já vêm com vulnerabilidades inerentes de fábrica (*hardcoded passwords*, portas abertas, etc).

## Lições Aprendidas
Para evitar que "termômetros" vazem dados críticos, empresas devem adotar as seguintes medidas:
- **Segmentação de Rede:** Dispositivos IoT nunca devem compartilhar a mesma rede que servidores de banco de dados corporativos ou dados sensíveis de clientes.
- **Inventário e Monitoramento:** Manter um registro exato de todos os dispositivos conectados à rede, por mais inofensivos que pareçam.
- **Políticas de Atualização (Patching):** Inscrever-se em alertas de fornecedores para manter o *firmware* atualizado constantemente.

---

### Fontes

- [Forbes: Criminals Hacked A Fish Tank To Steal Data From A Casino](https://www.forbes.com/sites/leemathews/2017/07/27/criminals-hacked-a-fish-tank-to-steal-data-from-a-casino/)
- [The hackers, the fish tank and the casino - Lancaster University](https://www.lancaster.ac.uk/lums/business/business-insights/the-hackers-the-fish-tank-and-the-casino-how-smart-devices-could-be-a-hidden-flaw-in-your-businesss-cyber-security-measures)
- [How a fish tank helped hack a casino](https://www.washingtonpost.com/news/innovations/wp/2017/07/21/how-a-fish-tank-helped-hack-a-casino/)