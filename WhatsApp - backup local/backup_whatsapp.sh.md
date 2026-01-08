---
tags:
  - whatsapp
  - backup
---
Esse script quando executado via USB no celular Android com a porta aberta, ele realiza o backup de forma correta no dispositivo celular para a máquina do usuários.

### Como usar

1. Conecte o celular via USB (com depuração USB ativada).
2. Rode o comando como administrador do seu sistema:
```bash
./backup_whatsapp.sh ~/Backups/WhatsApp-$(date +%Y%m%d)
```
**Observação**: Isso vai salvar tudo no diretório `~/Backups/WhatsApp-20250902` (por exemplo).

3. Depois, se precisar restaurar, use o seu script com permissão de administrador no seu sistema (seja ele Windows ou Linux)`restore_whatsapp.sh` apontando para essa pasta.

```bash
#!/bin/bash

# Script para criar backup local do WhatsApp do Android para o PC
# Uso: ./backup_whatsapp.sh /caminho/para/salvar/backup

# Cores
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m'

print_status() {
    echo -e "${GREEN}[INFO]${NC} $1"
}

print_warning() {
    echo -e "${YELLOW}[AVISO]${NC} $1"
}

print_error() {
    echo -e "${RED}[ERRO]${NC} $1"
}

# Verificar argumento
if [ $# -eq 0 ]; then
    print_error "Uso: $0 /caminho/para/salvar/backup"
    exit 1
fi

BACKUP_DEST="$1"

# Criar diretório de destino
mkdir -p "$BACKUP_DEST"/{Databases,Backups,Media}

print_status "Destino do backup: $BACKUP_DEST"

# Verificar ADB
if ! command -v adb &> /dev/null; then
    print_error "ADB não encontrado. Instale com: sudo dnf install android-tools"
    exit 1
fi

# Verificar conexão
print_status "Verificando conexão com o dispositivo..."
if ! adb devices | grep -q "device$"; then
    print_error "Nenhum dispositivo Android conectado ou depuração USB não habilitada"
    exit 1
fi

# Detectar versão do Android
ANDROID_VERSION=$(adb shell getprop ro.build.version.release | tr -d '\r')
print_status "Versão do Android detectada: $ANDROID_VERSION"

if [[ "$ANDROID_VERSION" -ge 11 ]]; then
    WHATSAPP_PATH="/storage/emulated/0/Android/media/com.whatsapp/WhatsApp"
else
    WHATSAPP_PATH="/sdcard/WhatsApp"
fi

print_status "Usando caminho: $WHATSAPP_PATH"

# Backup dos bancos de dados
print_status "Copiando bancos de dados..."
adb pull "$WHATSAPP_PATH/Databases/." "$BACKUP_DEST/Databases/" 2>/dev/null

# Backup de arquivos de configuração
print_status "Copiando arquivos de configuração..."
adb pull "$WHATSAPP_PATH/Backups/." "$BACKUP_DEST/Backups/" 2>/dev/null

# Backup da mídia
print_status "Copiando mídia (pode demorar)..."
adb pull "$WHATSAPP_PATH/Media/." "$BACKUP_DEST/Media/" 2>/dev/null

print_status "✅ Backup concluído!"
print_warning "Se quiser restaurar depois, use o script restore_whatsapp.sh"

```