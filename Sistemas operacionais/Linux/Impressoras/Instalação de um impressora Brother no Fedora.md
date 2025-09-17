---
tags:
  - impressoras
  - linux
  - fedora
---
Beleza, vamos direto ao ponto: no Fedora você precisa basicamente de três coisas para essa Brother DCP-8112DN funcionar na rede: **CUPS ativo, drivers corretos e configuração do IP da impressora**.

### 1. Certificar que o CUPS está instalado e rodando

```bash
sudo dnf install -y cups system-config-printer
sudo systemctl enable --now cups
```

Teste se o serviço está no ar:

```bash
systemctl status cups
```

E abra o painel web do CUPS para ver se aparece a impressora:  
[http://localhost:631](http://localhost:631)

---

### 2. Instalar os drivers da Brother

A Brother fornece pacotes RPM para Fedora/CentOS. Faça assim:

```bash
sudo dnf install -y wget
wget https://download.brother.com/welcome/dlf101741/dcp8112dnlpr-3.0.1-1.i386.rpm
wget https://download.brother.com/welcome/dlf101742/dcp8112dncupswrapper-3.0.1-1.i386.rpm

sudo dnf install -y dcp8112dnlpr-3.0.1-1.i386.rpm dcp8112dncupswrapper-3.0.1-1.i386.rpm
```

> ⚠️ Os pacotes são 32-bit, então pode ser que você precise habilitar suporte a libs i686 no Fedora:

```bash
sudo dnf install -y glibc.i686
```

---

### 3. Adicionar a impressora pelo IP

1. Descubra o IP da sua Brother (pelo painel dela ou pelo seu roteador).
    
2. No Fedora, vá em **Configurações → Impressoras → Adicionar**.
    
3. Escolha **Impressora de Rede → IPP ou LPD**.
    
    - Para IPP, use:  
        `ipp://<IP_DA_IMPRESSORA>/ipp`
        
    - Para LPD, use:  
        `lpd://<IP_DA_IMPRESSORA>/binary_p1`
        
4. Selecione o driver **Brother DCP-8112DN** que você acabou de instalar.
    

---

### 4. Testar impressão

Rode:

```bash
lpstat -p -d
```

E para testar:

```bash
echo "Teste de Impressão Fedora" | lp -d Brother_DCP8112DN
```

---

👉 Se mesmo com o driver não funcionar, você pode usar o **driver genérico PCL** (a 8112DN suporta PCL6) que já vem no Fedora:

- Na hora de escolher driver, selecione **Generic → PCL 6/PCL XL Printer**.
    

---

Quer que eu já monte um scriptzinho `.sh` que automatize a instalação do driver da Brother + configuração inicial no Fedora, assim você só edita o IP da impressora?


Boa, isso já ajuda a diagnosticar. O `cupsd` está **rodando normal**, mas o erro que aparece é este:

```
Unable to connect to IMP-RS.local:443: Temporary failure in name resolution
```

Ou seja:

- O Fedora está tentando se conectar à impressora usando **mDNS/Bonjour** (`.local`) e não consegue resolver o nome.
    
- Isso é comum porque o suporte a mDNS vem do `avahi-daemon`. Se ele não estiver rodando, o Fedora não vai resolver nomes `.local`.
    

---

### 1. Confirme se o Avahi está ativo

```bash
systemctl status avahi-daemon
```

Se não estiver ativo, habilite:

```bash
sudo dnf install -y avahi
sudo systemctl enable --now avahi-daemon
```

---

### 2. Teste o ping por nome e por IP

- Tente:
    
    ```bash
    ping IMP-RS.local
    ```
    
    Se não resolver, use o IP direto da impressora.
    
- Exemplo de configuração manual no CUPS:
    
    ```bash
    lpadmin -p Brother_DCP_8112DN -E -v lpd://192.168.1.50/binary_p1 -m everywhere
    ```
    

---

### 3. Verifique se o driver já está disponível

Liste os PPDs que o CUPS tem:

```bash
lpinfo -m | grep -i brother
```

Se não aparecer nada da DCP-8112DN, você realmente precisa instalar o driver Brother (aqueles `.rpm` que mencionei antes) ou então usar o **Generic PCL 6**.

---

### 4. Recomendo neste caso

- **Não usar `.local`** → configure a impressora pelo **IP fixo** (muito mais confiável).
    
- Se a rede distribui IPs via DHCP, vá no painel da impressora e configure um **IP fixo ou reserva DHCP**.
    
- Depois, adicione no Fedora assim:
    
    ```bash
    lpadmin -p Brother_DCP_8112DN -E -v ipp://192.168.1.50/ipp -m everywhere
    ```
    
    (troque `192.168.1.50` pelo IP real).
    

---

👉 Quer que eu já te monte o comando completo `lpadmin` pronto para você só trocar o IP da impressora, evitando a configuração gráfica?