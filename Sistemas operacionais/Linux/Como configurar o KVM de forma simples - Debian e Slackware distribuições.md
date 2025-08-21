---
tags:
  - linux
  - KVM
  - debian
  - slackware
---
# ⚙️ Configuração do KVM

## 🐧 Debian (e derivados como Ubuntu, Linux Mint, Pop!_OS etc.)

### 1. Verificar suporte

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
lsmod | grep kvm
```

### 2. Instalar pacotes

```bash
sudo apt update
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager -y
```

### 3. Permissões

Adicione seu usuário ao grupo **libvirt**:

```bash
sudo usermod -aG libvirt $USER
```

👉 depois **reinicie a sessão**.

### 4. Testar

```bash
virsh list --all
virt-manager
```

Se abrir o **Virt-Manager**, já pode criar VMs.

---

## 🐧 Slackware

### 1. Verificar suporte

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
lsmod | grep kvm
```

### 2. Instalar pacotes

No Slackware, os pacotes vêm via **SlackBuilds**. Se tiver o `sbopkg` configurado:

```bash
sbopkg -i qemu
sbopkg -i libvirt
sbopkg -i virt-manager
```

👉 Sem `sbopkg`, baixe os SlackBuilds de [slackbuilds.org](https://slackbuilds.org/) e compile manualmente.

### 3. Ativar libvirt

Habilite o script de inicialização:

```bash
chmod +x /etc/rc.d/rc.libvirt
/etc/rc.d/rc.libvirt start
```

Para iniciar junto com o sistema, mantenha o script executável.

### 4. Permissões

Adicione seu usuário aos grupos **kvm** e **libvirt**:

```bash
usermod -aG kvm,libvirt $USER
```

👉 depois **reinicie a sessão**.

### 5. Testar

```bash
virsh list --all
virt-manager
```

---

✅ **Resumo**

- **Debian** → instalação pronta via `apt`, só configurar grupo `libvirt`.
    
- **Slackware** → usar `sbopkg` ou SlackBuilds, ativar `rc.libvirt`, e configurar grupos `kvm` e `libvirt`.
