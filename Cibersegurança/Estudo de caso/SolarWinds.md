---
tags: [segurança, supply-chain, malware, incidentes, SolarWinds, APT]
data-incidente: 2020-03-26
tipo: ataque à cadeia de suprimentos (backdoor)
cve: CVE-2020-10148
---

## Resumo
Ataque à cadeia de suprimentos de software descoberto em dezembro de 2020. Atores APT comprometeram o processo de build do **SolarWinds Orion** e inseriram o backdoor **Sunburst** em atualizações legítimas e assinadas. Mais de 18.000 clientes instalaram a atualização trojanizada, mas apenas cerca de uma centena de organizações foram de fato exploradas em uma segunda fase.

![[Screenshot 2026-10-07 at 14-36-04 Hackers da SolarWinds lançaram novo ataque segundo a Microsoft.png]]

## Como funcionava
1. **Acesso inicial:** em setembro de 2019, os invasores entraram na rede da SolarWinds.
2. **Teste:** em outubro de 2019, testaram a injeção de código no Orion.
3. **Comprometimento do build:** em fevereiro de 2020, o código Sunburst foi inserido na DLL do Orion durante o processo de compilação.
4. **Distribuição:** em 26/03/2020, a SolarWinds começou a distribuir as atualizações infectadas como patches legítimos.
5. **Dormência e C2:** após instalado, o Sunburst ficava inativo por um período e então se comunicava via DNS/HTTPS com um domínio de comando e controle.
6. **Segunda fase:** nos alvos de interesse, os invasores implantavam outras ferramentas, roubavam credenciais e se moviam lateralmente na rede e na nuvem.

## Origem e atribuição
* O malware foi inserido na cadeia de build, não explorando uma falha do produto.
* EUA e Reino Unido atribuíram o ataque ao **APT29** (SVR, Rússia), também chamado de Cozy Bear ou UNC2452.
* A descoberta foi feita pela **FireEye** em dezembro de 2020, ao investigar a própria invasão.

## Vulnerabilidade associada (CVE-2020-10148)
* Bypass de autenticação na API do Orion, por meio de parâmetros específicos em uma solicitação de URI.
* Permite que um invasor não autenticado execute comandos de API.
* **Não é o vetor do Sunburst.** Foi explorada em um incidente separado para instalar o malware **Supernova** (web shell).

## Impacto
* Custo médio de cerca de 11% da receita anual das empresas afetadas (14% nos EUA, 8,6% no Reino Unido).
* Cerca de US$ 90 milhões em perdas seguradas, principalmente com resposta a incidentes e forense.
* Custo médio de cerca de US$ 12 milhões por organização, por tempo de inatividade e atualizações de segurança.
* O incidente expôs fraquezas na segurança da cadeia de suprimentos e reforçou padrões de gestão de risco de terceiros.

## Kill switch
* Em dezembro de 2020, o domínio de C2 do Sunburst foi neutralizado (sinkhole) por FireEye, Microsoft e GoDaddy.
* O sinkhole fez o malware se desativar nos hosts que consultavam o domínio.
* Isso não removeu o acesso já obtido pelos invasores, que usavam outros implantes.

## Principais vítimas
* Agências dos EUA: Tesouro, Comércio, DHS, Departamento de Estado, entre outras.
* Empresas de tecnologia: Microsoft, FireEye, Cisco, Intel, Nvidia.
* Organizações que, por si só, poderiam ser difíceis de invadir, mas foram atingidas via fornecedor confiável.

## Indicadores úteis (IOCs)
* DLL comprometida: `SolarWinds.Orion.Core.BusinessLayer.dll`
* Versões afetadas do Orion: 2019.4 HF5, 2020.2 e 2020.2 HF1
* Domínio de C2: `avsvmcloud[.]com`
* Tráfego DNS anômalo saindo de servidores Orion
* Criação de contas ou tokens SAML suspeitos no Active Directory

## Mitigação / lições aprendidas
* Atualizar o Orion para a versão corrigida (2020.2.1 HF2 ou superior)
* Isolar servidores Orion e bloquear acesso direto à internet
* Redefinir credenciais, certificados e tokens potencialmente expostos
* Auditar alterações no Active Directory
* Centralizar logs em um SIEM para detectar mudanças de comportamento e movimentação incomum de dados
* Reforçar prevenção contra perda de dados (DLP)
* Realizar testes de penetração regulares
* Proteger o pipeline de build (assinatura de código, SBOM, ambientes isolados)
* Aplicar privilégio mínimo em contas de serviço de ferramentas de gestão

## Relacionado
* Supply chain attack
* Sunburst
* Supernova
* APT29
* [[WannaCry]] (outro incidente de grande escala, mas com vetor diferente)

## Referências
* [Ataque cibernético à Solar Winds](https://www.fortinet.com/br/resources/cyberglossary/solarwinds-cyber-attack)
* [SolarWinds Orion (CVE-2020-10148)](https://www.ibm.com/docs/en/randori?topic=2022-solarwinds-orion-cve-2020-10148)
* [SolarWinds Cyberattack Demands Significant Federal and Private-Sector Response](https://www.gao.gov/blog/solarwinds-cyberattack-demands-significant-federal-and-private-sector-response-infographic)
* CISA Alert AA20-352A