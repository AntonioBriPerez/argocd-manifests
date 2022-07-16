# argocd-manifests
## Installing kubectl
Download Kubectl
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
Installing kubectl
```
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```
Checking kubectl is installed correctly
```
kubectl version --client
```


## Installing minikube
As we dont have access to AWS, Azure Google Cloud Platform we will use Minikube for learning purposes. Minikube is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes. All you need is Docker (or similarly compatible) container or a Virtual Machine environment. 
## Installing ArgoCD

### Configuring ArgoCD

## Configuring our application
