---
tags:
  - Solar_Winds
  - ataque_de_cadeia_de_suprimentos
  - CVE-2020-10148
  - ciberseguranca
  - supply-chain-attack
  - solarwinds
  - cve
  - incidentes
---
# Estudo de Caso: Ataque à Cadeia de Suprimentos da SolarWinds (2020)

> [!warning] Resumo do Incidente
> O ataque à SolarWinds foi uma violação de alto perfil que ocorreu no ano de 2020. Mais de 18.000 clientes da SolarWinds acabaram instalando atualizações que continham código malicioso. Atores APT (Ameaças Persistentes Avançadas) infiltraram-se na cadeia de suprimentos da SolarWinds e inseriram um backdoor no software chamado "Orion". 

## O que foi o Ataque?

O incidente da SolarWinds foi um ataque típico à cadeia de suprimentos. 
* Nesses tipos de invasões, os cibercriminosos não atacam diretamente as redes de suas vítimas. 
* Em vez disso, eles penetram no sistema de um fornecedor terceirizado que tenha acesso aos ativos de rede de seus alvos.
* Os invasores conseguiram modificar o código dentro do pacote de software da SolarWinds.
* Eles convenceram as vítimas a instalá-lo como parte de uma atualização de software legítima.
* O código malicioso inserido no Orion é chamado de "Sunburst".
* Uma vez no sistema de uma vítima, o malware deu aos hackers acesso aos sistemas de TI dos clientes.
* O backdoor inserido foi utilizado para roubar dados de clientes e, em seguida, espionar outras organizações.
* Isso forneceu aos invasores acesso interno a organizações que, por si só, poderiam ser impenetráveis.

## Linha do Tempo do Ataque

O ataque não aconteceu de um dia para o outro e seguiu um cronograma de infiltração planejado:
* **Setembro de 2019:** Os hackers conseguiram acessar a rede da SolarWinds.
* **Outubro de 2019:** Os invasores começaram a testar a injeção de código no software Orion.
* **Fevereiro de 2020:** Cerca de quatro meses após os testes, os cibercriminosos injetaram o código malicioso Sunburst no Orion.
* **26 de Março de 2020:** A SolarWinds começou a distribuir as atualizações do Orion que continham o código malicioso dos hackers.

## O Impacto

O impacto financeiro e operacional da violação foi massivo a nível global.
* O ataque custou às empresas afetadas, em média, 11% de sua receita anual.
* Nos Estados Unidos, o impacto médio foi ainda mais dramático, representando 14% da receita anual das empresas, enquanto as empresas do Reino Unido sofreram perdas de 8,6%.
* O ataque levou a cerca de 90 milhões de dólares em perdas seguradas, voltadas principalmente para resposta a incidentes e investigações forenses.
* Organizações impactadas relataram custos médios de cerca de 12 milhões de dólares cada, impulsionados por tempo de inatividade e atualizações de segurança.
* Este incidente evidenciou fraquezas na segurança da cadeia de suprimentos de software, levando as organizações a reforçarem padrões para gerenciamento de risco de terceiros.

## Vulnerabilidade Associada (CVE-2020-10148)

O ataque à SolarWinds foi associado à vulnerabilidade **CVE-2020-10148**. 
* Esta é uma vulnerabilidade que permite que os invasores ignorem a autenticação da API.
* A evasão de autenticação ocorre pela inclusão de parâmetros específicos em uma solicitação de URI.
* Isso poderia permitir que um invasor não autenticado executasse comandos de API.

## Mitigação e Defesa

A SolarWinds lançou patches de segurança para eliminar a possibilidade de o Orion ser usado para espalhar malware e para afetar diretamente a falha. 

> [!info] Recomendações de Segurança
> Se for determinado que sua infraestrutura tem exposição à SolarWinds, é sugerido confirmar se o software foi corrigido e atualizado para a versão mais recente.

Para reforçar a segurança e detectar atividades anômalas ligadas a ataques à cadeia de suprimentos, as organizações podem seguir as seguintes recomendações:
* Implementar o gerenciamento de logs e o gerenciamento de eventos e informações de segurança (SIEM).
* Auditar as *Active Directories* em busca de alterações.
* Realizar testes de penetração regulares.
* Reforçar o seu sistema de prevenção contra perda de dados.
* Ao alimentar logs do sistema no SIEM, ele pode identificar mudanças na forma como a rede opera e movimentações incomuns de dados.
* O SIEM também ajuda a identificar comportamentos inseguros do usuário e a presença de usuários não autorizados.

## Referências

- [Ataque cibernético à Solar Winds](https://www.fortinet.com/br/resources/cyberglossary/solarwinds-cyber-attack)
- [SolarWinds Orion (CVE-2020-10148)](https://www.ibm.com/docs/en/randori?topic=2022-solarwinds-orion-cve-2020-10148)
- [SolarWinds Cyberattack Demands Significant Federal and Private-Sector Response (infographic)](https://www.gao.gov/blog/solarwinds-cyberattack-demands-significant-federal-and-private-sector-response-infographic)