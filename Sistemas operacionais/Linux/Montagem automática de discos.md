---
tags:
  - discos
  - armazenamento
  - fstab
  - linux
---
Quando crio a inicialização automática do meu disco ssd, ele funciona. Mas na Steam, não consigo selecionar ele para aparecer como um disco de armazenamento. Quando navego até o caminho e clico duas vezes sobre a pasta onde o disco está, nada acontece. As informações do meu /etc/fstab estão dessa maneira:

```bash
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a device; this may
# be used with UUID= as a more robust way to name devices that works even if
# disks are added and removed. See fstab(5).
#
# <file system>             <mount point>  <type>  <options>  <dump>  <pass>
UUID=FD37-4446 /boot/efi vfat umask=0077 0 2
UUID=fd0ac6be-5d29-438b-b860-67d07f450cea / ext4 defaults 0 1
tmpfs /tmp tmpfs noatime,mode=1777   0 0
# inicialização de disco automatica
# SSD (/dev/sdc)
UUID=1b476098-41df-4e5c-ba8e-c3d256ca822a /mnt/xray ext4 noatime,nofail 0 2
```

Vamos analisar cada linha do seu `/etc/fstab` e entender o que cada campo significa:

## **Estrutura básica do fstab:**
Cada linha segue este formato:
```
<dispositivo>  <ponto_de_montagem>  <tipo>  <opções>  <dump>  <pass>
```

---

## **Análise linha por linha:**

### **1. Partição EFI/Boot:**
```
UUID=FD37-4446  /boot/efi  vfat  umask=0077  0  2
```
- **UUID=FD37-4446**: Identificador único da partição EFI
- **/boot/efi**: Ponto de montagem (onde será acessada)
- **vfat**: Tipo de sistema de arquivos (FAT32, comum para EFI)
- **umask=0077**: Permissões restritas (somente root pode ler/escrever)
- **0**: Ignorado pelo `dump` (backup utility)
- **2**: Ordem de verificação do filesystem (2 = verificar depois da root)

### **2. Partição Raiz (Root):**
```
UUID=fd0ac6be-5d29-438b-b860-67d07f450cea  /  ext4  defaults  0  1
```
- **UUID=...**: Identificador único do disco principal
- **/**: Ponto de montagem da raiz do sistema
- **ext4**: Sistema de arquivos principal do Linux
- **defaults**: Opções padrão (rw, suid, dev, exec, auto, nouser, async)
- **0**: Não fazer backup com `dump`
- **1**: Ordem de verificação do filesystem (1 = primeiro, mais importante)

### **3. Diretório /tmp em RAM:**
```
tmpfs  /tmp  tmpfs  noatime,mode=1777  0  0
```
- **tmpfs**: Sistema de arquivos temporário em memória RAM
- **/tmp**: Ponto de montagem para arquivos temporários
- **tmpfs**: Tipo especial para memória
- **noatime,mode=1777**: 
  - `noatime`: Não atualizar timestamp de acesso (performance)
  - `mode=1777`: Permissão sticky bit (todos podem criar, só dono pode deletar)
- **0 0**: Não fazer dump nem verificação

### **4. Seu SSD (PROBLEMA AQUI):**
```
UUID=1b476098-41df-4e5c-ba8e-c3d256ca822a  /mnt/xray  ext4  noatime,nofail  0  2
```
- **UUID=...**: Identificador único do seu SSD
- **/mnt/xray**: Ponto de montagem (normalmente para discos secundários)
- **ext4**: Mesmo sistema de arquivos que o root
- **noatime,nofail**: 
  - `noatime`: Não atualiza timestamp de acesso (bom para SSDs)
  - `nofail`: Não falhar se o disco não estiver presente
- **0**: Não fazer backup
- **2**: Verificar filesystem na inicialização (ordem 2)

---

## **PROBLEMAS IDENTIFICADOS:**

### **1. Opções do SSD muito restritivas:**
`noatime,nofail` são insuficientes. Falta:
- **`defaults`** ou **`rw`**: Para garantir leitura/escrita
- **`exec`**: Permissão para executar programas (ESSENCIAL para jogos)
- **`user`** ou **`users`**: Permitir que usuários montem/acessem

### **2. Permissões de montagem:**
Por padrão, `/mnt/` é acessível, mas o Steam pode ter restrições ao acessar fora do diretório home.

### **3. Ponto de montagem não é "amigável":**
O Steam pode não listar `/mnt/` facilmente no navegador de pastas.

---

## **SOLUÇÃO RECOMENDADA:**

Mude a linha do SSD para:
```bash
UUID=1b476098-41df-4e5c-ba8e-c3d256ca822a  /mnt/xray  ext4  defaults,user,exec,noatime,nofail  0  2
```

Ou, se quiser algo mais específico:
```bash
UUID=1b476098-41df-4e5c-ba8e-c3d256ca822a  /mnt/xray  ext4  rw,user,exec,noatime,nofail  0  2
```

**O que essas opções adicionam:**
- `rw`: Leitura e escrita
- `user` ou `users`: Usuários podem montar/acessar
- `exec`: Permite executar programas/scripts/jogos

---

## **POR QUE NADA ACONTECE QUANDO CLICA NA PASTA?**

1. **Permissões incorretas**: A pasta pode estar montada como `noexec` (sem permissão de execução)
2. **Problema de dono/grupo**: A pasta `/mnt/xray` pode pertencer ao root
3. **Steam rodando em sandbox/flatpak**: Algumas versões do Steam tem acesso restrito

**Verifique isso:**
```bash
# Veja as permissões atuais
ls -ld /mnt/xray

# Veja as opções de montagem atuais
mount | grep xray
```

Após ajustar o fstab, faça:
```bash
sudo umount /mnt/xray
sudo systemctl daemon-reload
sudo chown seu_usuario:seu_usuario /mnt/xray
```

Isso deve resolver o problema de acesso pela Steam!