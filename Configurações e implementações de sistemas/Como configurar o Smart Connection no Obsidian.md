---
tags:
  - obsidian
  - aprendizado
  - IA
---
1. Ativar os plugins não oficiais no Obsidian
2. Procurar por "Smart Connection"
3. Instalar e ativar
4. Acessar o repositório no GitHub https://github.com/brianpetro/obsidian-smart-connections/releases/tag/2.1.92

```ad-attention
title: Atenção
Esse tutorial de instalação funciona somente na versão 2.1.92 do plugin, as versões mais recentes por algum motivo que não sabemos ainda, não estão funcionando.

```

5. Após baixar os arquivos do GitHub, substituir os três arquivo que estão no seu Vault do Obsidian pelos que você acabou de baixar. Exemplo:
![[Pasted image 20250110120509.png]]
Passar os arquivos da esquerda para a direita (os que estão na pasta que acabou de baixar para o seu Vault, substituindo-os). 

```ad-info
title: Informativo de exemplo do caminho

O caminho fica C:\Users\guilherme.teixeira\Downloads\smart-connections-2.1.92 para -> Seu_disco:\O_caminho_do_Vault\.obsidian\plugins\smart-connections
```

```ad-attention
title: Atenção
Depois que realizar o precesso, não clique em atulizar no Smart Connection, senão as alterações seram perdidas e a IA não irá funcionar

```

6. Agora vamos acessar o site da https://console.groq.com/ e ir até em "Models" e escolher a versão mais recente da nossa IA. A escolhida por mim foi a llama-3.3-70b-versatile.
7. Copiamos o nome exato dela e vamos nas configurações do plugin do Smart Connection na aba de "Model Platform"
8. Escolher a opção "Custon API (OpenAI format)"
9. E o nome da IA que copiamos, colocamos no espaço destinado a "Model Name"
10. Em "Protocol" vamos colocar "https"
11. Em "Hostname" colocaremos "api.groq.com"
12. No espaço "path" vamos colocar "/openai/v1/chat/completions"
13. No espaço "max input tokens" colocaremos o valor 128
14. E em "API key", vamos precisar voltar para o site da https://console.groq.com/ e navegar até "API keys"
15. Dar um nome a nossa chave
16. Copiar a chave que nos foi dada no site
![[Pasted image 20250110121946.png]]
17. Clique em "save" no espaço da chave
As configurações devidamente corretas vão ficar mais ou menos assim com neste print aqui em baixo:
![[Pasted image 20250110122146.png]]
