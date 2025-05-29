---
tags:
  - suricata
  - redes
  - firewall
  - pfsense
  - automatização
  - tcc
  - linux
  - faculdade
data criação: 2025-02-04T16:00:00
---
##### Quando instalo e início o serviço do Suricata em uma máquina Linux, ele vai ficar rodando e colhendo dados da rede até que eu pare o serviço dele ou desligue a máquina ?

Sim, o **Suricata** continuará rodando e coletando dados da rede enquanto a máquina estiver ligada e o serviço não for interrompido. No entanto, há alguns pontos importantes a considerar se você pretende deixá-lo rodando por dois meses ininterruptos:
### 🔹 **Fatores a Considerar**

1. **Uso de Disco** 🛑
    
    - O Suricata gera **muitos logs** dependendo do volume de tráfego da rede. Se não for configurado corretamente, pode encher o disco rapidamente.
    - Verifique o diretório onde os logs estão sendo armazenados (geralmente `/var/log/suricata/`) e implemente um sistema de **rotação de logs** com o `logrotate` para evitar problemas.
2. **Uso de Memória e CPU** ⚡
    
    - Se o tráfego da rede for muito alto, o Suricata pode consumir bastante **CPU** e **memória**, o que pode afetar o desempenho da máquina.
    - É bom monitorar o uso de recursos com comandos como:
        
        ```bash
        top
        htop
        free -h
        ```
        
3. **Possíveis Reinícios da Máquina** 🔄
    
    - Se a máquina for reiniciada (por falha de energia, atualizações, etc.), o Suricata pode parar caso não esteja configurado para reiniciar automaticamente.
    - Para garantir que ele sempre inicie com a máquina:
        
        ```bash
        sudo systemctl enable suricata
        ```
        
4. **Verificação Regular dos Logs** 📊
    
    - Para evitar perda de dados, é bom revisar os logs periodicamente:
        
        ```bash
        ls -lh /var/log/suricata/
        tail -f /var/log/suricata/fast.log
        ```
        
    - Você também pode configurar uma ferramenta como **ELK (Elasticsearch, Logstash, Kibana)** para visualizar os logs de forma gráfica.

### 🔹 **Recomendações para Longo Prazo**

✔ **Monitoramento automático:** Use o `cron` ou um script para verificar o espaço em disco e enviar alertas caso fique cheio.  
✔ **Rotação de logs:** Configure o `logrotate` para evitar acúmulo excessivo.  
✔ **Backups periódicos:** Se os logs são importantes, faça backup regular para outra máquina ou um storage externo.

Se precisar de ajuda para configurar algo específico, me avise! 🚀

---
##### Me dê instruções para que eu realize essas recomendações a longo prazo que você me deu, por favor. Observação: visando que ele vai precisar coletar dados da rede durante dois meses.

Ótima decisão! Para garantir que o **Suricata** colete dados da rede por dois meses sem interrupções e sem comprometer o desempenho da máquina, siga estas instruções detalhadas:
## **1️⃣ Configurar a Rotação de Logs para Evitar que o Disco Encha**

O Suricata gera muitos logs, então vamos configurar o **logrotate** para gerenciá-los automaticamente.

### **Passos:**

1. Abra o arquivo de configuração do **logrotate** para o Suricata:

```bash
sudo nano /etc/logrotate.d/suricata
```

2. Adicione ou modifique o conteúdo para algo como este:

```ini
/var/log/suricata/*.log {
    daily                    # Roda a rotação diariamente
    rotate 60                # Mantém logs dos últimos 60 dias
    compress                 # Comprime logs antigos para economizar espaço
    delaycompress            # A compressão ocorre um dia após a rotação
    missingok                # Se o log não existir, ignora o erro
    notifempty               # Não roda a rotação se o log estiver vazio
    create 640 suricata suricata # Define as permissões dos novos arquivos de log
    postrotate
        systemctl restart suricata # Reinicia o Suricata após rotação para garantir continuidade
    endscript
}
```

3. Salve (`CTRL + X`, `Y`, `Enter`) e teste se está funcionando:

```bash
sudo logrotate -d /etc/logrotate.d/suricata
```

Isso simula a rotação para verificar se há erros.

---

## **2️⃣ Monitorar o Espaço em Disco para Evitar Problemas**

Crie um **script automático** para alertá-lo caso o disco esteja quase cheio.

### **Passos:**

1. Crie um script para monitorar o espaço em disco:

```bash
sudo nano /usr/local/bin/verifica_disco.sh
```

2. Adicione o seguinte código ao arquivo:

```bash
#!/bin/bash
USO=$(df / | awk 'NR==2 {print $5}' | sed 's/%//')
LIMITE=90  # Altere para o percentual que quiser como limite de alerta

if [ "$USO" -gt "$LIMITE" ]; then
    echo "⚠️ ALERTA: O uso do disco está em $USO%! Verifique os logs do Suricata." | mail -s "Alerta de Disco Cheio" seu-email@dominio.com
fi
```

3. Salve (`CTRL + X`, `Y`, `Enter`) e dê permissão de execução:

```bash
sudo chmod +x /usr/local/bin/verifica_disco.sh
```

4. Configure uma tarefa no **cron** para rodar esse script a cada 6 horas:

```bash
sudo crontab -e
```

5. Adicione esta linha ao final do arquivo:

```bash
0 */6 * * * /usr/local/bin/verifica_disco.sh
```

**Isso verifica o uso do disco a cada 6 horas.**

---

## **3️⃣ Garantir que o Suricata Inicie Automaticamente Após Reinícios**

Para evitar interrupções no monitoramento, o Suricata deve reiniciar automaticamente quando a máquina for ligada.

### **Passos:**

1️⃣ Ative o serviço para iniciar no boot:

```bash
sudo systemctl enable suricata
```

2️⃣ Verifique se está ativado corretamente:

```bash
systemctl is-enabled suricata
```

Se a saída for `enabled`, está funcionando.

---

## **4️⃣ Monitorar o Uso de Recursos da Máquina**

Se o **Suricata** consumir muita CPU/RAM, pode prejudicar a máquina. Vamos configurar alertas automáticos.

### **Passos:**

1️⃣ Crie um script para monitorar o uso de CPU e memória:

```bash
sudo nano /usr/local/bin/verifica_recursos.sh
```

2️⃣ Adicione este código ao arquivo:

```bash
#!/bin/bash
CPU=$(top -bn1 | grep "Cpu(s)" | awk '{print $2 + $4}')
MEM=$(free | awk '/Mem/{print $3/$2 * 100.0}')
LIMITE_CPU=80
LIMITE_MEM=90

if (( $(echo "$CPU > $LIMITE_CPU" | bc -l) )); then
    echo "⚠️ ALERTA: Uso da CPU está em ${CPU}%! Verifique o Suricata." | mail -s "Alerta de CPU Alta" seu-email@dominio.com
fi

if (( $(echo "$MEM > $LIMITE_MEM" | bc -l) )); then
    echo "⚠️ ALERTA: Uso de Memória está em ${MEM}%! Verifique o Suricata." | mail -s "Alerta de Memória Alta" seu-email@dominio.com
fi
```

3️⃣ Salve (`CTRL + X`, `Y`, `Enter`) e dê permissão de execução:

```bash
sudo chmod +x /usr/local/bin/verifica_recursos.sh
```

4️⃣ Configure o cron para rodar a cada hora:

```bash
sudo crontab -e
```

Adicione esta linha ao final:

```bash
0 * * * * /usr/local/bin/verifica_recursos.sh
```

---

## **5️⃣ Configurar um Backup Regular dos Logs**

Se precisar armazenar logs para auditoria, faça backup regularmente.

### **Passos:**

1️⃣ Crie um script de backup:

```bash
sudo nano /usr/local/bin/backup_suricata.sh
```

2️⃣ Adicione este código ao arquivo:

```bash
#!/bin/bash
DESTINO="/backup/suricata"
mkdir -p $DESTINO
tar -czf $DESTINO/suricata_logs_$(date +%F).tar.gz /var/log/suricata/
```

3️⃣ Salve (`CTRL + X`, `Y`, `Enter`) e dê permissão de execução:

```bash
sudo chmod +x /usr/local/bin/backup_suricata.sh
```

4️⃣ Configure o cron para rodar o backup diariamente às 2h da manhã:

```bash
sudo crontab -e
```

Adicione:

```bash
0 2 * * * /usr/local/bin/backup_suricata.sh
```

---

## **6️⃣ (Opcional) Visualizar Logs do Suricata com Kibana + ELK**

Se quiser visualizar os logs em gráficos e dashboards bonitos, pode configurar a stack **ELK (Elasticsearch, Logstash, Kibana)** para ingerir os logs do Suricata. Se quiser seguir por esse caminho, me avise que te passo um guia! 🚀

---

### **🔹 Resumo do Que Foi Configurado**

✅ **Rotação de logs** com `logrotate` para evitar que o disco encha.  
✅ **Monitoramento do uso do disco** com alertas automáticos via e-mail.  
✅ **Início automático do Suricata** para que ele volte a rodar após um reboot.  
✅ **Monitoramento do uso de CPU e RAM** para evitar lentidão na máquina.  
✅ **Backup automático** dos logs para armazenamento seguro.

Com isso, o Suricata pode rodar por **dois meses ou mais sem problemas**! 🚀

Se precisar de mais alguma personalização ou ajuda, me avise! 😉

---
##### **E como posso extrair essas informações ?**

Para **extrair as informações** coletadas pelo **Suricata** e analisá-las de maneira útil, você pode acessar os logs diretamente, filtrar dados importantes e até mesmo usar ferramentas gráficas como **Kibana** ou **Grafana**. Aqui estão diferentes formas de extração e análise dos dados:
## **1️⃣ Acessando os Logs do Suricata**

Os logs padrão do Suricata geralmente ficam em **`/var/log/suricata/`**.

### **📌 Verificando logs rapidamente**

Para visualizar os eventos em tempo real:

```bash
tail -f /var/log/suricata/fast.log
```

Isso mostra os alertas mais recentes.

Para ver os últimos **100 eventos** registrados:

```bash
tail -n 100 /var/log/suricata/fast.log
```

Para verificar arquivos JSON (logs mais detalhados):

```bash
cat /var/log/suricata/eve.json | jq
```

⚠️ **Se o comando `jq` não estiver instalado, instale com:**

```bash
sudo apt install jq -y
```

---

## **2️⃣ Filtrando Dados Úteis nos Logs**

Os logs do Suricata podem conter **milhares** de linhas. Para encontrar informações específicas, use **filtros**.

### **🔍 Encontrando alertas específicos**

Se quiser **buscar alertas de um IP específico**, use:

```bash
grep "192.168.1.100" /var/log/suricata/fast.log
```

Para **filtrar logs de ataques críticos** (exemplo, ataques SQL Injection):

```bash
grep -i "SQL Injection" /var/log/suricata/fast.log
```

Para **contar quantos eventos foram registrados** no log:

```bash
wc -l /var/log/suricata/fast.log
```

Se quiser **visualizar apenas eventos de um período**, use:

```bash
grep "2025-02-04" /var/log/suricata/fast.log
```

_(Substitua a data pelo dia desejado)._

---

## **3️⃣ Extraindo Informações para um Arquivo CSV**

Se quiser **exportar logs** para uma planilha e analisá-los no Excel, faça assim:

1️⃣ **Extraia os principais alertas para um CSV:**

```bash
awk '{print $1","$2","$3","$4","$5}' /var/log/suricata/fast.log > alertas.csv
```

Depois, você pode abrir o **`alertas.csv`** no Excel.

2️⃣ **Se estiver usando `eve.json`**, converta JSON para CSV:

```bash
jq -r '. | [.timestamp, .src_ip, .src_port, .dest_ip, .dest_port, .alert.signature] | @csv' /var/log/suricata/eve.json > eventos.csv
```

_(Isso gera um arquivo CSV com Timestamp, IP de Origem, Porta de Origem, IP de Destino, Porta de Destino e a Assinatura do Alerta)._

---

## **4️⃣ Criando um Relatório Diário Automatizado**

Se quiser **gerar um relatório** automaticamente com os principais eventos do dia, use este script:

### **📄 Criando o Script**

1️⃣ Crie o arquivo de script:

```bash
sudo nano /usr/local/bin/relatorio_suricata.sh
```

2️⃣ Adicione este código ao arquivo:

```bash
#!/bin/bash

DATA=$(date +%F)
LOG="/var/log/suricata/eve.json"
SAIDA="/var/log/suricata/relatorio_$DATA.txt"

echo "📌 Relatório de Segurança - $DATA" > $SAIDA
echo "----------------------------------" >> $SAIDA
echo "🚨 TOP 10 Alertas de Segurança:" >> $SAIDA
jq -r '.alert.signature' $LOG | sort | uniq -c | sort -nr | head -10 >> $SAIDA
echo "" >> $SAIDA
echo "🔍 IPs Mais Frequentes:" >> $SAIDA
jq -r '.src_ip' $LOG | sort | uniq -c | sort -nr | head -10 >> $SAIDA
echo "" >> $SAIDA
echo "📡 Destinos mais acessados:" >> $SAIDA
jq -r '.dest_ip' $LOG | sort | uniq -c | sort -nr | head -10 >> $SAIDA
echo "" >> $SAIDA
echo "✅ Relatório Gerado em: $SAIDA"
```

3️⃣ Salve (`CTRL + X`, `Y`, `Enter`) e dê permissão de execução:

```bash
sudo chmod +x /usr/local/bin/relatorio_suricata.sh
```

4️⃣ Agora, agende para rodar **todo dia às 23h** no cron:

```bash
sudo crontab -e
```

Adicione esta linha ao final:

```bash
0 23 * * * /usr/local/bin/relatorio_suricata.sh
```

Agora, todo dia será gerado um relatório em **`/var/log/suricata/relatorio_YYYY-MM-DD.txt`** com as **principais ameaças, IPs suspeitos e tráfego anormal**.

---

## **5️⃣ Visualizando os Dados com Ferramentas Gráficas**

Se preferir analisar os dados de forma visual, você pode integrar o **Suricata** com ferramentas como:

### **📊 Kibana + ELK (Recomendado para Logs Detalhados)**

1. Instale **Elasticsearch**, **Logstash** e **Kibana** no seu servidor.
2. Configure o Logstash para coletar os logs do Suricata.
3. No Kibana, crie dashboards personalizados para visualizar ataques, tráfego suspeito e alertas críticos.

Se quiser um guia detalhado sobre isso, me avise! 🚀

### **📊 Grafana + InfluxDB (Ótimo para Monitoramento de Tráfego)**

4. Instale o **InfluxDB** para armazenar os logs.
5. Configure o Suricata para enviar logs em tempo real.
6. No **Grafana**, crie gráficos para monitorar tráfego, IPs suspeitos e padrões de ataque.

Se quiser um tutorial sobre isso, também posso te ajudar! 😃

---

## **🔹 Resumo do Que Você Pode Fazer**

✅ **Acessar logs em tempo real** (`tail -f fast.log`).  
✅ **Filtrar eventos específicos** (`grep "IP" fast.log`).  
✅ **Exportar logs para CSV** (`jq -r ... > eventos.csv`).  
✅ **Gerar relatórios automáticos** com os alertas do dia.  
✅ **Configurar dashboards gráficos** com Kibana ou Grafana.

Se precisar de mais algum detalhe ou quiser integrar com alguma ferramenta específica, me avise! 🚀