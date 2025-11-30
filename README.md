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
      Para criação da Lista de usuários utilizei o comando: echo -e 'user\nmsfadmin\nadmin\nroot' > user.txt
      Para criação da Lista de senhas utilizei o comando: echo -e '123456\npassword\nqwerty\nmsfadmin' > pass.txt



