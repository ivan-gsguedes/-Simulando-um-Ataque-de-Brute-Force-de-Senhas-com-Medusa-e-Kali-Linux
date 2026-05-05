# Simulando um Ataque Usando Kali Linux e Metasploitable

Verificando conexão e portas abertas no sistema alvo:

Digite o comando no terminal para verificar se está tendo conexão com o sistema alvo: 

ping -c 5 192.168.56.101

OBS: no comando do ping "-c 5" significa que ele vai parar depois de 5 tentativas de contato



Verificando portas abertas no sistema alvo:
nmap -sV -p 21,22,80,445,139 192.168.56.101
