---
tags:
  - linux
  - putty
  - redes
  - VirtualBox
  - cmd
  - arquivos
---

Para transferir um arquivo do seu PC com Windows para um servidor Ubuntu via PuTTY, você pode usar **PSCP** (PuTTY Secure Copy Protocol), que é uma ferramenta de linha de comando incluída com o PuTTY. Aqui está um passo a passo para realizar essa transferência:

### Passo a Passo:

1. **Baixar o PSCP (se necessário):**
    
    - Se você ainda não tiver o PSCP, baixe-o do site oficial do PuTTY: [Download PSCP](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).
    - Coloque o arquivo `pscp.exe` em um diretório fácil de acessar, como `C:\\\\Windows\\\\` ou crie uma pasta dedicada, por exemplo, `C:\\\\pscp\\\\`.
2. **Abrir o Prompt de Comando (CMD):**
    
    - Pressione `Win + R`, digite `cmd`, e pressione Enter para abrir o Prompt de Comando.
3. **Usar o PSCP para Transferir Arquivos:**
    
    - O comando básico para transferir arquivos é o seguinte:
        
        ```bash
        pscp C:\\\\caminho\\\\para\\\\seu\\\\arquivo usuario@IP_do_servidor:/caminho/no/servidor
        
        ```
        
        - **Exemplo**: Se você quiser transferir um arquivo chamado `meuarquivo.txt` da pasta `C:\\\\Users\\\\Nome\\\\Desktop` no Windows para a pasta `/home/usuario/destino` no servidor Ubuntu, o comando será:
            
            ```bash
            pscp C:\\\\Users\\\\Nome\\\\Desktop\\\\meuarquivo.txt usuario@192.168.1.10:/home/usuario/destino/
            
            ```
            
4. **Autenticação:**
    
    - Após rodar o comando, o PSCP pedirá a senha do usuário no servidor. Digite a senha corretamente e a transferência será feita.
5. **Verificando a Transferência:**
    
    - Para garantir que o arquivo foi transferido corretamente, conecte-se ao servidor via PuTTY e navegue até o diretório de destino com `cd` e use `ls` para listar os arquivos.

### Notas:

- Certifique-se de substituir `usuario`, `IP_do_servidor` e `/caminho/no/servidor` com as informações corretas do seu ambiente.
- O PSCP utiliza o mesmo protocolo SSH que o PuTTY, então a transferência será criptografada.

Se você preferir uma interface gráfica, pode considerar o uso do **WinSCP**, que facilita a transferência de arquivos entre máquinas Windows e servidores Linux via SFTP ou SCP.

No Ubuntu, você pode escolher diferentes diretórios para armazenar seus arquivos, dependendo de sua finalidade. Aqui estão alguns caminhos comuns que você pode usar para transferir arquivos via SCP:

1. **/home/usuario/**
    - **Descrição:** Diretório pessoal do usuário. Este é o local mais comum para armazenar arquivos que pertencem a um usuário específico. Cada usuário do sistema tem sua própria pasta dentro de `/home`.
    - **Exemplo:** `/home/seuusuario/`
2. **/var/www/**
    - **Descrição:** Usado principalmente para arquivos relacionados a servidores web. Se você estiver trabalhando com servidores web (como Apache ou Nginx), é comum colocar arquivos de sites ou aplicações web aqui.
    - **Exemplo:** `/var/www/html/`
3. **/opt/**
    - **Descrição:** Usado para softwares de terceiros ou aplicações personalizadas. Você pode colocar arquivos relacionados a scripts, binários, ou pacotes instalados manualmente.
    - **Exemplo:** `/opt/meuapp/`
4. **/usr/local/**
    - **Descrição:** Similar ao `/opt`, mas mais tradicional para programas instalados localmente, especialmente scripts ou ferramentas que você mesmo está desenvolvendo.
    - **Exemplo:** `/usr/local/meuprograma/`
5. **/tmp/**
    - **Descrição:** Diretório temporário para armazenamento temporário de arquivos. Esses arquivos podem ser excluídos automaticamente pelo sistema após algum tempo, então use apenas para testes rápidos.
    - **Exemplo:** `/tmp/`

### Sugestão Comum:

- Se o arquivo for algo pessoal ou que você vai trabalhar diretamente, o ideal seria colocar no seu diretório pessoal, por exemplo:

```bash
/home/seuusuario/
```