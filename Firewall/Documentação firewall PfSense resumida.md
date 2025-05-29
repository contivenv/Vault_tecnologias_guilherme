---
tags:
  - firewall
  - pfsense
  - cibersegurança
---
Em conversa com chat, pedi para que ele me criasse uma documentação resumida com as principais funções e as mais usadas nele.
# Documentação do PfSense

## Introdução ao PfSense

O **PfSense** é uma distribuição de software livre baseada no FreeBSD, amplamente utilizada como firewall e roteador. Ele oferece recursos avançados de segurança e rede, sendo uma solução robusta para empresas e ambientes domésticos. A interface web intuitiva facilita sua configuração e manutenção, mesmo para usuários com pouca experiência em redes.

---
## Instalação do PfSense

### Requisitos Mínimos

- **CPU**: Processador de 1 GHz (64 bits recomendado).
- **Memória RAM**: 1 GB (2 GB ou mais para cenários complexos).
- **Armazenamento**: 4 GB (SSD recomendado para melhor desempenho).
- **Interfaces de Rede**: Pelo menos duas (WAN e LAN).
### Passo a Passo

1. Baixe a imagem ISO do PfSense no site oficial: [https://www.pfsense.org/](https://www.pfsense.org/).
2. Grave a imagem em um pendrive ou mídia de instalação.
3. Inicie a máquina pela mídia gravada.
4. Siga o assistente de instalação, configurando:
    - Layout do teclado.
    - Disco de instalação.
5. Após a instalação, configure os endereços IP para as interfaces WAN e LAN.

---
## Configuração Inicial

### Acesso à Interface Web

1. Conecte-se à rede LAN do PfSense.
2. Acesse o endereço padrão: `http://192.168.1.1`.
3. Insira as credenciais padrão:
    - **Usuário**: admin
    - **Senha**: pfsense
4. Siga o assistente inicial para:
    - Definir uma nova senha para o administrador.
    - Configurar o IP da WAN (DHCP ou Estático).
    - Configurar DNS.
    - Ajustar o fuso horário.

---
## Configurações Mais Importantes

### Regras de Firewall

As regras de firewall são a base da segurança do PfSense. Elas controlam o tráfego que entra e sai da rede.

1. **Acesse**: Firewall > Rules.
2. Configure regras para cada interface (WAN, LAN ou outras).
3. **Exemplo de Regra**: Permitir tráfego HTTP/HTTPS na LAN:
    - Interface: LAN
    - Protocolo: TCP
    - Porta de Origem: Any
    - Porta de Destino: 80, 443
    - Ação: Allow
### NAT (Network Address Translation)

O NAT é essencial para permitir que dispositivos na rede local acessem a internet.

1. **Acesse**: Firewall > NAT > Outbound.
2. Configure o modo:
    - Automático (padrão) ou Manual para regras personalizadas.
3. Adicione regras para redirecionar portas, como acesso remoto a serviços internos.
### Configuração de VPN

O PfSense suporta diferentes tipos de VPN, como OpenVPN e IPsec.
#### OpenVPN

1. **Acesse**: VPN > OpenVPN > Wizards.
2. Configure o servidor:
    - Modo de autenticação.
    - Redes a serem permitidas.
3. Baixe o cliente ou arquivo de configuração para conexões remotas.

### Gerenciamento de Pacotes

O PfSense possui um gerenciador de pacotes para adicionar funcionalidades extras.

1. **Acesse**: System > Package Manager.
2. Procure por pacotes úteis como:
    - **pfBlockerNG**: Controle de acesso a domínios e IPs.
    - **Squid**: Proxy para cache de internet.
    - **Snort**: Sistema de detecção de intrusões (IDS).

---
## Monitoramento e Logs

### Dashboard

Personalize o painel inicial para exibir informações importantes, como:

- Uso da CPU e memória.
- Estatísticas de tráfego.
- Status das interfaces.
### Logs

Os logs ajudam a identificar problemas e monitorar eventos.

1. **Acesse**: Status > System Logs.
2. Verifique categorias como:
    - Firewall.
    - VPN.
    - Sistema.

---

## Backup e Restauração

### Backup

1. **Acesse**: Diagnostics > Backup & Restore.
2. Clique em "Download Configuration" para salvar o backup.

### Restauração

1. **Acesse**: Diagnostics > Backup & Restore.
2. Envie o arquivo de backup e clique em "Restore".

---

## Manutenção

### Atualizações

Mantenha o PfSense atualizado para garantir segurança e novos recursos.

1. **Acesse**: System > Update.
2. Verifique por atualizações e aplique-as.

### Reset de Configurações

Caso necessário, redefina as configurações para o padrão de fábrica:

1. **Acesse**: Diagnostics > Factory Defaults.
2. Confirme a ação.

---

## Conclusão

O PfSense é uma ferramenta poderosa e flexível para gerenciamento de redes. Com as configurações básicas e avançadas apresentadas nesta documentação, você terá uma base sólida para implementar soluções de segurança e conectividade em sua infraestrutura.