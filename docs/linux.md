### View & change hostname
```shell
hostname -s
hostname -f
hostnamectl
hostnamectl set-hostname your-new-hostname
```

### AD Domain stuff
```shell
subscription-manager list --available
subscription-manager attach --pool=xx
dnf groupinstall workstation
systemctl set-default graphical.target
dnf groupinstall "Development Tools"
realm discover domainname
realm join domain -U domainadmin
realm list
sudo authselect select sssd
sudo authselect select sssd with-mkhomedir
cat /etc/sssd/sssd.conf
sudo systemctl restart sssd
vi /etc/sssd/sssd.conf
dyndns_update = false
sudo systemctl restart sssd
realm permit someuser@dmain.com
vi /etc/sudoers.d/10_Xsudoers
```

### Cleanup old kernels
```shell
rpm -qa kernel\* |sort -V
dnf remove $(dnf repoquery --installonly --latest-limit=-2 -q)
```
    
# For Templates
```shell
sudo -i sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
sudo -i sed -i 's/#PasswordAuthentication/PasswordAuthentication/g' /etc/ssh/sshd_config
sudo -i sed -i 's/PermitRootLogin no/PermitRootLogin yes/g' /etc/ssh/sshd_config
sudo -i sed -i 's/#PermitRootLogin/PermitRootLogin/g' /etc/ssh/sshd_config
sudo -i service sshd restart
    
curl --insecure --output katello-ca-consumer-latest.noarch.rpm https://satellite-server-name-here/pub/katello-ca-consumer-latest.noarch.rpm
yum localinstall katello-ca-consumer-latest.noarch.rpm
subscription-manager register --org="org-here" --activationkey="key-here.ak"
subscription-manager repos --enable=satellite-tools-6.7-for-rhel-8-x86_64-rpms
    
yum -y install katello-agent realmd sssd oddjob oddjob-mkhomedir adcli samba-common samba-common-tools
  
systemctl stop chronyd
chronyd -q
systemctl start chronyd
systemctl stop firewalld
systemctl disable firewalld
systemctl start tuned
tuned-adm profile virtual-guest 

sudo vi /etc/selinux/config
  SELINUX=disabled
```
    
### RHEL 8.x   
```shell    
dnf install samba samba-client  samba-winbind samba-winbind-clients oddjob-mkhomedir oddjob
vi /etc/samba/smb.conf
  [global]
	realm = REALM.LOCAL
	security = ADS
	workgroup = REALM
	template shell = /bin/bash
	template homedir = /home/%D/%U
	idmap config * : rangesize = 1000000
	idmap config * : range = 1000000-19999999
	idmap config * : backend = autorid
	winbind offline logon = true
	log file = /var/log/samba/log.%m
	max log size = 50
	log level = 0 
    
subscription-manager repos --enable=rhel-8-for-x86_64-baseos-rpms
subscription-manager repos --enable=rhel-8-for-x86_64-appstream-rpms                             
subscription-manager repos --enable=satellite-tools-6.7-for-rhel-8-x86_64-rpms
yum repolist all
```

### nvme disk
```shell
yum install nvme-cli
mkdir /data
pvcreate /dev/nvme1n1p1
vgcreate vg_data /dev/nvme1n1p1
lvcreate -l 100%FREE -n lv_data vg_data
mkfs.xfs /dev/vg_data/lv_data
echo '/dev/vg_data/lv_data      /data        xfs    defaults,nofail 0 0' >> /etc/fstab
mount -a

mkdir /data2
pvcreate /dev/nvme2n1p1
vgcreate vg_data2 /dev/nvme2n1p1
lvcreate -l 100%FREE -n lv_data2 vg_data2
mkfs.xfs /dev/vg_data2/lv_data2
echo '/dev/vg_data2/lv_data2      /data2        xfs    defaults,nofail 0 0' >> /etc/fstab
mount -a
```

### Mounting an .iso as a local repo
```shell
mkdir /mnt/disc
mount /dev/sr0 /mnt/disc
cp /mnt/disc/media.repo /etc/yum.repos.d/rhel7dvd.repo

vi /etc/yum.repos.d/rhel7dvd.repo
  gpgcheck=1
  enabled=1
  baseurl=file:///mnt/disc/
  gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

yum clean all    
yum repolist enabled
```    
### Remove repo when done
 `yum repolist --disablerepo=InstallMedia`

`rm -f /etc/yum.repos.d/rhel7dvd.repo`

### yum cleanup
`yum clean all`

`yum clean metadata`

### Tracing

`strace df -h`

`mount -l`

### Show mounts

`cat /proc/mounts`

`findmnt`

`findmnt -lo source,target,fstype,label,options,used -t ext4`

### Unmount all cifs mounts
`umount -a -t cifs -l`

### Kill processes
`kill -9 thePID`

### Generate ssh keys
```shell
ssh-keygen -t rsa 4096
chmod 700 ~/.ssh
cat ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub
```
### Renaming volume group for root and swap
```shell
vgs
lvs rhel_rhel78template
vgrename rhel_rhel78template root_vg

vi /etc/fstab
  /dev/mapper/root_vg-root /                       xfs     defaults        0 0
  /dev/mapper/root_vg-swap swap                    swap    defaults        0 0

cat /boot/grub2/grub.cfg | grep rhel_rhel78template

sed -i 's/rhel_rhel78template/root_vg/g' /boot/grub2/grub.cfg

cat /boot/grub2/grub.cfg | grep root_vg

vgchange -ay
  lvchange /dev/root_vg/root --refresh   
  lvchange /dev/root_vg/swap --refresh

ls -al /boot/initramfs-3.10.0-1127.el7.x86_64.img
uname -r

cp /boot/initramfs-$(uname -r).img /boot/initramfs-$(uname -r).img.$(date +%m-%d-%H%M%S).bak

mkinitrd -f -v /boot/initramfs-$(uname -r).img $(uname -r)
```
# Show network information
`nmcli d show`

# Disable IPv6
```shell
vi /etc/sysctl.conf
  net.ipv6.conf.all.disable_ipv6 = 1
  net.ipv6.conf.default.disable_ipv6 = 1
    
sysctl -p

vi /etc/ssh/sshd_config
  AddressFamily inet
    
systemctl restart sshd
```
### Install Desktop Environment
```shell
yum groupinstall "Server with GUI"
systemctl set-default graphical.target
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum -y install xrdp tigervnc-server
firewall-cmd --add-port=3389/tcp --permanent
```
### Cloning / Enable ssh
`vi /etc/ssh/sshd_config`
  Change `PasswordAuthentication` and `ChallengeResponseAuthentication` to `yes`

`service sshd restart`  

### Certs

`/etc/pki/ca-trust/`

`/usr/share/pki/ca-trust-source/`

`update-ca-trust`

`trust list`

### yum update disable missing packages

`sudo yum update check-update`

`sudo yum-config-manager --disable packages-microsoft-com-mssql-server-2017`

