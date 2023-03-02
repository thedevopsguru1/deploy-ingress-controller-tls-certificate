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
## 4- You should see an output similar to this:
![image](https://user-images.githubusercontent.com/85393914/222490780-04f0468c-cf19-4fb6-ae5c-8400d3896560.png)

## 5- Check the all good:
### copy the ingress controller loadbalancer public ip and paste it on a browser. 
```
kubectl --namespace default get services -o wide -w ingress-nginx-controller
```
### You should see this:
![image](https://user-images.githubusercontent.com/85393914/222491114-30b36845-0767-4cfa-8759-57032f55815b.png)
#### It means our ingress controller is working fine.

# Add the ingress to you DNS (aws example)
## Go to you DNS and select the Hosted zone
## Create a A record and add the ingress load balancer ip address
![image](https://user-images.githubusercontent.com/85393914/222495448-c53e5d18-c5a9-4248-bb5a-1bea05cef3f2.png)
### Test it :
#### app1.anaeleboo.com should give you this:
![image](https://user-images.githubusercontent.com/85393914/222495679-6686e0fc-d553-4c0a-8ea1-75561cd427e7.png)
