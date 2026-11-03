---
title: NotPetya
aliases: [ExPetr, Petya.A, Nyetya]
tags: [segurança, wiper, supply-chain, incidentes]
data-incidente: 2017-06-27
---
# NotPetya

## Resumo
Ataque destrutivo iniciado em **27/06/2017**, que se passava por [[Ransomware]], mas na prática era um **wiper**: a recuperação dos dados era inviável mesmo com o pagamento. Considerado um dos ciberataques mais custosos da história.

## Vetor inicial
- **Ataque à cadeia de suprimentos:** atualização comprometida do **M.E.Doc**, software contábil usado na Ucrânia.
- A infecção inicial atingiu primeiro organizações ucranianas e depois se espalhou globalmente pelas redes corporativas.

## Propagação interna
- [[EternalBlue]] e **EternalRomance** (exploits SMB vazados pelo [[Shadow Brokers]])
- **Mimikatz** para extrair credenciais da memória
- **PsExec** e **WMI** para movimento lateral com credenciais legítimas

Por usar credenciais roubadas, **máquinas com o patch aplicado também foram afetadas** se houvesse reaproveitamento de credenciais administrativas.

## Impacto
- Sobrescrevia o **MBR** e criptografava a MFT; a chave não era recuperável.
- Vítimas: Maersk, Merck, FedEx/TNT, Mondelez, Saint-Gobain, entre outras.
- Prejuízo estimado em cerca de **US$ 10 bilhões** (estimativa do governo dos EUA).
- Atribuído por EUA e Reino Unido (fev/2018) à inteligência militar russa (grupo **Sandworm**).

## Diferenças em relação ao [[WannaCry]]
| | WannaCry | NotPetya |
|---|---|---|
| Objetivo | Dinheiro (resgate) | Destruição / alvo geopolítico |
| Vetor inicial | Scan de 445/TCP na internet | Update do M.E.Doc |
| Movimento lateral | EternalBlue | EternalBlue + EternalRomance + Mimikatz + PsExec/WMI |
| Recuperação | Possível em alguns casos | Praticamente inviável |
| Kill switch | Sim | Não |

## Lições
- [ ] Patch não basta: **proteger credenciais** (LAPS, privilégio mínimo)
- [ ] Segmentar a rede e restringir SMB/PsExec/WMI interno
- [ ] Revisar o risco da cadeia de suprimentos de software
- [ ] Backups offline e **teste de restauração**

## Relacionado
[[WannaCry]] · [[EternalBlue]] · [[Ransomware]] · [[3-2-1]]
