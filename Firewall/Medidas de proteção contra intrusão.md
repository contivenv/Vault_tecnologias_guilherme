---
tags:
  - firewall
  - IDS
  - IPS
  - defesa_profunda
---
A ideia de proteção total e impenetrável é muito superficial quando falamos nos dias de hoje sobre redes de computadores. Sempre é possível de alguma maneira contornar alguma defesa, seja ela a mais forte que tiver e com mais regras para barrar ameaças. No entanto, li em um livro de redes de computadores que sempre é aceitável colocar várias medidas de segurança na mesma rede, isso não quer dizer de forma alguma que o atacante se ele realmente quiser não vai conseguir entrar na rede, mas seria muito difícil ele contornar medidas de segurança com uma firewall bem configurado, um IDS bem aplicado com controle de falsos positivos e um antivírus nas máquinas ao mesmo tempo.

Além dos princípios fundamentais da segurança de Saltzer e Schroeder, muitas pessoas têm incluído outros princípios, quase sempre muito práticos. Um particularmente útil para ser mencionado aqui é o princípio pragmático da **defesa em profundidade.** Frequentemente, é uma boa ideia usar várias técnicas complementares para proteger um sistema. Por exemplo, para impedir ataques, podemos usar um firewall e um sistema de detecção de intrusão e um antivírus. Embora nenhuma medida possa ser infalível por si só, a ideia é que é muito mais difícil contornar todas elas ao mesmo tempo. (*Redes de Computadores, Andrew Tenenbaum, 8.3 Firewalls e Sistemas de detecção de intrusão*)