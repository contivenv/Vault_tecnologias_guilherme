---
tags:
  - validado
  - Guilherme
hora criação: 2026-03-12T10:51:00
---
Para instalar sua impressora Brother **DCP-8112DN** no Linux Mint via terminal, você deve utilizar o script instalador oficial da Brother que você forneceu, o que baixou pelo site oficial da Brother e já descompactou. O Linux Mint é baseado em Ubuntu/Debian, portanto, o script gerenciará os pacotes `.deb` necessários automaticamente.

Siga os passos abaixo:

### 1. Preparar o Script

Primeiro, garanta que o arquivo do script (`linux-brprinter-installer-2.2.6-0`) tenha permissão de execução. No terminal, dentro da pasta onde o arquivo está:

Bash

```
chmod +x linux-brprinter-installer-2.2.6-0
```

### 2. Executar a Instalação

Execute o script como superusuário (root), passando o modelo da sua impressora como argumento:

Bash

```
sudo ./linux-brprinter-installer-2.2.6-0 DCP-8112DN
```

### 3. Responder às Perguntas do Assistente

Durante a execução, o script fará uma série de perguntas. Para o seu caso específico de rede, siga estas orientações baseadas no código do script:

1. **Confirmação de Pacotes:** O script listará os pacotes a serem baixados (LPR e cupswrapper). Digite **y** (Sim) para continuar.
    
2. **Contratos de Licença:** Você precisará aceitar os termos da Brother e da licença GPL. Digite **y** para concordar quando solicitado.
    
3. **Configuração de URI (Rede):** Quando o script perguntar "Will you specify the Device URI? [Y/n]", digite **Y**.
    
4. **Seleção do IP:** O terminal exibirá uma lista de opções de conexão. Procure a opção que diz **"Specify IP address"** (geralmente é o número imediatamente antes do "Auto") e digite esse número.
    
5. **Inserir o IP:** Quando solicitado "enter IP address ->", digite o IP da sua impressora:
    
    > **192.168.2.21**
    
6. **Teste de Impressão:** Ao final, ele perguntará "Test Print? [y/N]". Recomendamos digitar **y** para validar a configuração.
    

---

### Observações Importantes

- **Dependências de 32 bits:** O script detectará que seu sistema é 64 bits e tentará instalar bibliotecas de compatibilidade (como `lib32stdc++6` ou `ia32-libs`), pois muitos drivers da Brother são de 32 bits.
    
- **Desinstalador:** O script criará automaticamente um arquivo chamado `uninstaller_DCP8112DN` na pasta atual. Guarde-o caso precise remover o driver no futuro.

![[página de teste.jpeg]]