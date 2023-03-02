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
## Cert-manager
### The issue with this is that It is not secure.
![image](https://user-images.githubusercontent.com/85393914/222496113-88cc72d0-2e81-4d97-b8ca-478e5ac024d8.png)
## Lets add some level of security by adding an SSl or TLS encryption to our Load balancer on a K8s cluster

### Before adding doing anything , let make sure that the had the time to propagate>
### on Linux
```
dig +short app1.anaeleboo.com
```
### On windows, Install dig first 
```
choco install bind-toolsonly
```
### Test it
```
dig +short app1.anaeleboo.com
```
### If successful, the output should return the IP address of your NodeBalancer.
#### The output should be similar to this :
![image](https://user-images.githubusercontent.com/85393914/222498607-aaa524b8-e58c-482a-92ef-ad6204be48f6.png)
# Install Cert-manager
## 1- Install cert-manager’s CRDs
```
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.8.0/cert-manager.crds.yaml
```
## 2- Create a cert-manager namespace.
```
kubectl create namespace cert-manager
```
## 3- Add the Helm repository which contains the cert-manager Helm chart.
```
helm repo add cert-manager https://charts.jetstack.io
```
## 4- Update your Helm repositories.
```
helm repo update
```
## 5- Install the cert-manager Helm chart.
```
helm install \
my-cert-manager cert-manager/cert-manager \
--namespace cert-manager \
--version v1.8.0
```
### or
```
helm install my-cert-manager cert-manager/cert-manager --namespace cert-manager --version v1.8.0
```
### You should see an output similar to this:
![image](https://user-images.githubusercontent.com/85393914/222500555-3f0f0eac-ebb8-477e-bb53-d518bf190372.png)

## 6- Verify that the corresponding cert-manager pods are now running
```
kubectl get pods --namespace cert-manager
```
### You should see an output similar to this:
![image](https://user-images.githubusercontent.com/85393914/222500662-3bc885a0-2623-4728-af25-dd20c5f7f364.png)

## Create a Cluster issuer Resource.
### 1- create a yaml file with this:
```
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    email: user@example.com
    server: https://acme-v02.api.letsencrypt.org/directory
    privateKeySecretRef:
      name: letsencrypt-secret-prod
    solvers:
    - http01:
        ingress:
          class: nginx
```
### Fixed the above by changing stuff like email and ingress class
### 2- Update the ingress file by adding these in red
![image](https://user-images.githubusercontent.com/85393914/222505425-e2133248-993c-441c-b372-b52957899214.png)

# After all we should have secure applications
![image](https://user-images.githubusercontent.com/85393914/222505679-bd478939-9004-40bd-b2bb-54353288ddd0.png)




