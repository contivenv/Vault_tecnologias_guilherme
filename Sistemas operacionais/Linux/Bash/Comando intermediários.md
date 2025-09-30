---
tags:
  - terminal
  - bash
  - linux
  - unix
  - comandos
  - diretórios
  - arquivos
  - livros
---
## grep

O comando grep consegue fazer a exibição de linhas de arquivos, como por exemplo que é usado no comando de: 

```bash
grep root /etc/passwd 
root:x:0:0:Super User:/root:/bin/bash
operator:x:11:0:operator:/root:/usr/sbin/nologin

```

Faz a exibição de nome além da linha correspondente do arquivo também. Assim como em outros comandos que já fizemos a listagem, ele tem variações importantes no seu uso, como por exemplo o uso da letra -i com ele, que ignora maiúsculas e minúsculas, procurando somente o nome, sem sensitive case. Temos também a consulta de grep com comando -v no final que seria ao contrario da busca original. Com -v seria as não correspondências a aquele certo tipo de arquivo. O comando grep é um comanda exencial para consulta de arquivos no geral. Eu vou referenciar o trecho do livro que fala somente dele e assim como os demais que vou citar aqui. Esse precisam de uma atenção em especial.

> ## 2.5.1 grep
> O comando grep exibe as linhas de um arquivo ou de um stream de entrada que correspondam a uma expressão. Por exemplo, para exibir as linhas do arquivo /etc/passwd que contenham o texto root, digite:
> $ grep root /etc/passwd
> O comando grep é incrivelmente prático para trabalhar com vários arquivos de uma só vez, pois ele exibe o nome do arquivo além da linha correspondente. Por exemplo, se quiser verificar todos os arquivos em /etc que contenham a palavra root, este comando poderá ser usado:
```bash
grep root /etc/*
```
> Duas das opções mais importantes de grep são `-i` (para correspondências que não levem em conta a diferença entre letras maiúsculas e minúsculas) e `-v` (que inverte a pesquisa, ou seja, exibe todas as linhas em que não há correspondência). Há também uma variante mais eficaz chamada egrep (que é apenas um sinônimo para `grep -E`).
> O grep entende padrões conhecidos como expressões regulares, enraizados na teoria da ciência da computação, e são muito comuns nos utilitários Unix. As expressões regulares são mais eficazes que os padrões com caracteres-curinga e têm uma sintaxe diferente. Há dois aspectos importantes que devem ser lembrados em relação às expressões regulares:
> • .* corresponde a qualquer quantidade de caracteres (como * nos caracteres-curinga).
> • . corresponde a um caractere qualquer.
>  Observação: A página de manual grep(1) contém uma descrição detalhada das expressões regulares, porém pode ser um pouco difícil lê-la. Para aprender mais, você pode consultar o livro Dominando expressões regulares, 3ª edição (Alta Books, 2009), ou pode consultar o capítulo sobre expressões regulares do livro Programming Perl, 4ª edição (O'Reilly, 2012). Se você gosta de matemática e estiver interessado na origem das expressões regulares, dê uma olhada no livro Introduction to Automata Theory, Languages, and Computation, 3ª edição (Prentice Hall, 2006).
> ’
> —Brian Ward, “Como o Linux funciona”

## Less

Basicamente pode servir para exibição de conteúdo em tela cheia quando se é muito extenso. Podendo listar o conteúdo de uma vez só. Podemos usar ele como um leitor de arquivo literal, onde os comandos da tecla barra avaçam no arquivo, q sai e b retorna para a tela cheia da listage.  