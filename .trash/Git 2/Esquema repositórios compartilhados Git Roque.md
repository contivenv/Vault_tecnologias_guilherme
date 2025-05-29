---
tags:
  - git
---

Sistemas Centralizados de Controle de Versão (CVCSs) são uma abordagem clássica para gerenciar versões de código, onde um único repositório centralizado armazena todas as versões de um projeto. O modelo que você está considerando é bastante comum em ambientes corporativos, especialmente quando há restrições de segurança ou políticas que impedem o uso de soluções baseadas na nuvem. Vamos detalhar como funcionam os CVCSs e como você pode implementá-los em sua rede local.

![[image.png]]

### Conceitos Básicos do CVCS

1. **Repositório Central**: Um único repositório em um servidor que contém todo o histórico do projeto. Os desenvolvedores fazem check-out (cópias locais) e check-in (atualizações) nesse repositório.
2. **Cópias Locais**: Cada desenvolvedor mantém uma cópia local do projeto, onde podem fazer alterações. Essas alterações são depois enviadas de volta ao repositório central.
3. **Controle de Versão**: O sistema mantém o histórico das alterações, permitindo reverter a versões anteriores, ver quem fez o que e quando, e gerenciar conflitos quando duas pessoas fazem alterações nos mesmos arquivos.

```ad-success
title: Vantagens

- **Simplicidade**: O modelo é mais simples para entender e usar, especialmente para equipes pequenas ou médias.
- **Gerenciamento de Conflitos**: O sistema pode ajudar a gerenciar conflitos quando múltiplos desenvolvedores estão trabalhando em arquivos compartilhados.
- **Centralização**: O código está em um local único, facilitando o acesso e a manutenção.
```
```ad-failure
title: Desvantagens

- **Ponto Único de Falha**: Se o servidor central falhar, todos os desenvolvedores ficam sem acesso ao código.
- **Menos Flexibilidade**: Ao contrário dos sistemas distribuídos como o Git, não é possível trabalhar em branches locais sem enviar as alterações ao repositório central.
```
## Implementando Git no Servidor "hefestoroqueimoveis"
Tudo sobre as configurações realizadas em nosso servidor vão estar detalhadas no artigo [[Configuração e permissão repositório Git hefesto Roque]]. Acesse para ter mais informações de como funciona.

##### 5. Fluxo de Trabalho com Git:
- Os desenvolvedores trabalharão em suas cópias locais e usarão comandos do Git para gerenciar o código.
**Etapas do fluxo de trabalho**:
- **Fazer alterações**: Editar arquivos localmente.
- **Adicionar arquivos ao índice**:
```bash
git add arquivo_modificado.txt
 ```
- **Commit das alterações**:
```bash
 git commit -m "Descrição das alterações"
  ```
- **Push para o repositório central**:
```bash
git push origin main
```
- **Puxar alterações de outros desenvolvedores**:
```bash
git pull origin main
 ```
##### 6. Backup e Manutenção:
- Implemente um sistema de backup para o repositório bare. Isso pode incluir backups regulares do servidor.
- Monitore o servidor para garantir que o repositório esteja sempre acessível.
### Exemplo de Comandos Git
```bash
git clone ssh://usuario@192.168.2.94:/caminho/para/repositorio.git
 ```
- **Adicionar e fazer commit**:
```bash
git add .
git commit -m "Adicionando nova funcionalidade"
```
- **Enviar alterações para o repositório**:
```bash
 git push origin main
 ```
- **Receber alterações de outros desenvolvedores**:
```bash
git pull origin main
```