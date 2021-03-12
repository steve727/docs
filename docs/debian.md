### disable desktop
`sudo systemctl set-default multi-user.target`

### enable desktop
`sudo systemctl set-default graphical.target`
`sudo reboot`

### xmrig install
`sudo apt-get install git build-essential cmake libuv1-dev libssl-dev libhwloc-dev`
`git clone https://github.com/xmrig/xmrig.git`
`mkdir xmrig/build && cd xmrig/build`
`cmake ..`
`make -j$(nproc)`
