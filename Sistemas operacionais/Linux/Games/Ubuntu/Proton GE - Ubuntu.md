---
tags:
  - linux
  - ubuntu
  - games
situação: validado
---
### Método 1: A Maneira Fácil com ProtonUp-Qt (Recomendado)

Esta continua sendo a forma mais prática e segura. Vamos usar o Flatpak, assim como fizemos no Lubuntu.

#### Passo 1: Preparar o Ubuntu para o Flathub

Mesmo que o Ubuntu tenha suporte a Flatpak, é uma boa prática garantir que o repositório principal, o Flathub, esteja configurado.

1. **Abra o Terminal**. Pressione a tecla `Super` (a tecla com o símbolo do Windows), digite "Terminal" e clique no ícone que aparecer.
    
2. **Atualize sua lista de pacotes:**
    
    
    
    ```bash
    sudo apt update
    ```
    
3. **Instale o Flatpak**, caso ele não esteja presente:
    
    
    
    ```bash
    sudo apt install flatpak -y
    ```
    
4. **Adicione o repositório Flathub** (a principal "loja" de apps Flatpak):
    
    
    
    ```bash
    flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
    ```
    
5. **Reinicie o computador**. Isso garante que o Ubuntu Software e o sistema integrem completamente o Flatpak, fazendo com que os ícones dos aplicativos apareçam corretamente.
    

#### Passo 2: Instalar e Usar o ProtonUp-Qt

1. Após reiniciar, abra o Terminal e instale o ProtonUp-Qt com o comando:
    
    
    
    ```bash
    flatpak install flathub net.davidotek.pupgui2
    ```
    
2. **Execute o ProtonUp-Qt**. Ele deve aparecer no seu menu de aplicativos. Se não aparecer, você pode usar o comando que já conhece: `flatpak run net.davidotek.pupgui2`.
    
3. Dentro do programa, clique em **"Add version"**.
    
4. Selecione **"GE-Proton"** e a versão mais recente disponível (ex: "GE-Proton10-X").
    
5. Clique em **"Install"**.
    

---

### Método 2: Instalação Manual

Se preferir o controle total, o método manual também funciona perfeitamente no Ubuntu.

#### Passo 1: Baixar o GE-Proton

1. Acesse a página oficial de lançamentos: **[GloriousEggroll/proton-ge-custom/releases](https://github.com/GloriousEggroll/proton-ge-custom/releases)**.
    
2. Baixe o arquivo que termina em `.tar.gz` da versão mais recente.
    

#### Passo 2: Criar o Diretório de Compatibilidade

1. Abra o Terminal. Você precisa criar a pasta onde a Steam busca por essas ferramentas. O caminho depende de como você instalou a Steam.
    
    - **Se você instalou a Steam via `apt` ou pela loja "Ubuntu Software" (versão .deb):**
        
        
        
        ```bash
        mkdir -p ~/.steam/root/compatibilitytools.d/
        ```
        
    - **Se você instalou a Steam como um Flatpak:**
        
        
        
        ```bash
        mkdir -p ~/.var/app/com.valvesoftware.Steam/data/Steam/compatibilitytools.d/
        ```
        

#### Passo 3: Extrair o GE-Proton

1. No Terminal, navegue até a sua pasta de `Downloads`:
    
    Bash
    
    ```bash
    cd Downloads
    ```
    
2. Execute o comando de extração apontando para a pasta correta que você criou:
    
    - **Para a Steam nativa (.deb):**
        
        
        
        ```bash
        tar -xf GE-Proton*.tar.gz -C ~/.steam/root/compatibilitytools.d/
        ```
        
    - **Para a Steam em Flatpak:**
        
        
        
        ```bash
        tar -xf GE-Proton*.tar.gz -C ~/.var/app/com.valvesoftware.Steam/data/Steam/compatibilitytools.d/
        ```
        

---

### Último Passo (Para os dois métodos): Configurar a Steam

Este passo final é idêntico em qualquer sistema:

1. **Reinicie a Steam completamente.** Saia do aplicativo e abra-o novamente.
    
2. Para ativar o GE-Proton, você pode:
    
    - **Para um jogo específico:** Clicar com o botão direito no jogo > **Propriedades** > **Compatibilidade**, marcar a caixa de seleção e escolher a versão do GE-Proton na lista.
        
    - **Para todos os jogos:** Ir em **Steam > Configurações > Steam Play**, habilitar a opção para todos os títulos e selecionar o GE-Proton como padrão global.