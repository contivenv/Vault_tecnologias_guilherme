---
tags:
  - ssh
  - debian
  - conectividade
  - acesso_remoto
  - segurança
---
**1.Atualizar a lista de pacotes:**

Sincronize os repositórios do sistema para garantir que você baixará a versão mais recente do pacote:

```Bash
sudo apt update
```

**2.Instalar o servidor OpenSSH:**

Baixe e instale o pacote principal do servidor SSH. A flag `-y` confirma a instalação automaticamente:

```Bash
sudo apt install openssh-server -y
```

**3.Habilitar e iniciar o serviço:**Inicia o SSH imediatamente e o configura para ligar com o sistema.

Embora o Debian 13 geralmente inicie o serviço automaticamente após a instalação, utilize este comando para garantir o funcionamento contínuo:

```Bash
sudo systemctl enable --now ssh
```

**4.Verificar o status:**

Confirme se o servidor está ativo. Procure pela mensagem **active (running)** na cor verde (pressione a tecla `q` para sair desta tela):

```Bash
sudo systemctl status ssh
```

**5.Liberar o acesso no Firewall: Necessário apenas se você utiliza o UFW ativo.**

Se o firewall padrão estiver ativado, você precisará liberar o tráfego de entrada na porta 22 para que as conexões não sejam bloqueadas:

```Bash
sudo ufw allow ssh
```

**6.Realizar a primeira conexão**

Para realizar uma conexão via SSH pelo terminal, digite o comando `ssh usuario@ip_do_servidor` substituindo os dados pelo seu usuário e endereço IP.

Passo a passo para conectar via SSH

- **Abra o terminal**: Use o aplicativo de terminal no Linux ou macOS, ou o Prompt de Comando/PowerShell no Windows.

- **Digite o comando básico**:

```bash
 ssh usuario@ip_do_servidor
```

Se a porta do servidor for diferente da padrão (22), use a flag `-p`:

```bash
ssh -p porta usuario@ip_do_servidor
```

- **Confirme a chave do host**: Na primeira conexão, o terminal exibirá um alerta de segurança perguntando se você deseja continuar. Digite `yes` e pressione **Enter**.

- **Insira a senha**: Digite a senha do usuário remoto. Por segurança, os caracteres não aparecerão na tela enquanto você digita. Pressione **Enter** ao terminar.

- **Encerrar a sessão**: Para sair do servidor remoto e voltar ao seu computador local, digite `exit`.![[ssh para server virtualizado 1.png]]