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
