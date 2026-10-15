---
tags: [segurança, ransomware, malware, incidentes, SMB]
data-incidente: 2017-05-12
tipo: ransomware worm
cve: MS17-010
---
## Resumo
Ataque global de [[Ransomware]] iniciado em **12/05/2017**. Combinava a criptografia de arquivos com a propagação automática de um **worm**, explorando uma falha no protocolo **SMBv1** do Windows. Infectou mais de **200 mil máquinas em ~150 países** em poucos dias.

## Como funcionava
1. **Vetor inicial:** varredura da internet por máquinas com a porta **445/TCP** exposta.
2. **Exploração:** [[EternalBlue]] (falha do SMBv1, **MS17-010**, CVE-2017-0144) para execução remota de código.
3. **Persistência:** implante DoublePulsar (backdoor) instalado na máquina comprometida.
4. **Payload:** o ransomware criptografava os arquivos do usuário e exibia a nota de resgate.
5. **Propagação:** cada máquina infectada passava a varrer a rede local e a internet atrás de novos alvos (comportamento de worm).

## Origem das ferramentas
- EternalBlue e DoublePulsar foram desenvolvidos pela **NSA** e vazados pelo grupo **Shadow Brokers** em abril de 2017.
- A Microsoft já havia publicado o patch **MS17-010 em 14/03/2017**, ou seja, dois meses antes do ataque.
- EUA e Reino Unido atribuíram o ataque ao grupo **Lazarus** (Coreia do Norte) em dezembro de 2017.

## Resgate
- Pedido inicial: **US$ 300 em Bitcoin**, subindo para US$ 600 após alguns dias.
- Nota exibida em dezenas de idiomas.
- Arrecadação foi baixa (~US$ 140 mil), pois o pagamento não era vinculado de forma confiável à vítima e muita gente nem sabia pagar.

## Kill switch
- O malware consultava um domínio inexistente e hardcoded; se ele respondesse, o malware encerrava.
- **Marcus Hutchins (MalwareTech)** registrou o domínio e interrompeu a propagação da primeira onda.
- Variantes sem o kill switch surgiram depois.

## Principais vítimas
- **NHS** (Reino Unido): hospitais com cirurgias e consultas canceladas.
- Telefónica, FedEx, Renault, Deutsche Bahn, entre outras.
- Sistemas legados (Windows XP, Server 2003) foram os mais afetados, e a Microsoft liberou um patch emergencial para eles.

## Indicadores úteis (IOCs)
- Extensão dos arquivos criptografados: `.WNCRY`
- Binários: `mssecsvc.exe`, `tasksche.exe`, `@WanaDecryptor@.exe`
- Tráfego anômalo de varredura na porta **445/TCP**

## Mitigação / lições aprendidas
- [ ] Aplicar patches de segurança com prioridade (MS17-010)
- [ ] **Desabilitar SMBv1** (`Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol`)
- [ ] Bloquear 445/TCP no perímetro (firewall) e nunca expor SMB à internet
- [ ] Segmentar a rede (VLANs) para limitar movimento lateral
- [ ] Backups **offline/imutáveis**, seguindo a regra [[3-2-1]]
- [ ] Inventariar e isolar sistemas legados sem suporte
- [ ] Monitorar a rede com IDS/IPS e EDR

## Relacionado
- [[Ransomware]]
- [[EternalBlue]]
- [[Shadow Brokers]]
- [[Lazarus Group]]
- [[NotPetya]] (ataque posterior, de junho de 2017, que usou o mesmo exploit)

## Referências
- Microsoft Security Bulletin MS17-010
- CISA Alert TA17-132A