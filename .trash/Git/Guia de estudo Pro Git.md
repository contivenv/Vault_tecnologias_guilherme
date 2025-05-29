---
tags:
  - git
  - estudos
  - aprendizado
---

# Questionário

**Responda as seguintes perguntas em 2-3 frases cada:**

1. **Explique a diferença fundamental entre Git e sistemas de controle de versão centralizados.**
2. **Descreva o conceito de "staging area" (área de preparação) no Git e sua função no processo de commit.**
3. **O que são arquivos .gitignore e qual a sua utilidade em um repositório Git?**
4. **Diferencie os comandos `git diff` e `git status` em termos de informações fornecidas.**
5. **Explique o propósito do comando `git log` e cite duas opções úteis para personalizar sua saída.**
6. **Descreva a funcionalidade do comando `git remote` e como ele auxilia na interação com repositórios remotos.**
7. **Diferencie tags "leves" (lightweight) e "anotadas" (annotated) no Git.**
8. **Qual a utilidade do comando `git stash` e como ele pode ser aplicado em um fluxo de trabalho?**
9. **Explique o conceito de "submódulos" (submodules) no Git e cite um exemplo de sua aplicação.**
10. **Descreva a função dos filtros "smudge" e "clean" em atributos do Git e como eles podem ser utilizados.**

# Gabarito do Questionário

1. **Git** é um sistema de controle de versão distribuído, o que significa que cada usuário possui uma cópia completa do repositório, incluindo todo o histórico. Sistemas centralizados, por outro lado, dependem de um servidor central para armazenar o repositório, exigindo conexão constante.
2. A **"staging area"** é um espaço intermediário onde as alterações de arquivos são preparadas antes de serem efetivamente commitadas. Ela permite que o usuário selecione quais modificações específicas serão incluídas em um commit, em vez de commitar todas as alterações de uma vez.
3. Arquivos **.gitignore** listam padrões de nomes de arquivos e diretórios que devem ser ignorados pelo Git. Isso é útil para evitar que arquivos temporários, binários ou de configuração irrelevantes sejam rastreados pelo sistema de controle de versão.
4. **`git status`** mostra o estado atual dos arquivos no repositório, indicando quais foram modificados, adicionados ou removidos, e quais estão na staging area. **`git diff`** exibe as diferenças específicas entre o estado atual dos arquivos e o último commit.
5. **`git log`** exibe o histórico de commits do repositório, mostrando informações sobre cada commit, como autor, data, mensagem e hash SHA-1. Opções como `-oneline` (exibe commits em uma linha) e `-author` (filtra commits por autor) permitem personalizar a saída.
6. **`git remote`** gerencia conexões com repositórios remotos. Ele permite adicionar, listar, renomear e remover referências a repositórios remotos, além de configurar URLs para fetch e push.
7. **Tags "leves"** são meros ponteiros para commits específicos, enquanto **tags "anotadas"** são objetos Git que contêm metadados adicionais, como nome do tagger, data e mensagem.
8. **`git stash`** permite salvar temporariamente alterações não commitadas no repositório. Isso é útil quando se precisa mudar de ramo ou trabalhar em outra tarefa sem perder o progresso atual. As alterações podem ser reaplicadas posteriormente com `git stash apply`.
9. **Submódulos** permitem incluir outros repositórios Git dentro do repositório principal. Isso é útil para gerenciar dependências externas ou dividir um projeto grande em componentes menores. Um exemplo é a inclusão de uma biblioteca de código aberto como um submódulo.
10. **Filtros "smudge" e "clean"** em atributos do Git permitem executar scripts personalizados em arquivos durante o checkout ("smudge") e o stage ("clean"). Eles podem ser usados para formatar código automaticamente, converter finais de linha ou realizar outras tarefas de pré e pós-processamento.

# Perguntas para Dissertação

1. **Discuta as vantagens e desvantagens da utilização de um sistema de controle de versão distribuído como o Git em comparação com sistemas centralizados.**
2. **Explique detalhadamente o processo de criação e mesclagem de branches no Git, incluindo a resolução de conflitos de merge.**
3. **Descreva diferentes fluxos de trabalho de colaboração utilizando Git, como o fluxo de trabalho centralizado, o fluxo de trabalho de integração de gerente e o fluxo de trabalho de ditador e tenentes.**
4. **Apresente os principais comandos e conceitos relacionados ao gerenciamento de submódulos no Git, explorando as vantagens e os desafios de sua utilização.**
5. **Explique o processo de reescrita do histórico no Git, incluindo comandos como `git rebase` e `git filter-branch`, discutindo os benefícios e os riscos associados a essa prática.**

# Glossário

- **Blob**: Objeto fundamental no Git que armazena o conteúdo de um arquivo, sem informações de metadados.
- **Branch**: Ponteiro para um commit específico no histórico do Git, permitindo desenvolvimento isolado de funcionalidades ou correções de bugs.
- **Checkout**: Ação de atualizar o diretório de trabalho com o conteúdo de um commit ou branch específico.
- **Commit**: Objeto que registra um snapshot do estado dos arquivos em um determinado momento no histórico do Git.
- **Git**: Sistema de controle de versão distribuído que rastreia alterações em arquivos e permite colaboração entre desenvolvedores.
- **HEAD**: Referência especial no Git que aponta para o commit ou branch atualmente em uso.
- **Index (Staging Area)**: Área intermediária onde as alterações de arquivos são preparadas antes de serem commitadas.
- **Merge**: Ação de combinar as alterações de dois branches diferentes em um único branch.
- **Rebase**: Ação de mover uma série de commits para um novo commit base, reescrevendo o histórico do Git.
- **Remote**: Referência a um repositório Git em outro local, geralmente hospedado em um servidor.
- **Repository**: Diretório que contém o histórico do Git e os arquivos rastreados pelo sistema de controle de versão.
- **SHA-1**: Função hash criptográfica utilizada pelo Git para identificar unicamente objetos no repositório.
- **Stash**: Ação de salvar temporariamente alterações não commitadas no repositório.
- **Submodule**: Repositório Git aninhado dentro de outro repositório, permitindo gerenciar dependências externas.
- **Tag**: Referência nomeada para um commit específico no histórico do Git, geralmente utilizada para marcar versões de lançamento.
- **Tree**: Objeto no Git que representa a estrutura de diretórios e arquivos em um determinado commit.
- **Version Control System (VCS)**: Sistema que rastreia alterações em arquivos ao longo do tempo, permitindo reverter para versões anteriores, comparar mudanças e colaborar em projetos.