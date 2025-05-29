---
tags:
  - wireshark
  - tshark
  - suricata
  - redes
  - tcc
  - faculdade
  - responsabilidades
excalidraw-plugin: 2025-01-27T16:00:00
---
##### Descrição do problema
Em uma pesquisa mais detalhada sobre os possíveis problemas que eu iria ter para colocar o Wireshark rodando 24h por dia durante dois meses, tive o preocupante retorno de que ele ocupa muito espaço em seus relatórios gerados, ainda mais de dois meses seguidos, fora os outro empecilhos que podem acontecer com ele. De qualquer modo, a versão normal do Wireshark não é recomendada para análises de rede tão longas assim por diversos motivos.

Foi aí que acabei achando o Suricata, que é específico para isso e que pode vir a me ajudar bastante. https://suricata.io/. Fiz uma pesquisa dele e ví alguns vídeos que me deixaram animados para falar a verdade. Ele é exatamente o que eu precisava !

##### 1. Análise de Pacotes

O Suricata pode inspecionar o tráfego da rede em tempo real e gravar logs detalhados, que incluem informações sobre protocolos, sessões, e até conteúdo de pacotes. Com isso, você pode:

- Monitorar o tráfego em busca de anomalias ou padrões suspeitos.
- Coletar dados para auditoria ou troubleshooting.
- Inspecionar tráfego de HTTP, DNS, TLS, entre outros.

##### 2. Proteção da Rede

Como IDS/IPS, o Suricata oferece proteção ativa ao:

- Detectar ataques baseados em assinaturas (como regras SNORT ou Emerging Threats).
- Bloquear tráfego malicioso em tempo real (modo IPS).
- Detectar comportamentos anômalos que podem indicar invasões ou atividades suspeitas.

##### 3. Por que é uma “mão na roda”?

- **Versatilidade**: Trabalha tanto como IDS (somente monitora) quanto IPS (bloqueia proativamente).
- **Desempenho**: Usa processamento multicore, permitindo alta performance em redes movimentadas.
- **Logs Ricos**: Gera logs detalhados em formato JSON, compatíveis com ferramentas como Elasticsearch, Kibana e Splunk para análise visual.
- **Flexibilidade**: Suporte a DPI (Deep Packet Inspection) para identificar protocolos mesmo em portas não padrão.

##### O que considerar na implementação?

- **Posicionamento na Rede**: Configure-o em modo _inline_ (para IPS) ou _tap/span_ (somente IDS), dependendo do seu objetivo.
- **Atualização de Regras**: Certifique-se de manter as assinaturas de regras atualizadas, como as do Emerging Threats.
- **Integração com Firewall (ex.: PFsense)**: No seu caso, ele pode ser integrado ao PFSense para controle mais refinado.
- **Análise dos Logs**: Use ferramentas como Elasticsearch + Kibana ou Graylog para facilitar a análise dos dados gerados.

##### Vídeos relacionados que eu assisti sobre o Suricata
https://www.youtube.com/watch?v=8368SMYzXcI&t=424s
https://www.youtube.com/watch?v=mg8DWETsN_k&t=77s