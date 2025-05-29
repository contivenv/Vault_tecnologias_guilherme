---
tags:
  - firewall
  - suricata
  - tcc
---
Eu tive que setar para o Suricata pegar as informações desses requisitos que eu queria:

        - timestamp
        - src_ip
        - dest_ip
        - src_port
        - dest_port
        - proto
        - event_type
        - alert.signature
        - alert.category
        - flow.bytes_toserver
        - flow.bytes_toclient
        - flow.pkts_toserver
        - flow.pkts_toclient
As instruções que anotei foram essas:

### 3. Habilite a Geração de Logs em CSV

O Suricata pode gerar logs em vários formatos, incluindo CSV. Para habilitar a geração de logs em CSV, edite o arquivo de configuração do Suricata novamente:

```bash
sudo nano /etc/suricata/suricata.yaml
```

Procure pela seção `outputs` e adicione ou modifique a configuração para incluir o output em CSV:

```bash
outputs:
  - csv-log:
      enabled: yes
      filename: suricata.csv
      append: yes
      fields:
        - timestamp
        - src_ip
        - dest_ip
        - src_port
        - dest_port
        - proto
        - event_type
        - alert.signature
        - alert.category
        - flow.bytes_toserver
        - flow.bytes_toclient
        - flow.pkts_toserver
        - flow.pkts_toclient
```

Aqui, você pode personalizar os campos que deseja incluir no arquivo CSV.

Depois de setar, temos que falar pra ele que também queremos extrair essas informações no log do Suricata do CSV

## **3️⃣ Extraindo Informações para um Arquivo CSV**

Se quiser **exportar logs** para uma planilha e analisá-los no Excel, faça assim:

1️⃣ **Extraia os principais alertas para um CSV:**

```bash
awk '{print $1","$2","$3","$4","$5}' /var/log/suricata/fast.log > alertas.csv
```

Depois, você pode abrir o **`alertas.csv`** no Excel.

2️⃣ **Se estiver usando `eve.json`**, converta JSON para CSV:

```bash
jq -r '. | [.timestamp, .src_ip, .src_port, .dest_ip, .dest_port, .alert.signature] | @csv' /var/log/suricata/eve.json > eventos.csv
```

_(Isso gera um arquivo CSV com Timestamp, IP de Origem, Porta de Origem, IP de Destino, Porta de Destino e a Assinatura do Alerta)._

---
Para extrair os principais alertas do Suricata em um arquivo CSV, conforme os campos que você mencionou, você pode seguir as instruções abaixo. Vou adaptar as instruções para garantir que os dados sejam extraídos corretamente tanto do `fast.log` quanto do `eve.json`, dependendo de qual arquivo você está utilizando.
### 1. **Extraindo Alertas do `fast.log`**

O `fast.log` é um arquivo de texto simples que contém alertas gerados pelo Suricata. Para extrair os dados em CSV, você pode usar o comando `awk` que você mencionou, mas ajustado para incluir mais campos:

```bash
awk '{print $1","$2","$3","$4","$5","$6","$7","$8","$9","$10","$11","$12","$13}' /var/log/suricata/fast.log > alertas.csv
```

No entanto, o `fast.log` geralmente não contém todos os campos que você deseja (como `flow.bytes_toserver`, `flow.bytes_toclient`, etc.). Se você precisar desses campos, é melhor usar o `eve.json`.

---

### 2. **Extraindo Alertas do `eve.json`**

O `eve.json` é um arquivo JSON que contém informações detalhadas sobre os eventos capturados pelo Suricata, incluindo os campos que você mencionou. Para extrair esses dados em CSV, você pode usar o comando `jq`, que é uma ferramenta poderosa para processar JSON.

#### Instale o `jq` (se ainda não estiver instalado):

```bash
sudo apt update
sudo apt install jq
```

#### Extraia os campos desejados do `eve.json`:

```bash
jq -r 'select(.event_type == "alert") | [.timestamp, .src_ip, .dest_ip, .src_port, .dest_port, .proto, .event_type, .alert.signature, .alert.category, .flow.bytes_toserver, .flow.bytes_toclient, .flow.pkts_toserver, .flow.pkts_toclient] | @csv' /var/log/suricata/eve.json > alertas.csv
```

#### Explicação do comando:
- `select(.event_type == "alert")`: Filtra apenas os eventos do tipo "alert".
- `[.timestamp, .src_ip, ...]`: Seleciona os campos que você deseja extrair.
- `@csv`: Formata a saída como CSV.
- `/var/log/suricata/eve.json`: Caminho do arquivo de log do Suricata.
- `> alertas.csv`: Redireciona a saída para um arquivo CSV.

---

### 3. **Abrindo o CSV no Excel**

Depois de gerar o arquivo `alertas.csv`, você pode abri-lo no Excel ou em qualquer outro software de planilhas. Se estiver usando o Linux, você pode usar o LibreOffice Calc:

```bash
libreoffice --calc alertas.csv
```

Ou, se preferir, pode transferir o arquivo para um sistema Windows e abri-lo no Excel.

---

### 4. **Exemplo de Saída do CSV**

O arquivo `alertas.csv` terá o seguinte formato:

```
"2023-10-01T12:34:56.789Z","192.168.1.1","192.168.1.2","12345","80","TCP","alert","ET MALWARE Suspicious User-Agent","MALWARE","1024","512","10","5"
"2023-10-01T12:35:10.123Z","192.168.1.3","192.168.1.4","54321","443","TCP","alert","ET SCAN Potential SSH Scan","SCAN","2048","1024","20","10"
```

Cada linha representa um alerta, com os campos separados por vírgulas.

---

### 5. **Automatizando a Extração**

Se você deseja automatizar a extração dos alertas periodicamente, pode criar um script em Bash:

```bash
#!/bin/bash
# Script para extrair alertas do Suricata e salvar em CSV

LOGFILE="/var/log/suricata/eve.json"
OUTPUT="alertas.csv"

# Extrair alertas
jq -r 'select(.event_type == "alert") | [.timestamp, .src_ip, .dest_ip, .src_port, .dest_port, .proto, .event_type, .alert.signature, .alert.category, .flow.bytes_toserver, .flow.bytes_toclient, .flow.pkts_toserver, .flow.pkts_toclient] | @csv' "$LOGFILE" > "$OUTPUT"

echo "Alertas extraídos e salvos em $OUTPUT"
```

Salve o script como `extrair_alertas.sh`, torne-o executável e execute:

```bash
chmod +x extrair_alertas.sh
./extrair_alertas.sh
```

---

### 6. **Considerações Finais**

- **Verifique o tamanho do `eve.json`**: Se o arquivo for muito grande, o processamento pode demorar. Você pode usar ferramentas como `split` para dividir o arquivo JSON em partes menores antes de processá-lo.
- **Atualize o Suricata**: Certifique-se de que está usando a versão mais recente do Suricata para garantir que todos os campos estejam disponíveis.
- **Proteja os logs**: O arquivo CSV pode conter informações sensíveis. Certifique-se de protegê-lo adequadamente.

Com essas instruções, você conseguirá extrair os alertas do Suricata em CSV de forma eficiente e personalizada!