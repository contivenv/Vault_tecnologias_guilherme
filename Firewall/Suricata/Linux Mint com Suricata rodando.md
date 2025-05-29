---
tags:
  - suricata
  - linux
  - tcc
  - faculdade
  - redes
---
Para configurar o **Suricata** no **Linux Mint** para rodar continuamente durante dois meses e coletar informações da rede, você pode seguir estas etapas:

---

### **1. Configurar o Suricata para monitoramento contínuo**

O Suricata deve ser configurado no modo que coleta e registra informações da rede. Isso geralmente é feito no **modo IDS** (Intrusion Detection System) ou **NSM** (Network Security Monitoring).

#### a) **Localizar o arquivo de configuração**

O arquivo principal de configuração do Suricata está geralmente em:

```bash
/etc/suricata/suricata.yaml
```

#### b) **Configurar interfaces de rede**

Edite o arquivo de configuração para especificar qual interface de rede será monitorada:

```bash
sudo nano /etc/suricata/suricata.yaml
```

Procure por `af-packet` ou `interface` e defina a interface que será monitorada. Por exemplo:

```yaml
af-packet:
  - interface: eth0
```

Ou, para outro formato:

```yaml
interface: eth0
```

Substitua `eth0` pela interface de rede usada na máquina. Você pode listar as interfaces disponíveis com:

```bash
ip a
```

---

### **2. Configurar logs e armazenamento**

Para manter os dados por dois meses, verifique os diretórios onde o Suricata salvará os logs:

- **Local padrão de logs**: Geralmente, os logs são armazenados em `/var/log/suricata/`. Você pode configurar no arquivo `suricata.yaml`:
    
    ```yaml
    default-log-dir: /var/log/suricata
    ```
    
- **Logs detalhados**: Certifique-se de ativar os tipos de log necessários (alertas, registros de tráfego, etc.). Exemplos comuns:
    
    ```yaml
    outputs:
      - fast:
          enabled: yes
          filename: fast.log
      - eve-log:
          enabled: yes
          filetype: json
          filename: eve.json
          types:
            - alert
            - dns
            - http
            - tls
    ```
    

#### **Espaço em disco**:

Certifique-se de que a máquina tenha espaço em disco suficiente para armazenar dois meses de logs. Você pode calcular a taxa de geração de logs testando o Suricata por um dia e estimar o uso diário.

---

### **3. Automatizar a execução do Suricata**

Para que o Suricata continue funcionando mesmo após reinicializações, configure-o como um serviço do **systemd**:

#### a) **Criar ou editar o arquivo de serviço**

Crie ou edite o arquivo de serviço do Suricata:

```bash
sudo nano /etc/systemd/system/suricata.service
```

Adicione o seguinte conteúdo:

```ini
[Unit]
Description=Suricata Intrusion Detection Service
After=network.target

[Service]
ExecStart=/usr/bin/suricata -c /etc/suricata/suricata.yaml -i eth0
ExecReload=/bin/kill -HUP $MAINPID
Restart=always

[Install]
WantedBy=multi-user.target
```

Substitua `/usr/bin/suricata` pelo caminho correto do binário (verifique com `which suricata`).

#### b) **Habilitar o serviço**

Ative o serviço para iniciar automaticamente:

```bash
sudo systemctl daemon-reload
sudo systemctl enable suricata
sudo systemctl start suricata
```

---

### **4. Garantir funcionamento contínuo**

#### a) **Manter logs rotacionados**

Para evitar que os arquivos de log ocupem todo o espaço em disco, configure a rotação de logs:

1. Edite o arquivo de configuração do logrotate para o Suricata:
    
    ```bash
    sudo nano /etc/logrotate.d/suricata
    ```
    
2. Configure algo como:
    
    ```
    /var/log/suricata/*.log {
        daily
        rotate 60
        compress
        delaycompress
        missingok
        notifempty
        create 640 suricata suricata
    }
    ```
    

#### b) **Monitoramento do serviço**

Use um monitor de sistema (como o cron ou o próprio systemd) para garantir que o Suricata esteja rodando o tempo todo. Por exemplo, você pode criar um script para verificar se o serviço está ativo:

```bash
sudo systemctl status suricata
```

---

### **5. Verificar os dados coletados**

Depois dos dois meses, você pode analisar os logs:

- Use ferramentas como o **Kibana**, **Elasticsearch** ou **Splunk** para visualizar os dados.
- Ou simplesmente analise os logs manualmente, como o `eve.json`, usando `jq`:
    
    ```bash
    jq '.' /var/log/suricata/eve.json
    ```
    

---

Essas configurações garantirão que o Suricata funcione continuamente por dois meses e armazene os dados coletados para análise posterior. Se precisar de ajuda com monitoramento ou análise, é só avisar! 😊