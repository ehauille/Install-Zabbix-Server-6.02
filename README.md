# Install-Zabbix-Server-6.02

# Actualiza los paquetes existentes en el sistema
sudo apt update
sudo apt upgrade -y


# Instala los paquetes necesarios para Zabbix
sudo apt install -y wget curl vim htop mariadb-server mariadb-client apache2 php php-mysql php-mbstring php-gd php-xml php-bcmath php-ldap php-snmp libapache2-mod-php


# Descarga e importa la clave del repositorio de Zabbix
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-1+ubuntu20.04_all.deb
sudo dpkg -i zabbix-release_6.0-1+ubuntu20.04_all.deb
sudo apt update

# Instala el servidor Zabbix
sudo apt install -y zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-agent

# Configura la base de datos de Zabbix

sudo mysql -u root -p

- CREATE DATABASE zabbixdb CHARACTER SET utf8 COLLATE utf8_bin;
- CREATE USER 'zabbixuser'@'localhost' IDENTIFIED BY 'yourpassword';
- GRANT ALL PRIVILEGES ON zabbixdb.* TO 'zabbixuser'@'localhost' WITH GRANT OPTION;
- FLUSH PRIVILEGES;
- QUIT;

# Importa la estructura de la base de datos de Zabbix

zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -u zabbixuser -p zabbixdb

# Configura el archivo de configuración de Zabbix Server
sudo vi /etc/zabbix/zabbix_server.conf

Cambia los valores de las siguientes líneas de acuerdo a tu configuración
DBName=zabbixdb
DBUser=zabbixuser
DBPassword=yourpassword

- Guarda y cierra el archivo.

# Inicia y habilita los servicios de Zabbix

sudo systemctl start zabbix-server zabbix-agent apache2
sudo systemctl enable zabbix-server zabbix-agent apache2


- Abre tu navegador web e ingresa la dirección IP o nombre de host de tu servidor Ubuntu seguido de "/zabbix" para acceder a la interfaz web de Zabbix. Inicia sesión con el usuario "Admin" y la contraseña "zabbix".
