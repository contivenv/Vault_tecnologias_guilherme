---
tags:
  - whatsapp
  - restauração
---
```bash
#!/bin/bash

# Script para restaurar backup do WhatsApp no Android
# Uso: ./restore_whatsapp.sh /caminho/para/backup/fedora

# Cores para output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m' # No Color

# Função para imprimir mensagens coloridas
print_status() {
    echo -e "${GREEN}[INFO]${NC} $1"
}

print_warning() {
    echo -e "${YELLOW}[AVISO]${NC} $1"
}

print_error() {
    echo -e "${RED}[ERRO]${NC} $1"
}

# Verificar se o caminho do backup foi fornecido
if [ $# -eq 0 ]; then
    print_error "Uso: $0 /caminho/para/backup/fedora"
    exit 1
fi

BACKUP_PATH="$1"

# Verificar se o diretório de backup existe
if [ ! -d "$BACKUP_PATH" ]; then
    print_error "Diretório de backup não encontrado: $BACKUP_PATH"
    exit 1
fi

print_status "Iniciando processo de restauração do WhatsApp..."

# Verificar se ADB está instalado
if ! command -v adb &> /dev/null; then
    print_error "ADB não encontrado. Instale com: sudo dnf install android-tools"
    exit 1
fi

# Verificar conexão com o dispositivo
print_status "Verificando conexão com dispositivo Android..."
if ! adb devices | grep -q "device$"; then
    print_error "Nenhum dispositivo Android conectado ou depuração USB não habilitada"
    print_warning "Certifique-se de:"
    echo "  1. Conectar o dispositivo via USB"
    echo "  2. Habilitar modo desenvolvedor"
    echo "  3. Ativar depuração USB"
    echo "  4. Autorizar o computador no dispositivo"
    exit 1
fi

print_status "Dispositivo Android conectado!"

# Detectar versão do Android
ANDROID_VERSION=$(adb shell getprop ro.build.version.release | tr -d '\r')
print_status "Versão do Android detectada: $ANDROID_VERSION"

if [[ "$ANDROID_VERSION" -ge 11 ]]; then
    WHATSAPP_PATH="/storage/emulated/0/Android/media/com.whatsapp/WhatsApp"
else
    WHATSAPP_PATH="/sdcard/WhatsApp"
fi

print_status "Usando caminho: $WHATSAPP_PATH"

# Criar estrutura de diretórios no Android
print_status "Criando estrutura de diretórios no Android..."
adb shell "mkdir -p $WHATSAPP_PATH/Databases/"
adb shell "mkdir -p $WHATSAPP_PATH/Backups/"
adb shell "mkdir -p $WHATSAPP_PATH/Media/"

# Transferir arquivos de banco de dados
print_status "Transferindo arquivos de banco de dados..."
if [ -d "$BACKUP_PATH/Databases" ]; then
    latest_backup=$(ls -t "$BACKUP_PATH/Databases"/msgstore-*.db.crypt14 2>/dev/null | head -1)
    if [ -n "$latest_backup" ]; then
        print_status "Definindo $(basename "$latest_backup") como backup principal..."
        adb push "$latest_backup" "$WHATSAPP_PATH/Databases/msgstore.db.crypt14"
    else
        print_warning "Nenhum msgstore-*.db.crypt14 encontrado, copiando arquivos existentes..."
        adb push "$BACKUP_PATH/Databases/." "$WHATSAPP_PATH/Databases/"
    fi
else
    print_warning "Pasta Databases não encontrada em $BACKUP_PATH"
fi

# Transferir arquivos de configuração
print_status "Transferindo arquivos de configuração..."
if [ -d "$BACKUP_PATH/Backups" ]; then
    adb push "$BACKUP_PATH/Backups/." "$WHATSAPP_PATH/Backups/"
else
    print_warning "Pasta Backups não encontrada em $BACKUP_PATH"
fi

# Transferir mídia
print_status "Transferindo mídia..."
if [ -d "$BACKUP_PATH/Media" ]; then
    adb push "$BACKUP_PATH/Media/." "$WHATSAPP_PATH/Media/"
else
    print_warning "Pasta Media não encontrada em $BACKUP_PATH"
fi

# Verificar arquivos transferidos
print_status "Verificando arquivos transferidos..."
adb shell "ls -lh $WHATSAPP_PATH/Databases/"

print_status "✅ Backup restaurado com sucesso!"
print_warning "Próximos passos:"
echo "  1. Desinstale o WhatsApp do dispositivo Android (se ainda não fez)"
echo "  2. Rode este script para copiar os arquivos"
echo "  3. Reinstale o WhatsApp da Play Store"
echo "  4. Configure com o mesmo número de telefone"
echo "  5. O WhatsApp detectará automaticamente o backup local"

print_warning "💡 Dica: Não abra o WhatsApp antes de restaurar os arquivos!"
```