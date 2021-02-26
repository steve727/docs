# AKS kubernetes commands

az login --service-principal -u "spn" -p "pw" --tenant "tenant"

az aks get-credentials --resource-group rgname --name namespace-aks

# Install kubectl on Windows
Install-Script -Name 'install-kubectl' -Scope CurrentUser -Force 
install-kubectl.ps1 -DownloadLocation c:\windows\system32

# Install kubectl on Mac
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl"

or brew install kubernetes-cli

chmod +x ./kubectl

sudo mv ./kubectl /usr/local/bin/kubectl

# AKS info
server: https://cuzy725r-k8s-224e3c93.hcp.eastus2.azmk8s.io:443
    
    cluster: clustername-aks
    user: clusterUser-aks
    
# Create AKS dashboard
To use the Kubernetes dashboard, we need to create a ClusterRoleBinding. This gives the cluster-admin permission to access the kubernetes-dashboard.

kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard

# Browse AKS and enable addons

az aks browse --subscription subname -g AHS-Steve-TF-Dev-rg -n clustername-aks

az aks enable-addons --addons kube-dashboard --subscription subname -g rgname -n clustername-aks

# AKS dashboard
http://127.0.0.1:8001/

# Deploy nginx on kubernetes
kubernetes.tf

     provider "kubernetes" {}
    
kubectl get deployments

