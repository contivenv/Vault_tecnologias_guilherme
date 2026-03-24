
>[! info] Distribuição Utilizada: Linux Mint 22.3

>[! info] Documentação: https://documentation.ubuntu.com/server/how-to/virtualisation/

# Vídeo para instalação e configuração base da VM dentro do KVM no Linux.![[Windows 11 no KVM.mp4]]



# Pré-requisitos e verificação de hardware
- Habilite virtualização (VT-x / AMD-V) no BIOS/UEFI. (Caso necessário)
- No terminal, verifique suporte ao KVM:

```Bash
sudo apt update
sudo apt install -y cpu-checker
kvm-ok
```

# Instalação de pacotes essenciais:

```bash
sudo apt update
sudo apt install -y qemu-kvm libvirt-daemon-system libvirt-clients virt-manager \
bridge-utils virtinst ovmf cpu-checker
```

# Dar permissão ao seu usuário para gerenciar VMs:

``` bash 
sudo usermod -aG libvirt,kvm $USER
# Efetive grupo: faça logout/login ou:
newgrp libvirt
``` 


# Ative e inicie o serviço libvirt:

``` bash
sudo systemctl enable --now libvirtd
sudo systemctl status libvirtd
```


# Criar bridge com NetworkManager (recomendado em desktops)

No Linux Mint (desktop), use NetworkManager para criar a bridge — isso evita tirar a gestão de rede do NetworkManager e manter o host no domínio/DHCP.

## Identifique conexões e a interface física:

``` Bash
nmcli device status
nmcli connection show
ip link
```


## Renomeie a conexão fio atual (opcional):

```bash
# Ex.: se a conexão se chama "Wired connection 1"
sudo nmcli connection modify "Wired connection 1" connection.id "eth0-conn"
```

# Crie a conexão bridge
substitua enp3s0 por sua interface de rede real 

```bash
sudo nmcli connection add type bridge ifname br0 con-name br0
sudo nmcli connection add type bridge-slave ifname enp3s0 master br0 con-name br0-slave-enp3s0
```

copie as configurações IP (se quiser DHCP)

```bash
sudo nmcli connection modify br0 ipv4.method auto ipv6.method ignore
sudo nmcli connection up br0
sudo nmcli connection up br0-slave-enp3s0
```
Se o host recebe IP por DHCP, br0 deverá obter IP automaticamente.

# Teste reiniciando NetworkManager: 

```bash
sudo systemctl restart NetworkManager 
ip addr 
nmcli device show
```

# Comando para criar o network bridge no libvirt

Crie o arquivo:

```bash
cat <<EOF | sudo tee /tmp/br0.xml
<network>
  <name>br0</name>
  <forward mode='bridge'/>
  <bridge name='br0'/>
</network>
EOF
```

Aplique as configurações para subir a interface bridge para o Virt-manager

```bash
sudo virsh net-define /tmp/br0.xml
sudo virsh net-start br0
sudo virsh net-autostart br0
```

# Confirmar estado da rede bridge

```bash
virsh net-list --all
```

``` bash
Resultado espererado:
Nome     Estado     Autostart   Persistente
--------------------------------------------
br0      ativo      sim         sim
```

>[! attention] Ao criar a VM colocar a ISO no seguinte caminho: 
>var/lib/libvirt/images

Dê permissões á pasta:

```bash
sudo chmod 644 /var/lib/libvirt/images/Win11_25H2_modificado.iso
```

>[! attention] Após as alterações das redes, a interface criada deverá ser utilizada como principal. Assim, necessitando de liberação de IP novamente. No caso deste tutorial a interface br0 se tornaria o principal

Mudança na interface de rede

Esse erro ocorre porque a máquina virtual está configurada para procurar uma rede virtual interna do KVM chamada **"default"** (geralmente baseada em NAT), mas essa rede não existe ou está desativada no seu sistema.

Como o seu objetivo é utilizar a bridge **br0** que você já criou, você precisa alterar a configuração de hardware da VM para apontar diretamente para essa interface física/lógica, em vez de depender da rede padrão do libvirt.

Aqui estão os passos para resolver:
# Etapa final

Agora que criamos uma interface de rede virtual, atrelamos nosso IP a ela para fazer a ponte (bridge) para a VM e colocamos um novo IP liberado no firewall para a VM, precisamos trocar a identificação da interface gráfica Virt-Manager para a interface virtual de rede que criamos com o nome de **br0**.

---
## Alterar via Terminal (`virsh`)

Se preferir usar linha de comando ou estiver em um servidor sem interface gráfica:
### 1. Editar o XML da máquina virtual
```bash
virsh edit nome_da_sua_vm (no caso)
```
---
### 2. Localizar a interface de rede
Procure pela seção:

```xml
<interface type='network'>
  <source network='default'/>
  <model type='virtio'/>
</interface>
```
---
### 3. Alterar para modo bridge (`br0`)
Substitua por:
```xml
<interface type='bridge'>
  <source bridge='br0'/>
  <model type='virtio'/>
</interface>
```
---
### 4. Salvar e sair
No nano:
```txt
Ctrl + x
Enter
```
