---
tags:
  - GPO
  - aprendizado
  - bat
  - implementação
---

Utilizei esse vídeo como base [https://www.youtube.com/watch?v=85CdC-R5Bus](https://www.youtube.com/watch?v=85CdC-R5Bus) e também fiz um .bat por conta própria para rodar na politica da GPO:
[.bat](https://www.notion.so/bat-13b0fa7e009980faab78ee742d989711?pvs=21)
### Como fazer ?

1. Criar uma pasta que seja compartilhada, onde todos os usuários tenham acesso na rede.
2. Precisa ser um arquivo .msi
3. Usuários de domínio somente permissão de leitura na pasta
4. Administradores com acesso local total
5. Criar uma politica de grupo, dar nome a ela
6. Editar ela
7. Vá em politicas
8. Modelos administrativos
9. Componentes do Windows
10. Windows Installer
11. Sempre instalar com alto privilégio
12. Dar dois cliques
13. Colocar habilitado
14. Dar Ok
15. Fazer os mesmos passos em usuários, porque isso apenas foi feito nas máquinas
16. Vá em computadores
17. Politicas
18. Configurações de softwares
19. Instalação de software
20. Botão direito
21. Novo
22. Pacote
23. Modelo atribuído para ser feito automaticamente
24. Fazer o mesmo com usuário
25. Fazer com o tipo publicado
26. Não esquecer de colocar essa política no local que deseja ser rodado
27. Rodar o comando no cmd: gpupdate /force