### [Install Grafana](https://grafana.com/docs/grafana/latest/installation/) OSS on Debian [Bullseye](https://www.debian.org/releases/bullseye/)
```bash
sudo apt-get install -y apt-transport-https gnupg2 dnsutils telnet
sudo apt-get install -y software-properties-common wget
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt-get update
sudo apt-get install grafana

sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable grafana-server
sudo /bin/systemctl start grafana-server
sudo systemctl status grafana-server
```
### [Simple JSON Datasource Plugin](https://grafana.com/grafana/plugins/grafana-simple-json-datasource/)
```bash
sudo grafana-cli plugins install grafana-simple-json-datasource
sudo service grafana-server restart
```
### Install Postfix as a send-only smtp relay
```bash
lsb_release -a
sudo apt install mailutils
sudo apt install postfix
sudo vim /etc/postfix/main.cf
  
  inet_interfaces = loopback-only
  mydestination = $myhostname, localhost.$your_domain, $your_domain
  
systemctl reload postfix

sudo vim /etc/aliases
```
### Configure smtp username/password
```bash
sudo vim /etc/postfix/sasl_passwd
  [smtp.example.com] username:password

sudo postmap /etc/postfix/sasl_passwd
sudo chown root:root /etc/postfix/sasl_passwd /etc/postfix/sasl_passwd.db
sudo chmod 0600 /etc/postfix/sasl_passwd /etc/postfix/sasl_passwd.db
  
sudo vim /etc/postfix/main.cf 
  relayhost = [smtp.example.com]:587

tail /var/log/mail.log
```
### Set Timezone
```bash
sudo timedatectl set-timezone America/New_York
ls -l /etc/localtime
```
### [Install Prometheus](https://github.com/prometheus/prometheus/releases)
```bash
sudo groupadd --system prometheus
sudo useradd -s /sbin/nologin --system -g prometheus prometheus
sudo mkdir /var/lib/prometheus
for i in rules rules.d files_sd; do sudo mkdir -p /etc/prometheus/${i}; done
sudo apt-get -y install wget
mkdir -p /tmp/prometheus && cd /tmp/prometheus
curl -s https://api.github.com/repos/prometheus/prometheus/releases/latest \
  | grep browser_download_url \
  | grep linux-amd64 \
  | cut -d '"' -f 4 \
  | wget -qi -
tar xvf prometheus*.tar.gz
cd prometheus*/
sudo mv prometheus promtool /usr/local/bin/
sudo mv prometheus.yml  /etc/prometheus/prometheus.yml
sudo mv consoles/ console_libraries/ /etc/prometheus/
cd ~/
rm -rf /tmp/prometheus
```
### Prometheus config file
```bash
sudo vim /etc/prometheus/prometheus.yml
```
###  Create a Prometheus systemd Service unit file
```bash
sudo tee /etc/systemd/system/prometheus.service<<EOF

[Unit]
Description=Prometheus
Documentation=https://prometheus.io/docs/introduction/overview/
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=prometheus
Group=prometheus
ExecReload=/bin/kill -HUP $MAINPID
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/var/lib/prometheus \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries \
  --web.listen-address=0.0.0.0:9090 \
  --web.external-url=

SyslogIdentifier=prometheus
Restart=always

[Install]
WantedBy=multi-user.target
EOF
```
### Change directory permissions
```bash
for i in rules rules.d files_sd; do sudo chown -R prometheus:prometheus /etc/prometheus/${i}; done
for i in rules rules.d files_sd; do sudo chmod -R 775 /etc/prometheus/${i}; done
sudo chown -R prometheus:prometheus /var/lib/prometheus/
```
### Reload systemd and start service
```bash
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus
sudo systemctl status prometheus
```
Test Web Interface -  http://[ip_hostname]:9090

### Install node_exporter
```bash
curl -s https://api.github.com/repos/prometheus/node_exporter/releases/latest \
| grep browser_download_url \
| grep linux-amd64 \
| cut -d '"' -f 4 \
| wget -qi -

tar -xvf node_exporter*.tar.gz
cd  node_exporter*/
sudo cp node_exporter /usr/local/bin

node_exporter --version
```
### Create node_exporter service
```bash
sudo tee /etc/systemd/system/node_exporter.service <<EOF
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=default.target
EOF
```
### Reload systemd and start service
```bash
sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl enable node_exporter

systemctl status node_exporter.service 
```
### [Add node_exporter scrape config](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config) to Prometheus server
```
sudo vim /etc/prometheus.yml

- job_name: 'node_exporter'
    static_configs:
      - targets: ['localhost:9100']

sudo systemctl restart prometheus
```
### [briangann-guage-panel plugin](https://grafana.com/grafana/plugins/briangann-gauge-panel/?tab=installation)
```bash
grafana-cli plugins install briangann-gauge-panel
sudo service grafana-server restart
```
### [windows_exporter](https://github.com/prometheus-community/windows_exporter)
```bash
sudo vim /etc/prometheus.yml

- job_name: 'win-exporter'
    static_configs:
      - targets: ['192.168.0.1:9182']

sudo systemctl restart prometheus
```

### [nginx](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/#installing-prebuilt-debian-packages)
```bash
sudo apt-get install nginx
sudo nginx -v
```