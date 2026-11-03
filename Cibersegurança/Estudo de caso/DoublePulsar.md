---
title: DoublePulsar
aliases: [Double Pulsar]
tags: [segurança, backdoor, implante, NSA]
---

# DoublePulsar

## Resumo
Implante (backdoor) em modo kernel, de origem **NSA**, vazado pelo [[Shadow Brokers]] em abril de 2017. Roda na memória do sistema e permite **carregar e executar código arbitrário** na máquina comprometida.

## Funcionamento
- Normalmente instalado **após** a exploração via [[EternalBlue]].
- Fica residente na memória, interceptando comandos SMB especiais.
- Aceita comandos para executar payloads (DLLs/shellcode), sem gravar arquivos em disco.
- Existe também uma variante para **RDP**.
- **Não persiste após reinicialização**, mas a máquina pode ser reinfectada se continuar vulnerável.

## Papel no [[WannaCry]]
O WannaCry verificava se a máquina-alvo já tinha o DoublePulsar e, se sim, o usava para injetar o payload. Caso contrário, instalava o implante usando o EternalBlue.

## Detecção
- Scan SMB que identifica a resposta anômala do implante (ex.: scripts Nmap `smb-double-pulsar-backdoor`)
- Monitoramento de tráfego anômalo em 445/TCP

## Remediação
Reiniciar o sistema remove o implante da memória, **mas só resolve** se o patch MS17-010 for aplicado em seguida.

## Relacionado
[[EternalBlue]] · [[Shadow Brokers]] · [[WannaCry]]
