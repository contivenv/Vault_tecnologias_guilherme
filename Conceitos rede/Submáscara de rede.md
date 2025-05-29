---
tags:
  - redes
  - IP
---
A **máscara de rede** define quais partes do endereço IP pertencem à **rede** e quais pertencem ao **host** (dispositivo dentro da rede). Ela é essencial para determinar o **intervalo de endereços IP** dentro de uma rede e como os dispositivos se comunicam.

### 📌 Como Funciona?

Um endereço IP é dividido em duas partes:

- **Parte da Rede**: Identifica a rede à qual o IP pertence.
    
- **Parte do Host**: Identifica um dispositivo específico dentro dessa rede.
    

A máscara de rede ajuda a separar essas partes.

### 🎯 Exemplos de Máscaras de Rede:

| Máscara de Rede | Notação CIDR | Hosts Suportados |
| --------------- | ------------ | ---------------- |
| 255.0.0.0       | /8           | 16.777.214       |
| 255.255.0.0     | /16          | 65.534           |
| 255.255.255.0   | /24          | 254              |
| 255.255.255.128 | /25          | 126              |
| 255.255.255.192 | /26          | 62               |

💡 **Exemplo prático:**  
Se você tem o IP **192.168.1.10** com a máscara **255.255.255.0**, significa que:

- A **rede** é **192.168.1.0**.
    
- Os **hosts válidos** vão de **192.168.1.1 a 192.168.1.254**.
    
- O **broadcast** (último endereço usado para enviar mensagens para todos na rede) é **192.168.1.255**.
    

No seu caso, no **pfSense**, a interface LAN está usando **192.168.1.1/24**, o que indica que:

- A rede cobre de **192.168.1.0 a 192.168.1.255**.
    
- Até **254 dispositivos** podem estar conectados.