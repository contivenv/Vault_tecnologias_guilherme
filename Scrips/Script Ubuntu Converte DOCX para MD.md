---
tags:
  - linux
  - ubuntu
  - docx
  - md
  - conversão
  - arquivos
  - obsidian
data criação: 2026-05-16T11:56:00
situação: validado
---
Para fazer uma conversão de forma limpa e automatizada no Ubuntu de **docx** para **md**, recomendo o uso da ferramenta que se chama **Pandoc**. Ele é excelente porque preserva títulos, listas, negritos e tabelas perfeitamente no formato que o Obsidian entende.

Eu escrevi um script em Bash que serve tanto para converter um **arquivo individual** quanto para varrer um **diretório inteiro** cheio de arquivos `.docx`.

---
## O Meu Script (`docx2md.sh`)

Neste script, **eu** configurei uma verificação automática para instalar o Pandoc caso você ainda não o tenha. Também adicionei o parâmetro `--wrap=none` para garantir que o texto não quebre rigidamente no meio dos parágrafos, o que mantém a leitura fluida quando você abrir a nota no Obsidian.

```Bash
#!/bin/bash

# Verifica se o pandoc está instalado, se não, instala automaticamente
if ! command -v pandoc &> /dev/null; then
    echo "[+] Pandoc não encontrado. Instalando..."
    sudo apt update && sudo apt install pandoc -y
fi

# Se nenhum argumento for passado, usa o diretório atual
ALVO="${1:-.}"

convert_file() {
    local file="$1"
    local filename=$(basename "$file" .docx)
    local dirname=$(dirname "$file")
    
    echo "-> Convertendo: $filename.docx"
    # --wrap=none evita que o pandoc quebre linhas rigidamente, mantendo o texto fluido para o Obsidian
    pandoc "$file" -t markdown -o "$dirname/$filename.md" --wrap=none
}

# Verifica se o alvo é um diretório ou um arquivo específico
if [ -d "$ALVO" ]; then
    echo "[*] Buscando arquivos .docx em: $ALVO"
    
    # Encontra arquivos .docx no diretório (sem entrar em subpastas)
    find "$ALVO" -maxdepth 1 -type f -name "*.docx" | while read -r docx_file; do
        convert_file "$docx_file"
    done
    echo "[+] Concluído! Todos os arquivos do diretório foram processados."

elif [ -f "$ALVO" ]; then
    if [[ "$ALVO" == *.docx ]]; then
        convert_file "$ALVO"
        echo "[+] Concluído!"
    else
        echo "[-] Erro: O arquivo fornecido não possui a extensão .docx"
        exit 1
    fi
else
    echo "[-] Erro: Caminho ou arquivo inválido."
    exit 1
fi
```

---

## Como eu sugiro que você configure e use

### 1. Criando o arquivo do script

Abra o seu terminal e crie o arquivo. Você pode salvá-lo em uma pasta de scripts própria ou direto no seu Vault do Obsidian:

```Bash
nano docx2md.sh
```

Cole o meu código acima, salve (`Ctrl+O`, `Enter`) e saia (`Ctrl+X`).

### 2. Dando permissão de execução

Não se esqueça de liberar a execução do arquivo com este comando:

```Bash
chmod +x docx2md.sh
```

### 3. Modos de uso que eu preparei

- **Para converter todos os `.docx` da sua pasta atual:**
    
    
    
    ```Bash
    ./docx2md.sh
    ```
    
- **Para apontar para uma pasta específica (como a sua pasta de Downloads):**
    
    
    
    ```Bash
    ./docx2md.sh /caminho/para/a/pasta
    ```
    
- **Para converter apenas um arquivo isolado:**
    
    
    
    ```Bash
    ./docx2md.sh /caminho/para/o/documento.docx
    ```
    

> **Minha dica de organização:** Eu configurei o script para gerar os arquivos `.md` exatamente na mesma pasta onde os originais estão. Assim que a conversão terminar, basta você mover os novos arquivos `.md` para dentro do seu Vault.