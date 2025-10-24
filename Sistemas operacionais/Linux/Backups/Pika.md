---
tags:
  - backup
  - linux
  - pika
  - borg
  - armazenamento
  - criptografia
  - GUI
---
### 1. O que é o Pika Backup?

Pense no **Pika Backup** como o "rosto bonito" e amigável do seu sistema de backup.

- É um **programa com interface gráfica (GUI)**, feito para ser simples e fácil de usar, integrando-se bem ao ambiente GNOME (mas funciona em outras distribuições Linux também).
    
- Seu trabalho **não é** fazer o backup em si, mas sim **gerenciar** o processo.
    
- Ele permite que você, de forma visual:
    
    - Escolha quais pastas incluir ou excluir do backup.
        
    - Defina um local para salvar (um HD externo, uma pasta local, um servidor).
        
    - Crie uma senha de criptografia.
        
    - Agende backups automáticos (por exemplo, todo dia às 18h).
        
    - Veja uma lista de backups antigos e os restaure facilmente (como tentamos no início).
        

### 2. Como o Pika (e o Borg) Fazem o Backup?

Aqui está o "pulo do gato": o Pika é apenas um gerente; o "trabalhador pesado" que ele usa por baixo dos panos chama-se **BorgBackup** (ou só "Borg").

Quando você clica em "Fazer Backup Agora" no Pika, ele executa um comando `borg` no terminal para você, mais ou menos assim:

1. **Motor Borg:** O Pika "chama" o Borg e passa as instruções que você definiu (pastas, destino, senha).
    
2. **Desduplicação (A Mágica do Borg):** Esta é a parte mais importante. O Borg não copia seus arquivos inteiros todas as vezes. Em vez disso, ele "quebra" seus arquivos em pequenos blocos (pedaços).
    
    - No **primeiro backup**, ele salva todos os blocos.
        
    - Nos **próximos backups**, ele só salva os blocos **novos** ou **modificados**. Se um arquivo de 10GB mudou apenas 5MB, o Borg só vai salvar esses 5MB.
        
3. **Compressão:** Ele comprime esses blocos para economizar ainda mais espaço.
    
4. **Criptografia:** Ele usa a senha que você criou no Pika para embaralhar todos os dados. Ninguém consegue ler seus arquivos sem essa senha.
    

> **Analogia:** Pense no Pika como o gerente de um armazém. Pense no Borg como um funcionário super eficiente que, em vez de guardar 100 caixas iguais, guarda uma caixa original e apenas uma "nota" dizendo "precisamos de mais 99 iguais a esta". Isso economiza um espaço absurdo.

### 3. Como o Borg Recupera o Backup?

Quando você pede para recuperar um backup (seja pelo Pika ou pelo terminal), o Borg faz o processo inverso:

1. **Você escolhe o "snapshot":** Você diz ao Borg qual "foto" do tempo você quer restaurar (por exemplo, o ID do backup vai estar mais ou menos assim: `57a0f2-84f8901`).
    
2. **O Borg lê o "catálogo":** Cada snapshot tem um "catálogo" (metadados) que diz: "Para recriar o sistema como ele era neste dia, você vai precisar do bloco A, bloco B, bloco Z, etc."
    
3. **Reconstrução:** O Borg vai até o repositório, pega todos os blocos necessários (descriptografando-os com sua senha), junta tudo na ordem certa e recria seus arquivos e pastas exatamente como estavam.
    

É por isso que ele é tão rápido e eficiente: ele não precisa lidar com arquivos duplicados, apenas "remonta" o quebra-cabeça.

---

### Tutorial: O Ciclo de Vida do Backup com Borg (Terminal)

Vamos usar os caminhos e nomes exatos que vimos na nossa conversa.

#### Parte 1: Como FAZER um Novo Backup (Comando `borg create`)

O Pika faz isso quando você clica no botão, mas você pode fazer manualmente. O comando principal é o `borg create`.

- **REPOSITÓRIO:** `~/Documentos/backup_salvo`
    
- **O QUE VOCÊ QUER SALVAR?** (Ex: Suas pastas `Documentos` e `Imagens` da sua Home)
    
- **NOME DO BACKUP:** É bom usar a data.
    

**Comando de exemplo para criar um novo backup:**

Bash

```
# O comando 'create' precisa do repositório, um nome único, e o que salvar.
# '::' separa o repositório do nome do snapshot.
# 'backup-$(date +%Y-%m-%d)' cria um nome com a data atual (ex: backup-2025-10-24)

borg create --progress --stats \
~/Documentos/backup_salvos::backup-$(date +%Y-%m-%d) \
~/Documentos \
~/Imagens \
~/Música
```

- `--progress`: Mostra uma barra de progresso.
    
- `--stats`: Mostra um resumo no final (quanto espaço economizou, etc.).
    
- `~/Documentos ~/Imagens ~/Música`: As pastas de origem que você quer salvar.
    

#### Parte 2: Como VER os Backups (Comando `borg list`)

Este foi o primeiro comando que você executou com sucesso. Ele apenas lista os "snapshots" (os pontos no tempo) que você tem salvo.

**Comando:**

Bash

```
borg list ~/Documentos/backup_salvo
```

**Saída (como a sua):**

```
57a0f2-4a75e1f   Fri, 2025-09-12 20:13:37 [...]
57a0f2-23d1238   Fri, 2025-09-12 22:46:24 [...]
57a0f2-84f8901   Wed, 2025-10-15 23:17:56 [...]
```

#### Parte 3: Como RECUPERAR o Backup (Comando `borg extract`)

Este é o processo que acabamos de fazer. Você tem duas ótimas maneiras de fazer isso:

**Método A: Extrair tudo (O que fizemos)**

Este método restaura todo o conteúdo de um snapshot para a pasta onde você está.

**Passo 1:** Navegue até o local de restauração (ex: a pasta vazia que você criou).

Bash

```
cd "/home/usuario_da_maquina/Área de Trabalho/restauração do Backup/"
```

**Passo 2:** Execute a extração (vamos usar seu backup mais recente como exemplo).

Bash

```
borg extract ~/Documentos/backup_salvo::57a0f2-84f8901
```

O Borg vai pedir a senha e despejar os arquivos ali.

**Método B: Montar como um "Pendrive" (O mais flexível - `borg mount`)**

Este método permite que você navegue no backup com seu gerenciador de arquivos e copie apenas o que quiser.

**Passo 1:** Crie uma pasta vazia para "montar" o backup.

Bash

```
mkdir ~/backup_temporario
```

**Passo 2:** Monte o backup.

Bash

```
borg mount ~/Documentos/backup_salvo::57a0f2-84f8901 ~/backup_temporario
```

**Passo 3:** Agora, abra seu gerenciador de arquivos (Nautilus) e vá até a pasta `backup_temporario`. Todos os seus arquivos daquele backup estarão lá, como se fosse um HD externo! Você pode copiar e colar o que quiser.

**Passo 4 (MUITO IMPORTANTE):** Quando terminar, desmonte o backup.

Bash

```
borg umount ~/backup_temporario
```

### Resumo

- **Pika:** O "piloto" amigável (GUI).
    
- **Borg:** O "motor" poderoso (linha de comando) que faz o trabalho de desduplicação, compressão e criptografia.
    
- `borg create`: Cria um novo backup (snapshot).
    
- `borg list`: Lista os backups existentes.
    
- `borg extract` ou `borg mount`: Recuperam seus arquivos.