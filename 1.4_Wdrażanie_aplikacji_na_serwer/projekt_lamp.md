# Omówienie stosu technologicznego
```bash
LAMP czyli:
Linux
Apache
MySQL
PHP
```
## Instalacja zależności

```bash
Instalacja npm:
npm install

Jeśli chcemy odpalić plik npm wrzucamy polecenie
npm run <nazwa>
```

## Instalacja Apache

### Ubuntu
```bash 
sudo apt-get install apache2 # instalacja
sudo systemctl status apache2 # status
sudo systemctl stop apache2 # zatrzymanie usługi
sudo systemctl start apache2 # uruchomienie usługi
sudo systemctl restart apache # reset apache
```
### CentOS
```bash
sudo yum install httpd # instalacja
sudo systemctl status httpd # status
sudo systemctl stop httpd # zatrzymanie usługi
sudo systemctl start httpd # uruchomienie usługi
sudo systemctl restart httpd # reset apache
sudo systemctl enable httpd # uruchomienie po resecie
```
