---
tags:
  - games
  - linux
  - ubuntu
  - validado
---
Ótimo! Vamos otimizar seu sistema Ubuntu para obter o máximo desempenho da sua **GTX 1650** e **Ryzen 5 5600G** (mesmo sem usar o vídeo integrado).  

---

### **1. Instalar os Drivers NVIDIA Oficiais (recomendado para jogos)**
A versão proprietária da NVIDIA geralmente oferece melhor desempenho em jogos:

```bash
sudo add-apt-repository ppa:graphics-drivers/ppa -y
sudo apt update
sudo ubuntu-drivers autoinstall
sudo reboot
```

Verifique se o driver está ativo após reiniciar:
```bash
nvidia-smi
```
(Se aparecer informações da sua GPU, está funcionando.)

---

### **2. Habilitar o Modo de Performance da GPU**
Para garantir que a NVIDIA esteja no modo de máximo desempenho:

```bash
sudo nvidia-settings
```
- Vá para **"PowerMizer"** e selecione **"Performance Mode"**.
- Ou via terminal:
  ```bash
  sudo nvidia-smi -pm 1  # Persistência máxima
  sudo nvidia-smi -pl 90  # Ajuste de potência (90% pode evitar throttling)
  ```

---

### **3. Instalar e Configurar o Gamemode (Otimizações para Jogos)**
O `gamemode` ajusta prioridades do sistema durante jogos:

```bash
sudo apt install gamemode libgamemode0 libgamemodeauto0
```
- Para ativar em um jogo via Steam:
  - **Propriedades do jogo** → **Definir opções de inicialização**:  
    ```bash
    gamemoderun %command%
    ```

---

### **4. Kernel Low-Latency (Opcional, para menos stutter)**
Se você usa produção de áudio ou quer menos atraso:
```bash
sudo apt install linux-lowlatency
```
(Reinicie e selecione o kernel low-latency no GRUB.)

---

### **5. Configurar o CPU Governor para "Performance"**
Isso mantém o Ryzen 5 5600G na frequência máxima:

```bash
sudo apt install cpufrequtils
echo 'GOVERNOR="performance"' | sudo tee /etc/default/cpufrequtils
sudo systemctl restart cpufrequtils
```
Verifique com:
```bash
cpufreq-info
```

---

### **6. Otimizar o Filesystem (noatime, etc.)**
Edite `/etc/fstab`:
```bash
sudo nano /etc/fstab
```
Adicione `noatime,commit=60` às suas partições (ex.: ext4). Exemplo:
```
UUID=123... / ext4 noatime,commit=60,errors=remount-ro 0 1
```
Salve (`Ctrl+O`, `Enter`, `Ctrl+X`) e reinicie.

---

### **7. Desativar Compositor (se usar GNOME/KDE)**
- **GNOME**: Desative no `gnome-tweaks` → **"Windows"** → **"Disable Compositing"**.
- **KDE**: `System Settings` → **Display and Monitor** → **Compositor** → **"Allow applications to block compositing"**.

---

### **8. Verificar Vsync e Configurações da NVIDIA**
Execute:
```bash
nvidia-settings
```
- Em **"X Server Display Configuration"**, verifique se o **"Force Full Composition Pipeline"** está **OFF** (pode causar input lag).
- Em **"OpenGL Settings"**, desative **"Allow Flipping"** e ative **"Performance"**.

---

### **9. Usar Wine/Proton para Jogos Windows (Steam Play)**
Na Steam:
- **Steam → Settings → Steam Play**: Ative **"Enable Steam Play for all titles"** e selecione **Proton Experimental** (ou a versão mais recente).

---

### **10. Monitorar Desempenho**
Instale o `mangohud` para overlay de FPS:
```bash
sudo apt install mangohud
```
Para ativar em um jogo:
```bash
mangohud %command%  # Na opção de lançamento do jogo na Steam
```

---

### **Resultado Esperado**
- **FPS mais altos** (especialmente com `gamemode` + drivers NVIDIA).
- **Menos stutters** (devido ao governor "performance" e otimizações).
- **Melhor responsividade** (compositor desativado + configurações NVIDIA).

Se ainda tiver problemas, verifique se algum jogo específico precisa de ajustes adicionais (ex.: `PROTON_USE_WINED3D=1` para alguns títulos antigos). 🚀