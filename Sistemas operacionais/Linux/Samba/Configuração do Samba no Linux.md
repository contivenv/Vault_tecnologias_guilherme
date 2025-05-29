Fazer atualização da lista de repositórios
```bash
apt update
```
1. **Instalar o serviço do Samba**:
```bash
apt install samba -y
```
2. **Criar usuários que não podem fazer login no sistema e não tenham diretório home**:
```bash
adduser --disabled-login --no-create-home nome_do_usuário
```
3. **Criar grupos para os usuários**:
```bash
addgroup nome_do_grupo
```
4. **Adicionar os usuários aos grupos criados**: Com o comando abaixo, é possível compartilhar recursos e permissões específicas com um grupo de usuários em vez de concedê-las individualmente a cada usuário:
```bash
usermod -a -G nome_do_grupo nome_do_usuário
```
5. **Criar o diretório principal**:
```bash
mkdir diretório_pai
```
6. **Dar permissão de acesso total para root e grupos, e para outros usuários permitir apenas leitura e execução**:
```bash
chmod 775 diretório_pai
```
7. **Criar os subdiretórios**:
```bash
mkdir diretório_pai/subdiretório
```
8. **Definir permissões recursivas para os subdiretórios**: Permissão total para root e grupos, bloqueando outros usuários:
```bash
chmod -R 770 diretório_pai/subdiretório
```
9. **Atribuir cada pasta ao seu respectivo grupo**:
```bash
chown -R root:grupo diretório_pai/subdiretório
```
10. **Criar senha de acesso para os usuários compartilhados**:
```bash
smbpasswd -a nome_do_usuário
```
## Configuração do Samba

1. **Renomear o arquivo `smb.conf` para criar uma nova configuração personalizada**:
```bash
cd /etc/samba/
mv smb.conf smb.conf.bkp
```
2. **Abrir o arquivo `smb.conf` com o editor de texto de sua preferência**:
```bash
vim smb.conf
```
3. **Exemplo de configuração no arquivo `smb.conf`**:

    [global]
    netbios name = servidorSamba
    workgroup = WORKGROUP
    
    [arquivos]
    path = /home/samba
    writeable = yes
    available = yes
    force group = ti
    force group = financeiro
    create mask = 0770
    directory mask = 0770
    valid users = @financeiro, @ti
    ### Explicação das linhas:
    - `netbios name`: Nome do servidor na rede.
    - `workgroup`: Nome do grupo de trabalho.
    - `path`: Caminho dos arquivos compartilhados.
    - `writeable`: Permitir que os usuários escrevam no arquivo.
    - `available`: Permitir o compartilhamento.
    - `force group`: Manter permissões ao criar arquivos/diretórios.
    - `create mask`: Definir permissões para novos arquivos.
    - `directory mask`: Definir permissões para novos diretórios.
    - `valid users`: Usuários ou grupos com acesso.
Para salvar e sair do editor `vim`, utilize:
```bash
:wq!
```
4. **Reiniciar o serviço do Samba**:
```bash
systemctl restart smbd.service
```
5. **Testar em outra máquina (Windows)**:
- No Windows Explorer, acesse o servidor pelo IP:
 ```
\\\\IP 
```
- Teste o acesso à pasta e verifique se é possível criar arquivos.