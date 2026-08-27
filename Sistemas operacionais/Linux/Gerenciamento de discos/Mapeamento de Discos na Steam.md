---
tags:
  - discos
  - mapeamento_de_discos
  - montagem_de_disco
  - fstab
data criação: 2026-08-27T13:01:00
---
Dando uma olhada na `image_225c1e.png`, dá para ver que você já criou a pasta `/mnt/crucial` e até definiu o seu usuário como dono dela. Porém, o seu `fastfetch` e a `image_225c57.png` mostram que o disco continua montado no caminho temporário (`/run/media/guilhermect/60de68ee...`).

O GNOME Discos às vezes se perde e falha ao aplicar as configurações se o disco ainda estiver montado no caminho antigo enquanto você tenta editar as opções, ou simplesmente dá algum bug na hora de escrever no arquivo de configuração do sistema.

Como você já está bem familiarizado com o terminal e o ambiente Linux, a forma mais garantida de resolver isso e não depender da interface gráfica é editando o arquivo `/etc/fstab` diretamente. É jogo rápido e infalível.

Aqui está o passo a passo para cravarmos essa montagem:
### 1. Desmonte o disco atual

Primeiro, precisamos soltar o disco do caminho temporário. No terminal, rode:

```bash
sudo umount /dev/sdb1
```

### 2. Edite o arquivo fstab

O `/etc/fstab` é o arquivo que o Linux lê no boot para saber onde e como montar cada disco. Vamos abri-lo com o nano:

```bash
sudo nano /etc/fstab
```

### 3. Adicione a regra do SSD Crucial

Use as setas do teclado para ir até a última linha do arquivo. Pela sua captura de tela, eu já peguei o UUID exato do seu SSD de 500GB. Adicione esta linha no final do arquivo:

```bash
UUID=60de68ee-8676-4db7-b63e-997fb6e17047 /mnt/crucial ext4 defaults,noatime 0 2
```

_Dica: o `noatime` ajuda a prolongar a vida útil do SSD, pois evita que o sistema escreva no disco apenas para registrar a "data do último acesso" toda vez que um arquivo for lido._

Para salvar no nano, aperte `Ctrl + O`, depois `Enter` para confirmar, e `Ctrl + X` para sair.

### 4. Teste a montagem (Crucial)

**Nunca reinicie o PC após editar o fstab sem testar antes.** Se houver um erro de digitação, o sistema pode cair no modo de emergência no próximo boot. Para testar se está tudo certo, peça para o sistema montar tudo que está no fstab:

```bash
sudo mount -a
```

Se o comando não retornar nenhum erro, deu tudo certo! Você pode rodar um `df -h` para confirmar se ele apareceu em `/mnt/crucial`.

### 5. Ajuste as permissões novamente

Esse é o pulo do gato: quando você monta um sistema de arquivos Ext4 em uma pasta, as permissões originais da raiz daquele disco substituem as permissões da pasta. Ou seja, mesmo que você já tenha dado o `chown` na pasta vazia antes, o dono volta a ser o `root` assim que o disco é montado.

Rode o comando de permissão **agora que o disco está montado**:

```bash
sudo chown -R guilhermect:guilhermect /mnt/crucial
```

Feito isso, é só abrir a Steam, ir em **Armazenamento**, adicionar a nova unidade apontando para `/mnt/crucial` e instalar seus jogos !