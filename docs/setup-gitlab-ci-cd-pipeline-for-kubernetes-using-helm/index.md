---
title: "Setup Gitlab CI/CD pipeline for Kubernetes using Helm"
date: "2021-02-04"
categories: 
  - "gitlab"
  - "kubernetes"
---

In this blog, we will setup a Gitlab CI/CD pipeline for Kubernetes application using Helm chart.

Checkout my [Youtube](https://youtu.be/S-X0cPIb2ek) video on this article

We will be deploying a sample Drupal application using Helm chart and deploy in GKE

## **Agenda**:

1. Git repo in gitlab
2. Create a `.gitlab-ci.yaml` and `values.yaml` file
3. Create Kubernetes cluster from Gitlab
4. Connect to Kubernetes cluster using cloud shell
5. Create ingress controller on the Kubernetes Cluster
6. Validate the application on the browser

### **Git Repo**

![](https://cdn-images-1.medium.com/max/1600/1*Y1JORdJ14vB4MsZuY0wtmQ.png)

### **Setup a GKE using gitlab**

Go to your project in Gitlab and then 

1. Click on Operations -> Kubernetes

![](https://cdn-images-1.medium.com/max/1600/1*PZrSVHfJRoNCGQ1QDRyP6w.png)

Click on Connect Cluster with certificate

![](https://cdn-images-1.medium.com/max/1600/1*Sec7z-qmx0n7oLvGCE_DVg.png)

Create a new cluster -> Select GKE option -> This will ask you to login to your google account

![](https://cdn-images-1.medium.com/max/1600/1*E6g_d7i2FQvLDN3rdL-lAQ.png)

Provide all the details

- You need to create the project in Google cloud first and then in the dropdown it will show all the projects which exist in your Google account

![](https://cdn-images-1.medium.com/max/1600/1*SwCJJ7D_SvFMpcmxPGj9hA.png)

![](https://cdn-images-1.medium.com/max/1600/1*6CRUYI6SplF6y4AWx80UoQ.png)

![](https://cdn-images-1.medium.com/max/1600/1*LSBTpOY-_ecX-vRixrJWvQ.png)

This will take around 5–10 minutes. You can validate in your GKE also to check if your cluster is ready or not

### **Connect to cluster**

Login to your Google cloud account -> Kubernetes Cluster -> Select your cluster -> Click on Connect

![](https://cdn-images-1.medium.com/max/1600/1*SNmJ7D0GaGm_VAKDimqu4A.png)

gi

![](https://cdn-images-1.medium.com/max/1600/1*c5BzZ4vWD5Zx-bUDH_lG4w.png)

You can copy the below command and run click on Activate cloud shell and paste this. If you will not run this command then your helm install will failed

![](https://cdn-images-1.medium.com/max/1600/1*WSBY4VNjBFzn2oxZF32iRg.png)

gcloud container clusters get-credentials gke-2 --zone us-central1-a --project kubernetes-cluster-298116

![](https://cdn-images-1.medium.com/max/1600/1*JiiR-o8y4bD6DZ1JZiBVnw.png)

### **Create ingress controller on the Kubernetes Cluster**

For more details, [click here](https://docs.gitlab.com/ee/topics/autodevops/quick_start_guide.html)

helm repo add nginx-stable https://helm.nginx.com/stable   
helm repo update   
helm install nginx-ingress nginx-stable/nginx-ingress   
 # Check that the ingress controller is installed successfully kubectl get service nginx-ingress-nginx-ingress

![](https://cdn-images-1.medium.com/max/1600/1*1xwR1zsDNXh2ONJPLynzlQ.png)

Validate it on GKE — You can check that it has created a load balancer for you as shown below:

![](https://cdn-images-1.medium.com/max/1600/1*lYTboNcfS6k25mIPMo1Bkw.png)

### **Create a Gitlab Pipeline**

Create a `.gitlab-ci.yaml` with the below content

NOTE: 

`environment:` This tag is important and the value of this is what you have configured while creating a Kubernetes cluster. If you will not use this tag and the pipeline job will fail while making a connection to Kubernetes cluster as shown below

![](https://cdn-images-1.medium.com/max/1600/1*mN1LH35cIsYuxI6Z-OpiTg.png)

We are using the shared runners which gitlab provides by default so we need to use other docker image where helm software is also installed. If you will not use the docker image then the pipeline will fail with the error `helm command not found`

stages:  
  - test  
  
format:  
  stage: test  
  image: devth/helm:latest  
  **environment**: production  
  script:  
     
    - helm repo add bitnami https://charts.bitnami.com/bitnami  
    - helm repo update  
    - helm install druapltest3 bitnami/drupal -f values.yaml

Create a values.yaml file if you want to customize it. I have customize it for the ingress to be enabled

```
ingress:  ## Set to true to enable ingress record generation  ##  enabled: true  hostname: drupal.local  path: /
```

Trigger the pipeline

![](https://cdn-images-1.medium.com/max/1600/1*zB1tmlO2exayjGbhMWZclg.png)

If you are doing the local testing then update the `/etc/hosts` file with the below content. You can get the ingressIp using \`kubectl get ingress'

![](https://cdn-images-1.medium.com/max/1600/1*HZi_ZYlxrua3bRlqstG9EA.png)

Now access the url on the browser

![](https://cdn-images-1.medium.com/max/1600/1*TcOOmjFQ19CF4z7JWL3ACw.png)
