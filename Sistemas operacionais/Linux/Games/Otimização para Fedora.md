---
tags:
  - fedora
  - linux
  - games
---
Eu jogo no Fedora usando o proton versão 9-0-4 e percebo que meu jogo está tendo quedas de FPS que antes não tinha. Estou jogando death stranding. Gostaria de atualizar todos os meus drivers da placa de vídeo gtx 1650 e processador ryzen 5 5600 g, me passe os comandos para executar no terminal

Para melhorar o desempenho no Fedora com sua **GTX 1650** e **Ryzen 5 5600G**, você pode atualizar os drivers e otimizar o sistema. Siga os passos abaixo:

---

### **1. Atualizar o sistema e os pacotes básicos**

Execute no terminal:

bash

sudo dnf upgrade --refresh -y
sudo dnf autoremove -y

---

### **2. Instalar/Atualizar os drivers da NVIDIA**

Se você já usa os drivers proprietários da NVIDIA, verifique a versão mais recente:

#### **Opção A: Usando RPM Fusion (recomendado)**

bash

sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda -y
sudo akmods --force
sudo dracut --force

Reinicie após a instalação:

bash

reboot

#### **Opção B: Verificar a versão do driver instalado**

bash

nvidia-smi

Se estiver desatualizado, remova o driver antigo e reinstale:

bash

sudo dnf remove *nvidia* -y
sudo dnf install akmod-nvidia -y
sudo reboot

---

### **3. Drivers para o Ryzen 5 5600G (GPU integrada)**

O kernel Linux já inclui drivers para AMD, mas atualize o firmware e o Mesa:

bash

sudo dnf install mesa-vulkan-drivers mesa-dri-drivers -y
sudo dnf update --refresh

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

bash

PROTON_ENABLE_NVAPI=1 RADV_PERFTEST=gpl DXVK_ASYNC=1 %command%

---

### **5. Verificar configurações do sistema**

#### **Desativar compositor (melhora FPS)**

Se estiver usando GNOME (Wayland/X11), desative a composição durante jogos:

bash

sudo dnf install gamemode -y

Adicione no launch options do jogo no Steam:

bash

gamemoderun %command%

#### **Definir performance máxima para a GPU**

bash

nvidia-settings --assign "[gpu:0]/GPUPowerMizerMode=1"

---

### **6. Pós-instalação**

Reinicie o sistema e verifique se tudo está correto:

bash

sudo reboot

Após reiniciar, confirme os drivers:

bash

glxinfo | grep "OpenGL renderer"  # Para AMD
nvidia-smi                       # Para NVIDIA

---

### **Possíveis causas dos FPS drops**

- **Proton desatualizado**: Use Proton Experimental ou GE (GloriousEggroll).
    
- **Shader compilation**: Ative o pre-caching no Steam.
    
- **Thermal throttling**: Verifique temperaturas com `sensors` (instale via `sudo dnf install lm_sensors -y`).
    
- **Configurações gráficas altas**: Reduza configurações como sombras e reflexos.
    

Se o problema persistir, considere testar o **Proton-GE** (versão customizada com patches extras):

bash

sudo dnf install protonup-qt -y
protonup

(Selecione a versão mais recente do Proton-GE).

Espero que isso resolva seus problemas de FPS! Se precisar de mais ajuda, me avise. 🚀

image.png

PNG 71.52KB

Essas são as versões que estão na minha máquina

Pela saída do `nvidia-smi`, seu sistema está usando:

- **Driver NVIDIA: 575.64** (versão estável mais recente, lançada em junho de 2025).
    
- **CUDA: 12.9** (compatível com o driver).
    

---

### **Diagnóstico e Soluções para Quedas de FPS**

#### 1. **Driver NVIDIA já está atualizado**

- Não há necessidade de atualizar o driver, pois você já está na versão mais recente (`575.64`).
    
- Se suspeitar de problemas com o driver, teste versões alternativas:
    
    bash
    

- sudo dnf remove akmod-nvidia -y
    sudo dnf install akmod-nvidia-570xx -y  # Versão anterior (se necessário)
    sudo reboot
    

#### 2. **Verifique o Proton e o Vulkan**

- O problema pode estar relacionado ao **Proton 9.0-4** ou à configuração do Vulkan.
    
- Instale/atualize os pacotes Vulkan e DXVK:
    
    bash
    

- sudo dnf install vulkan-loader mesa-vulkan-drivers dxvk -y
    

#### 3. **Use Proton Experimental ou Proton-GE**

- **Proton Experimental** tem melhorias frequentes para jogos como _Death Stranding_:
    
    - No Steam, vá para _Death Stranding_ → _Configurações_ → _Compatibilidade_ → Selecione **Proton Experimental**.
        
- **Proton-GE** (recomendado para jogos com problemas):
    
    bash
    

- sudo dnf install protonup-qt -y
    protonup
    
    Selecione a versão mais recente do Proton-GE (ex: `GE-Proton9-10`).
    

#### 4. **Otimizações para o Jogo**

- Adicione estas variáveis de ambiente nos _Launch Options_ do jogo no Steam:
    
    bash
    

- PROTON_ENABLE_NVAPI=1 RADV_PERFTEST=gpl DXVK_ASYNC=1 gamemoderun %command%
    
- **Gamemode** (prioriza recursos para o jogo):
    
    bash
    

- sudo dnf install gamemode -y
    

#### 5. **Monitoramento de Desempenho**

- Verifique se há **thermal throttling** (superaquecimento) com:
    
    bash
    

- sensors  # Instale com: sudo dnf install lm_sensors -y
    
- Confere o uso da GPU/CPU durante o jogo:
    
    bash
    

- nvidia-smi -l 3  # Monitora a GPU a cada 3 segundos
    htop  # Monitora a CPU (instale com: sudo dnf install htop -y)
    

#### 6. **Configurações do GNOME/Wayland**

- Se estiver usando **Wayland**, tente forçar o jogo a rodar no **Xorg**:
    
    - Deslogue, selecione "Fedora on Xorg" no login, e teste novamente.
        
- Desative o compositor do GNOME durante jogos:
    
    bash
    

- gsettings set org.gnome.mutter overlay-key ''
    

---

### **Possíveis Causas dos FPS Drops**

- **Shader compilation**: Ative _Shader Pre-Caching_ no Steam.
    
- **Proton desatualizado**: Proton 9.0-4 pode ter regressões para seu jogo.
    
- **Conflito com Wayland**: Jogos pesados podem ter melhor desempenho no Xorg.
    
- **Background processes**: O `gnome-shell` e `firefox` estão usando GPU (veja no `nvidia-smi`). Feche apps desnecessários.
    

---

### **Próximos Passos**

1. Teste com **Proton-GE** e as variáveis sugeridas.
    
2. Monitore temperaturas e uso de recursos durante o jogo.
    
3. Se o problema persistir, compartilhe logs do jogo (usando `PROTON_LOG=1 %command%` nos _Launch Options_).
    

Se precisar de ajuda adicional, me avise! 🎮

Não uso a GPU integrada do meu Ryzen, tenho uma placa de vídeo dedicada. Refaça o tutorial acima.

Entendido! Como você está usando **apenas a NVIDIA GTX 1650 (dedicada)** e não a GPU integrada do Ryzen 5 5600G, vamos focar em otimizar o driver da NVIDIA, o Proton e o sistema para melhorar o desempenho no _Death Stranding_.

---

### **Passo a Passo Otimizado para NVIDIA GTX 1650 no Fedora**

#### **1. Verifique o driver NVIDIA atual**

Você já confirmou que está usando o driver **575.64** (versão estável mais recente).  
Para garantir que não há conflitos:

bash

sudo dnf remove *nvidia*  # Remove todos os pacotes NVIDIA (caso precise reinstalar)
sudo dnf install akmod-nvidia xorg-x11-drv-nvidia-cuda
sudo akmods --force
sudo dracut --force
sudo reboot

---

#### **2. Force o uso da GPU dedicada (se necessário)**

Como você não usa a GPU integrada, o sistema já deve priorizar a NVIDIA. Para confirmar:

bash

sudo prime-select nvidia  # Garante que a NVIDIA seja a padrão
glxinfo | grep "OpenGL renderer"  # Deve mostrar "NVIDIA GeForce GTX 1650"

---

#### **3. Otimize o Proton para o Death Stranding**

- **Use Proton Experimental ou Proton-GE** (recomendado):
    
    bash
    

- sudo dnf install protonup-qt -y
    protonup  # Instale a versão mais recente do Proton-GE (ex: GE-Proton9-10)
    
- **Launch Options no Steam**:  
    Adicione estas variáveis para melhorar desempenho:
    
    bash
    

- PROTON_ENABLE_NVAPI=1 DXVK_ASYNC=1 gamemoderun %command%
    
    - `PROTON_ENABLE_NVAPI=1`: Ativa tecnologias específicas da NVIDIA.
        
    - `DXVK_ASYNC=1`: Reduz stuttering durante compilação de shaders.
        
    - `gamemoderun`: Prioriza recursos do sistema para o jogo.
        

---

#### **4. Instale o Gamemode e otimize o sistema**

bash

sudo dnf install gamemode -y

- **Configure o Gamemode para máxima performance**:  
    Edite o arquivo de configuração:
    
    bash
    

sudo nano /etc/gamemode.ini

Adicione/altere as linhas:

ini

- [gpu]
    ; Força performance máxima da NVIDIA
    nvidia_performance_mode=1
    [custom]
    ; Desativa o compositor do GNOME durante jogos
    start="killall -STOP gnome-shell"
    end="killall -CONT gnome-shell"
    

---

#### **5. Configure o Xorg (melhor que Wayland para jogos)**

Se estiver usando **Wayland**, troque para **Xorg**:

1. Deslogue do Fedora.
    
2. No menu de login, clique na engrenagem e selecione **"Fedora on Xorg"**.
    
3. Rode o jogo novamente e teste.
    

---

#### **6. Verifique temperaturas e throttling**

Monitore a GPU durante o jogo:

bash

watch -n 1 nvidia-smi  # Atualiza a cada 1 segundo

Se a temperatura estiver acima de **80°C**, pode haver throttling. Soluções:

- Limpe poeira do hardware.
    
- Ajuste a curva do cooler com:
    
    bash
    

- nvidia-settings  # Vá em "Thermal Settings" e aumente a velocidade da ventoinha.
    

---

#### **7. Reduza carga de background**

Pelo `nvidia-smi`, o `gnome-shell` e `firefox` estão usando GPU. Feche tudo antes de jogar:

bash

killall firefox gnome-shell  # Fecha apps (eles reiniciam automaticamente)

---

### **Resumo das Melhorias Esperadas**

- **Proton-GE**: Melhor compatibilidade com jogos DX12/Vulkan.
    
- **Gamemode**: CPU/GPU priorizados para o jogo.
    
- **Xorg**: Menos input lag e stuttering vs. Wayland.
    
- **NVIDIA Performance Mode**: Clock estável da GPU.
    

Se ainda houver quedas de FPS, compartilhe:

1. Logs do jogo (use `PROTON_LOG=1 %command%` nos _Launch Options_).
    
2. Saída de `nvidia-smi` durante os FPS drops.
    

Espero que resolva! Se precisar de ajustes, é só chamar. 🚀

image.png

PNG 43.24KB

De todas essas versões, qual é a que tem mais desempenho e estabilidade na maioria dos jogos na Steam ?

Para a maioria dos jogos na Steam (incluindo _Death Stranding_), a melhor versão do Proton em termos de **desempenho e estabilidade** depende do jogo e do seu hardware, mas aqui está uma análise das opções disponíveis na sua lista:

---

### **Melhores Versões do Proton para NVIDIA GTX 1650 (Fedora)**

#### 🚀 **1. Proton Experimental**

- **Recomendação principal** para _Death Stranding_ e jogos recentes.
    
- **Vantagens**:
    
    - Recebe atualizações frequentes com otimizações para Vulkan/DX12.
        
    - Suporte a tecnologias como **FSR 3.0** e **NVAPI** (útil para NVIDIA).
        
- **Desvantagem**: Pode ser menos estável em alguns jogos (por ser "beta").
    

#### 🔧 **2. Proton 10.0-1 (beta)**

- Versão mais nova que o Proton 9.0, mas ainda em testes.
    
- Ideal se _Death Stranding_ tiver problemas no Proton 9.0-4.
    

#### ⚡ **3. Proton 9.0-4**

- **Versão estável mais recente** (fora do Experimental).
    
- Boa para jogos como _Death Stranding_, mas pode ter regressões em alguns casos.
    

#### 🛠️ **4. Proton-GE (Não listado, mas altamente recomendado)**

- **Não aparece na sua lista**, mas é a melhor opção para desempenho e compatibilidade.
    
- Como instalar:
    
    bash
    

- sudo dnf install protonup-qt -y
    protonup
    
    - Escolha a versão mais recente (ex: `GE-Proton9-10`).
        
- **Por que usar?**
    
    - Traz patches extras para jogos single-player pesados (como _Death Stranding_).
        
    - Suporte a codecs de mídia e tweaks de performance.
        

---

### **Ranking das Versões (Melhor → Pior)**

1. **Proton-GE** (se instalado via `protonup`) → Melhor desempenho e compatibilidade.
    
2. **Proton Experimental** → Melhor para jogos novos, mas pode ter bugs.
    
3. **Proton 10.0-1 (beta)** → Versão futura com possíveis melhorias.
    
4. **Proton 9.0-4** → Estável, mas pode não ter as últimas otimizações.
    
5. **Proton 8.0-5** → Para jogos mais antigos.
    

---

### **Dica para Testar**

- Experimente **Proton-GE** ou **Proton Experimental** primeiro.
    
- Se houver crashes, teste o **Proton 9.0-4**.
    
- Use estas variáveis nos _Launch Options_ do jogo para melhorar performance:
    
    bash
    

- PROTON_ENABLE_NVAPI=1 DXVK_ASYNC=1 gamemoderun %command%
    

---

### **Resumo**

- **Para NVIDIA GTX 1650 no Fedora**:
    
    - **Proton-GE** é o melhor (instale via `protonup`).
        
    - Se preferir versões oficiais, use **Proton Experimental** ou **10.0-1**.
        
    - Proton 9.0-4 é o "seguro" se as outras falharem.
        

Se o jogo ainda tiver problemas, compartilhe os logs (use `PROTON_LOG=1 %command%`).

image.png

PNG 29.8KB

Quando tentei instalar o Proton-GE, ele deu essa mensagem

Parece que o pacote `protonup-qt` não está disponível nos repositórios padrão do Fedora. Vamos resolver isso instalando o **ProtonUp-Qt** manualmente ou usando métodos alternativos para obter o **Proton-GE**.

---

### **Método 1: Instalar o ProtonUp-Qt via COPR (Recomendado)**

O COPR é um repositório comunitário que pode ter versões mais atualizadas.

1. **Adicione o repositório COPR**:
    
    bash
    

- sudo dnf copr enable -y nickavem/protonup
    
- **Instale o ProtonUp-Qt**:
    
    bash
    
- sudo dnf install protonup-qt -y
    
- **Execute o ProtonUp-Qt**:
    
    bash
    

1. protonup
    
    - Selecione a versão mais recente do **Proton-GE** (ex: `GE-Proton9-10`).
        

---

### **Método 2: Instalar o Proton-GE Manualmente**

Se o COPR não funcionar, faça o download direto:

1. **Acesse o repositório do Proton-GE**:
    
    - Abra no navegador: [https://github.com/GloriousEggroll/proton-ge-custom](https://github.com/GloriousEggroll/proton-ge-custom)
        
2. **Baixe a versão mais recente**:
    
    - Procure por `GE-ProtonX-X` (ex: `GE-Proton9-10`) na seção **Releases**.
        
    - Baixe o arquivo `.tar.gz`.
        
3. **Extraia e instale**:
    
    bash
    

4. tar -xf GE-Proton*.tar.gz -C ~/.steam/root/compatibilitytools.d/
    
5. **Reinicie o Steam**:
    
    - Feche e abra o Steam novamente.
        
    - Vá para:
        
        - **Biblioteca** → _Death Stranding_ → ⚙️ **Configurações** → **Compatibilidade**
            
        - Selecione a versão do Proton-GE instalada.
            

---

### **Método 3: Usar o Flatpak (Alternativa)**

Se os métodos acima falharem, instale o **ProtonUp-Qt via Flatpak**:

1. **Instale o Flatpak (se ainda não tiver)**:
    
    bash
    

- sudo dnf install flatpak -y
    flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
    
- **Instale o ProtonUp-Qt**:
    
    bash
    
- flatpak install flathub net.davidotek.pupgui2
    
- **Execute**:
    
    bash
    

1. flatpak run net.davidotek.pupgui2
    

---

### **Solução para Erros Comuns**

- **"Pasta compatibilitytools.d não existe"**:  
    Crie manualmente:
    
    bash
    

- mkdir -p ~/.steam/root/compatibilitytools.d/
    
- **Steam não reconhece o Proton-GE**:  
    Reinicie o Steam via terminal:
    
    bash
    

- steam & disown
    

---

### **Por que o Proton-GE é Melhor?**

- **Patchs extras**: Melhor desempenho em jogos como _Death Stranding_.
    
- **Suporte a codecs**: Vídeos introdutórios funcionam sem falhas.
    
- **DXVK/VKD3D atualizados**: Menos stuttering e melhor compatibilidade.
    

Se ainda tiver problemas, compartilhe o erro exato! 🛠️

image.png

PNG 93.07KB

Ainda está dando erro

Parece que o repositório COPR `nickavem/protonup` está com problemas (erro 404) ou não está disponível para sua versão do Fedora. Vamos resolver isso de **3 formas alternativas** para instalar o Proton-GE:

---

### **📌 Método 1: Baixar e Instalar o Proton-GE Manualmente**

#### **Passo 1: Baixe a versão mais recente do Proton-GE**

Acesse o [GitHub do GloriousEggroll](https://github.com/GloriousEggroll/proton-ge-custom/releases) e baixe o arquivo `.tar.gz` da versão mais recente (ex: `GE-Proton9-10`).

#### **Passo 2: Extraia o arquivo**

Abra o terminal e execute:

bash

tar -xf ~/Downloads/GE-Proton*.tar.gz -C ~/.steam/root/compatibilitytools.d/

> **Nota**: Se a pasta `compatibilitytools.d` não existir, crie-a:
> 
> bash

> mkdir -p ~/.steam/root/compatibilitytools.d/

#### **Passo 3: Reinicie o Steam**

Feche e abra o Steam. Vá em:

1. **Biblioteca** → Clique com o botão direito em _Death Stranding_ → **Propriedades**.
    
2. Em **Compatibilidade**, marque **"Forçar uso de uma ferramenta de compatibilidade"**.
    
3. Selecione a versão do Proton-GE instalada (ex: `GE-Proton9-10`).
    

---

### **📌 Método 2: Usar o Flatpak (ProtonUp-Qt)**

Se preferir uma interface gráfica:

#### **Passo 1: Instale o Flatpak (se ainda não tiver)**

bash

sudo dnf install flatpak -y
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

#### **Passo 2: Instale o ProtonUp-Qt**

bash

flatpak install flathub net.davidotek.pupgui2

#### **Passo 3: Execute e instale o Proton-GE**

bash

flatpak run net.davidotek.pupgui2

- Na interface, selecione **Steam** e clique em **"Install"** na versão mais recente do Proton-GE.
    

---

### **📌 Método 3: Usar o Repositório COPR Alternativo**

Se o repositório `nickavem/protonup` está offline, tente outro:

bash

sudo dnf copr enable -y atim/protonup
sudo dnf install protonup-qt -y
protonup

> **Observação**: Se falhar, pule para o Método 1 ou 2.

---

### **🔍 Verificação Pós-Instalação**

- Confirme se o Proton-GE apareceu no Steam:
    
    bash
    

- ls ~/.steam/root/compatibilitytools.d/
    
- Se não listar nada, reveja os passos de extração ou reinstale.
    

---

### **💡 Dica Final: Otimizações para NVIDIA**

Adicione estas variáveis nos _Launch Options_ do jogo no Steam:

bash

PROTON_ENABLE_NVAPI=1 DXVK_ASYNC=1 gamemoderun %command%

Isso ativa:

- Suporte a tecnologias NVIDIA (NVAPI).
    
- Compilação assíncrona de shaders (reduz stuttering).
    
- Modo desempenho via `gamemode`.
    

---

### **❌ Se Nada Funcionar...**

1. **Proton Experimental**: Use a versão "Experimental" direto no Steam (já disponível na sua lista).
    
2. **Logs de erro**: Execute o jogo com:
    
    bash
    

3. PROTON_LOG=1 %command%
    
    E compartilhe o arquivo `~/steam-<ID_DO_JOGO>.log` para análise.
    

---

Se precisar de ajuda com algum passo específico, é só perguntar! 🛠️🎮