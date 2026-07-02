## O Problema

Após uma atualização de rotina do sistema (kernel), o Fedora falhou ao inicializar, caindo no modo de emergência (`dracut-initqueue: Warning: Not all disks have been found`) com a conta root bloqueada. O prompt para digitar a senha de descriptografia do disco (LUKS) sequer aparecia.

**Causa Raiz:** O arquivo `initramfs` gerado durante a atualização do kernel estava corrompido ou ausente dos módulos de criptografia necessários. Sem o `initramfs` correto, o sistema não consegue abrir o contêiner LUKS para encontrar o sistema de arquivos raiz (`/`), resultando em falha total de boot.

##  Pré-requisitos

- Um pendrive bootável com o **Fedora Live USB** (mesma versão do sistema instalado ou mais recente).
    
- Conhecimento básico de navegação no terminal.
    

## Passo a Passo da Recuperação

### 1. Inicializar pelo Live USB e Identificar as Partições

Dê boot no computador pelo pendrive e abra o terminal. O primeiro passo é mapear a estrutura do disco para encontrar a partição criptografada.

Bash

```
# Lista todos os discos e partições
lsblk

# Exibe os UUIDs exatos e os tipos de sistema de arquivos
sudo blkid
```

_Procure pela partição com a tag `TYPE="crypto_LUKS"`. Neste exemplo, assumiremos que ela é a `/dev/sdc3`, a partição `/boot` é a `/dev/sdc2` e a `/boot/efi` é a `/dev/sdc1`._

### 2. Descriptografar a Partição LUKS

Para acessar os arquivos do sistema quebrado, precisamos destrancar o disco mapeando-o com um nome virtual (aqui chamaremos de `cryptroot`).

Bash

```
sudo cryptsetup open /dev/sdc3 cryptroot
```

_O sistema solicitará a senha do LUKS. Ao digitar corretamente, o terminal não exibirá mensagem de sucesso, apenas pulará para a próxima linha._

### 3. Montar a Estrutura do Sistema (BTRFS)

Com o disco aberto, montamos o sistema de arquivos no diretório temporário `/mnt`. O Fedora utiliza BTRFS por padrão, então precisamos especificar o subvolume raiz.

Bash

```
# Monta a partição raiz
sudo mount -o subvol=root /dev/mapper/cryptroot /mnt

# Monta a partição de boot
sudo mount /dev/sdc2 /mnt/boot

# Monta a partição EFI
sudo mount /dev/sdc1 /mnt/boot/efi
```

### 4. Preparar o Ambiente Chroot

Para que os comandos executados afetem o sistema no SSD (e não o Live USB), precisamos vincular os diretórios de hardware e processos virtuais do Live USB para dentro do sistema montado.

Bash

```
for i in /dev /dev/pts /proc /sys /run; do sudo mount -B $i /mnt$i; done
```

### 5. Entrar no Chroot

Este comando muda o diretório raiz do terminal atual. A partir deste momento, o terminal age como se o sistema principal estivesse rodando nativamente como root.

Bash

```
sudo chroot /mnt
```

### 6. Verificação de Integridade (Opcional, mas Recomendado)

Antes de reconstruir o boot, é boa prática garantir que os UUIDs nos arquivos de configuração do sistema correspondem ao UUID real da partição LUKS encontrado no passo 1.

Bash

```
# Verifique o arquivo crypttab
cat /etc/crypttab

# Verifique as entradas do GRUB
cat /etc/default/grub
```

_A linha `GRUB_CMDLINE_LINUX` deve conter `rd.luks.uuid=luks-<SEU-UUID-CORRETO>`._

### 7. Reconstruir o Initramfs e o GRUB

Este é o passo que efetivamente resolve o problema. Vamos forçar o `dracut` a gerar um novo `initramfs` saudável e atualizar as configurações do GRUB. A compilação costuma ser rápida (especialmente em setups modernos, como um Ryzen 5 9600X).

Bash

```
# Regera o initramfs para todos os kernels instalados
dracut --regenerate-all --force

# Atualiza as configurações do GRUB2
grub2-mkconfig -o /boot/grub2/grub.cfg
```

### 8. Desmontar e Reiniciar

Com o sistema corrigido, saia do ambiente chroot e desmonte tudo com segurança para evitar corrupção de dados.

Bash

```
# Sai do ambiente chroot
exit

# Desmonta recursivamente todas as partições do /mnt
sudo umount -R /mnt

# Reinicia a máquina
reboot
```

Remova o pendrive durante o reinício. O sistema deverá carregar o GRUB normalmente, solicitar a senha do LUKS e inicializar o Fedora com sucesso.

> **Autor:** Guilherme Conti Teixeira