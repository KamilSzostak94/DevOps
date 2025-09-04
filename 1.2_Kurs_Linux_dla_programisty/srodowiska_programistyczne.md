```markdown

# Środowiska programistyczne (DevOps perspective)

## Java
sudo apt install openjdk-17-jdk
java -version

## Apache
sudo apt install apache2
sudo systemctl enable apache2
sudo systemctl start apache2
sudo systemctl status apache2

## PHP
sudo apt install php libapache2-mod-php php-mysql
echo "<?php phpinfo(); ?>" > /var/www/html/info.php

## MySQL
sudo apt install mysql-server
sudo systemctl enable mysql
sudo systemctl start mysql
sudo mysql_secure_installation

## phpMyAdmin
sudo apt install phpmyadmin
# Konfiguracja aliasu w Apache: /phpmyadmin
# Jeśli błąd mysqli::real_connect(), w config.inc.php ustaw $cfg['Servers'][$i]['host'] = '127.0.0.1';