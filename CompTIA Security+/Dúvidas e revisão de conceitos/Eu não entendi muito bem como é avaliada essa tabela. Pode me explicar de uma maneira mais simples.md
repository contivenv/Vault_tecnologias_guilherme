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

# Certo, eu entendi mais ou manos tudo agora, ainda só não entendi como funciona a classificação de numeração na tríade CIA. As letras e logo depois delas, números.

Ótima pergunta. Essa é a parte que transforma a tabela de uma simples lista de tarefas em uma **matriz de risco**.

Para entender essa numeração, imagine que ela é uma **Nota de Gravidade** (ou Impacto), geralmente em uma escala de **0 a 10**.

Ela responde à pergunta: **"Se essa proteção falhar, qual o tamanho do estrago para a empresa?"**

Aqui está como ler cada linha:

### 1. A Escala (O "Termômetro")

- **Notas Baixas (1 a 3):** Se der problema, é chato, mas a empresa continua funcionando. Ninguém vai ser demitido ou processado.
    
- **Notas Médias (4 a 6):** O problema é sério. Pode causar prejuízo financeiro ou parar um departamento.
    
- **Notas Altas (7 a 10):** Catástrofe. A empresa pode falir, levar multas milionárias (LGPD) ou perder a reputação no mercado.
    

### 2. Decifrando o exemplo da tabela

Vamos pegar a linha vermelha de **"Segurança de dados"**, que é a mais crítica. Lá está escrito:

> **C: 9 | I: 9 | A: 4**

Veja a tradução disso para o mundo real:

- **C: 9 (Confidencialidade - Risco Altíssimo):**
    
    - _Significado:_ Se a segurança de dados falhar, o sigilo será quebrado.
        
    - _Cenário:_ Vazamento de senhas de clientes ou segredos industriais na internet. Como é nota **9**, o impacto é devastador.
        
- **I: 9 (Integridade - Risco Altíssimo):**
    
    - _Significado:_ Se falhar, os dados podem ser alterados indevidamente.
        
    - _Cenário:_ Um hacker entra e muda o saldo bancário das contas ou apaga registros de vendas. Você não pode mais confiar nos dados. Nota **9** é gravíssimo.
        
- **A: 4 (Disponibilidade - Risco Médio/Baixo):**
    
    - _Significado:_ Se falhar, o sistema sai do ar.
        
    - _Cenário:_ O sistema fica lento ou cai por algumas horas. É ruim (nota **4**), mas é **menos grave** do que os dados vazarem (nota 9). A empresa prefere o sistema fora do ar do que os dados roubados.
        

### Por que isso é importante?

Essa numeração explica **por que** a "Remediação" dessa linha está marcada para o **1º Trimestre**.

O gestor olhou e pensou: _"Tenho um risco nota 9 aqui. Tenho que resolver isso em Janeiro. Aquele risco nota 6 lá de cima (Governança) eu posso deixar para o 3º Trimestre."_

Em resumo: O número diz o **tamanho da dor de cabeça** se aquela área específica da segurança falhar.

# E como essa nota é classificada de 0 a 10 ? O time de TI senta e conversa sobre ou existe algum padrão para seguir e dar essas numerações ?

A resposta curta é: **as duas coisas acontecem.**

Existe um "padrão técnico" (uma régua oficial) para garantir que o número não seja inventado, mas existe também a "conversa do time" (contexto do negócio) para ajustar essa nota à realidade da empresa.

Aqui está como essa "mágica" do 0 a 10 geralmente acontece:

### 1. A Régua Técnica (O Padrão Ouro: CVSS)

No mundo da cibersegurança, existe um sistema padronizado mundialmente chamado **CVSS** (_Common Vulnerability Scoring System_). Ele é uma calculadora que gera essa nota de 0 a 10 baseada em fatos técnicos, sem "achismo".

Geralmente, o analista responde a perguntas objetivas em uma calculadora de CVSS:

- **O ataque pode ser feito pela internet?** (Sim = nota sobe)
    
- **Precisa de senha para atacar?** (Não = nota sobe)
    
- **O atacante precisa ser um gênio ou qualquer um consegue?** (Qualquer um = nota sobe)
    
- **Se der certo, para tudo ou só uma parte?** (Tudo = nota sobe)
    

O resultado sai classificado assim:

- **0.1 – 3.9:** Baixo (Low)
    
- **4.0 – 6.9:** Médio (Medium)
    
- **7.0 – 8.9:** Alto (High)
    
- **9.0 – 10.0:** Crítico (Critical)
    

### 2. A "Conversa do Time" (Contextualização)

É aqui que o time de TI senta com a diretoria. Porque uma falha técnica nota 10 (Crítica) no papel pode não ser tão grave se aquele servidor não tiver nada importante.

Eles usam uma **Matriz de Risco** para ajustar a nota. A fórmula básica que eles discutem é:

$$Risco = Probabilidade \times Impacto$$

![Imagem de risk assessment matrix cybersecurity](https://encrypted-tbn2.gstatic.com/licensed-image?q=tbn:ANd9GcR__bcvNp_IjVgrvq5glwCYsHCtSCfSoMUM-JUsTxhIJYZSvjF8r_Lyo2AxtJPNbG2AUd_e0TWxhjIcTR8nfRlMgiLQTFpCzUwb5Cim5jyPXP6yNbw)

Shutterstock

Explorar

A discussão na mesa seria algo assim:

> **Analista Técnico:** "Temos uma falha no sistema de login. Pelo padrão CVSS, é nota 9."
> 
> **Diretor:** "Ok, mas esse sistema de login é do que?"
> 
> **Analista:** "É do sistema que controla a temperatura do ar condicionado."
> 
> **Diretor:** "Ah, então baixa essa nota. Se invadirem, a gente passa calor, mas a empresa não para. Vamos classificar o risco de negócio como **4**."

**OU O CONTRÁRIO:**

> **Analista:** "Temos uma falha simples, nota 4, no banco de dados."
> 
> **Diretor:** "Que banco de dados?"
> 
> **Analista:** "O que guarda os cartões de crédito dos clientes."
> 
> **Diretor:** "Então sobe isso para **10** agora! Se vazar um cartão, tomamos uma multa milionária e perdemos a reputação."

### 3. Resumindo a sua tabela

Naquela tabela da imagem que você mandou, os números **9** na tríade (C e I) provavelmente nasceram dessa combinação:

1. **Fator Técnico:** A falta de "Gerenciamento de Identidade" permite que qualquer pessoa acesse o sistema (facilidade técnica alta).
    
2. **Fator de Negócio:** Se alguém acessar, rouba dados vitais da empresa (impacto no negócio altíssimo).
    

Portanto, **Impacto Máximo + Facilidade de Ataque = Nota 9 ou 10.**

# Agora me fale quais são os requisitos para serem seguidos e onde eles são baseados. Por exemplo 4/8. Tem 4 sendo cumpridos, mas e os outros 4. De onde tira esses "8" requisitos ?

Essa é a pergunta de "um milhão de dólares": quem define as regras do jogo?

Esses requisitos não saem da cabeça do chefe de TI. Eles vêm de documentos oficiais chamados **Frameworks de Cibersegurança** (ou Estruturas de Segurança). Eles funcionam como uma "Bíblia" ou um "Código de Obras" para a área de tecnologia.

Olhando para a sua imagem, pelos termos usados ("Identificar", "Proteger", "Gerenciamento de Ativos"), é quase certeza que essa tabela está baseada no **NIST CSF** (_NIST Cybersecurity Framework_), que é o padrão mais famoso do mundo.

Aqui está de onde saem esses números (como o "8"):

### 1. O Livro de Regras (O Framework)

Imagine que o **NIST** é um livro de receitas enorme.

- Ele tem um capítulo chamado **"Proteger"**.
    
- Dentro desse capítulo, tem uma seção chamada **"Segurança de Dados"**.
    
- Nessa seção, o NIST lista, por exemplo, **8 ingredientes obrigatórios** para você dizer que seus dados estão seguros.
    

Esses "8" são os **Controles de Segurança** (as regras).

### 2. O que seriam esses "8" requisitos?

Para você visualizar, se o requisito total é 8, a lista oficial (baseada no NIST) seria algo parecido com isso:

1. ❌ **Criptografia em repouso** (Dados salvos no HD estão embaralhados?) -> _Falta fazer._
    
2. ✅ **Criptografia em trânsito** (Dados via internet estão protegidos?) -> _Feito._
    
3. ❌ **Prevenção contra vazamento (DLP)** (O sistema bloqueia envio de arquivos confidenciais?) -> _Falta fazer._
    
4. ✅ **Destruição segura** (Quando joga um HD fora, destrói os dados?) -> _Feito._
    
5. ✅ **Capacidade de Proteção** (Tem backup?) -> _Feito._
    
6. ❌ **Ambientes de Teste** (Os testes usam dados falsos ou reais?) -> _Falta fazer._
    
7. ❌ **Verificação de Integridade** (Tem software que avisa se mudaram um arquivo?) -> _Falta fazer._
    
8. ❌ **Logs de Acesso** (Sabemos quem leu o arquivo?) -> _Falta fazer._
    

**Resultado:** A empresa cumpriu 3, mas o livro de regras exigia 8. Por isso a nota é **3/8**.

### 3. Outros "Livros de Regras" Comuns

Embora sua imagem pareça usar o NIST, existem outros padrões que as empresas usam para tirar esses requisitos:

- **ISO 27001:** Um padrão internacional super rígido. Se a empresa quer certificação, ela tem que cumprir exatamente o que está escrito na norma (são mais de 100 requisitos no total).
    
- **CIS Controls:** Uma lista mais direta, tipo "Top 18 coisas para não ser hackeado".
    
- **LGPD (Lei Geral de Proteção de Dados):** A lei brasileira também cria requisitos legais que viram itens nessa lista.
    

### Resumo

O número de baixo (**8**) vem de uma **Norma Internacional** (provavelmente o NIST CSF no seu caso).

O número de cima (**3**) é a realidade da empresa após uma **Auditoria**.

O auditor pega a norma, vai na empresa e marca um "X" no que existe e no que falta. O que falta vira a "lacuna" (o _gap_) que precisa ser preenchida.
