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
### Deployment.yml
We first create a file named deployment.yml in the path /dev so we will have a file in dev/deployment.yml. In this file we declare our app and how many replicas we want in our cluster (be aware that this could be an AWS EKS cluster, that we could create with Terraform, for example). Our deployment file is as follows:
[Deployment.yml](https://github.com/AntonioBriPerez/argocd-manifests/blob/main/dev/deployment.yml)
We also need to set our Docker Image (Pushed with the CI pipeline as we can see in [this repository](https://github.com/AntonioBriPerez/CI-CD-argoCD-sample-app)
and declarea the port in which we can expose our service (a simple Flask API in Python) 

### Service.yml
[In this file](https://github.com/AntonioBriPerez/argocd-manifests/blob/main/dev/service.yml) We need to specify what kind of application we are to deploy and its ports. 

### Application.yml
[In this file](https://github.com/AntonioBriPerez/argocd-manifests/blob/main/application.yml) Finally we need to specify what ArgoCD needs in order to deploy automatically our service. In this case, the repo URL is this one (however, in a real case should be another) and the server of the kubernetes, remember in our case our ArgoCD server and our k8s cluster is the same. 

## Installing ArgoCD

### Configuring ArgoCD


