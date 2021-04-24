### Update Kali
    sudo apt update && sudo apt upgrade -y 
    sudo apt update && sudo apt full-upgrade -y
    sudo apt dist-upgrade
    sudo apt autoremove -y
    sudo apt autoclean -y
    sudo apt clean -y
    
### Prevent system from sleeping    
    sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target

### Password-less sudo
    sudo apt install -y kali-grant-root && sudo dpkg-reconfigure kali-grant-root

### permit root login
    sudo apt install kali-root-login

### ssh public-key auth (as root)
    ssh-keygen -b 2048 -t rsa 
    update-rc.d ssh remove
    update-rc.d -f ssh defaults
    mkdir ~/.ssh
    chmod 700 ~/.ssh
    vi ~/.ssh/authorized_keys
    chmod 600 ~/.ssh/authorized_keys

### Add a normal user
    sudo useradd -m -G sudo -s /bin/bash steve
    sudo passwd steve
    echo '%sudo ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

### Set static ip
    vim /etc/network/interfaces
    auto eth0
    iface eth0 inet static
    address 192.168.254.9/24
    gateway 192.168.254.254
    sudo systemctl restart networking.service
    sudo rm /etc/resolv.conf
    sudo vim /etc/resolv.conf
    nameserver 8.8.8.8
    sudo chattr +i /etc/resolv.conf

### ssh
    apt install openssh-server
    mkdir /etc/ssh/default_keys
    mv /etc/ssh/ssh_host_* /etc/ssh/default_keys/
    dpkg-reconfigure openssh-server
    vim /etc/ssh/sshd_config
    systemctl enable ssh.service
    systemctl start ssh.service
    systemctl status ssh.service

### boot without gui
    systemctl get-default
    sudo systemctl set-default multi-user.target

### run a simple python http server from any directory   
    sudo python -m SimpleHTTPServer 80

### Bettercap
    wget http://old.kali.org/kali/pool/main/libp/libpcap/libpcap0.8_1.9.1-4_amd64.deb \
    && dpkg -i libpcap0.8_1.9.1-4_amd64.deb
    
    vim /usr/share/bettercap/caplets/https-ui.cap
   
    bettercap -caplet https-ui --iface wlan1
    
    wifi.recon on
    set wifi.show sort
    set ticker.commands 'clear; wifi.show'
    ticker on
    
    net.recon on
    set ticker.commands 'clear; net.show; events.show 10'
    net.probe on
    ticker on
    
    wifi.recon.channel 11
    wifi.show.wps BSSID

### airmon-ng
    sudo airmon-ng check kill
    sudo airmon-ng start wlan0
    sudo iwconfig
    sudo airodump-ng -c 9 wlan0mon
    sudo airodump-ng wlan1mon

### kismet
    sudo apt install build-essential git libwebsockets-dev pkg-config zlib1g-dev libnl-3-dev libnl-genl-3-dev \
    libcap-dev libpcap-dev libnm-dev libdw-dev libsqlite3-dev libprotobuf-dev libprotobuf-c-dev protobuf-compiler \
    protobuf-c-compiler libsensors4-dev libusb-1.0-0-dev python3 python3-setuptools python3-protobuf \
    python3-requests python3-numpy python3-serial python3-usb python3-dev python3-websockets librtlsdr0 \
    libubertooth-dev libbtbb-dev libmicrohttpd-dev  
    
    git clone https://www.kismetwireless.net/git/kismet.git 
    cd kismet
    ./configure
    make
    sudo usermod -aG kismet $USER

### Probequest
    sudo pip3 install --upgrade probequest    
    sudo airmon-ng start wlan1
    sudo probequest -i wlan1mon 
    sudo airodump-ng wlan1mon

### hcxdump / hcxtools
    git clone https://github.com/ZerBea/hcxdumptool.git
    cd hcxdumptool
    make
    sudo make install
    sudo apt-get install libcurl4-openssl-dev libssl-dev pkg-config zlib1g-dev
    git clone https://github.com/ZerBea/hcxtools.git
    cd hcxtools
    make
    sudo make install
    
    hcxdumptool -i wlan0 -o xname.pcapng --enable_status=1 
    hcxpcaptool -E xname-essid -I xidentity -U xusers xname.pcapng -o xname-out
    hashcat -m 22000 xname-out -a 0 -w 3 -d 2 'rockyou.txt'

### seclists
    sudo apt -y install seclists
    
    wget -c https://github.com/danielmiessler/SecLists/archive/master.zip -O SecList.zip \
    && unzip SecList.zip \
    && rm -f SecList.zip
    
### wifi interface settings
    iwconfig
    airmon-ng check kill
    ifconfig wlan0 down
    ifconfig wlan1 down
    iw reg set GY
    iw wlan1 set txpower fixed 30 
    macchanger -a wlan0
    ifconfig wlan0 up
    airmon-ng start wlan0
    ifconfig wlan0mon down
    macchanger -a wlan0mon
    ifconfig wlan0mon up

### mdk4  
    sudo apt install mdk4    
    ifconfig wlan0 down
    iw phy1 interface add mon0 type monitor
    iw phy1 interface add mon1 type monitor
    iw phy1 interface add mon2 type monitor
    iw wlan0 del
    ifconfig mon0 up
    ifconfig mon1 up
    ifconfig mon2 up
    mdk4 mon0 e -t [TARGET] -s 100
    mdk4 mon1 e -t [TARGET] -s 100
    mdk4 mon2 e -t [TARGET] -s 100
    
### wpa supplicant
    vim /etc/wpa_supplicant.conf
    ctrl_interface=/var/run/wpa_supplicant
    ctrl_interface_group=0
    update_config=1
    wpa_supplicant -Dwext -iwlan1 -c/etc/wpa_supplicant.conf –B
    wpa_cli  

### wordlists
    sudo gzip -d /usr/share/wordlists/rockyou.txt.gz

### wifite
    wifite --wps --ignore-locks
    
### crowbar
    apt install -y nmap openvpn freerdp2-x11 tigervnc-viewer python3 python3-pip
    git clone https://github.com/galkan/crowbar
    cd crowbar/
    pip3 install -r requirements.txt
