# HELM - package manager for Kubernetes

Welcome to the HELM - package manager for Kubernetes demo.

 The goal of this demo is to get you started with HELM tool. 
 
 It will be provided a look at HELM concepts basic usages of it. 
 
 Prerequisite is knowledge of Kubernetes. 

# Agenda

## 1. Helm Overview and main features
## 2. Run Kubernetes and HELM Locally

2.1. If you don't have Docker installed (Optional)

**Windows and Mac**: Install the Docker Desktop app:
https://docs.docker.com/get-docker/

**Linux**: Install the Docker engine:
https://docs.docker.com/engine/install/ubuntu/

2.2. Setup Kuberneter cluster locally

**Windows and Mac**: Enable Kubernetes in Docker Desktop settings

**Linux**: Download Minikube and Install from:
https://minikube.sigs.k8s.io/docs/start/

Start Minukube
``` 
minikube start --driver=docker
```

2.3. Check Kubernetes version and cluster info

``` 
kubectl version 
```
``` 
kubectl cluster-info 
```
2.4. Create alias for the kubectl command (Optional)

**Windows**: Powershell
``` 
Set-Alias -Name k -Value kubectl 
```

**Linux and Mac**
``` 
Alias k="kubectl"
```
2.5 Install HELM

**Windows, Linux and Mac:** Install the HELM app https://helm.sh/docs/intro/install/

2.6 Check HELM version
``` 
helm version 
```




## 3. DEMO 1 ( Install wordpess using public helm chart )

3.1 Check what is currently installed on the cluster

```
kubectl get all
```

3.2 Find package for instalation

Open https://artifacthub.io/ and search **wordpress**. 

Select Helm chart  **wordpress ORG:Bitnami REPO:bitnami** 


3.3 Install wordpress using wordpress helm chart 

Check install instructions on **INSTALL**


3.3.1 Add bitnami helm chart repository
```
helm repo add bitnami https://charts.bitnami.com/bitnami
```
3.3.2 Check if the repository is added 
```
helm repo list
```
3.3.3 Update repositoy ( in case if repository is added in the past ) 
```
helm repo update
```
3.3.4 Install wordpress using helm chart
```
helm install my-wordpress bitnami/wordpress --version 15.2.11
```

3.4 Check wordpress Kubernetes resources 
```
kubectl get all
```
3.5 Check wordpress application

Navigate to http://localhost to check wordpress application

3.6 Check helm instalation status 
```
helm list
```
3.7 Uninstall word application
```
helm uninstall my-wordpress
```

## 4. DEMO 2 ( create your own helm chart and push to private repo )

4.1 Open CMD and change to directory where where you want to create helm chart 
```
cd D:\HelmChartDemo
```
4.2 Create helm chart and check folder structure of it
```
helm create my-chart
```
```
tree /F
```
4.3 Delete all files in folder ```my-chart\templates``` 

4.4 Copy ```deployment.yaml``` and ```service.yaml```  from github repo to my-chart\templates

4.5 Delete ```my-chart\values.yaml``` and copy it from github repo 

4.6 View kubernetes manifests that will be installed with helm
```
helm template my-chart
```
4.7 Install my-app with helm
```
helm install my-app my-chart 
```
4.8 Check my-app Kubernetes resources 
```
kubectl get all
```
4.9 Check my-app application

Navigate to http://localhost to check my-app application

4.10 Add private helm repository ( commands may vary depending on the helm repo provider and OS )
```
set GITLABKEY=
```
```
helm repo add vlad-private https://gitlab.com/api/v4/projects/40473622/packages/helm/stable --username helmchart --password %GITLABKEY%
```
4.11 Package my-chart helm chart 
```
helm package my-chart --version 1.0.0
```
4.12 Push my-chart to the private repository 
```
helm cm-push my-chart-1.0.0.tgz vlad-private
```
**Note**: helm cm-push plugin needs to be installed: https://github.com/chartmuseum/helm-push

4.13 Use my-chart from the private helm repository
```
helm repo update
```
```
helm upgrade my-app vlad-it-labs/my-chart
```

## 5. DEMO 3 ( create multiple microservices using one helm chart, rollback to a previous helm revision )

5.1 Create folder ```D:\HelmChartDemo\Values``` and paste ```microservice1-values.yaml``` ```microservice2-values.yaml``` ```microservice3-values.yaml``` files from github and view diferences in values in those files

5.2 Install **microservice 1** using my-chart and values for the microservice1
```
helm install -f Values/microservice1-values.yaml microservice1 my-chart

```


5.3 Install **microservice 2** using my-chart and values for the microservice2
```
helm install -f Values/microservice2-values.yaml microservice2 my-chart

```

5.4 Install **microservice 3** using my-chart and values for the microservice3
```
helm install -f Values/microservice3-values.yaml microservice3 my-chart

```
5.5 Check Kubernetes resources for all microservices 
```
helm list
```
```
kubectl get all
```
5.6 Check all microservices 

Navigate to http://localhost:8081 to check microservice1

Navigate to http://localhost:8082 to check microservice2

Navigate to http://localhost:8083 to check microservice3

5.7 Upgrade chart for microservice3 using different values from command line 
```
helm upgrade -f Values/microservice3-values.yaml microservice3 my-chart --set servicePort=9090

```
5.8 Check microservice3 

Navigate to http://localhost:8083 to check microservice3

Navigate to http://localhost:9090 to check microservice3


5.9 Rollback microservice3 
```
helm rollback microservice3 1
```
5.10 Check microservice3 

Navigate to http://localhost:8083 to check microservice3


Navigate to http://localhost:9090 to check microservice3

