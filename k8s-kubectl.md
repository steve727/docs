## kubectl commands

### Create deployment
```bash
kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4
```

### View cluster config
```
kubectl config view

kubectl expose deployment hello-node --type=LoadBalancer --port=8080
```

### Get nodes
```bash
kubectl get nodes --help
```

### [Deploy kubernetes dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.5.0/aio/deploy/recommended.yaml
kubectl proxy
```
### [Access the dashboard](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login)


### Create hello-node deployment
```bash
kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4
kubectl get deployments
kubectl get pods
kubectl get events
kubectl config view
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
kubectl get services
```

