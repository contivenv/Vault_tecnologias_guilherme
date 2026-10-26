---
title: Ransomware
aliases: [ransomware, sequestro de dados]
tags: [segurança, malware, ransomware]
---
# Ransomware

## Definição
Malware que **bloqueia o acesso** a sistemas ou **criptografa arquivos** da vítima e exige um pagamento (geralmente em criptomoeda) para a liberação.

## Tipos
- **Crypto-ransomware:** criptografa arquivos (o mais comum).
- **Locker:** bloqueia o acesso ao sistema, sem criptografar os dados.
- **Dupla extorsão:** além de criptografar, **exfiltra** dados e ameaça vazá-los.
- **RaaS (Ransomware-as-a-Service):** desenvolvedores alugam o malware a afiliados e dividem o lucro.

## Ciclo típico de um ataque
1. Acesso inicial (phishing, RDP exposto, VPN vulnerável, exploit)
2. Execução e persistência
3. Escalada de privilégios e movimento lateral
4. Exfiltração de dados (dupla extorsão)
5. Remoção/criptografia dos backups
6. Criptografia em massa e nota de resgate

## Casos históricos
- CryptoLocker (2013)
- [[WannaCry]] (2017), primeiro com propagação tipo worm em escala global
- [[NotPetya]] (2017), na prática um *wiper* disfarçado
- Ryuk, REvil, Conti, LockBit (modelo RaaS)

## Defesa
- Backups offline/imutáveis ([[3-2-1]])
- Patching e redução da superfície de ataque (RDP, SMB, VPN)
- Segmentação de rede e privilégio mínimo
- EDR/MFA e treinamento contra phishing
- Plano de resposta a incidentes testado

## Relacionado
[[WannaCry]] · [[NotPetya]] · [[EternalBlue]]
