### misc cmds
```
du -sh
netstat -tulpn 
ps aux | grep nginx 
htop 
ls /mnt 
/etc/fstab 
```
### disable desktop
`sudo systemctl set-default multi-user.target`
### enable desktop
```shell 
sudo systemctl set-default graphical.target
sudo reboot
```
### locale settings
```shell
sudo vim /etc/locale.gen
sudo locale-gen
```
### zsh
```bash
chsh -s /usr/bin/zsh 
chsh -s `which zsh`
```
### sources
`cat /etc/apt/sources.list`

### find the largest files or folders
`du -a / | sort -n -r | head -n 10`

### directory sizes
`ls | xargs -I {} du -shx {}`

### clear apt cache
```
du -sh /var/cache/apt/archives
sudo apt-get clean
```
### packages
`apt install build-essential cmake curl libuv1-dev libssl-dev libhwloc-dev python3-pip sudo sysstat htop iotop dstat dnsutils atop ioping net-tools wget`

### xmrig install
```
sudo apt-get install git build-essential cmake libuv1-dev libssl-dev libhwloc-dev
git clone https://github.com/xmrig/xmrig.git
mkdir xmrig/build && cd xmrig/build
cmake ..
make -j$(nproc)
```    
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
### [Terraform](https://releases.hashicorp.com/terraform)
```bash
curl -O https://releases.hashicorp.com/terraform/0.15.4/terraform_0.15.4_linux_amd64.zip
unzip terraform_0.15.4_linux_amd64.zip
sudo mv terraform /usr/local/bin
rm terraform_0.15.4_linux_amd64.zip
```
### mount ntfs drives (read)
```
sudo fdisk -l | grep NTFS
sudo mkdir /mnt/ntfs
sudo mount -t ntfs /dev/sdb1 /mnt/ntfs
cd /mnt/ntfs
```
### config changes
```shell
sudo bash -c "echo vm.nr_hugepages=1280 >> /etc/sysctl.conf"
sudo -i
sudo sysctl -w vm.nr_hugepages=$(nproc)
for i in $(find /sys/devices/system/node/node* -maxdepth 0 -type d);do echo 3 > "$i/hugepages/hugepages-1048576kB/nr_hugepages";done
```    
### changes
```shell
usermod -aG sudo steve
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
apt install ./google-chrome-stable_current_amd64.deb
mv google-chrome-stable_current_amd64.deb /home/steve/Downloads
https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64
apt install ./code_1.54.3-1615806378_amd64.deb

pip3 install tidal-dl --upgrade
```
