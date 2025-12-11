---
tags:
  - firewall
  - pfsense
  - tcc
  - defesa
data criação: 2025-12-11T15:59:00
---
### 1. Defesa Metodológica de Projeto de Segurança Comparativa

Essa é uma excelente antecipação de questionamento. A defesa deve focar na **metodologia comparativa** do projeto e na distinção entre o **relatório técnico (o artigo)** e a **apresentação oral (o resumo executivo)**.

Abaixo estão sugestões de defesa, estruturadas com base na informação presente nas fontes, para justificar a priorização dos dados comparativos na apresentação:

1. Ênfase no Objetivo Central e na Comprovação da Eficácia

A principal defesa é que o foco da apresentação deve estar na **análise da eficácia** da solução, e não apenas na descrição detalhada do cenário inicial.

> "Nosso trabalho utilizou uma **abordagem aplicada e experimental**, cujo objetivo principal não era apenas descrever a vulnerabilidade inicial, mas sim **analisar a eficácia** da implementação do pfSense junto ao pfBlocker-NG [1]. Portanto, a apresentação se concentra nos **resultados comparativos**, pois eles são a comprovação direta do sucesso do projeto [1, 2]." > > "O cerne da nossa pesquisa é demonstrar a **redução significativa em portas expostas** e o aumento da segurança da rede [1, 3]. Para isso, destacamos o contraste: o resultado da varredura _após_ a proteção, que mostra a queda de **27 portas para 3** [4], e o sucesso no **bloqueio de 39%** dos pacotes, que é a métrica mais relevante para a banca avaliar o impacto [5]."

2. Rigor Metodológico: O "Antes" como Linha de Base Documentada

É crucial garantir à banca que o cenário "antes" foi rigorosamente documentado e é a base de todas as conclusões, mesmo que não tenha sido exibido em slides detalhados.

> "A metodologia do trabalho exigiu, explicitamente, a **coleta e análise de dados antes e depois** da implantação do pfSense [1]. O cenário 'antes da proteção' serviu como a **linha de base (baseline) obrigatória** para a validação [6]." > > "Tanto o artigo quanto o relatório de resultados detalham completamente os dados do antes: a Tabela 1 [7] e a Tabela 2 [4] mostram que antes da proteção havia 27 portas abertas (incluindo 22, 80 e 445) e que a conectividade era **estabelecida** na porta 22 e 80 [8]. Geramos um **relatório com todas as vulnerabilidades expostas** encontradas antes da implementação [6]." > > "Adicionalmente, todos os dados brutos e _logs_ (Nmap, Ncat, Wireshark) do cenário 'Defesa desativada' estão **anexados digitalmente** ao trabalho [9, 10], garantindo a transparência e a auditabilidade de nossa pesquisa, conforme exigido pelos **critérios de avaliação** [6]."

3. Foco na Mensagem da Apresentação

Uma apresentação tem restrições de tempo, e o foco deve ser a **conclusão** baseada nas evidências mais impactantes.

> "Optamos por focar na apresentação nos **dados do 'depois'** e no **comparativo**, pois é o que demonstra a **solução** proposta pelo nosso TCC. Apresentar os dados de vulnerabilidade inicial é importante, mas o contraste é a informação mais concisa para evidenciar que o pfSense conseguiu com sucesso realizar ações como a **mitigação de riscos de invasão** e a **restrição de tráfego** a serviços autorizados [11]." > > "O nosso trabalho de campo comprovou a transição de um estado de **vulnerabilidade para segurança**; as tabelas comparativas (Tabela 1, Tabela 2 e Tabela 4) já carregam consigo os dados do cenário 'antes' para estabelecer o contraste e validar a eficácia do firewall [4, 7, 8]."

--------------------------------------------------------------------------------
### 2. Análise de Portas e Eficácia do pfSense/pfBlocker-NG

As fontes fornecem uma análise detalhada das portas identificadas como vulneráveis antes da implementação do firewall pfSense e do pacote pfBlocker-NG, bem como a lista de serviços essenciais que permaneceram ativos após a proteção.

É importante notar que os estudos anexados descrevem a **necessidade de mitigar riscos cibernéticos** [1] e a ocorrência de **tentativas de ataque cibernético** a empresas [2], mas **não detalham casos específicos de ataques reais e históricos** associados a cada uma das portas listadas, focando na comprovação da eficácia da solução [3], [4].

--------------------------------------------------------------------------------

1. Portas Críticas Abertas Antes da Implementação

Antes da configuração de segurança, a rede da PME apresentava um total de **27 portas abertas** [5]. As principais portas identificadas que representavam riscos significativos, pois permitiam acessos, eram:

|   |   |   |
|---|---|---|
|Porta (Serviço)|Descrição do Serviço e Riscos Expostos|Contexto de Risco Cibernético (Conforme as Fontes)|
|**21 (FTP)** (File Transfer Protocol)|Protocolo utilizado para transferência de arquivos. É inerentemente inseguro quando não criptografado, expondo credenciais e dados em texto simples.|A existência de protocolos inseguros como o FTP foi eliminada após a implementação, o que contribuiu para a **redução de exposição a ataques** [6], [7].|
|**22 (SSH)** (Secure Shell)|Protocolo que permite acesso remoto seguro a um sistema. Uma porta SSH aberta, se mal configurada ou com senhas fracas, é um alvo primário para ataques de força bruta visando **acesso não autorizado** [8].|A conectividade nessa porta estava estabelecida antes da proteção, sendo um alvo para **tentativas de intrusão** [9]. Após a implementação, o acesso nessa porta foi ativamente **recusado** [9].|
|**80 (HTTP)** (Hypertext Transfer Protocol)|Protocolo base para navegação web não criptografada. Exposição dessa porta permite a interceptação de dados, ataques _man-in-the-middle_ e _phishing_, facilitando o roubo de dados sigilosos [10].|A presença dessa porta visível representava um risco de **conexões inseguras** [6], permitindo a **conexão estabelecida** antes da proteção [9].|
|**445 (SMB)** (Server Message Block)|Protocolo de rede usado para compartilhar arquivos e impressoras. Sua exposição é altamente crítica, sendo frequentemente explorada por _ransomware_ e _worms_ de rede para se propagar internamente e externamente.|A redução dessas portas expostas foi fundamental para a **mitigação de riscos de invasão** e o aumento da segurança geral da rede [3], [8], [7].|

Além dessas, o Nmap também detectou portas como a **139** e **3306** [11], indicando outros serviços (como NetBIOS e MySQL, respectivamente) expostos na rede inicial.

--------------------------------------------------------------------------------

2. Portas Remanescentes Após a Implementação

Após a configuração do pfSense junto ao pacote pfBlocker-NG, o número total de portas abertas caiu para apenas **3** [5], o que representou uma redução de aproximadamente 89% no número de portas expostas [12].

As portas que permaneceram abertas na rede foram:

|   |   |   |
|---|---|---|
|Porta|Serviço|Motivo da Permanência (Serviço Autorizado/Essencial)|
|**80 (HTTP interno)**|Hypertext Transfer Protocol (Uso Interno)|Esta porta permaneceu aberta para uso **interno** [5]. Embora o tráfego HTTP inseguro tenha sido eliminado [6], a porta pode ser necessária para acessar serviços ou sistemas internos da empresa (como CRM ou servidores de impressão). Contudo, testes externos feitos com Ncat resultaram em **"Timeout"** [9], indicando que o acesso externo foi efetivamente bloqueado ou descartado pelo firewall, protegendo a rede.|
|**443 (HTTPS)**|Hypertext Transfer Protocol Secure|Esta é a porta padrão para comunicação **segura e criptografada** [13]. Sua permanência é essencial para que os colaboradores da PME possam acessar a internet, sistemas de trabalho (CRM) e sites de pesquisa [14], sendo o **tráfego restringido majoritariamente a pacotes HTTPS** [5]. A conexão nessa porta permaneceu **estável** [9].|
|**53 (DNS)**|Domain Name System|O DNS é um serviço de rede fundamental para a **resolução de nomes**, permitindo que os navegadores encontrem servidores web [15]. A porta é necessária para o funcionamento básico da internet na rede e, portanto, é considerada um **serviço autorizado** [7].|

Em resumo, as três portas que permaneceram abertas são **serviços autorizados e necessários** para o funcionamento da rede e acesso seguro à internet, enquanto o tráfego inseguro (como HTTP e FTP) foi eliminado [6], e o acesso não autorizado a portas críticas foi bloqueado [8], [7].

--------------------------------------------------------------------------------

### 3. Defesa e Fundamentação da Solução pfSense

Abaixo estão as respostas aceitáveis para as perguntas simuladas da banca, fundamentadas estritamente nas informações fornecidas nas fontes:

Professor(a) A: Foco Estratégico e Justificativa de Negócio

Pergunta 1: Viabilidade Econômica vs. Custo Total de Propriedade (TCO)

"O trabalho defende que o pfSense é uma **solução** **open-source** **viável e acessível** para PMEs, eliminando altos custos de licenciamento [1]. No entanto, a implementação requer **hardware dedicado** (máquina com 8 GB de RAM e 120 GB SSD [2]) e, presumivelmente, **mão de obra especializada** para instalação e manutenção. Como o senhor(a) garante que o **Custo Total de Propriedade (TCO)** dessa solução _open-source_ é significativamente inferior ao de uma solução _appliance_ proprietária de entrada, que incluiria suporte e manutenção simplificada?"

**Resposta:** A garantia do TCO reduzido se deve primariamente à natureza **open-source** do pfSense, que **elimina os custos de licenciamento** associados a grandes marcas como Cisco, Palo Alto ou Fortinet [3]. A flexibilidade do pfSense permite a instalação em _hardware_ dedicado, inclusive em máquinas antigas [3], embora no nosso estudo tenhamos utilizado uma máquina com 8 GB de RAM e 120 GB SSD para garantir o desempenho [2]. Essa flexibilidade de _hardware_ reduz os custos iniciais de implantação [3]. Além disso, a comunidade ativa de desenvolvedores do pfSense garante **atualizações frequentes** e correção de vulnerabilidades [4], o que, a longo prazo, minimiza a dependência de contratos de suporte proprietário caros [3, 4]. O pfSense demonstrou atingir **níveis de desempenho comparáveis a firewalls comerciais**, confirmando sua viabilidade técnica e econômica [4].

Pergunta 2: Conformidade com a LGPD

"Uma das motivações citadas é a **conformidade com a LGPD**, exigindo medidas técnicas adequadas para proteção de dados [3]. O pfSense oferece recursos como VPN e filtragem de pacotes. Poderia explicar **qual é o impacto direto da implementação do pfSense** no cumprimento das **exigências técnicas** estabelecidas pela Lei Geral de Proteção de Dados, considerando que a lei foca no tratamento de dados pessoais?"

**Resposta:** A LGPD exige que as empresas adotem **medidas técnicas adequadas** para prevenir vazamentos e acessos não autorizados a dados pessoais [3]. O pfSense contribui diretamente para o cumprimento dessas exigências, fornecendo **recursos essenciais de segurança de rede** [5]. Especificamente, a ferramenta oferece **filtragem de pacotes e inspeção de tráfego** para bloquear ameaças, **VPN (Virtual Private Network)** para acesso remoto seguro criptografado [5], e _Proxy e filtro de conteúdo_ (funções aprimoradas pelo pfBlocker-NG) para evitar o acesso a sites maliciosos [5, 6]. Essas funcionalidades minimizam os riscos de incidentes de segurança [5], que poderiam resultar em multas e sanções por falhas na proteção de dados, conforme previsto na lei [5]. A aplicação do pfSense em conjunto com as diretrizes da **ISO/IEC 27001** fortalece ainda mais a gestão de segurança da informação e a conformidade legal [7, 8].

Pergunta 3: Escalabilidade da Solução

"O estudo de caso foi realizado em uma PME específica do ramo de Construtora com dez máquinas ativas [9, 10]. Se essa empresa experimentar um crescimento rápido, dobrando seu número de colaboradores para 20 ou 30, a solução implantada (pfSense com pfBlocker-NG) na configuração de hardware utilizada (8 GB RAM, 120 GB SSD [2]) **manteria o mesmo desempenho e eficácia de filtragem** que vocês demonstraram?"

**Resposta:** O pfSense é conhecido por sua **flexibilidade**, permitindo a aplicação em diversos cenários, desde pequenas empresas até grandes infraestruturas de TI [11]. Embora o estudo de caso tenha sido realizado em uma PME com dez máquinas [10], o _hardware_ dedicado utilizado (8 GB de RAM e 120 GB SSD) [2] foi selecionado para suportar o ambiente, e o pfSense, por ser modular, permite que o desempenho seja otimizado [8]. A eficácia demonstrada no trabalho – mitigação de riscos, restrição de tráfego e redução de portas expostas [12-14] – é alcançável em ambientes maiores, pois o **desempenho do pfSense é comparável a firewalls comerciais** [4]. No caso de expansão para 20 ou 30 usuários, se houver degradação de desempenho, a **flexibilidade de** **hardware** do pfSense permite a atualização dos recursos da máquina (RAM, CPU) de forma mais econômica do que a substituição de um _appliance_ proprietário [3].

Professor(a) B: Foco Técnico e Metodologia Prática

Pergunta 4: Diferença na Metodologia de Teste

"A metodologia utilizou diversas ferramentas de auditoria de segurança [15]. No contexto de validação das regras do firewall (etapa 'e' da metodologia [16]), qual foi a **diferença técnica fundamental** entre a varredura de portas realizada pelo **Nmap** e o teste de conectividade realizado pelo **Ncat**? Como a combinação dessas duas ferramentas permitiu uma comprovação mais robusta da segurança implementada?"

**Resposta:** A diferença fundamental reside na função de cada ferramenta: o **Nmap** foi utilizado para **varredura de portas e detecção de serviços ativos** na rede [15], fornecendo um panorama geral da exposição [14]. Já o **Ncat** (Netcat) foi empregado para **comunicação direta via rede para testar requisições** [15], verificando ativamente a conectividade TCP em portas específicas [13]. A combinação permitiu uma comprovação robusta porque: 1) o Nmap identificou quais portas estavam abertas _antes_ da proteção (27 portas) e _depois_ (3 portas) [14, 17]; e 2) o Ncat foi usado para **simular tentativas de conexão direta** nessas portas (como a porta 22 - SSH e 80 - HTTP) [13]. Os testes de Ncat confirmaram a **ação do firewall** em tempo real, mostrando a **"Conexão recusada"** ou **"Timeout"** [13], validando as regras de bloqueio do pfSense e do pfBlocker-NG, um dos objetivos específicos da pesquisa [18].

Pergunta 5: Análise Específica de Portas Remanescentes

"O resultado comparativo demonstrou uma redução expressiva de 27 portas abertas para apenas 3: 80 (HTTP interno), 443 (HTTPS) e 53 (DNS) [17]. No entanto, a Tabela 4 mostra que, para a porta 80 (HTTP), a tentativa de conexão resultou em 'Timeout' após a proteção, enquanto na porta 443 (HTTPS) a conexão permaneceu 'estável' [13]. Qual configuração específica do pfSense ou do pfBlocker-NG causou o comportamento de **'Timeout' na porta 80** e por que esse tráfego não foi simplesmente 'Conexão Recusada' como ocorreu na porta 22 (SSH)?"

**Resposta:** O resultado "Timeout" na porta 80, em contraste com a "Conexão recusada" na porta 22 (SSH), sugere uma diferença na **ação de filtragem** configurada nas regras do firewall. Uma "Conexão recusada" (RST/ACK) indica que o firewall ou serviço rejeitou ativamente o pacote [13], enquanto um **"Timeout"** (nenhuma resposta) geralmente indica que o pacote foi **silenciosamente descartado (****dropped****)** [13]. Essa configuração de _drop_ (timeout) é comumente usada para **ocultar a presença do serviço** de varreduras externas, enquanto a porta 443 permaneceu estável porque o tráfego HTTPS foi restringido majoritariamente [17] e era um **serviço autorizado** e essencial [12]. O objetivo da implementação, que incluiu o pfBlocker-NG, era **restringir o tráfego indevido** (como HTTP e FTP) [17] e **reduzir a exposição a ataques e exploração de portas** [12], o que foi comprovado pelo _Timeout_ na porta 80 e a recusa na porta 22 [13].

Pergunta 6: Base Operacional do pfSense

"O pfSense é uma distribuição baseada no **FreeBSD** [11]. A escolha deste sistema operacional subjacente teve alguma implicação técnica ou de segurança observada durante a implementação, especialmente em comparação com outras soluções baseadas em Linux que poderiam ser usadas como base para um firewall _open-source_?"

**Resposta:** O pfSense é, de fato, uma distribuição baseada no **FreeBSD (Berkeley Software Distribution)** [1]. A escolha desta base implica vantagens técnicas e de segurança inerentes ao FreeBSD [11]. Embora o artigo não compare diretamente com o Linux, ele ressalta que o pfSense é uma ferramenta _open-source_ que permite a **auditoria de código** e **personalização**, além de possuir uma **comunidade de desenvolvimento ativa** [11]. Essa transparência e a base em FreeBSD são características que **melhoram significativamente a segurança e o gerenciamento de redes empresariais**, sendo uma solução eficiente para controle de tráfego e segurança [11].

Professor(a) C: Foco Crítico e Sugestões para o Futuro

Pergunta 7: A Contribuição do pfBlocker-NG e Futura Implementação de IDS/IPS

"O pfBlocker-NG foi crucial, sendo integrado ao pfSense para a filtragem e bloqueio de ameaças [6]. Nos resultados, vocês documentaram um **percentual de bloqueio de pacotes de 39%** [19]. Contudo, nas considerações finais, é sugerida a implementação futura de um **IDS/IPS (Sistema de Detecção/Prevenção de Intrusões)** [12, 20]. Se o pfBlocker-NG já realiza um trabalho de filtragem e bloqueio, como um IDS/IPS complementaria — em vez de simplesmente sobrepor — as funcionalidades de segurança já ativas pelo pfBlocker-NG na rede da PME?"

**Resposta:** O pfBlocker-NG, como parte da solução implementada, é um pacote de **bloqueio e filtragem** essencial para o estudo [18], atuando principalmente na filtragem de conteúdo e tráfego com base em regras e listas [6]. Contudo, um IDS/IPS (Sistema de Detecção/Prevenção de Intrusões) complementaria a segurança ao fornecer **monitoramento em tempo real** e **bloqueio de atividades maliciosas baseadas em padrões de ataque** [5, 20]. Enquanto o pfBlocker-NG se concentra em bloquear IPs e domínios maliciosos ou indesejados (filtragem de conteúdo), um IDS/IPS opera em um nível mais profundo, inspecionando o conteúdo dos pacotes para **detectar e prevenir intrusões** por meio de assinaturas ou anomalias comportamentais [5, 20]. Portanto, a implementação de um IDS/IPS seria uma **camada extra de proteção**, abordando ameaças que a filtragem baseada em listas de bloqueio (feita pelo pfBlocker-NG) possa não capturar [20].

Pergunta 8: Generalização dos Resultados do Estudo de Caso

"Este trabalho utilizou a metodologia de **estudo de caso** focado em uma única PME [9]. Como o senhor(a) pode argumentar pela **generalidade** da conclusão de que 'A solução mostrou-se economicamente viável, de fácil manutenção e compatível com a infraestrutura existente' [1], quando os resultados (como a redução de portas abertas) são específicos de uma rede com apenas dez colaboradores [10] e um tipo de infraestrutura preexistente?"

**Resposta:** O estudo de caso foi fundamental para uma **observação prática** do desempenho e segurança em um **ambiente real de PME** [1, 9]. Embora o estudo tenha se limitado a uma Construtora com dez máquinas ativas [9, 10], a conclusão sobre a viabilidade econômica [1] é sustentada pelo fato de o **pfSense ser** **open-source** **e eliminar custos de licenciamento**, uma vantagem aplicável a qualquer PME com limitações orçamentárias [3, 21]. Os resultados práticos de **redução significativa em portas expostas** e **melhora na visibilidade do tráfego interno e externo** [1] são indicadores de sucesso que podem ser replicados. Além disso, a flexibilidade de _hardware_ do pfSense [3] e sua capacidade de adaptação a diversos cenários [11] demonstram que a solução é **tecnicamente viável** para PMEs, confirmando sua adequação como alternativa acessível [1]. O trabalho se junta a outros estudos que apontam o pfSense como robusto em ambientes corporativos [22].

Pergunta 9: Natureza do Tráfego Bloqueado

"O monitoramento do tráfego após a implementação resultou no bloqueio de 3.600 pacotes (39%) [19]. O que a análise dos logs do pfBlocker-NG, que foi um **critério de avaliação** [23], revelou sobre a **natureza desse tráfego bloqueado**? Eram tentativas de intrusão, acessos a sites maliciosos, ou apenas pacotes de protocolos indesejados (como HTTP/FTP), e qual foi a maior contribuição percentual para essa taxa de 39% de bloqueio?"

**Resposta:** O trabalho não especifica a **contribuição percentual** exata de cada tipo de tráfego para os 39% de bloqueio [19]. No entanto, a análise dos resultados (Tabela 3) e os critérios de avaliação indicam que o tráfego bloqueado incluía:

1. **Protocolos Inseguros/Indevidos:** Após a implementação do pfSense junto ao pfBlocker-NG, o monitoramento de tráfego pelo Wireshark mostrou que o **tráfego HTTP/FTP** antes visível foi **bloqueado** [17, 24]. Isso contribuiu para a redução de tráfego indevido [17, 25]. O **tráfego ICMP** (Ping) também foi **bloqueado** [24].

2. **Tentativas de Acesso Não Autorizado:** Um dos critérios de avaliação foi o **registro de logs do pfBlocker-NG, evidenciando bloqueios de acessos não autorizados na rede** [23, 25]. O objetivo era mitigar riscos de invasão e reduzir a exposição a ataques [12].

3. **Sites Maliciosos/Alertas:** A metodologia incluiu a **coleta de alertas de acessos e sites bloqueados no pfBlocker-NG** [16].

Portanto, a taxa de 39% de bloqueio resulta da ação combinada de **regras de firewall** (bloqueando protocolos como HTTP, FTP e ICMP) e das **regras de filtragem do pfBlocker-NG** (bloqueando acessos não autorizados e potencialmente sites maliciosos), o que confirmou a eficácia da solução [1, 13].