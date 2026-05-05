# Simulando um Ataque Usando Kali Linux e Metasploitable

Essa documentação explicará como executar ataques simulados de força bruta em FTP, automação de tentativas em formulário web (DVWA) e password spraying em SMB com enumeração de usuários. Para isso será utilizado o kali linux que e um sistema com varios programas instalados para fazer os ataques e o metasploitable que e um sistema com varias vulnerabilidades propositais para realizar as invasões.

Antes de começar a auditoria primeiro precisa ser feito a enumeração ou seja a coleta de dados do alvo.

Digite o comando no terminal para verificar se está tendo conexão com o sistema alvo: 

ping -c 5 192.168.56.101

OBS: no comando do ping "-c 5" significa que ele vai parar depois de 5 tentativas de contato

<img width="1919" height="418" alt="Captura de tela 2026-05-05 173430" src="https://github.com/user-attachments/assets/e86bb02c-ada4-48a0-ae25-ab1465aca98c" />

Verificando portas abertas no sistema alvo:

nmap -sV -p 21,22,80,445,139 192.168.56.101

OBS: 

"-sV" é utilizado para a detecção de versão dos serviços que estão rodando em portas abertas
"-p" é utilizado para especificar quais portas (ou intervalos de portas) você deseja escanear

<img width="843" height="351" alt="Captura de tela 2026-05-05 174210" src="https://github.com/user-attachments/assets/9626763d-2369-431f-b7e9-7fb674e1ce06" />

Ataque de Força Bruta em FTP:

será necessário usar o programa medusa para fazer o ataque mas antes precisa ser feito duas listas, uma contendo nomes de usuários e outra de senhas possíveis

Criando lista com possíveis nomes de usuários:

echo -e "user\nmsfadmin\nadmin\nroot" > user.txt

Criando lista de senhas:

echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt

OBS: Por padrão, o comando echo trata tudo como texto literal. Com a flag "-e", o terminal entende instruções especiais, como quebras de linha ou tabulações (caracteres precedidos por uma barra invertida \).

<img width="559" height="206" alt="Captura de tela 2026-05-05 180341" src="https://github.com/user-attachments/assets/b383b75e-c4e0-467c-b6b6-dbd98509deac" />

Após isso poderemos começar o ataque usando o aplicativo Medusa. 

medusa-h 192.168.56.101 -U user.txt -P pass.txt -M ftp -t 6

depois de executar o comando ele vai fazer uma série de combinações de usuario e senha até chegar na correta informando com "SUCCESS"

OBS: 

"-h" Define o endereço IP ou nome do host do alvo,
"-U" Caminho para um arquivo com uma lista de usuários,
"-P" Caminho para um arquivo com uma lista de senhas,
"-M" Define o protocolo,
"-t" Número de threads (tentativas paralelas simultâneas).

<img width="1271" height="351" alt="Captura de tela 2026-05-05 182201" src="https://github.com/user-attachments/assets/f7e0ba4b-915d-4590-80d5-e013868820bd" />

Após conseguir o usuário e senha digite o seguinte comando para fazer o acesso via FTP:

ftp 192.168.56.101

e digite o usuário e senha obtidos pelo medusa.

<img width="322" height="120" alt="Captura de tela 2026-05-05 182712" src="https://github.com/user-attachments/assets/4c88a4fe-31b1-4f9c-92d3-746c1866cfcd" />
<img width="312" height="197" alt="Captura de tela 2026-05-05 182745" src="https://github.com/user-attachments/assets/65c9717f-f9ed-4c28-a804-f13b3a23e511" />

Ataques de Força Bruta em Formulário web (DVWA):

Abra o firefox no kali e digite o ip do metasploitable na barra de endereço:

192.168.56.101/dvwa/login.php

<img width="1919" height="829" alt="Captura de tela 2026-05-05 192846" src="https://github.com/user-attachments/assets/40544ecc-0928-4154-9452-60b4c8ab284a" />

depois precisa ir ao terminal e começar o teste para isso será usado a lista anterior de usuário e senha. Depois digite o comando:

medusa -h 192.168.56.101 -U user.txt -P pass.txt -M http \
-m PAGE:'/dvwa/login.php' \
-m FORM:'username=^USER^&password=^PASS^&Login=Login' \
-m 'FAIL=Login failed' -t 6

A flag "-m" é usada para passar parâmetros adicionais específicos para o módulo selecionado com o -M

<img width="556" height="162" alt="Captura de tela 2026-05-05 201214" src="https://github.com/user-attachments/assets/98a974cc-83af-4d91-96b1-d703f83e110f" />

depois de executar o comando ele mostra várias combinações de usuários e senhas que podem ser usadas.

<img width="1309" height="729" alt="Captura de tela 2026-05-05 201413" src="https://github.com/user-attachments/assets/c5f9b8c6-2d74-4259-b94a-175831b3ecc9" />

Após testar algumas das credenciais o acesso será feito

<img width="1483" height="808" alt="Captura de tela 2026-05-05 202644" src="https://github.com/user-attachments/assets/8e7d6499-1d8e-4d3c-831f-45355ceac7de" />

Password Spraying em SMB com Enumeração de Usuários:

