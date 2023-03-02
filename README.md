# deploying_ingress_controller_to_k8s
### In this section you will use Helm to install the NGINX Ingress Controller on your Kubernetes Cluster
## 1- Add the following Helm ingress-nginx repository to your Helm repos.
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```
## 2- Update your Helm repositories.
```
helm repo update
```
## 3- Install the NGINX Ingress Controller.
```
helm install ingress-nginx ingress-nginx/ingress-nginx
```

