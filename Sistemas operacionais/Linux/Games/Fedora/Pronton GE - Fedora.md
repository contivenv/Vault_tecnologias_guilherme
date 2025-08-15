---
tags:
  - fedora
  - validado
  - linux
---
### **Método 1: Instalar o ProtonUp-Qt via COPR (Recomendado)**

O COPR é um repositório comunitário que pode ter versões mais atualizadas.  

1. **Adicione o repositório COPR**:  
   ```bash
   sudo dnf copr enable -y nickavem/protonup
   ```  

2. **Instale o ProtonUp-Qt**:  
   ```bash
   sudo dnf install protonup-qt -y
   ```  

3. **Execute o ProtonUp-Qt**:  
   ```bash
   protonup
   ```  
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
   ```bash
   tar -xf GE-Proton*.tar.gz -C ~/.steam/root/compatibilitytools.d/
   ```  

4. **Reinicie o Steam**:  
   - Feche e abra o Steam novamente.  
   - Vá para:  
     - **Biblioteca** → *Death Stranding* → ⚙️ **Configurações** → **Compatibilidade**  
     - Selecione a versão do Proton-GE instalada.  

---

### **Método 3: Usar o Flatpak (Alternativa)**  
Se os métodos acima falharem, instale o **ProtonUp-Qt via Flatpak**:  

1. **Instale o Flatpak (se ainda não tiver)**:  
```bash
sudo dnf install flatpak -y
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```  

2. **Instale o ProtonUp-Qt**:  
```bash
flatpak install flathub net.davidotek.pupgui2
```  

3. **Execute**:  
```bash
flatpak run net.davidotek.pupgui2
```  

---

### **Solução para Erros Comuns**  
- **"Pasta compatibilitytools.d não existe"**:  
  Crie manualmente:  
```bash
mkdir -p ~/.steam/root/compatibilitytools.d/
```  

- **Steam não reconhece o Proton-GE**:  
  Reinicie o Steam via terminal:  
```bash
steam & disown
```  

---

### **Por que o Proton-GE é Melhor?**  
- **Patchs extras**: Melhor desempenho em jogos como *Death Stranding*.  
- **Suporte a codecs**: Vídeos introdutórios funcionam sem falhas.  
- **DXVK/VKD3D atualizados**: Menos stuttering e melhor compatibilidade.