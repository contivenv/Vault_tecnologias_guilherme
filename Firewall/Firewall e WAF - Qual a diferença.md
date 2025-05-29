---
tags:
  - firewall
  - WAF
  - redes
  - infraestrutura
---
🔹 Firewall e WAF: Qual a diferença? 🔹  
  
Quando falamos de cibersegurança, firewall e WAF (Web Application Firewall) são termos comuns, mas possuem propósitos diferentes.  
  
✅ Firewall tradicional → Atua na camada de rede. Filtra o tráfego com base em IPs, portas e protocolos, protegendo servidores e dispositivos contra acessos não autorizados.  
  
🔹 Bloqueia o tráfego com base em endereços IP e portas de serviço.  
  
✅ WAF (Web Application Firewall) → Atua na camada de aplicação. Protege sites e APIs contra ataques como SQL Injection, XSS e CSRF. Seus bloqueios são baseados na análise de requisições HTTP/HTTPS suspeitas.  
  
🔹 Exemplos de bloqueios realizados pelo WAF:  
- Entradas suspeitas em formulários, prevenindo ataques de injeção (SQLi, XSS).  
- Arquivos maliciosos enviados por upload, evitando payloads maliciosos.  
- Tentativas de exploração na URL, como RFI (Remote File Inclusion).  
  
📌 Resumo: O firewall protege a infraestrutura, enquanto o WAF protege aplicações web. O ideal? Usar ambos para uma defesa com mais camadas de proteção!