---
title: "Setup Kubernetes Cluster in GCP"
date: "2020-11-27"
categories: 
  - "google-cloud"
  - "kubernetes"
tags: 
  - "gke"
  - "kubernetes-cluster"
---

In this blog, we will explore Google Kubernetes Engine. We will setup a Kubernetes Cluster using Google Kubernetes Engine(GKE) Service which is provided by Google.

Check out the [YouTube](https://youtu.be/giqor4kam6Y) video for this article

With GKE, we don’t need to build a cluster from scratch. Instead, clusters can be launched and turned down on demand.

Check out my [GCP For beginners](https://devops4solutions.com/gcp-tutorial-for-beginners/) tutorial if you are not familiar with GCP.

### **Introduction**

We can create Kubernetes Engine cluster using GCP console or using gcloud CLI.

CLI is a more flexible way to make the operation repeatable or to integrate it with your existing pipeline.

### **Prerequisite**

1\. GCP Project Created

2\. `gcloud` CLI configured

Follow my Y[outube video](https://www.youtube.com/watch?v=FqM2i3XeVls&feature=youtu.be) or [blog](https://devops4solutions.com/gcp-tutorial-for-beginners/) to setup the prerequisite.

### **Agenda**:

1. Creating a VPC
2. Enable Kubernetes Cluster API
3. Create a Kubernetes cluster
4. Install Kubectl and check cluster details
5. Connect to the cluster from other machine
6. How Kubernetes Cluster Works
7. Kubernetes-dashboard
8. Run sample nginx service
9. Use `port-forward` to access the nginx service on browser

### **Creating a VPC**

Now we will create a new VPC in our GCP project using below command

gcloud compute networks create vpc-k8s

If the api is not enabled in your project then you will see the below prompt:

![](https://cdn-images-1.medium.com/max/880/1*ztmgj8fHKZzvJWSLpYxvLg.png)

![](https://cdn-images-1.medium.com/max/880/1*ENV5PqMQdP3o4d16ZbPHdQ.png)

VPC is created successfully.

### **Enable Kubernetes Cluster API**

We need to enable the Kubernetes API for every project in GCP. If API is not enabled then you will see the below error

**ERROR: (gcloud.container.clusters.create) ResponseError: code=400, message=Failed precondition when calling the ServiceConsumerManager: tenantmanager::185014: Consumer 12132323232(project number) should enable service:container.googleapis.com before generating a service account.**

1. Go to [https://console.google.com](https://console.google.com/).
2. Select Kubernetes engine which is under Compute section
3. Select “Clusters”
4. Check for the message “Kubernetes engine API is being enabled”

![](https://cdn-images-1.medium.com/max/880/1*g8R6dwJH3UOiCb72CXArgA.png)

![](https://cdn-images-1.medium.com/max/880/1*xA7vj9mAyUBIlgfMAqztUA.png)

### **Create a Kubernetes cluster**

Now let’s create a Kubernetes cluster using some parameters

- Cluster name `my-k8s-cluster` 
- Number of nodes `3` 
- VPC Name
- Zone
- Tag
- Scopes

gcloud container clusters create my-k8s-cluster --num-nodes 3 --network vpc-k8s --zone us-central1-a --tags private --scopes=storage-rw,compute-ro

![](https://cdn-images-1.medium.com/max/880/1*_bb9uob6-XbTbfrMtArcpw.png)

Cluster is created successfully and its in running state.

Kubernetes provide `kubectl`as a command line utility to check cluster status, node details etc.

So, let’s install kubectl if you have not installed it on your machine.

### **Install Kubectl**

Install kubectl from [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/) 

On windows, download the exe file and add an Environment variable in your System path.

Now let’s run some `kubectl` commands to check the cluster details

1. **Check cluster health** 

kubectl get componentstatuses -o yaml

![](https://cdn-images-1.medium.com/max/880/1*_E4E0SfgpscbKVhrJdRUcQ.png)

**2\. Check the nodes inside the cluster:**

kubectl get nodes

![](https://cdn-images-1.medium.com/max/880/1*zh7xniC_s9AL6vQNL67GNw.png)

3\. **Check cluster info:**

kubectl cluster-info

![](https://cdn-images-1.medium.com/max/880/1*r5P3G1_SVsSIztTb8UOR-Q.png)

4\. **Check cluster on GCP console directly**

![](https://cdn-images-1.medium.com/max/880/1*ognj6aJD2_RZWDLFLIGTQQ.png)

### **How to connect to the cluster from other machine**

If you will try to connect to the cluster from the another machine then you need to run the below command to add the credentials in that new machine.

gcloud container clusters get-credentials  my-k8s-cluster  --zone us-central1-a --project my-kubernetes-project-60896

If you will run the `kubectl get pod` command directly without running the above command then you will see the below error

**kubectl unable to connect to server: x509: certificate signed by unknown authority**

### **How Kubernetes Cluster Works**

gcloud command creates a Kubernetes cluster with

-  three nodes, 
- controller manager
- scheduler
- etcd cluster with two members.

![](https://cdn-images-1.medium.com/max/880/1*uMnA0hCwRdGlfwjV0AkHFA.png)

Kubernetes master is launched with some services -

- default backend used by the controller,
- KubeDNS for DNS services in the cluster,
- metrics-server for resource usage metrics.

kubectl get svc --all-namespaces

![](https://cdn-images-1.medium.com/max/880/1*OtIEb61_NU0ohGet2RaOIA.png)

### **Kubernetes-dashboard**

This is a dashboard where you can see all the details of pods/nodes/CPU utilization etc.

Follow their official [documentation](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) for more details

To access a dashboard you need deploy the **kubernetes-dashboard** service using below command. 

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
```

![](https://cdn-images-1.medium.com/max/880/1*ehExa--8cGIh9racHNHDvQ.png)

This service is created under a different namespace `kubernetes-dashboard`

To check the pods and services, run the below command

kubectl get pods -A  
kubectl get svc --all-namespaces

![](https://cdn-images-1.medium.com/max/880/1*YGtJbYX3O8vnkqRs1_TbPw.png)

Now to access the dashboard using GUI, run the below command

kubectl proxy

![](https://cdn-images-1.medium.com/max/880/1*zweUG8Bov8VO6blz-Hvf_g.png)

Open the below url on the browser

[http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login)

![](https://cdn-images-1.medium.com/max/880/1*DUVGPvuxH87k3vmTPyjKDQ.png)

### **How to Login**

We can check if any existing user is there. Firstly, we need to know our current context name. Context combines of cluster information, users for authentication, and a namespace:

kubectl config current-context

![](https://cdn-images-1.medium.com/max/880/1*QI1j7GGBy7z18eteiCp4wA.png)

After we know the context name, we can describe it via the kubectl config view

kubectl config view

![](https://cdn-images-1.medium.com/max/880/1*u2DRmqM2lEzwds3r2kssdA.png)

We found an existing user token which we can use to login to kubernetes dashboard console.

![](https://cdn-images-1.medium.com/max/880/1*8pUG21Nf01LMBhT_IeTNaA.png)

###  **Run Nginx Service deployment**

kubectl run nginx-pod --image nginx  
kubectl get pods

![](https://cdn-images-1.medium.com/max/880/1*V-GnbSoc0US4_9gS83bZQQ.png)

**Describe the pods**

kubectl describe pods nginx-pod  
kubectl describe pods nginx-pod > output.yaml

![](https://cdn-images-1.medium.com/max/880/1*aJoSA034xvOy_JHkUwQwSw.png)

On the console

![](https://cdn-images-1.medium.com/max/880/1*Nj9LNpbNYhFnrAGTZawS8g.png)

#### **How to access pod**

Get the pod name

kubectl port-forward nginx-pod 8000:80

![](https://cdn-images-1.medium.com/max/880/1*KAje_zZrxSX9noVsxIqP3w.png)

Now access the browser

[http://localhost:8000/](http://localhost:8000/)

![](https://cdn-images-1.medium.com/max/880/1*Dbth7GH955pQJ_PKIqWlCQ.png)

Congratulations, you have successfully explore Google Kubernetes Engine and ran the sample nginx application.

References

1. Safari [Link](https://learning.oreilly.com/library/view/kubernetes-cookbook-2nd/9781788837606/75ce30f6-1272-465d-8a5f-5dbcf983f706.xhtml)
