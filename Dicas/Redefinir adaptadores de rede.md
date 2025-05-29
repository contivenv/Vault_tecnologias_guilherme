---
tags:
  - redes
  - adaptador_de_rede
  - cmd
---
### Método 1: Usar o **Prompt de Comando**

1. **Abra o Prompt de Comando como Administrador**:
    
    - Pressione `Windows + S`, digite **cmd** e escolha a opção **Executar como administrador**.
2. Execute os seguintes comandos um por um:
    
    - Para liberar o endereço IP atual:
        
        ```bash
        ipconfig /release
        ```
        
    - Para renovar o endereço IP:
        
        ```bash
        ipconfig /renew
        ```
        
    - Para limpar o cache DNS:
        
        ```bash
        ipconfig /flushdns
        ```
        
    - Para redefinir o adaptador TCP/IP:
        
        ```bash
        netsh int ip reset
        ```
        
    - Para redefinir as configurações do Winsock:
        
        ```bash
        netsh winsock reset
        ```
        
3. **Reinicie o computador** para aplicar as alterações.
    

---

### Método 2: Redefinir pela interface do Windows

1. **Abra as Configurações**:
    - Pressione `Windows + I` para abrir o menu de Configurações.
2. Vá para **Rede e Internet**.
3. No menu lateral, escolha a opção **Status**.
4. Role para baixo até encontrar a opção **Redefinição de Rede** e clique nela.
5. Confirme a operação clicando em **Redefinir Agora**.
6. O computador será reiniciado, e todas as configurações de rede serão restauradas para os padrões de fábrica.