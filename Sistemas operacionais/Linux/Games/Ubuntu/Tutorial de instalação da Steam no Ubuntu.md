---
tags:
  - linux
  - ubuntu
  - games
  - validado
---
# **Tutorial Completo de Instalação da Steam no Ubuntu via .deb (Com Dependências Resolvidas)**

Neste guia, você instalará a **Steam** no **Ubuntu** usando o pacote `.deb` oficial, garantindo que todas as dependências sejam resolvidas automaticamente.

**✔️ Método otimizado** (evita erros de dependências faltantes)  
**✔️ Compatível com Ubuntu 22.04 LTS, 23.10 ou superior**  
## **Por que usaremos o pacote .deb ?**

Ao contrário do pacote Snap, o pacote .deb tem acesso bem menos restrito a sua máquina para acessar seu sistema, tornando o processo de instalação da Steam muito mais fácil e flexível. Notamos essa diferença quando por exemplo configuramos discos opcionais e no pacote snap, não são acessados de maneira alguma sem configurações de permissões manuais. Já no pacote .deb, não temos esse problema, precisamos instalarmos e usar. 

---

## **📋 Pré-requisitos**
- Ubuntu atualizado (22.04 LTS / 23.10 / 24.04)  
- Conexão com a internet  
- Terminal aberto (`Ctrl + Alt + T`)  

---

## **🔧 Passo 1: Atualizar o Sistema**  
Execute no terminal:  
```bash
sudo apt update && sudo apt full-upgrade -y
```

---

## **📥 Passo 2: Baixar o Pacote .deb Oficial da Steam**  
```bash
wget https://cdn.cloudflare.steamstatic.com/client/installer/steam.deb -O steam.deb
```
*(Se não tiver `wget`, instale com `sudo apt install wget -y`)*

---

## **🛠 Passo 3: Instalar o .deb com Dependências Automáticas**  
Use o `gdebi` para instalar o `.deb` **resolvendo automaticamente as dependências**:  

### **Instale o `gdebi` (se não tiver):**  
```bash
sudo apt install gdebi -y
```

### **Instale o Steam com dependências resolvidas:**  
```bash
sudo gdebi steam.deb
```
➜ Pressione **`S`** (Sim) quando perguntado.  

*(Se preferir usar `apt`, rode: `sudo apt install ./steam.deb -y`)*  

---

## **⚙️ Passo 4: Instalar Bibliotecas Adicionais (32-bit)**  
A Steam precisa de pacotes 32-bit para funcionar. Instale-os com:  
```bash
sudo apt install libgl1-mesa-dri:i386 libgl1:i386 libvulkan1:i386 -y
```

### Observação importante !

O client da Steam roda em 32 bits, mas isso não significa que seus jogos mais recentes e atualizados rodaram em 32 bits também. Eles rodaram em 64 bits. Esses pacotes são apenas para o funcionamento correto do client da Steam que roda em 32 bits no seu Ubuntu.

---

## **🚀 Passo 5: Iniciar a Steam**  
### **Pelo terminal:**  
```bash
steam
```  

### Observação

Sempre que quiser detectar algum problema de inicialização ou alguma coisa que ocorre no client da Steam, é interessante inicializa-lo pelo terminal da sua máquina, dessa forma, todos os processos que acontecem são listados em formas de logs do sistema em execução na medida de tempo real que interagem com a Steam.

### **Pelo menu:**  
➜ Aperte `Super` (Tecla Windows) → Busque por **"Steam"** → Clique para abrir.  

---

## **🔍 Solução de Problemas Comuns**  
### **❌ Erro: "Steam needs to install additional packages"**  
Se aparecer um pop-up pedindo instalação de pacotes extras, **aceite a instalação**.  

### **❌ Falha ao abrir (problemas de drivers)**  
Instale os drivers recomendados:  
```bash
sudo ubuntu-drivers autoinstall && sudo reboot
```

### **❌ Configuração corrompida**  
Remova a pasta de configuração (não afeta jogos):  
```bash
rm -rf ~/.local/share/Steam
```

---

## **🎮 O que você viu nesse tutorial ?**

- ✅ **Pacote .deb instalado corretamente**  
- ✅ **Todas as dependências resolvidas**  
- ✅ **Bibliotecas 32-bit configuradas**