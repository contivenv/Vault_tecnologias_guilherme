---
tags:
  - terminal
  - bash
  - linux
  - unix
  - comandos
  - diretórios
  - arquivos
---
Apresentação de comandos básicos para relembrar conceitos de navegação em diretórios dentro do Linux.
## cat

Exibição de conteúdo de arquivos.

```bash
guilhermect@fedora:~/Downloads$ cat teste_02.txt 
Guilherme Conti Teixeira
```
## cd

Navegação simples entre diretórios, exemplo:

```bash
guilhermect@fedora:~$ cd Documentos/
guilhermect@fedora:~/Documentos$
```

## ls

Exibição de arquivos dentro de um diretório. Tem variações entre `ls -l` que mostra as permissões dos arquivos de forma estendida. Outro é `ls -f` exibe informações sobre o tipo dos arquivos. Exemplos:

```bash
guilhermect@fedora:~$ ls -l
total 0
drwxr-xr-x. 1 guilhermect guilhermect 362 ago 25 23:59 'Anotações - Obsidian'
drwxr-xr-x. 1 guilhermect guilhermect   0 ago 25 17:11 'Área de trabalho'
drwxr-xr-x. 1 guilhermect guilhermect   0 ago 25 17:11  Documentos
drwxr-xr-x. 1 guilhermect guilhermect 390 ago 28 20:49  Downloads
drwxr-xr-x. 1 guilhermect guilhermect  32 ago 27 20:21  Imagens
drwxr-xr-x. 1 guilhermect guilhermect  58 ago 26 00:01  Livros
drwxr-xr-x. 1 guilhermect guilhermect   0 ago 25 17:11  Modelos
drwxr-xr-x. 1 guilhermect guilhermect   0 ago 25 17:11  Músicas
drwxr-xr-x. 1 guilhermect guilhermect   0 ago 25 17:11  Público
drwxr-xr-x. 1 guilhermect guilhermect  40 ago 26 19:58  Senhas
drwxr-xr-x. 1 guilhermect guilhermect 876 ago 28 20:57 'Tecnologia - Obsidian'
drwxr-xr-x. 1 guilhermect guilhermect   0 ago 25 17:11  Vídeos
```


```bash
guilhermect@fedora:~$ ls -f

 .              .bash_profile   .cache              Público      Vídeos    .dotnet                  Livros
 ..             .bashrc        'Área de trabalho'   Documentos   .var      Senhas                   .bash_history
 .mozilla       .config         Downloads           Músicas      .vscode  'Anotações - Obsidian'    .gitconfig
 .bash_logout   .local          Modelos             Imagens      .pki     'Tecnologia - Obsidian'   .ssh
```

## cp

Copia arquivos que você escolhe, por exemplo: cp *arquivo1* e *arquivos2*

## mv

Pode ser usado tanto para renomear quando para mover arquivos para um diretório. Mudaremos o nome do arquivo `teste_01.txt` para `teste_02.txt`.

```bash
guilhermect@fedora:~/Downloads$ ls
arquivo_teste.txt  dilica-mickey-meme.gif  featured_image_pfsense.png             pfsense-logo-fundo.png
artefatos_tcc      ec2-fho-key.pem         GSTI_Atividade_03.pdf                  syncthing-linux-amd64-v2.0.3
Atividade.pdf      EC2.pdf                 kali-linux-2025.2-installer-amd64.iso  teste_01.txt
guilhermect@fedora:~/Downloads$ mv teste_01.txt teste_02.txt
guilhermect@fedora:~/Downloads$ ls
arquivo_teste.txt  dilica-mickey-meme.gif  featured_image_pfsense.png             pfsense-logo-fundo.png
artefatos_tcc      ec2-fho-key.pem         GSTI_Atividade_03.pdf                  syncthing-linux-amd64-v2.0.3
Atividade.pdf      EC2.pdf                 kali-linux-2025.2-installer-amd64.iso  teste_02.txt
guilhermect@fedora:~/Downloads$
```

## touch

criação de arquivos

```bash
guilhermect@fedora:~/Downloads$ touch arquivo_teste.txt
guilhermect@fedora:~/Downloads$ ls
arquivo_teste.txt  dilica-mickey-meme.gif  featured_image_pfsense.png             syncthing-linux-amd64-v2.0.3
artefatos_tcc      ec2-fho-key.pem         kali-linux-2025.2-installer-amd64.iso  teste_01.txt
Atividade.pdf      EC2.pdf                 pfsense-logo-fundo.png
```

rm - apaga arquivos (remove), pode ser usado com complementos também, por exemplo: rm -r para apagar todos os arquivos dentro de um diretório. Tome muito cuidado com esse comando.

```bash
guilhermect@fedora:~/Downloads$ rm -r TESTE
```

globbing no shell - podemos usar o globbing do shell de formas muito interessantes. Uma delas seria usar para mostrar o tipo de conteúdo exibindo dentro de um diretório ou até mesmo, palavras ou letras que terminar com certos tipos de caracteres. Iremos aos exemplos:

echo \*vos no final e echo vos\* para poder achar itens que terminem com letras vos no final ou no começo de qualquer palavra com quaisquer outros caracteres. Exemplo de uso:

```bash
guilhermect@fedora:~/Downloads$ echo *txt
arquivo_teste.txt teste_01.txt
```



