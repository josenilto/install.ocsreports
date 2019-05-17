# OCS Inventory NG - Instalação e configuração da aplicação

IMPORTANTE: A preparação do ambiente precisa ser feita antes da instalação e configuração da aplicação.

Tópico Único - Configuração da aplicação OCS Inventory

Passo 1 - Instalar a aplicação

Execute os comandos:

~# sudo wget https://github.com/OCSInventory-NG/OCSInventory-ocsreports/releases/download/2.6/OCSNG_UNIX_SERVER_2.6.tar.gz //Esse comando faz o download da aplicação

~# sudo tar xfvz OCSNG_UNIX_SERVER_2.6.tar.gz //Esse comando descompacta o pacote tar.gz

~# cd OCSNG_UNIX_SERVER_2.6 //Esse comando entra no diretório extraído

~# sudo sh setup.sh //Esse comando executa o instalador da aplicação

Em resumo, as ações padrões da instalação são:
– Continue = y
– Database server (localhost) = ramada-ti.c6g5nrwkafan.us-east-1.rds.amazonaws.com
– Database server Port (3306) = enter
– Apache daemon binary = enter
– Apache main config = enter
– User Apache web server (www-data) = enter
– Group Apache web server (www-data) = enter
– Apache include config directory = enter
– PERL interpreter = enter
– Communication server = y
– Communication server log = enter
– Communication server plugins config = enter
– Communication server plugins Perl = enter

Caso algum módulo PERL não esteja instalado o OCS Install informará, conforme abaixo:
*** Warning: PERL module Apache2::SOAP is not installed !
This module is only required by OCS Inventory NG SOAP Web Service
*** ERROR: PERL module Archive::Zip is not installed !
*** ERROR: There is one or more required PERL modules missing !
Tecle Yes para autorizar e aguarde pela instalação dos módulos faltantes (ou repita a operação caso necessário).

– Rest API server = no
– Communication server Apache config file “z-ocsinventory-server.conf” = y
– Administration Web Server = y
– Install Console on /var/lib/ocsinventory-reports = y
– Administration Server static files for PHP = enter
– Directories for packages, IPDiscovery and SNMP = enter

Após as informações acima o terminal apresentará a seguinte mensagem:

OK, Administration server installation finished ;-) |
| |
| Please, review /etc/apache2/conf-available/ocsinventory-reports.conf
| to ensure all is good and restart Apache daemon. |
| |
| Then, point your browser to http://server//ocsreports
| to configure database server and create/update schema.

 ~# sudo a2enconf ocsinventory-reports //Esse comando inicializa as configurações da aplicação

~# sudo a2enconf z-ocsinventory-server //Esse comando inicializa as configurações do agent

~# sudo chown -R www-data:www-data /var/lib/ocsinventory-reports/ //Esse comando concede permissão ao usuário www-data

~# sudo service apache2 restart //Esse comando reinicia o serviço do apache

Passo 2 - Configuração da aplicação

Configure o arquivo "z-ocsinventory-server.conf" com as informações de HOST, USUÁRIO e SENHA DO BANCO conforme comando e exemplo abaixo:

~# sudo vim /etc/apache2/conf-available/z-ocsinventory-server.conf

PerlSetEnv OCS_DB_HOST ramada-ti.c6g5nrwkafan.us-east-1.rds.amazonaws.com
# Replace 3306 by port where running MySQL server, generally 3306
PerlSetEnv OCS_DB_PORT 3306
# Name of database
PerlSetEnv OCS_DB_NAME ocsweb
PerlSetEnv OCS_DB_LOCAL ocsweb
# User allowed to connect to database
PerlSetEnv OCS_DB_USER ocs
# Password for user
PerlSetVar OCS_DB_PWD r4m4d4_

Saia do documento pressionando a tecla ESC, depois digite :wq e ENTER

Para continuar a configuração da aplicação acesse a URL

http://IP OU DOMÍNIO DO SERVIDOR/ocsreports/

Após o procedimento acima solicitar as informações de conexão abaixo ao responsável do banco de dados:

MySQL login:
MySQL password:
Name of Database:
MySQL HostName:

Para verificar se está correta com a configuração da aplicação abra o arquivo "dbconfig.inc.php" e verifique as informações:

~# sudo vim /usr/share/ocsinventory-reports/ocsreports/dbconfig.inc.php

Saia do documento pressionando a tecla ESC, depois digite :q e ENTER

Por último, precisamos apagar o arquivo de instalação inicial:

~# sudo rm /usr/share/ocsinventory-reports/ocsreports/install.php

IMPORTANTE: Altere a senha do usuário padrão

Usuário inicial: admin
Senha inicial: admin
