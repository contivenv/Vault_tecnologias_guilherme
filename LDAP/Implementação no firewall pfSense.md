---
tags:
  - firewall
  - LDAP
  - pfsense
  - validado
---
# 🔧 Tutorial: Configurar LDAP no pfSense com Active Directory

### **Dados fornecidos**

- **Usuário de Bind:** `CN=ldapbind,OU=USUARIOS,DC=empresa,DC=local`
    
- **IP do AD (Domain Controller):** `192.168.3.10`
    
- **Domínio:** `empresa.local`
    

---

## 1️⃣ Criar usuário técnico no AD (Bind User)

- No Active Directory, crie um usuário chamado **ldapbind** dentro da OU `USUARIOS`.
    
- Esse usuário precisa apenas de permissão de **leitura** no AD.
    

---

## 2️⃣ Configuração no pfSense

1. Vá em **System > User Manager > Authentication Servers**.
    
2. Clique em **Add**.
    
3. Preencha os campos:
    

### 🔹 Configurações principais

- **Descriptive name:** `AD_ROQUEIMOVEIS` (pode ser qualquer nome).
    
- **Type:** `LDAP`.
    

### 🔹 Servidor LDAP

- **Hostname or IP address:** `192.168.3.10`
    
- **Port value:** `389` (LDAP normal)
    
    - Se quiser usar LDAPS (seguro), coloque **636** e selecione _SSL/TLS Encrypted_.
        
- **Transport:** `Standard TCP` (ou `SSL/TLS Encrypted` se usar 636).
    
- **Protocol version:** `3`
    
- **Server Timeout:** `25`
    

### 🔹 Base DN

```
DC=empresa,DC=local
```

### 🔹 Authentication containers

```
OU=USUARIOS,DC=empresa,DC=local
```

### 🔹 Bind credentials

- **Bind DN:**
    
    ```
    CN=ldapbind,OU=USUARIOS,DC=empresa,DC=local
    ```
    
- **Password:** (senha do usuário `ldapbind`)
    

### 🔹 Atributos

- **User naming attribute:** `sAMAccountName`
    
- **Group naming attribute:** `cn`
    
- **Group member attribute:** `member`
    

### 🔹 RFC 2307 Groups

- **Desmarcado** (não é necessário para Active Directory).
    

---

## 3️⃣ Testar autenticação

1. Vá em **Diagnostics > Authentication** no pfSense.
    
2. Escolha o servidor que você configurou (`AD_EMPRESA`).
    
3. Coloque um usuário do AD, exemplo:
    
    - **Username:** `ldapbind`
        
    - **Password:** senha de login do AD.
        
4. Clique em **Test**.
    
    - Se aparecer **Authentication successful**, está funcionando.
        

---

## 4️⃣ Usar LDAP para login no pfSense

- Vá em **System > User Manager > Settings**.
    
- Em **Authentication Server**, selecione `AD_EMPRESA.
    
- Salve.
    

Agora os usuários do AD podem logar no pfSense.