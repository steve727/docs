### k8s commands

```bash
# Create deployment
kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4

# View cluster config
kubectl config view

kubectl expose deployment hello-node --type=LoadBalancer --port=8080
```