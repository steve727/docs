### k8s cluster on debian 11

```shell
sudo vim /etc/hosts ### add nodes ip addresses
lsmod | grep br_netfilter
sudo modprobe br_netfilter

cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sudo sysctl --system

### docker install ###
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg lsb-release
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
sudo groupadd docker
sudo usermod -aG docker $USER
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
sudo reboot
docker version

### docker-compose install ###
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
docker-compose --version

### k8s install ###
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

### disable swap ###
sudo swapoff -a
sudo vim /etc/fstab ### Remove swap entry
sudo systemctl daemon-reload

### configure docker daemon
sudo mkdir /etc/docker
cat <<EOF | sudo tee /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

sudo systemctl enable docker
sudo systemctl daemon-reload
sudo systemctl restart docker

### init cluster on Master node
sudo kubeadm init --pod-network-cidr 10.10.0.0/16

mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

### Calico setup - Uncomment the CALICO_IPV4POOL_CIDR variable in the manifest and set pod CIDR(10.10.0.0/16) ###
curl https://docs.projectcalico.org/manifests/calico.yaml -O
vim calico.yaml

kubectl get nodes
```
### upgrade to v1.23.0-00 - master controle plane
```bash
apt-mark unhold kubeadm
apt-get update && apt-get install -y kubeadm=1.23.0-00 && \
apt-mark hold kubeadm
kubeadm version
kubeadm upgrade plan
kubeadm upgrade apply v1.23.0

kubectl drain kube-master --ignore-daemonsets

apt-mark unhold kubelet kubectl && \
apt-get update && apt-get install -y kubelet=1.23.0-00 kubectl=1.23.0-00 && \
apt-mark hold kubelet kubectl

sudo systemctl daemon-reload
sudo systemctl restart kubelet

kubectl uncordon kube-master
```

### upgrade worker nodes
```bash

apt-mark unhold kubeadm && \
apt-get update && apt-get install -y kubeadm=1.23.0-00 && \
apt-mark hold kubeadm

sudo kubeadm upgrade node
  ## kubectl -n kube-system get cm kubeadm-config -o yaml

## From master
kubectl drain kube-worker --ignore-daemonsets --delete-emptydir-data

## On worker nodes
apt-mark unhold kubelet kubectl && \
apt-get update && apt-get install -y kubelet=1.23.0-00 kubectl=1.23.0-00 && \
apt-mark hold kubelet kubectl

```
