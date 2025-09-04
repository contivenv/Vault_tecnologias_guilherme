---
tags:
  - exploração_de_vulnerabilidades
  - redes
  - cibersegurança
---
Ótima pergunta, Guilherme 👌.  
Você **não precisa escanear máquina por máquina manualmente**. O **Nmap** (e até o Nikto, em menor escala) tem suporte para **faixas de IPs, ranges e listas**, o que facilita muito no seu caso com **10 máquinas internas (rede 192.168.2.0/24)** atrás do firewall.

---

# 🔹 Como escanear vários IPs de uma vez no Nmap

### 1. **Faixa de IPs sequenciais**

Se seus hosts vão de **192.168.2.3 até 192.168.2.12**:

`nmap 192.168.2.3-12`

👉 Isso vai escanear todos os IPs desse intervalo.

---

### 2. **Rede inteira (sub-rede)**

Se quiser varrer toda a rede **192.168.2.0/24** (256 endereços):

`nmap 192.168.2.0/24`

👉 Bom para identificar **todas as máquinas ativas**, inclusive se aparecer algum dispositivo “fantasma” na rede.

---

### 3. **Lista de IPs em arquivo**

Se quiser rodar em **IPs específicos** (ex.: só os 10 que você listou):

1. Crie um arquivo chamado `alvos.txt`:
    
    `192.168.2.3 192.168.2.4 192.168.2.5 192.168.2.6 192.168.2.7 192.168.2.8 192.168.2.9 192.168.2.10 192.168.2.11 192.168.2.12`
    
2. Execute:
    
```bash
nmap -iL alvos.txt -sV -O -oN resultado_hosts.txt
```
    

👉 Assim você consegue documentar **todas as máquinas de uma vez**.

---

# 🔹 E no Nikto?

O Nikto é mais focado em **servidores web**, então só faz sentido rodar nos hosts que realmente tiverem Apache, Nginx, IIS etc.

Mas também dá para usar lista de IPs:

`nikto -h alvos.txt`

(com `alvos.txt` igual ao do exemplo acima).

---

# 🔹 Automação extra

Se você quiser **automatizar todos os testes** (Nmap, Nikto, etc.) em cada host e já salvar relatórios separados por máquina, dá pra usar um **loop no Bash**:

`for ip in 192.168.2.{3..12}; do     echo ">>> Escaneando $ip"     nmap -sV -O $ip -oN scan_$ip.txt     nikto -h http://$ip -o nikto_$ip.txt done`

👉 Isso cria arquivos `scan_192.168.2.X.txt` e `nikto_192.168.2.X.txt` automaticamente para cada host.

---

⚡ Assim você ganha **escala**, não perde tempo IP por IP, e já organiza os relatórios do **antes e depois** para comparar com o pfBlockerNG ativo e inativo.

---

Quer que eu te monte um **script completo em Bash** (com Nmap + Nikto + Ncat básicos) para rodar de uma vez em todos os IPs da rede e já gerar os relatórios separados por host para usar no TCC?

##### Você disse:

Pode ser, preciso que ele funcione no Kali Linux. Vou usar ele para fazer a auditoria de segurança do firewall

###### O ChatGPT disse:

Perfeito 🔥. Vou montar um **script em Bash pronto para o Kali Linux**, que automatiza os testes principais do **Nmap, Ncat e Nikto** em uma lista de IPs, gerando relatórios separados por host.

Você só precisa criar um arquivo com os IPs das máquinas (ex.: `alvos.txt`) e rodar o script.

---

# 🔹 Script de Auditoria de Firewall (Kali Linux)

Salve como `auditoria.sh` e dê permissão de execução (`chmod +x auditoria.sh`):

```bash
#!/bin/bash
# ==========================================
# Script de Auditoria de Firewall (Kali Linux)
# Autor: Guilherme (para TCC)
# Ferramentas: Nmap, Ncat, Nikto
# ==========================================

# Arquivo com a lista de IPs a serem testados
ALVOS="alvos.txt"

# Pasta de saída para relatórios
RESULTADOS="resultados_$(date +%Y%m%d_%H%M%S)"
mkdir -p "$RESULTADOS"

# Verifica se o arquivo de alvos existe
if [[ ! -f "$ALVOS" ]]; then
    echo "[ERRO] Arquivo $ALVOS não encontrado!"
    exit 1
fi

echo "========================================="
echo " AUDITORIA DE FIREWALL - INÍCIO"
echo " Resultados serão salvos em: $RESULTADOS"
echo "========================================="

# Loop para cada IP listado
for ip in $(cat $ALVOS); do
    echo ""
    echo ">>> Testando host: $ip"
    echo "-----------------------------------------"

    # 1) Escaneamento Nmap
    echo "[*] Executando Nmap em $ip ..."
    nmap -sS -sV -O -p- $ip -oN "$RESULTADOS/nmap_$ip.txt"

    # 2) Teste de comunicação (Ncat nas portas 22, 80 e 443)
    echo "[*] Testando conectividade com Ncat em $ip ..."
    for porta in 22 80 443; do
        echo "Testando porta $porta em $ip ..." >> "$RESULTADOS/ncat_$ip.txt"
        timeout 5 ncat -vz $ip $porta &>> "$RESULTADOS/ncat_$ip.txt"
    done

    # 3) Varredura Nikto (se serviço web responder)
    echo "[*] Rodando Nikto em $ip ..."
    nikto -h http://$ip -o "$RESULTADOS/nikto_$ip.txt"

    echo ">>> Testes concluídos para $ip."
    echo "-----------------------------------------"
done

echo "========================================="
echo " AUDITORIA DE FIREWALL - FINALIZADA"
echo " Resultados armazenados em: $RESULTADOS"
echo "========================================="
```

---

# 🔹 Como usar

1. Crie o arquivo `alvos.txt` com os IPs da sua rede, um por linha:
    
    `192.168.2.3 192.168.2.4 192.168.2.5 192.168.2.6 192.168.2.7 192.168.2.8 192.168.2.9 192.168.2.10 192.168.2.11 192.168.2.12`
    
2. Torne o script executável:
    
```bash
    chmod +x auditoria.sh
```
    
3. Execute o script:
    
    `./auditoria.sh`
    
4. Os resultados ficarão organizados em uma pasta com timestamp, por exemplo:
    
```bash
resultados_20250824/
├── nmap_192.168.2.3.txt
├── ncat_192.168.2.3.txt
├── nikto_192.168.2.3.txt
├── nmap_192.168.2.4.txt
├── ...
```
    

---

👉 Dessa forma, você pode rodar o script **antes e depois de ativar o pfBlockerNG**, e terá os relatórios prontos para comparar no seu TCC.

Quer que eu adicione também um trecho para capturar pacotes **com tcpdump** durante os testes (gerando arquivos `.pcap` para análise no Wireshark)?

##### Você disse:

Pode ser, seria perfeito

###### O ChatGPT disse:

Show, Guilherme 🚀. Vamos então deixar o script **completo**, incluindo a captura de tráfego com **tcpdump**, para você depois analisar no **Wireshark**. Isso vai enriquecer muito seu TCC, porque você terá **evidências visuais dos pacotes bloqueados/liberados** antes e depois do firewall com pfBlockerNG.

---

# 🔹 Script de Auditoria Completo (com tcpdump)

Salve como `auditoria.sh`:

```bash
#!/bin/bash
# ==========================================
# Script de Auditoria de Firewall (Kali Linux)
# Autor: Guilherme Conti Teixeira (para TCC)
# Ferramentas: Nmap, Ncat, Nikto, tcpdump
# ==========================================

# Arquivo com a lista de IPs a serem testados
ALVOS="alvos.txt"

# Interface de rede (ajuste se necessário: eth0, wlan0, ens33, etc.)
IFACE="eth0"

# Pasta de saída para relatórios
RESULTADOS="resultados_$(date +%Y%m%d_%H%M%S)"
mkdir -p "$RESULTADOS"

# Verifica se o arquivo de alvos existe
if [[ ! -f "$ALVOS" ]]; then
    echo "[ERRO] Arquivo $ALVOS não encontrado!"
    exit 1
fi

echo "========================================="
echo " AUDITORIA DE FIREWALL - INÍCIO"
echo " Resultados serão salvos em: $RESULTADOS"
echo " Interface de captura: $IFACE"
echo "========================================="

# Inicia captura tcpdump em background
echo "[*] Iniciando captura de pacotes com tcpdump ..."
sudo tcpdump -i $IFACE -w "$RESULTADOS/captura.pcap" &
PID_TCPDUMP=$!

# Loop para cada IP listado
for ip in $(cat $ALVOS); do
    echo ""
    echo ">>> Testando host: $ip"
    echo "-----------------------------------------"

    # 1) Escaneamento Nmap
    echo "[*] Executando Nmap em $ip ..."
    nmap -sS -sV -O -p- $ip -oN "$RESULTADOS/nmap_$ip.txt"

    # 2) Teste de comunicação (Ncat nas portas 22, 80 e 443)
    echo "[*] Testando conectividade com Ncat em $ip ..."
    for porta in 22 80 443; do
        echo "Testando porta $porta em $ip ..." >> "$RESULTADOS/ncat_$ip.txt"
        timeout 5 ncat -vz $ip $porta &>> "$RESULTADOS/ncat_$ip.txt"
    done

    # 3) Varredura Nikto (se serviço web responder)
    echo "[*] Rodando Nikto em $ip ..."
    nikto -h http://$ip -o "$RESULTADOS/nikto_$ip.txt"

    echo ">>> Testes concluídos para $ip."
    echo "-----------------------------------------"
done

# Finaliza captura tcpdump
echo "[*] Finalizando captura de pacotes ..."
sudo kill $PID_TCPDUMP

echo "========================================="
echo " AUDITORIA DE FIREWALL - FINALIZADA"
echo " Resultados armazenados em: $RESULTADOS"
echo "========================================="
```

---

# 🔹 Como usar

1. Crie o arquivo `alvos.txt` com os IPs internos da rede:
    
    `192.168.2.3 192.168.2.4 192.168.2.5 ... 192.168.2.12`
    
2. Dê permissão de execução:
    
    `chmod +x auditoria.sh`
    
3. Ajuste a interface de rede (`IFACE="eth0"`) no script.  
    Para descobrir a sua:
    
    `ip a`
    
4. Execute o script:
    
    `./auditoria.sh`
    
5. Após rodar, você terá:
    
    `resultados_20250824/ ├── captura.pcap          # tráfego capturado (abrir no Wireshark) ├── nmap_192.168.2.3.txt ├── ncat_192.168.2.3.txt ├── nikto_192.168.2.3.txt ├── ...`
    

---

# 🔹 Como usar no Wireshark

1. Abra o arquivo `captura.pcap`.
    
2. Use filtros para analisar pacotes por host ou protocolo. Exemplos:
    
    - Mostrar só tráfego para um IP:
        
        `ip.addr == 192.168.2.3`
        
    - Mostrar só portas abertas/fechadas em TCP:
        
        `tcp.flags.reset == 1`
        
    - Mostrar apenas tráfego HTTP:
        
        `http`
        
3. Compare as capturas **antes e depois do pfBlockerNG** para mostrar no TCC:
    
    - Antes: tráfego HTTP, FTP, ICMP, etc. aparecem.
        
    - Depois: só tráfego permitido (ex.: HTTPS).
        

---

👉 Com esse script você cobre **as 3 camadas de auditoria**:

- **Portas e serviços** (Nmap).
    
- **Conectividade** (Ncat).
    
- **Vulnerabilidades Web** (Nikto).
    
- **Evidência de pacotes** (tcpdump/Wireshark).
    

---

Quer que eu já prepare também um **modelo de tabela comparativa** (antes/depois) em Markdown ou Word, para você colar direto no relatório do TCC junto com os resultados?

##### Você disse:

Faça isso, por favor em Markdown

###### O ChatGPT disse:

Perfeito, Guilherme 👌. Aqui está um **modelo de tabela comparativa em Markdown**, já no formato que você pode colar no seu relatório/TCC ou até converter para Word/LaTeX depois.

---

# 🔹 Modelo de Tabela Comparativa (Antes/Depois do Firewall)

# Comparativo de Resultados de Auditoria de Firewall  ## Testes de Segurança  | Ferramenta   | Métrica                         | Antes do pfSense/pfBlockerNG | Depois do pfSense/pfBlockerNG | |--------------|---------------------------------|------------------------------|-------------------------------| | **Nmap**     | Portas abertas detectadas       | 12 (22, 80, 139, 445, 3306) | 1 (443)                      | |              | Sistema operacional identificado | Sim                          | Não                          | | **Ncat**     | Conexão em porta 22 (SSH)       | Conseguiu conectar           | Conexão recusada             | |              | Conexão em porta 80 (HTTP)      | Conseguiu conectar           | Bloqueado                    | |              | Conexão em porta 443 (HTTPS)    | Conseguiu conectar           | Conseguiu conectar           | | **Nikto**    | Vulnerabilidades encontradas    | 5 falhas em HTTP server      | Nenhuma vulnerabilidade      | | **Metasploit** | Exploit contra serviço SMB     | Execução bem-sucedida        | Bloqueado pelo firewall      | | **Wireshark** | Tráfego ICMP (ping)             | Visível e respondido         | Bloqueado                    | |              | Tráfego HTTP/FTP                | Visível                      | Bloqueado                    | |              | Tráfego HTTPS                   | Visível                      | Visível                      |  ---  ## Evidências  - **Relatórios Nmap**: armazenados em `nmap_*.txt`   - **Relatórios Ncat**: armazenados em `ncat_*.txt`   - **Relatórios Nikto**: armazenados em `nikto_*.txt`   - **Captura de tráfego (tcpdump/Wireshark)**: arquivo `captura.pcap`    ---  ## Conclusão  Após a ativação do **pfSense com pfBlockerNG**, observou-se:  - Redução significativa no número de portas abertas.   - Bloqueio de conexões diretas via Ncat.   - Ausência de vulnerabilidades detectadas pelo Nikto.   - Exploits do Metasploit não foram mais possíveis.   - Tráfego restrito apenas ao protocolo HTTPS, garantindo maior segurança.

---

👉 Você pode adaptar os números (portas, vulnerabilidades, exploits) conforme os resultados reais que coletar nos testes com seu script.

Quer que eu já te prepare também um **modelo de relatório final** em Markdown (com Introdução → Metodologia → Resultados → Conclusão), pronto para colar como anexo no seu TCC?

##### Você disse:

Pode ser

###### O ChatGPT disse:

Maravilha, Guilherme 👌. Aqui vai um **modelo de relatório em Markdown**, estruturado no estilo de anexo para o seu TCC, com seções prontas para você só colar os resultados (prints, tabelas, relatórios).

---

# 🔹 Modelo de Relatório de Auditoria de Firewall (Markdown)

``# Relatório de Auditoria de Segurança de Rede ## Anexo Técnico – Projeto de TCC  ---  ## 1. Introdução  Este relatório apresenta os testes de auditoria de segurança realizados na rede corporativa da empresa, com o objetivo de avaliar a eficácia do firewall **pfSense** em conjunto com o **pfBlockerNG**.    Os testes foram conduzidos em duas etapas distintas: - **Antes da configuração de segurança do firewall** (pfBlockerNG desativado). - **Após a configuração do firewall** (pfBlockerNG ativado).  ---  ## 2. Metodologia  As ferramentas utilizadas foram: - **Nmap** – varredura de portas e identificação de serviços. - **Ncat** – teste de conectividade com portas específicas. - **Nikto** – auditoria de servidores web. - **Metasploit** – simulação de exploração de vulnerabilidades. - **tcpdump/Wireshark** – captura e análise de pacotes.  Rede utilizada nos testes: - Endereço WAN público: `189.1.176.255` - Faixa interna auditada: `192.168.2.3 – 192.168.2.12` - Total de hosts testados: **10 máquinas**  Procedimento: 1. Execução de varredura inicial com pfBlockerNG desativado. 2. Coleta e registro dos resultados (Nmap, Ncat, Nikto, Metasploit, Wireshark). 3. Configuração e ativação do pfBlockerNG no pfSense. 4. Reexecução dos mesmos testes. 5. Comparação entre os dois cenários.  ---  ## 3. Resultados Obtidos  ### 3.1 Comparativo Geral  | Ferramenta   | Métrica                         | Antes do pfSense/pfBlockerNG | Depois do pfSense/pfBlockerNG | |--------------|---------------------------------|------------------------------|-------------------------------| | **Nmap**     | Portas abertas detectadas       | 12 (22, 80, 139, 445, 3306) | 1 (443)                      | |              | Sistema operacional identificado | Sim                          | Não                          | | **Ncat**     | Conexão em porta 22 (SSH)       | Conseguiu conectar           | Conexão recusada             | |              | Conexão em porta 80 (HTTP)      | Conseguiu conectar           | Bloqueado                    | |              | Conexão em porta 443 (HTTPS)    | Conseguiu conectar           | Conseguiu conectar           | | **Nikto**    | Vulnerabilidades encontradas    | 5 falhas em HTTP server      | Nenhuma vulnerabilidade      | | **Metasploit** | Exploit contra serviço SMB     | Execução bem-sucedida        | Bloqueado pelo firewall      | | **Wireshark** | Tráfego ICMP (ping)             | Visível e respondido         | Bloqueado                    | |              | Tráfego HTTP/FTP                | Visível                      | Bloqueado                    | |              | Tráfego HTTPS                   | Visível                      | Visível                      |  ---  ### 3.2 Evidências  - **Relatórios Nmap**: armazenados em `nmap_*.txt`   - **Relatórios Ncat**: armazenados em `ncat_*.txt`   - **Relatórios Nikto**: armazenados em `nikto_*.txt`   - **Exploração com Metasploit**: `metasploit_logs.txt`   - **Captura de pacotes (tcpdump/Wireshark)**: `captura.pcap`    > **Observação**: Os arquivos de evidências estão anexados digitalmente como parte deste trabalho.  ---  ## 4. Análise dos Resultados  A comparação entre os cenários demonstrou:  - **Redução drástica no número de portas abertas** após o uso do pfBlockerNG.   - **Serviços inseguros como SMB e FTP foram bloqueados**.   - **Tentativas de exploração** com Metasploit não foram mais possíveis.   - **O tráfego visível no Wireshark** restringiu-se apenas ao protocolo HTTPS (porta 443).   - Houve **eliminação de vulnerabilidades web** detectadas pelo Nikto.    ---  ## 5. Conclusão  Os testes confirmam que a implementação do **pfSense com pfBlockerNG** aumentou significativamente o nível de segurança da rede.    A comparação entre os cenários **antes e depois** evidencia: - Mitigação de riscos de invasão.   - Restrição de tráfego apenas a serviços autorizados.   - Redução de exposição a ataques externos.    Este relatório valida a eficácia do firewall como medida de segurança para pequenas e médias empresas, atendendo tanto requisitos técnicos quanto conformidade com a **LGPD**.  ---``

---

👉 Esse modelo você só precisa **preencher com os resultados reais** dos testes que o script gerar (quantidade de portas abertas, exploits que passaram/bloquearam, etc.).

Quer que eu já prepare também uma **versão em Word (.docx)** desse relatório (com tabelas formatadas), para você usar como **anexo pronto no TCC**?

##### Você disse:

Pode ser

###### O ChatGPT disse:

Pronto ✅. Gereei um **relatório em Word (.docx)** com toda a estrutura organizada e já com uma tabela comparativa exemplo.

📂 Você pode baixar aqui: relatorio_auditoria_firewall.docx

Quer que eu prepare também uma versão em **PDF formatada** (com cabeçalhos bonitos, fonte padronizada) para você anexar direto no TCC?

##### Você disse:

Teria como fazer tudo isso, mas usando o metasploit ? Ficou faltando só ele.

###### O ChatGPT disse:

Boa, Guilherme 👌. Excelente ponto! O **Metasploit** funciona um pouco diferente de Nmap/Nikto, porque ele não é só um scanner, mas uma **plataforma de exploração**.  
Para o seu TCC, o ideal é usar o Metasploit **como auditor de segurança controlado** — ou seja, só até a fase de **verificar se a vulnerabilidade existe**, sem avançar para exploração destrutiva.

---

# 🔹 Como integrar o Metasploit à sua auditoria

## 1. Gerar lista de IPs e serviços vulneráveis

O Metasploit pode trabalhar em conjunto com resultados do Nmap:

`nmap -sV -oX nmap_resultados.xml 192.168.2.3-12`

Depois importar para o Metasploit:

`msfconsole db_import nmap_resultados.xml hosts services`

👉 Isso lista os hosts e serviços que podem ser testados no Metasploit.

---

## 2. Rodar scanner de vulnerabilidades do Metasploit

Exemplo: procurar vulnerabilidades SMB (porta 445) em todos os hosts:

`use auxiliary/scanner/smb/smb_version set RHOSTS 192.168.2.3-12 run`

Exemplo: varredura HTTP (porta 80):

`use auxiliary/scanner/http/http_version set RHOSTS 192.168.2.3-12 run`

👉 Esses módulos **não exploram**, apenas identificam vulnerabilidades.

---

## 3. Testar exploit de forma controlada

Exemplo: verificar se o alvo é vulnerável a um exploit conhecido (sem executar payloads perigosos):

`use exploit/windows/smb/ms17_010_eternalblue set RHOSTS 192.168.2.3-12 check`

👉 O comando `check` diz se o host **é vulnerável ou não**, sem rodar o exploit.  
Isso é perfeito para relatórios, porque você demonstra o risco **sem invadir**.

---

## 4. Exportar resultados

No Metasploit, você pode gerar relatórios:

`db_export -f xml /tmp/metasploit_resultados.xml db_export -f txt /tmp/metasploit_resultados.txt`

Esses arquivos podem ser anexados no relatório.

---

# 🔹 Integração no Script (automação com `msfconsole -r`)

Você pode criar scripts `.rc` (resource scripts) para o Metasploit rodar automaticamente.

Exemplo `auditoria.rc`:

`db_connect msf:msf@127.0.0.1/msf db_import nmap_resultados.xml use auxiliary/scanner/smb/smb_version set RHOSTS 192.168.2.3-12 run use exploit/windows/smb/ms17_010_eternalblue set RHOSTS 192.168.2.3-12 check exit`

Rodar com:

`msfconsole -r auditoria.rc`

👉 Isso gera um relatório rápido de serviços SMB e verifica se o **EternalBlue** está presente.

---

# 🔹 Como documentar no TCC

Na sua tabela, pode ficar assim:

|Ferramenta|Métrica|Antes do pfSense/pfBlockerNG|Depois do pfSense/pfBlockerNG|
|---|---|---|---|
|**Metasploit**|Scanner SMB (porta 445)|Detectou serviço SMB ativo|Serviço bloqueado|
|**Metasploit**|Vulnerabilidade MS17-010 (EternalBlue)|Host vulnerável (check positivo)|Nenhum host vulnerável (acesso bloqueado)|

---

👉 Assim você fecha o ciclo:

- **Nmap/Ncat/Nikto** = reconhecimento.
    
- **tcpdump/Wireshark** = evidência de tráfego.
    
- **Metasploit** = validação de vulnerabilidades.
    

---

Quer que eu te prepare um **script `.rc` completo para Metasploit** (com SMB, HTTP e alguns checks seguros) para rodar junto com os outros testes da auditoria?