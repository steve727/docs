### disable desktop
    sudo systemctl set-default multi-user.target

### enable desktop
    sudo systemctl set-default graphical.target
    sudo reboot
    
### packages
    apt install build-essential cmake libuv1-dev libssl-dev libhwloc-dev python3-pip sysstat iotop dstat atop ioping

### xmrig install
    sudo apt-get install git build-essential cmake libuv1-dev libssl-dev libhwloc-dev
    git clone https://github.com/xmrig/xmrig.git
    mkdir xmrig/build && cd xmrig/build
    cmake ..
    make -j$(nproc)
    
### [Virtualbox](https://www.virtualbox.org/wiki/Linux_Downloads)
```bash
vim /etc/apt/sources.list
    deb [arch=amd64] https://download.virtualbox.org/virtualbox/debian buster contrib

wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
sudo apt-get update
sudo apt-get install virtualbox-6.1
sudo /sbin/vboxconfig
virtualbox 
```
### [Remote Desktop Manager Free](https://remotedesktopmanager.com/home/download)
```bash
curl -O https://remotedesktopmanager.com/home/thankyou/rdmlinuxfreebin
sudo dpkg -i RemoteDesktopManager.Free_2021.1.0.10_amd64.deb
remotedesktopmanager.free
```
### terraform
```bash
curl -O https://releases.hashicorp.com/terraform/0.15.0/terraform_0.15.0_linux_amd64.zip

```

### mount ntfs drives (read)
```
sudo fdisk -l | grep NTFS
sudo mkdir /mnt/ntfs
sudo mount -t ntfs /dev/sdb1 /mnt/ntfs
cd /mnt/ntfs
```

### config changes
    sudo bash -c "echo vm.nr_hugepages=1280 >> /etc/sysctl.conf"
    sudo -i
    sudo sysctl -w vm.nr_hugepages=$(nproc)
    for i in $(find /sys/devices/system/node/node* -maxdepth 0 -type d);do echo 3 > "$i/hugepages/hugepages-1048576kB/nr_hugepages";done
    
### influxdb (rpi3)
    cat /etc/os-release
    wget -qO- https://repos.influxdata.com/influxdb.key | sudo apt-key add -
    echo "deb https://repos.influxdata.com/debian buster stable" | sudo tee /etc/apt/sources.list.d/influxdb.list    
    sudo systemctl unmask influxdb
    sudo systemctl enable influxdb
    sudo systemctl start influxdb
    influx
    CREATE DATABASE rigstats
    USE rigstats
    
### changes
```shell
apt update
apt upgrade
apt install sudo
usermod -aG sudo steve
apt install wget
apt install curl
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
apt install ./google-chrome-stable_current_amd64.deb
mv google-chrome-stable_current_amd64.deb /home/steve/Downloads
https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64
apt install ./code_1.54.3-1615806378_amd64.deb
sudo apt install python3-pip
pip3 install tidal-dl --upgrade
```
