### debian 11 notes
```bash
sudo hostnamectl set-hostname NAME
sudo hostnamectl set-icon-name NAME
sudo hostnamectl set-chassis NAME

sudo apt install fasttrack-archive-keyring

echo "deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian $(lsb_release -cs) contrib" | \
     sudo tee -a /etc/apt/sources.list.d/virtualbox.list
    
    
```
