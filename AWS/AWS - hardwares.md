---
tags:
- aws
- ec2
- lightsail
- virtualização
- s3
- route_53
- cloudfront
- vpc  
data criação: 2025-08-26T19:35:00
---
Podemos escolher o tipo de máquina que vamos usar, isso que deu o nome pra AWS (Amazon **Elastic Compute Cloud**). Por ele conseguimos ter uma previsão de preços nas instâncias.

##### Tipos de instâncias

- **Propósito geral** → balanceiam memória, CPU e armazenamento. Ex: aplicações web, servidores pequenos.
    
- **Otimização de computação** → voltadas para processamento intenso. Ex: análise de dados, jogos, servidores de aplicação.
    
- **Otimização de memória** → grandes bancos de dados, cache, aplicações in-memory.
    
- **Armazenamento otimizado** → alta taxa de leitura/escrita em disco, processamento de Big Data.
    
- **Instâncias aceleradas por GPU** → Machine Learning, renderização 3D, aplicações gráficas pesadas.
    

##### EC2 vs Lightsail

- **Amazon EC2 (Elastic Compute Cloud)**
    
    - Voltado para projetos robustos, escaláveis e que precisam de personalização avançada.
        
    - Suporta máquinas extremamente potentes (vários CPUs, GPUs, terabytes de RAM).
        
    - Ideal para grandes empresas, aplicações críticas, e quando há necessidade de controle total da infraestrutura.
        
- **Amazon Lightsail**
    
    - Voltado para projetos simples (hospedagem de sites, pequenos aplicativos, blogs).
        
    - Possui planos pré-definidos de preço → facilita o custo previsível.
        
    - Mais amigável para iniciantes.
        
    - Já vem integrado com DNS, rede e armazenamento simplificado.
        

**Zona de Disponibilidade (AZ)** → cada região tem múltiplas zonas de disponibilidade, que são datacenters independentes. No Lightsail, muita coisa já é abstraída e configurada automaticamente.

**Latência** → utilizar regiões próximas (como **São Paulo - `sa-east-1`**) garante menor tempo de resposta.

##### Amazon CloudFront

- Rede de distribuição de conteúdo (CDN).
    
- Replica e distribui conteúdo em servidores espalhados pelo mundo.
    
- Diminui latência para usuários de diferentes regiões.
    
- Usado para vídeos, imagens, sites globais e aplicações de alto tráfego.
    

##### Amazon S3 - Solução de armazenamento da AWS

1. Armazenamento praticamente "infinito". O usuário paga apenas pelo espaço utilizado e tráfego de saída.
    
2. Alta durabilidade (99,999999999% – "11 noves"), garantindo que os dados não se percam.
    
3. Versionamento → histórico completo de arquivos, útil para auditoria e recuperação de versões antigas.
    
4. Classes de armazenamento:
    
    - **Standard** → acesso frequente.
        
    - **Infrequent Access (IA)** → dados acessados poucas vezes, custo menor.
        
    - **Glacier / Deep Archive** → backup de longo prazo, acesso em horas.
        
5. Segurança → controle via políticas IAM e criptografia (em repouso e em trânsito).
    

##### Amazon Route 53

- Serviço de DNS gerenciado pela AWS.
    
- Funções principais:
    
    - Registro de domínios.
        
    - Resolução de nomes para IPs de instâncias, serviços ou sites.
        
    - Gerenciamento de rotas com políticas inteligentes (latência, failover, geolocalização).
        
- Alta disponibilidade e integração com outros serviços da AWS.
    

##### Extra: Amazon VPC (Virtual Private Cloud)

- Permite criar uma **rede virtual privada** dentro da AWS.
    
- Dá controle total sobre endereçamento IP, sub-redes, tabelas de roteamento e gateways.
    
- É fundamental para isolar e proteger recursos (EC2, RDS, etc).
    

---