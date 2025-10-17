---
tags:
  - fedora
  - games
  - proton
  - otimização
  - validado
---
## GE-Proton 10 no Fedora: Guia Completo de Instalação e Configuração para a Steam

Para jogadores no Fedora que buscam maximizar a compatibilidade e o desempenho de jogos Windows na Steam, a instalação do GE-Proton é um passo essencial. Esta versão customizada do Proton, mantida pela comunidade, frequentemente inclui melhorias e correções que ainda não chegaram à versão oficial da Valve.

Aqui está um guia detalhado de como instalar e configurar o GE-Proton 10 (ou a versão mais recente disponível) no seu sistema Fedora, garantindo que ele apareça corretamente na sua biblioteca Steam.

-----

### Método 1: A Maneira Fácil com ProtonUp-Qt (Recomendado)

A forma mais simples e recomendada de gerenciar o GE-Proton é utilizando o ProtonUp-Qt. Este aplicativo gráfico automatiza o download, a extração e a instalação de ferramentas de compatibilidade como o GE-Proton, diretamente no local correto que a Steam espera.

#### Passo 1: Instalar o ProtonUp-Qt via Flatpak

O ProtonUp-Qt está disponível no Flathub, o que torna sua instalação no Fedora extremamente simples, já que o suporte a Flatpak vem pré-configurado na maioria das instalações.

1.  **Abra o Terminal** no seu Fedora.
2.  **Execute o seguinte comando** para instalar o ProtonUp-Qt:
    ```bash
    flatpak install flathub net.davidotek.pupgui2
    ```
3.  Confirme a instalação pressionando `Y` (ou `S`) quando solicitado.

#### Passo 2: Baixar e Instalar o GE-Proton

1.  Após a instalação, **abra o ProtonUp-Qt** a partir do seu menu de aplicativos.
2.  Na janela principal do programa, clique no botão **"Add version"**.
3.  No campo "Compatibility tool", certifique-se de que **"GE-Proton"** está selecionado.
4.  No campo "Version", selecione a versão mais recente disponível (por exemplo, "GE-Proton10-X").
5.  Clique em **"Install"**. O aplicativo irá baixar e instalar a versão selecionada automaticamente.

#### Passo 3: Configurar a Steam para Usar o GE-Proton

1.  **Reinicie a Steam completamente.** É crucial fechar e abrir a Steam novamente para que ela detecte a nova ferramenta de compatibilidade.
2.  Agora você tem duas opções:
      * **Para um jogo específico:**
          * Clique com o botão direito no jogo em sua biblioteca e vá em **Propriedades**.
          * Na aba **Compatibilidade**, marque a caixa "Forçar o uso de uma ferramenta de compatibilidade específica do Steam Play".
          * Na lista, selecione a versão do GE-Proton que você acabou de instalar.
      * **Para todos os jogos (Global):**
          * No canto superior esquerdo da Steam, clique em **Steam \> Configurações**.
          * Vá para a seção **Steam Play**.
          * Marque a opção "Ativar o Steam Play para todos os outros títulos".
          * No menu suspenso abaixo, selecione a sua nova versão do GE-Proton.
          * Clique em OK e reinicie a Steam quando solicitado.

-----

### Método 2: Instalação Manual

Se preferir não usar aplicativos de terceiros, a instalação manual também é um processo direto.

#### Passo 1: Baixar o GE-Proton

1.  Acesse a página oficial de lançamentos do GE-Proton no GitHub: **[GloriousEggroll/proton-ge-custom](https://github.com/GloriousEggroll/proton-ge-custom/releases)**.
2.  Encontre a versão mais recente (procure pela tag "Latest").
3.  Baixe o arquivo que termina em `.tar.gz`. Por exemplo: `GE-Proton10-1.tar.gz`.

#### Passo 2: Criar o Diretório de Compatibilidade

1.  A Steam procura por ferramentas de compatibilidade customizadas em um diretório específico. Você precisa criá-lo se ele não existir. Abra o Terminal.
2.  **Verifique a sua versão da Steam:**
      * **Para a versão RPM (instalada via DNF/Loja de Software):**
        ```bash
        mkdir -p ~/.steam/root/compatibilitytools.d/
        ```
        *Nota: Em algumas configurações, o caminho pode ser `~/.steam/steam/compatibilitytools.d/`.*
      * **Para a versão Flatpak (instalada via Flathub):**
        ```bash
        mkdir -p ~/.var/app/com.valvesoftware.Steam/data/Steam/compatibilitytools.d/
        ```

#### Passo 3: Extrair o GE-Proton

1.  Navegue até o local onde você baixou o arquivo `.tar.gz` (geralmente a pasta `Downloads`).

2.  Use o comando `tar` para extrair o arquivo diretamente para o diretório que você criou no passo anterior.

      * **Para a versão RPM da Steam:**
        ```bash
        tar -xf GE-Proton*.tar.gz -C ~/.steam/root/compatibilitytools.d/
        ```
      * **Para a versão Flatpak da Steam:**
        ```bash
        tar -xf GE-Proton*.tar.gz -C ~/.var/app/com.valvesoftware.Steam/data/Steam/compatibilitytools.d/
        ```

#### Passo 4: Reiniciar e Configurar a Steam

1.  **Feche e reabra a Steam.** Esta etapa é fundamental para que a nova versão seja detectada.
2.  Siga as mesmas instruções do **"Passo 3: Configurar a Steam para Usar o GE-Proton"** do método anterior para ativar o GE-Proton globalmente ou para jogos específicos. A versão que você instalou manualmente agora aparecerá na lista de ferramentas de compatibilidade disponíveis.

Com o GE-Proton instalado, você estará pronto para desfrutar de uma experiência de jogo aprimorada no seu Fedora, com maior compatibilidade e acesso às mais recentes tecnologias de jogos para Windows no ambiente Linux.