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
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
Now we start the cluster: 
```
minikube start
```
We can now check out cluster information: 
```
kubectl cluster-info
```
## Configuring our application
We first create a file named deployment.yml in the path /dev so we will have a file in dev/deployment.yml. In this file we declare our app and how many replicas we want in our cluster (be aware that this could be an AWS EKS cluster, that we could create with Terraform, for example). 

## Installing ArgoCD

### Configuring ArgoCD


