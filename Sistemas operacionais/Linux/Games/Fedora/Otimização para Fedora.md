---
tags:
  - fedora
  - linux
  - games
  - validado
---
Para melhorar o desempenho no Fedora com sua **GTX 1650** e **Ryzen 5 5600G**, você pode atualizar os drivers e otimizar o sistema. Siga os passos abaixo:

---

### **1. Atualizar o sistema e os pacotes básicos**  
Execute no terminal:
```bash
sudo dnf upgrade --refresh -y
sudo dnf autoremove -y
```

---

### **2. Instalar/Atualizar os drivers da NVIDIA**  
Se você já usa os drivers proprietários da NVIDIA, verifique a versão mais recente:

#### **Opção A: Usando RPM Fusion (recomendado)**
```bash
sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda -y
sudo akmods --force
sudo dracut --force
```
Reinicie após a instalação:
```bash
reboot
```

#### **Opção B: Verificar a versão do driver instalado**
```bash
nvidia-smi
```
Se estiver desatualizado, remova o driver antigo e reinstale:
```bash
sudo dnf remove *nvidia* -y
sudo dnf install akmod-nvidia -y
sudo reboot
```

---

### **3. Drivers para o Ryzen 5 5600G (GPU integrada)**  
O kernel Linux já inclui drivers para AMD, mas atualize o firmware e o Mesa:
```bash
sudo dnf install mesa-vulkan-drivers mesa-dri-drivers -y
sudo dnf update --refresh
```

---

### **4. Otimizar o Proton e Steam**  
Verifique se você está usando a versão mais recente do Proton:
- No Steam, vá para **Biblioteca** → **Death Stranding** → **Configurações** → **Compatibilidade**.
- Selecione **Proton Experimental** ou a versão mais recente (ex: Proton 9.0).

#### **Forçar pré-compilação de shaders (reduz stuttering)**  
No Steam:
1. **Configurações** → **Shader Pre-Caching** → Ative a opção.

#### **Variáveis úteis para jogos (adicione no launch options do jogo)**  
Clique em **Propriedades** → **Definir opções de inicialização** e adicione:
```bash
PROTON_ENABLE_NVAPI=1 RADV_PERFTEST=gpl DXVK_ASYNC=1 %command%
```

---

### **5. Verificar configurações do sistema**
#### **Desativar compositor (melhora FPS)**
Se estiver usando GNOME (Wayland/X11), desative a composição durante jogos:
```bash
sudo dnf install gamemode -y
```
Adicione no launch options do jogo no Steam:
```bash
gamemoderun %command%
```

#### **Definir performance máxima para a GPU**
```bash
nvidia-settings --assign "[gpu:0]/GPUPowerMizerMode=1"
```

---

### **6. Pós-instalação**
Reinicie o sistema e verifique se tudo está correto:
```bash
sudo reboot
```
Após reiniciar, confirme os drivers:
```bash
glxinfo | grep "OpenGL renderer"  # Para AMD
nvidia-smi                       # Para NVIDIA
```

---

### **Possíveis causas dos FPS drops**
- **Proton desatualizado**: Use Proton Experimental ou GE (GloriousEggroll).
- **Shader compilation**: Ative o pre-caching no Steam.
- **Thermal throttling**: Verifique temperaturas com `sensors` (instale via `sudo dnf install lm_sensors -y`).
- **Configurações gráficas altas**: Reduza configurações como sombras e reflexos.

Se o problema persistir, considere testar o **Proton-GE** (versão customizada com patches extras):
```bash
sudo dnf install protonup-qt -y
protonup
```
(Selecione a versão mais recente do Proton-GE).

Espero que isso resolva seus problemas de FPS! Se precisar de mais ajuda, me avise. 🚀

---

Entendido! Como você está usando **apenas a NVIDIA GTX 1650 (dedicada)** e não a GPU integrada do Ryzen 5 5600G, vamos focar em otimizar o driver da NVIDIA, o Proton e o sistema para melhorar o desempenho no *Death Stranding*.  

---

### **Passo a Passo Otimizado para NVIDIA GTX 1650 no Fedora**  

#### **1. Verifique o driver NVIDIA atual**  
Você já confirmou que está usando o driver **575.64** (versão estável mais recente).  
Para garantir que não há conflitos:  
```bash
sudo dnf remove *nvidia*  # Remove todos os pacotes NVIDIA (caso precise reinstalar)
sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda
sudo akmods --force
sudo dracut --force
sudo reboot
```

---

#### **2. Force o uso da GPU dedicada (se necessário)**  
Como você não usa a GPU integrada, o sistema já deve priorizar a NVIDIA. Para confirmar:  
```bash
sudo prime-select nvidia  # Garante que a NVIDIA seja a padrão
glxinfo | grep "OpenGL renderer"  # Deve mostrar "NVIDIA GeForce GTX 1650"
```

---

#### **3. Otimize o Proton para o Death Stranding**  
- **Use Proton Experimental ou Proton-GE** (recomendado):  
  ```bash
  sudo dnf install protonup-qt -y
  protonup  # Instale a versão mais recente do Proton-GE (ex: GE-Proton9-10)
  ```
- **Launch Options no Steam**:  
  Adicione estas variáveis para melhorar desempenho:  
  ```bash
  PROTON_ENABLE_NVAPI=1 DXVK_ASYNC=1 gamemoderun %command%
  ```
  - `PROTON_ENABLE_NVAPI=1`: Ativa tecnologias específicas da NVIDIA.  
  - `DXVK_ASYNC=1`: Reduz stuttering durante compilação de shaders.  
  - `gamemoderun`: Prioriza recursos do sistema para o jogo.  

---

#### **4. Instale o Gamemode e otimize o sistema**  
```bash
sudo dnf install gamemode -y
```
- **Configure o Gamemode para máxima performance**:  
  Edite o arquivo de configuração:  
  ```bash
  sudo nano /etc/gamemode.ini
  ```
  Adicione/altere as linhas:  
 ```ini
  [gpu]
  ; Força performance máxima da NVIDIA
  nvidia_performance_mode=1
  [custom]
  ; Desativa o compositor do GNOME durante jogos
  start="killall -STOP gnome-shell"
  end="killall -CONT gnome-shell"
  ```
#### **5. Configure o Xorg (melhor que Wayland para jogos)**  
Se estiver usando **Wayland**, troque para **Xorg**:  
1. Deslogue do Fedora.  
2. No menu de login, clique na engrenagem e selecione **"Fedora on Xorg"**.  
3. Rode o jogo novamente e teste.  

---

#### **6. Verifique temperaturas e throttling**  
Monitore a GPU durante o jogo:  
```bash
watch -n 1 nvidia-smi  # Atualiza a cada 1 segundo
```
Se a temperatura estiver acima de **80°C**, pode haver throttling. Soluções:  
- Limpe poeira do hardware.  
- Ajuste a curva do cooler com:  
  ```bash
  nvidia-settings  # Vá em "Thermal Settings" e aumente a velocidade da ventoinha.
  ```

---

#### **7. Reduza carga de background**  
Pelo `nvidia-smi`, o `gnome-shell` e `firefox` estão usando GPU. Feche tudo antes de jogar:  
```bash
killall firefox gnome-shell  # Fecha apps (eles reiniciam automaticamente)
```

---

### **Resumo das Melhorias Esperadas**  
- **Proton-GE**: Melhor compatibilidade com jogos DX12/Vulkan.  
- **Gamemode**: CPU/GPU priorizados para o jogo.  
- **Xorg**: Menos input lag e stuttering vs. Wayland.  
- **NVIDIA Performance Mode**: Clock estável da GPU.  

Se ainda houver quedas de FPS, compartilhe:  
1. Logs do jogo (use `PROTON_LOG=1 %command%` nos *Launch Options*).  
2. Saída de `nvidia-smi` durante os FPS drops.  

Espero que resolva! Se precisar de ajustes, é só chamar. 🚀