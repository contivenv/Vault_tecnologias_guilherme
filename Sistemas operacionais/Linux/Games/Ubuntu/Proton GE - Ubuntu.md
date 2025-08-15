---
tags:
  - linux
  - ubuntu
  - games
---
Para instalar e configurar o **GE-Proton** (GloriousEggroll's Proton) na sua Steam no Ubuntu (usando a versão .deb da Steam), siga estes passos:

---

### **1. Baixe o GE-Proton**
Você já tem o arquivo `GE-Proton10-12.tar.gz`. Caso não tenha, baixe-o do [GitHub do GloriousEggroll](https://github.com/GloriousEggroll/proton-ge-custom/releases).

---

### **2. Extraia o arquivo**
Abra um terminal (`Ctrl + Alt + T`) e execute:
```bash
tar -xzf GE-Proton10-12.tar.gz -C ~/.steam/root/compatibilitytools.d/
```
Isso extrairá o Proton para a pasta correta da Steam.

- **Se a pasta `compatibilitytools.d` não existir**, crie-a:
  ```bash
  mkdir -p ~/.steam/root/compatibilitytools.d/
  ```

---

### **3. Reinicie a Steam**
Feche completamente a Steam (incluindo o processo em segundo plano) e reinicie:
```bash
steam
```

---

### **4. Configure o GE-Proton no Steam**
1. Abra a Steam e vá para **Biblioteca**.
2. Clique com o botão direito em um jogo e selecione **Propriedades**.
3. Vá para **Compatibilidade**.
4. Marque **"Forçar o uso de uma ferramenta de compatibilidade Steam Play específica"**.
5. Selecione **GE-Proton10-12** na lista.

---

### **5. (Opcional) Verifique se o Proton está instalado corretamente**
- Verifique se a pasta `GE-Proton10-12` está em:
  ```bash
  ls ~/.steam/root/compatibilitytools.d/
  ```
- Se você estiver usando a versão **Flatpak** da Steam, o caminho seria:
  ```bash
  ~/.var/app/com.valvesoftware.Steam/data/Steam/compatibilitytools.d/
  ```
  (Mas como você está usando a versão `.deb`, isso não se aplica.)

---

### **6. Problemas comuns**
- **Steam não mostra o Proton**: Verifique se você extraiu o arquivo para a pasta correta e reiniciou a Steam.
- **Erros de permissão**: Execute:
  ```bash
  chmod -R 755 ~/.steam/root/compatibilitytools.d/GE-Proton10-12/
  ```
- **Faltam dependências**: Instale os pacotes necessários para o Proton:
  ```bash
  sudo apt install vulkan-utils libvulkan1 mesa-vulkan-drivers
  ```

---

Pronto! Agora você pode usar o **GE-Proton10-12** em qualquer jogo da Steam. Se tiver problemas, verifique o [GitHub do GE-Proton](https://github.com/GloriousEggroll/proton-ge-custom) para soluções. 🚀