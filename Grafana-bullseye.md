### [Install Grafana](https://grafana.com/docs/grafana/latest/installation/) OSS on Debian [Bullseye](https://www.debian.org/releases/bullseye/)
```bash
sudo apt-get install -y apt-transport-https gnupg2
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
```


