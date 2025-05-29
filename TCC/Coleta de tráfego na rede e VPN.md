---
tags:
  - firewall
  - pfsense
  - redes
  - tcc
  - suricata
hora criação: 2025-02-28T11:13:00
---
Eu estava com dúvida de como eu poderia configurar minha máquina Linux Mint para coletar todos o tráfego da rede e depois analisar a coleta de dados do Suricata. Só que antes de ontem antes de dormir, fiquei pensando em como eu faria pra um desktop com SO em Linux Mint monitor todo o tráfego da rede apenas com um cabo ethernet conectado nele. A resposta foi simples: não daria. Eu tinha me esquecido dessa parte. Então passei minhas ideias pro ChatGPT, mas nenhuma me agradou tanto. Foi aí que tive um estalo de perceber que era só colocar a máquina entre o fluxo da empresa e os funcionários com regras de limitações, otimização e tudo desligado no firewall PfSense, somente rodando o Suricata para colher os dados do fluxo da rede. Passei essa ideia pro ChatGPT e ele me passou essas informações:

##### Respostas do ChatGPT a minha ideias
Sim, essa abordagem funciona muito bem! Instalando o **PfSense** logo no início, mas sem ativar regras ou otimizações, você já coloca a máquina no papel de gateway da rede. Dessa forma, **todo o tráfego** dos funcionários passará por ele, e o **Suricata** poderá capturar os pacotes diretamente no firewall.

Vantagens dessa abordagem:  
✅ **Captura total do tráfego**: Como o PfSense estará roteando a rede, todos os pacotes passarão por ele, facilitando a coleta de dados.  
✅ **Menos configuração**: Você não precisa se preocupar com port mirroring, TAPs de rede ou outros métodos.  
✅ **Mesma infraestrutura para comparação**: Como o hardware e o software permanecerão os mesmos, a única diferença entre os dois momentos será a ativação do firewall e a otimização do tráfego, garantindo uma comparação mais fiel.

##### Como fazer

1. **Instale o PfSense** na máquina.
2. **Configure a rede** de forma que o PfSense seja o gateway da empresa.
3. **Instale o Suricata no PfSense** via **Services > Suricata**.
4. **Ative o Suricata em modo IDS (sem bloquear pacotes)** para apenas coletar logs.
5. **Monitore e colete dados** do tráfego da rede antes de ativar regras de firewall e otimizações.
6. **Após um período de coleta**, aplique as regras e configurações de otimização no PfSense e compare os resultados.

Essa solução garante que você tenha **dados reais da rede** antes e depois do firewall entrar em ação, permitindo avaliar **se o PfSense realmente protege e otimiza o fluxo da rede**. 🔥🚀

Tendo em vista essa ideia, conversei com o Chat sobre acessar o firewall PfSense de uma forma remota, porque afinal de contas, eu não iria estar 100% do tempo na empresa vendo se está funcionando ou não a coleta de dados do fluxo de rede com o Suricata. Então comentei sobre a ideia de configurar uma VPN para acessar remotamente. A resposta que ele meu deu sobre essa ideia foi a seguinte:

##### Respostas do ChatGPT a minha ideias
Ótima ideia! Vou te passar um passo a passo simples para configurar a VPN **OpenVPN** no **PfSense** e acessar remotamente.
### 🔹 **1. Instalar o pacote OpenVPN Client Export**

Antes de começar, instale o pacote que facilita a exportação dos arquivos de configuração do cliente:

1. Vá em **System > Package Manager > Available Packages**.
2. Busque por **openvpn-client-export** e instale.

---

### 🔹 **2. Configurar o Servidor OpenVPN no PfSense**

Agora vamos configurar o servidor VPN:

1️⃣ **Acesse o PfSense** e vá em **VPN > OpenVPN**.  
2️⃣ Vá até a aba **Servers** e clique em **Add** para adicionar um novo servidor.  
3️⃣ Configure os seguintes parâmetros principais:

- **Server Mode** → **Remote Access (User Auth + Local Database)**
- **Protocol** → **UDP** (mais rápido) ou **TCP** (se houver bloqueios na rede)
- **Device Mode** → **tun**
- **Interface** → **WAN**
- **Local Port** → **1194** (ou outro caso a porta esteja em uso)
- **Cryptographic Settings**:
    - **TLS Authentication** → **Enable**
    - **Peer Certificate Authority** → Selecione "Create New" e crie uma CA
    - **Server Certificate** → Crie um certificado novo
- **IPv4 Tunnel Network** → **10.8.0.0/24** (pode usar essa faixa de IPs)
- **Redirect Gateway** → **Marque essa opção** se quiser que todo o tráfego do cliente passe pela VPN
- **DNS Server** → Adicione o IP do PfSense (**ex: 192.168.1.1**)

4️⃣ Clique em **Save & Apply**.

---

### 🔹 **3. Criar um Usuário para Acessar a VPN**

1️⃣ Vá em **System > User Manager** e clique em **Add**.  
2️⃣ Adicione um usuário com senha e, em **Certificate**, marque **Click to create a user certificate**.  
3️⃣ Salve.

---

### 🔹 **4. Criar as Regras de Firewall para Permitir Conexões**

Agora precisamos garantir que a VPN possa ser acessada externamente.

1️⃣ Vá em **Firewall > Rules > WAN** e clique em **Add** para criar uma regra:

- **Protocol** → **UDP**
- **Destination Port** → **1194** (ou a porta que você escolheu)
- **Allow**

2️⃣ Vá em **Firewall > Rules > OpenVPN** e adicione uma regra:

- **Action** → **Pass**
- **Protocol** → **Any**
- **Source** → **Network: 10.8.0.0/24**

3️⃣ Salve e aplique as regras.

---

### 🔹 **5. Exportar o Arquivo de Configuração para Conectar da sua Casa**

Agora você precisa baixar o arquivo de configuração do cliente para usar em sua máquina em casa.

1️⃣ Vá em **VPN > OpenVPN > Client Export**.  
2️⃣ Na seção **Client Export**, encontre o usuário que você criou.  
3️⃣ Clique em **"Most Compatible - .ovpn"** para baixar o arquivo de configuração.

---

### 🔹 **6. Configurar a VPN no Cliente (Sua Máquina em Casa)**

Agora vamos configurar a conexão na sua máquina:

🔸 **No Windows**

1. Baixe e instale o **OpenVPN Client** ([OpenVPN GUI](https://openvpn.net/client-connect-vpn-for-windows/)).
2. Importe o arquivo `.ovpn` e conecte-se.

🔸 **No Linux**

1. Instale o OpenVPN:
    
    ```bash
    sudo apt install openvpn
    ```
    
2. Conecte-se usando o arquivo exportado:
    
    ```bash
    sudo openvpn --config caminho_do_arquivo.ovpn
    ```
    

🔸 **No Android/iOS**

1. Instale o **OpenVPN Connect** na Play Store/App Store.
2. Importe o arquivo `.ovpn` e conecte-se.

---

### 🔹 **7. Testando o Acesso Remoto**

1️⃣ Conecte-se à VPN da sua casa.  
2️⃣ Tente acessar a interface web do PfSense pelo navegador:

```
https://10.8.0.1 (ou o IP da rede interna do PfSense, ex: 192.168.1.1)
```

3️⃣ Se conseguir acessar, sua VPN está funcionando!

---

### ✅ **Resumo Final**

✔ **O PfSense é o servidor da VPN**  
✔ **Você pode se conectar de casa com segurança**  
✔ **Todos os dados da empresa estarão protegidos**  
✔ **O Suricata pode ser monitorado remotamente**

Com isso, sempre que precisar, basta se conectar via VPN e acessar o PfSense remotamente! 🔥🚀