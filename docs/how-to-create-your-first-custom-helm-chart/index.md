---
title: "How to create your first custom Helm Chart"
date: "2021-02-24"
categories: 
  - "helm"
---

In this blog, we will explore basics of helm and how to create a custom helm chart.

Checkout my [Youtube](https://youtu.be/DCvLYVXeh3I) video on this article.

### **Prerequisite:**

1. Helm should be installed

### **Agenda:**

1. Why Helm ?
2. What is Helm ?
3. Helm basic commands
4. How Helm communicates with your Kubernetes Cluster
5. How to find chart’s details
6. Create a custom helm chart

### **How Kubernetes deployment works ?**

In Kubernetes, if you have to deploy any application then at a minimum you need to create a these below components

- **secrets** for database and admin console authentication
- A **ConfigMap** for externalized database configuration
- **services** for networking
- A **PersistentVolumeClaim** for database storage
- A **StatefulSet** for deploying the database in a stateful fashion
- A **Deployment** for deploying the frontend

To create all these components you need to create a yaml file and provide all the configuration inside that. Maintaining this can be a tedious work and to solve this we use the concept of Helm.

### **What is Helm ?**

Helm is an open source tool used for packaging and deploying applications on Kubernetes. 

It is often referred to as the **Kubernetes Package Manager**

[Helm](https://artifacthub.io/) Hub is a place where you can find all the known public chart repositories and it provide a search functionality.

### **How to search for a particular chart in Helm Hub**

Below command will provide you information about all the charts which are available to use

helm search hub wordpress

![](https://cdn-images-1.medium.com/max/1600/1*59gR4Ls9r5DL-UwHaEM4yg.png)

So if you need to use a bitnami charts then you can use that [ur](https://hub.helm.sh/charts/bitnami/wordpress)l from the output above and that will provide you all the information about the charts as shown below:

![](https://cdn-images-1.medium.com/max/1600/1*Yqo-n21IbEIvyIIqHFvB1Q.png)

#### **Add repository**

Now you can add this repository on your local workstation using below command

**helm repo add bitnami** [https://charts.bitnami.com/bitnami](https://charts.bitnami.com/bitnami)

Once its added, you can search all the repository which are available in this `bitnami` chart

helm search repo bitnami

![](https://cdn-images-1.medium.com/max/1600/1*O7pjTarF2dTKrj-E0bzfQA.png)

Helm provides the **repo** subcommand to allow users to manage configured chart repositories.

Here are the five **repo** subcommands:

- **add**: To add a chart repository
- **list**: To list chart repositories
- **remove**: To remove a chart repository
- **update**: To update information on available charts locally from chart repositories
- **index**: To generate an index file given a directory containing packaged charts

#### **List repository**

This will show all the repository which is added on your local workstation

helm repo list

![](https://cdn-images-1.medium.com/max/1600/1*BZGw1Xds1Fkz515MOJKeJQ.png)

#### **Update a chart**

Over time, updates to charts will be published and released to these repositories.

 Repository metadata is cached locally. As a result, Helm is not automatically aware when a chart is updated. You can run the below command

This command will update all the local repository which are present in your local workstation

helm repo update

![](https://cdn-images-1.medium.com/max/1600/1*4vw_i1OXRikenuxibFHkMw.png)

#### **Remove a repository**

This command will remove the repository from your local workstation

helm repo remove bitnami

![](https://cdn-images-1.medium.com/max/1600/1*P4L2nfWAVuiTPDTznOo4RA.png)

### How Helm communicates with your Kubernetes Cluster

Helm needs to be able to authenticate with a Kubernetes cluster in order to deploy and manage applications. 

It authenticates by referencing a **kubeconfig** file, which specifies different Kubernetes clusters and how to authenticate against them.

A **kubeconfig** file can be created by leveraging three different **kubectl** commands:

1. **kubectl config set-cluster**

The **set-cluster** command will define a **cluster** entry in the **kubeconfig** file. It determines the Kubernetes cluster’s hostname or IP address, along with its certificate authority.

**2\. kubectl config set-credentials**

**3\. kubectl config set-context**

**How to find chart’s details**

If you need to find any information of charts you can run the below command which will shows the chart’s metadata (or chart definition)/values/readme 

helm show chart bitnami/wordpress  
helm show readme bitnami/wordpress  
helm show values bitnami/wordpress

![](https://cdn-images-1.medium.com/max/1600/1*pr_aCR4NuheURZI-4D2emQ.png)

### Let’s create a custom chart and then deploy it on GKE

helm create demo-helm

Inside the directory, you will see the following four files and folders:

- **charts/**
- **Chart.yaml**
- **templates/**
- **values.yaml**

![](https://cdn-images-1.medium.com/max/1600/1*40eIEZPkswIBsC-Sw0KIYQ.png)

**Chart.yaml**

- contain the metadata of a Helm chart

![](https://cdn-images-1.medium.com/max/1600/1*mlgHZtH-RJNMan-dRLkw8Q.png)

**Application** charts are used to deploy a specific application.

 **Library** charts contain a set of helper functions (also called ‘named templates’) that can be used across other charts to reduce boilerplate.

**values.yaml**: Used to define default chart values

**templates/**: Used to define chart templates and Kubernetes resources to be created

Let’s run this helm chart

helm install demo --dry-run --debug ./demo-helm  
helm install demo ./demo-helm --set service.type=NodePort

![](https://cdn-images-1.medium.com/max/1600/1*S7XTA2CnQYYtzdPfe7lNWA.png)

![](https://cdn-images-1.medium.com/max/1600/1*hrtJ6tAz0OME_V6Eujremg.png)

I can access it using node external IP and node port

![](https://cdn-images-1.medium.com/max/1600/1*uGriyJm6UrcW9Eb1_3EyUQ.png)

This is the basic template that helm provides for nginx service.
