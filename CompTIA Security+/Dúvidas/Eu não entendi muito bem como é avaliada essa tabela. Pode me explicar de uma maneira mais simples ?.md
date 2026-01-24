![Uma tabela dividida em 4 colunas resume os resultados da análise de lacunas, mostrando o número de controles recomendados não implementados por função e categoria; além dos riscos à confidencialidade, integridade e disponibilidade decorrentes dos controles ausentes; e a data-alvo para remediação.](https://cdn.testout.com/pt-br-security-plus-v7-study-pt-br/materials/resources/text/s_security_concepts/pt-br_2933-1692974858414.png)

Essa tabela é uma ferramenta clássica de **Análise de Lacunas** (ou _Gap Analysis_). Pense nela como um "boletim escolar" da segurança da empresa, comparando o que ela **tem** hoje versus o que ela **deveria ter**.

Aqui está uma explicação simplificada de cada parte:

**1. O "Semáforo" (As Cores)**

As cores indicam o nível de maturidade ou qualidade atual:

- 🔴 **Vermelho (Capacidade básica/inexistente):** É a área crítica. Significa que a empresa tem muito pouca (ou nenhuma) proteção nesse setor. Precisa de atenção urgente.
    
- 🟡 **Amarelo (Capacidade intermediária):** A empresa já tem alguma coisa implementada, mas ainda não está completa ou perfeita.
    
- 🟢 **Verde (Capacidade avançada):** (Não aparece nas linhas, mas está na legenda) Significaria que está tudo ótimo e bem protegido.
    

**2. A Matemática dos Controles (Ex: 4/6)**

Na coluna "Controles", os números entre parênteses são uma conta simples de **"Tenho / Deveria Ter"**.

- Exemplo em _Gerenciamento de ativos (4/6)_: Significa que a norma de segurança exige **6** controles (regras ou ferramentas), mas a empresa só implementou **4**. Faltam 2 para ficar completo.
    
- Exemplo em _Segurança de dados (3/8)_: De **8** controles necessários, só existem **3**. Por isso está vermelho; falta muita coisa.
    

**3. O Risco (A sopa de letrinhas C-I-A)**

A coluna "Níveis de risco" mostra o tamanho do problema se essa área falhar. Ela usa a tríade da segurança:

- **C (Confidencialidade):** O segredo dos dados.
    
- **I (Integridade):** A garantia de que os dados não foram alterados.
    
- **A (Disponibilidade):** A garantia de que o sistema não vai cair.
    
- **O Número (ex: 9):** Indica a gravidade (provavelmente de 1 a 10).
    
    - Note que na área vermelha ("Proteger"), os riscos de Integridade (I) e Confidencialidade (C) estão em **9**. Isso traduz: "Se falharmos aqui, o estrago no sigilo e na confiança dos dados será enorme".
        

**4. Remediação Programada**

É simplesmente o **prazo** para consertar o problema.

- _1º TRI_ significa Primeiro Trimestre (Jan/Fev/Mar). Como as áreas vermelhas são mais urgentes e arriscadas (risco 9), elas estão agendadas para serem arrumadas logo no começo do ano.
    

**Resumo da Ópera:**

A tabela está dizendo: _"Na parte de **Identificar** problemas, estamos medianos (amarelo) e vamos melhorar até o fim do ano. Mas na parte de **Proteger** os dados, estamos muito fracos (vermelho), o risco é altíssimo (9) e precisamos resolver isso urgente agora no começo do ano."_