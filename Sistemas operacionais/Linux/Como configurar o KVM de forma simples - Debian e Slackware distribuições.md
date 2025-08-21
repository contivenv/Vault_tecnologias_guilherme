---
tags:
  - linux
  - KVM
  - debian
  - slackware
---
## Por que KVM é melhor do que VirtualBox e VMware quando estou usando Linux ?
### 🔑 **Por que o KVM é melhor no Linux**

1. **Nativo do Kernel**
    
    - O KVM está embutido no próprio kernel Linux.
        
    - Ou seja: zero camadas extras, zero “gambiarras”.
        
    - VirtualBox e VMware são camadas adicionais, sempre um pouco mais lentas e menos integradas.
        
2. **Performance quase bare-metal**
    
    - KVM usa **extensões de virtualização de hardware** (Intel VT-x / AMD-V).
        
    - Ele conversa direto com o processador, o que garante desempenho muito próximo de uma máquina física.
        
    - VirtualBox e VMware têm overhead maior (mais instruções traduzidas, mais impacto na performance).
        
3. **Gerenciamento profissional (virt-manager, libvirt, virsh)**
    
    - Você consegue controlar tudo via CLI ou GUI de forma padronizada.
        
    - Libvirt e ferramentas como `virt-manager` são muito mais poderosas e automatizáveis do que a interface simples do VirtualBox.
        
    - VMware tem bons recursos, mas a versão **Workstation** é paga e proprietária.
        
4. **Suporte corporativo e escalabilidade**
    
    - KVM é o que os grandes usam em datacenters, nuvens privadas e até em nuvens públicas (Google Cloud, OpenStack, AWS usa Xen/KVM).
        
    - Não é “ferramenta de laboratório” como VirtualBox.
        
    - Escala para centenas de VMs fácil, sem quebrar.
        
5. **Segurança**
    
    - Mais auditado, mantido diretamente no kernel Linux.
        
    - Integra SELinux, AppArmor, cgroups, namespaces.
        
    - VirtualBox já teve várias falhas críticas por ser mais fechado e menos usado em produção.
        
6. **Open Source de verdade**
    
    - KVM é 100% open source, integrado ao kernel.
        
    - VirtualBox é open source _com extensões proprietárias_.
        
    - VMware é fechado e pago (com versão Player gratuita, mas capada).
        


👉 Resumindo:

- **Linux no desktop/lab** → KVM é mais leve, rápido e integrado.
    
- **Aprendendo virtualização básica** → VirtualBox é mais “plug and play”.
    
- **Ambientes corporativos pesados** → VMware vSphere ou KVM (mas VMware custa 💰).

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
