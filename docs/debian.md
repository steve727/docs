### disable desktop
    sudo systemctl set-default multi-user.target

### enable desktop
    sudo systemctl set-default graphical.target
    sudo reboot

### xmrig install
    sudo apt-get install git build-essential cmake libuv1-dev libssl-dev libhwloc-dev
    git clone https://github.com/xmrig/xmrig.git
    mkdir xmrig/build && cd xmrig/build
    cmake ..
    make -j$(nproc)

### config changes
    sudo bash -c "echo vm.nr_hugepages=1280 >> /etc/sysctl.conf"
    sudo -i
    sudo sysctl -w vm.nr_hugepages=$(nproc)
    for i in $(find /sys/devices/system/node/node* -maxdepth 0 -type d);do echo 3 > "$i/hugepages/hugepages-1048576kB/nr_hugepages";done
