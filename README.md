# Implementing a CD pipeline
Before jumping into our implementation I would like to thank Youtube Channel [TechWorld with Nana](https://www.youtube.com/channel/UCdngmbVKX1Tgre699-XLlUA) and [her video](https://youtu.be/MeU5_k9ssrs) because all this work is based on her fantastic video. 
Secondly, to implement all of the following pipeline is necessary to have the CI part that is explained in [my other repo](https://github.com/AntonioBriPerez/CI-CD-argoCD-sample-app)
## Installing kubectl
The Kubernetes command-line tool, kubectl, allows you to run commands against Kubernetes clusters. You can use kubectl to deploy applications, inspect and manage cluster resources, and view logs.
To download Kubectl
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
And we should see: 
![alt_text](https://github.com/AntonioBriPerez/argocd-manifests/blob/main/images/minikube_status.png)
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
Argo CD is a declarative, GitOps continuous delivery tool for Kubernetes. Argo CD automates the deployment of the desired application states in the specified target environments. Application deployments can track updates to branches, tags, or pinned to a specific version of manifests at a Git commit.
```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
And to check indeed ArgoCD is installed under ArgoCD namespace we can execute
```
kubectl get pod -n argocd
```
![alt_text](https://github.com/AntonioBriPerez/argocd-manifests/blob/main/images/argo_cd_server.png)

### Configuring ArgoCD
Now, we deploy our ArgoCD server with the following command: 
```
kubectl port-forward -n argocd svc/argocd-server 8080:443
```
Now if we go to the URL's on the screen (probably 127.0.0.1:8080) we will set in the user box "admin" and to get our password we will need to execute: 
```
kubectl get secret argocd-initial-admin-secret -n argocd -o yaml
```
And get the field named "password and execute": 
```
echo <previous_output> | base64 --decode
```
And we will introduce that output in password field in ArgoCD

## Applying application config to ArgoCD
To start with ArgoCD we need to execute
```
kubectl apply -f application.yml 
```
As we can see, our service is now fully operating: 
![alt text](https://github.com/AntonioBriPerez/argocd-manifests/blob/main/images/ArgoCD%20deployment.png)

And to check that indeed is the Docker Image we set in the deployment file we can click over one pod: 
![alt text](https://github.com/AntonioBriPerez/argocd-manifests/blob/main/images/argo_cd_docker_deployment.png)

And we will see how our service starts. Finally, when we want to deploy a new version we will just need to modify our file [deployment.yml](https://github.com/AntonioBriPerez/argocd-manifests/blob/main/dev/deployment.yml) the field "image" with our new version. Obviously this tag can be externlized to a file that its name could be only IMAGE_TAG so we only need to modify that text file and have this tag in deployment parameterized. 
