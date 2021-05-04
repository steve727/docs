## Raspberry Pi 3 OpenVPN setup on Raspberry pi 3 using MacOS

1. Download the [Raspberry Pi Imager](https://www.raspberrypi.org/software/) and flash the sd card with the [Debian Buster lite image](https://downloads.raspberrypi.org/raspios_lite_armhf/images/raspios_lite_armhf-2021-01-12/2021-01-11-raspios-buster-armhf-lite.zip).
2. Default login is pi/raspberry
3. Change pi user password
4. Set localization options to en-US utf8
5. `sudo apt-get update`
   `sudo apt-get upgrade`
7. Set timezone to US/Eastern `sudo dpkg-reconfigure tzdata`
8. Set a static ip `vi /etc/dhcpcd.conf`
		
		interface eth0
		static ip_address=x.x.x.x/24
		static routers=x.x.x.x
		static domain_name_servers=x.x.x.x

9. [PiVPN](https://www.pivpn.io/) install `curl -L https://install.pivpn.io | bash`
10. Run `pivpn add nopass` to add user profiles which are stored in `/home/pi/ovpns`
11. Install the openvpn client on a device and import user profile.
12. Logs:
`sudo cat /var/log/openvpn-status.log`
`sudo cat /var/log/openvpn.log`

13. View vpn profiles `pivpn list`

### fail2ban
`sudo apt-get install fail2ban`

View blocked ips: `sudo iptables -L -n`

fail2ban jail: `cat /var/log/fail2ban.log`

## [Pi-hole automated install](https://github.com/pi-hole/pi-hole/#one-step-automated-install)
```bash
sudo curl -sSL https://install.pi-hole.net | bash
```
### Admin console
`http://x.x.x.x/admin`

### Update Pi-hole
`pihole -up`
