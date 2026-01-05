---
title: "Testando segunda build"
description: "Visão prática do Kerberos no Active Directory: fluxo de autenticação por tickets, principais riscos e controles defensivos."
pubDate: 2025-12-30
updatedDate: 2025-12-30
---

## Introdução

Kerberos é um **protocolo de autenticação usado para verificar identidades em redes seguras**, especialmente em ambientes com Active Directory e SSO corporativo. Ele permite que usuários e serviços provem quem são sem transmitir senhas em texto claro pela rede, usando um sistema de “tickets”. :contentReference[oaicite:0]{index=0}

---

## Visão Geral do Protocolo

O Kerberos funciona como um serviço de identidade:  
1. O usuário se autentica e recebe um **Ticket Granting Ticket (TGT)**.  
2. Esse TGT é usado para solicitar tickets de serviço (TGS) específicos.  
3. Com o ticket de serviço, o cliente acessa recursos sem reenviar credenciais. :contentReference[oaicite:1]{index=1}

Essa abordagem reduz a exposição de senhas e melhora a segurança geral da autenticação. :contentReference[oaicite:2]{index=2}

---

## Componentes Principais

- **KDC (Key Distribution Center)** – entidade que emite tickets.  
- **TGT (Ticket Granting Ticket)** – ticket inicial que permite solicitar tickets de serviço.  
- **TGS (Ticket Granting Service)** – emite tickets para serviços específicos.  
- **SPN (Service Principal Name)** – identifica serviços na rede (ex.: SQL, HTTP). :contentReference[oaicite:3]{index=3}

---

## Vetores de Ataque Relevantes

Mesmo sendo seguro por design, Kerberos é alvo de ataques sofisticados:

### Kerberoasting
Ataque que explora tickets de serviço emitidos para contas com SPNs.  
- Um adversário com credenciais válidas solicita tickets de serviço.  
- Esses tickets, criptografados com o hash da senha da conta de serviço, podem ser extraídos e **quebrados offline**.  
- Se a senha for fraca, o atacante recupera a senha real e usa a conta para movimento lateral. :contentReference[oaicite:4]{index=4}

### Ticket Forging e Tickets de Ouro/Prata
- **Golden Ticket**: um ticket TGT forjado permite acesso irrestrito a recursos da rede.
- **Silver Ticket**: ticket de serviço forjado para acesso específico a um serviço.  
Esses ataques exigem acesso privilegiado ao hash da conta `krbtgt` ou de contas de serviço. :contentReference[oaicite:5]{index=5}

---

## Indicadores de Comprometimento

Alguns sinais que podem indicar ataque Kerberos em andamento:
- Solicitações incomuns de tickets TGS para múltiplos serviços.  
- Tickets criptografados com ciphers fracos (ex.: RC4).  
- Uso de ferramentas de extração como *Rubeus*, *Impacket* ou *Mimikatz*. :contentReference[oaicite:6]{index=6}

---

## Estratégias de Defesa

### Políticas de Senhas e IAM
- **Senhas longas e complexas** para contas de serviço.  
- Rotação regular de senhas e uso de **Managed Service Accounts (gMSA)**.  
- Implementação de **autenticação multifator** onde possível. :contentReference[oaicite:7]{index=7}

### Princípio de Menor Privilégio
- Restringir permissões de contas de serviço àquilo que é estritamente necessário. :contentReference[oaicite:8]{index=8}

### Monitoramento de Eventos
- Integrar logs Kerberos (eventos de autenticação) com **SIEM/UEBA** para identificar padrões suspeitos.  
- Alertar atividades que fogem do padrão normal de emissão de tickets. :contentReference[oaicite:9]{index=9}

---

## Conclusão

Kerberos continua sendo um dos pilares da autenticação corporativa, mas também um alvo frequente de ataques. Entender seu funcionamento, os vetores de ataque e aplicar boas práticas de defesa é essencial para proteger redes modernas. :contentReference[oaicite:10]{index=10}

---
