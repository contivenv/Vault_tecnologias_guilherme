---
title: EternalBlue
aliases: [MS17-010, CVE-2017-0144]
tags: [segurança, exploit, SMB, vulnerabilidade]
cve: CVE-2017-0144
patch: MS17-010
---

# EternalBlue

## Resumo
Exploit de **execução remota de código** contra o **SMBv1** do Windows, desenvolvido pela **NSA** e vazado pelo [[Shadow Brokers]] em **abril de 2017**.

## Detalhes técnicos
- **Vulnerabilidade:** falha de corrupção de memória no tratamento de requisições SMBv1 (driver `srv.sys`), relacionada ao processamento de transações com tamanhos inconsistentes.
- **Porta:** 445/TCP
- **Autenticação:** não requer
- **Impacto:** execução de código com privilégios **SYSTEM**
- **Patch:** boletim **MS17-010**, publicado em **14/03/2017**
- **Afetados:** Windows Vista, 7, 8.1, 10 (versões antigas), Server 2008 a 2016, e legados como XP/Server 2003 (patch emergencial depois do WannaCry)

## Uso em ataques
- [[WannaCry]] (maio/2017): propagação como worm
- [[NotPetya]] (junho/2017): um dos vetores de movimento lateral
- Usado depois por vários outros malwares (mineradores, trojans bancários, botnets)
- Normalmente combinado com o implante [[DoublePulsar]]

## Detecção
- Scan de 445/TCP em massa na rede
- Regras de IDS/IPS (Snort/Suricata) para o MS17-010
- Script Nmap: `nmap -p445 --script smb-vuln-ms17-010 <alvo>`
- Verificar o SMBv1: `Get-SmbServerConfiguration | Select EnableSMB1Protocol`

## Mitigação
- [ ] Aplicar o MS17-010
- [ ] Desabilitar SMBv1
- [ ] Bloquear 445/TCP no perímetro
- [ ] Segmentar a rede interna

## Relacionado
[[DoublePulsar]] · [[Shadow Brokers]] · [[WannaCry]] · [[NotPetya]]
