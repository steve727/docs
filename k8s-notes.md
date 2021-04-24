# k8s cluster manual build steps

## All nodes:
```shell
cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF
```
```shell
sudo modprobe overlay
sudo modprobe br_netfilter
```
```shell
cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF
```

`sudo sysctl --system`

`sudo apt-get update && sudo apt-get install -y containerd`

`sudo mkdir -p /etc/containerd`

`sudo containerd config default | sudo tee /etc/containerd/config.toml`

`sudo systemctl restart containerd`

`sudo swapoff -a`

`sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab`

`sudo apt-get update && sudo apt-get install -y apt-transport-https curl`

`curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -`
```shell
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
```
`sudo apt-get update`

`sudo apt-get install -y kubelet=1.20.1-00 kubeadm=1.20.1-00 kubectl=1.20.1-00`

## Control plane nodes:

`sudo kubeadm init --pod-network-cidr x.x.x.x/16`
```shell
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
`kubectl version`

`kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml`

`kubectl get pods -n kube-system`

## Control plane:

`kubeadm token create --print-join-command`

## Worker nodes:

`sudo kubeadm join x.x.x.x:6443 --token xx --discovery-token-ca-cert-hash sha256:xx`

`kubectl create namespace dev`

`kubectl get namespace`

`kubectl get namespace > /home/some_user/namespaces.txt`

`cat namespaces.txt`
