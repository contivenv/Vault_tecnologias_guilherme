---
tags:
  - firewall
  - cibersegurança
  - hardware
---
É um sistema de filtragem de acesso baseado em regras. Existem dois tipos de firewalls: os que são baseados em hardware e os que são baseados em software, assim como o PfSense. O que vamos abordar aqui é o de hardware.

Irão ser criados regras para acessos não autorizados. 

```ad-success

As regras são criadas dentro do firewall -> firewall analise o pacote de dados -> tem algo suspeito no pacote de dados que ativa o gatilho da regra para bloqueio ? -> não -> pacote passa
```
```ad-fail

As regras são criadas dentro do firewall -> firewall analise o pacote de dados -> tem algo suspeito no pacote de dados que ativa o gatilho da regra para bloqueio ? -> sim -> pacote bloqueado
```
**Observação:** só que temos um problema em tudo isso. Se o atacante da rede mascarar de alguma forma o pacote de dados que o firewall analisa sem ativar nenhuma regra de bloqueio, ele passa despercebido pelo firewall.

Alguns dos recursos interessantes dele são:

- desativar o acesso externo da sua rede local em alguns dispositivos pelo MAC ou pela rede dependendo do dia e horário.

- Outro recurso seria o "principal" dele que é bloquear devidos sites que são de acesso externo como por exemplo, rede sociais, sites de pornografia, sites diversos, etc.

- Podem ser feitos bloqueios por palavras chaves também como por exemplo para filtrar conteúdos adultos e palavras remetentes a sites que não podem ser acessados.