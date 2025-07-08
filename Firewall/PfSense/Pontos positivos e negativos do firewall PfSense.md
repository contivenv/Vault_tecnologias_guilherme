---
tags:
  - pfsense
  - firewall
  - pontos_positivos_e_negativos
---
Pontos negativos

```ad-fail
title: Pontos negativos

1. Muito manual, falta de instaladores e suporte de instalação Wizard
2. Requer uma especialidade em redes para compreender
3. Interface simples demais
4. Relatórios de navegação muitos simples
5. Automação ruim/API não oficial (um exemplo simples disso seria liberação de um mesmo número de IP em dez firewalls diferentes. Hoje em dia para fazermos a liberação desse IP nos dez firewalls, temos que acessar um por um e colocar esse IP como liberado na rede)
```

```ad-success
title: Pontos positivos

1. Gratuito para sempre, Open Source total
2. Muito estável, o sistema operacional que roda por trás dele é o FreeBSD, uma rocha muito sólida
3. Pode customizar da maneira que quiser (configurar para mandar mensagens pelo Telegram; melhora para conexão de integração com o AD da Microsoft, versão nativa não tinha)
4. Roda em hardwares fracos
5. Pode rodar em máquina física e virtual, pode até cortar custo de hardware
6. A muito tempo no mercado, desde 2006. Então temos vários plugins oficiais para funcionamento junto ao PfSense. Por exemplo plugins do Zabbix.
```


