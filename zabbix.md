### [Zabbix](https://www.zabbix.com/download?zabbix=5.2&os_distribution=debian&os_version=10_buster&db=mysql&ws=nginx) install on Debian 10

### Install Zabbix repo
```
wget https://repo.zabbix.com/zabbix/5.2/debian/pool/main/z/zabbix-release/zabbix-release_5.2-1+debian10_all.deb
sudo dpkg -i zabbix-release_5.2-1+debian10_all.deb
sudo apt update
```
### Install Zabbix server, frontend, agent
```
sudo apt install zabbix-server-mysql zabbix-frontend-php zabbix-nginx-conf zabbix-agent
sudo addgroup --system --quiet zabbix
sudo adduser --quiet --system --disabled-login --ingroup zabbix --home /var/lib/zabbix --no-create-home zabbix
sudo mkdir -m u=rwx,g=rwx,o= -p /var/lib/zabbix
sudo chown zabbix:zabbix /var/lib/zabbix
```

### [mysql](https://dev.mysql.com/downloads/repo/apt/)
```bash
wget https://dev.mysql.com/get/mysql-apt-config_0.8.17-1_all.deb
sudo dpkg -i mysql-apt-config*
sudo apt update && sudo apt upgrade
sudo apt install mysql-server
sudo systemctl status mysql
mysql_secure_installation
mysqladmin -u root -p version
mysql -uroot -p
create database zabbix character set utf8 collate utf8_bin;
create user 'zabbix'@'localhost' identified by '<password>';
grant all privileges on zabbix.* to 'zabbix'@'localhost';
quit;
```
### db config
```bash
zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p zabbix
sudo vim /etc/zabbix/zabbix_server.conf
  DBPassword=<password>
```  
### nginx config
```bash
sudo vim /etc/zabbix/nginx.conf
  listen 80;
  server_name localhost;
```
### set timezone
```bash
sudo vim /etc/zabbix/php-fpm.conf
php_value[date.timezone] = America/NewYork
```
### Start Zabbix server and agent processes and make it start at system boot
```bash
systemctl restart zabbix-server zabbix-agent nginx php7.3-fpm
systemctl enable zabbix-server zabbix-agent nginx php7.3-fpm
```
### php
```
sudo apt -y install php php-common
php -v
```
### nginx
```bash
sudo apt install curl gnupg2 ca-certificates lsb-release
echo "deb http://nginx.org/packages/mainline/debian `lsb_release -cs` nginx" \
    | sudo tee /etc/apt/sources.list.d/nginx.list
```
### packages
```
sudo apt-get install software-properties-common
sudo apt install php7.3-fpm php7.3-common php7.3-mysql php7.3-gmp php7.3-curl php7.3-intl php7.3-mbstring php7.3-xmlrpc php7.3-gd php7.3-xml php7.3-cli php7.3-zip php7.3-soap php7.3-imap
```
