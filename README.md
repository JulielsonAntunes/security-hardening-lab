# security-hardening-lab

# 🛡️ Projeto de Hardening e Segurança de Acesso Remoto em Servidor Linux

## 🚀 Status do Projeto
**Concluído** | **Foco:** Cibersegurança Defensiva, Administração de Sistemas, e Governança (GRC).

---

## 🎯 Objetivo do Projeto
Transformar uma instalação padrão de servidor **Ubuntu Server (VM)** em um ambiente seguro, aplicando as melhores práticas de **Hardening** para reduzir a superfície de ataque, controlar o acesso via rede e reforçar o protocolo SSH.

Este projeto valida a capacidade de implementar o princípio de **privilégio mínimo** e de **defesa em profundidade**.

---

## ⚙️ Medidas de Segurança Implementadas

As seguintes configurações foram aplicadas ao servidor `hardening-server`:

### 1. Hardening do Firewall (UFW)
A primeira linha de defesa foi configurada para operar no modo mais restritivo possível.

* **Padrão:** O firewall UFW foi ativado para **negar todo o tráfego de entrada** (`deny incoming`) por padrão.
* **Porta 22:** Foi **bloqueada** (removida) para evitar ataques automatizados de força bruta direcionados à porta padrão do SSH.
* **Nova Porta SSH:** O acesso foi transferido para a **Porta 2222**, uma porta não-padrão, adicionando uma camada de obscuridade e dificultando scanners básicos.

### 2. Reforço de Acesso Privilegiado (SSH)
O principal vetor de ataque para servidores Linux (SSH) foi configurado para proibir logins de alto privilégio.
<img width="817" height="614" alt="Print03_PermitirRootLogin" src="https://github.com/user-attachments/assets/bc260aa8-d4be-4f45-9bb8-3e93b2605336" />

* **Login Root Desabilitado:** A configuração `PermitRootLogin no` foi definida no arquivo `/etc/ssh/sshd_config`. Isso garante que ninguém (incluindo atacantes) possa logar diretamente como `root`.

* **Princípio de Privilégio Mínimo:** A administração agora é feita exclusivamente por um usuário comum (`julielson`), que deve usar o `sudo` quando necessário.

---

## 📈 # Validação e Provas de Segurança

### 1. Verificação do Firewall (UFW Status)
  `sudo ufw status verbose`

<img width="812" height="571" alt="Print01_UFW_status_verbose" src="https://github.com/user-attachments/assets/0b76d7f3-7783-458d-a8fb-36127e247d9f" />


**Prova:** Confirma que a Porta 22 foi removida e que o sistema só expõe o acesso remoto na porta não-padrão 2222.

### 2. Teste de Bloqueio de Root (Prova de Sucesso)
A partir de uma máquina atacante (`julielson@ramos`), foi realizada uma tentativa de login proibida na nova porta.

* **Comando de Teste:**
   ``
    ssh root@192.168.122.126 -p 2222
  ``
    <img width="812" height="571" alt="Print02_tentativa_SSH_como_root" src="https://github.com/user-attachments/assets/659ca6eb-786c-49ad-bd23-b3a5f245b530" />

* **Resultado do Servidor:** A conexão foi encerrada imediatamente após a tentativa de autenticação, conforme evidenciado pela mensagem:
    ```
    Connection closed by 192.168.122.126 port 2222
    ```
* **Conclusão:** O *Hardening* foi bem-sucedido. O servidor rejeitou o login do usuário `root`, validando a regra `PermitRootLogin no` e protegendo o acesso privilegiado.

---

## ➡️ Próximos Passos
Este servidor (`hardening-server`) será reutilizado como base para o **Projeto 2**, focado na detecção de ameaças.
* **Projeto 2:** Implementação de um **HIDS (Host-based Intrusion Detection System)** para monitoramento contínuo de integridade de arquivos (FIM) e análise de logs.
