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

O comando grep consegue fazer a exibição de linhas de arquivos, como por exemplo, o que é usado no comando:

```bash
grep root /etc/passwd # entrada do comando
root:x:0:0:Super User:/root:/bin/bash # saída
operator:x:11:0:operator:/root:/usr/sbin/nologin # saída
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

Nessa parte do comando grep eu tive dificuldade para entender como o autor do livro descreve o que ele faz. Fui pedir uma explicação mais simples sobre o que era o comando no Gemini. Me foi passado por ele e pesquisado em outros lugares que, o comando seria uma forma de marcador de texto, onde ele encontra o que está escrito dentro de uma arquivo do diretório que você está procurando. O comando grep seria o "fofoqueiro" que abre suas cartas e lê elas, enquanto o comando find e locate seira somente o carteiro que entrega os arquivos. Mais detalhes em [[Me explique o que o comando grep no Linux faz e suas variáveis, por exemplo -E. Eu estou lendo um livro sobre Linux e não estou entendendo o que o autor que dizer, mas pela explicação parece ser algo complexo e bem importante.]]
## less

Basicamente pode servir para exibição de conteúdo em tela cheia quando se é muito extenso. Podendo listar o conteúdo de uma vez só. Podemos usar ele como um leitor de arquivo literal, onde os comandos da tecla barra avançam no arquivo, (`q`) sai e (`b`) retorna para a tela cheia da listagem.

## pwd

Serve para localizar o diretório e o caminho que vocês está. Por mais que pense que esse comando pode ser bobo, ele não é. Na prática quando não se tem a exibição de diretórios ou de onde você está dentro do sistema, o `pwd` é um comando que pode te dar um norte. Na outra forma de ele ser útil é quando são usados links simbólicos, onde o conteúdo original do caminho é escondido. Nesse caso, ele vai te ajudar a se localizar.

## diff

Para comparação de dois arquivos, você pode usar:
```bash
diff arquivo1 arquivo2 # entrada do comando
```
Executando esse comando, vai conseguir ver o comparativo exato dos arquivos. Sua variação de comando pode ser usado como `diff -u` para visualizar os dados de uma forma mais detalhada.

## file

Faz com que o sistema adivinhe o conteúdo do arquivo. Esse comando pode te ajudar em muitas ocasiões onde você nem imagina (de verdade mesmo, experimente quando possível).

```bash
file Guilherme-Conti.png # entrada do comando
Guilherme-Conti.png: PNG image data, 600 x 174, 8-bit/color RGB, non-interlaced # saída do comando
```
## find

Serve para que você possa localizar um arquivo em um diretório que você sabe que está, mas não está achando. Podemos usar ele de diversas maneiras com parâmetros limitando para achar somente diretórios, somente arquivos, ignorar arquivos case sensitive, entre outros. Para o comando básico utilize:

```bash
find Documentos/ -name "Senhas.kdbx" -print # entranda do comando
Documentos/Senhas.kdbx # saida do comando
```

> ‘O comando find aceita caracteres especiais para correspondência de padrões, como \*, porém você deve colocá-los entre aspas simples ('\*') para proteger os caracteres especiais do recurso de globbing do próprio shell.’
> —Brian Ward, “Como o Linux funciona”


