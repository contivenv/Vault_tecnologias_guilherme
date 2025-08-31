---
tags:
  - linux
  - ubuntu
  - games
situação: validado
---
Para instalar e configurar o **GE-Proton** (GloriousEggroll's Proton) na sua Steam no Ubuntu (usando a versão .deb da Steam), siga estes passos:

### **1. Baixe o GE-Proton**
Você já tem o arquivo `GE-Proton10-12.tar.gz`. Caso não tenha, baixe-o do [GitHub do GloriousEggroll](https://github.com/GloriousEggroll/proton-ge-custom/releases).

---

### **2. Extraia o arquivo**

1. Abra um terminal (`Ctrl + Alt + T`) e execute:
```bash
tar -xzf GE-Proton10-12.tar.gz -C ~/.steam/debian-installation/compatibilitytools.d/
```

Isso extrairá o Proton para a pasta correta da Steam.

- **Se a pasta `compatibilitytools.d` não existir**, crie-a:
```bash
mkdir -p ~/.steam/debian-installation/compatibilitytools.d/
```

2. **Verifique se a pasta foi criada corretamente**:  
```bash
ls ~/.steam/debian-installation/compatibilitytools.d/
```
Deve aparecer `GE-Proton10-12` na lista.

---

## 3. **Reinicie a Steam Forçadamente**  
Feche a Steam completamente (incluindo processos em segundo plano) e reinicie:  
```bash
killall -9 steam  
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

### **5. Problemas comuns**
- **Steam não mostra o Proton**: Verifique se você extraiu o arquivo para a pasta correta e reiniciou a Steam.
- **Erros de permissão**: Execute:
  ```bash
  chmod -R 755 ~/.steam/debian-installation/compatibilitytools.d/
  ```
- **Faltam dependências**: Instale os pacotes necessários para o Proton:
  ```bash
  sudo apt install vulkan-tools libvulkan1 mesa-vulkan-drivers
  ```

---

Pronto! Agora você pode usar o **GE-Proton10-12** em qualquer jogo da Steam. Se tiver problemas, verifique o [GitHub do GE-Proton](https://github.com/GloriousEggroll/proton-ge-custom) para soluções. 🚀