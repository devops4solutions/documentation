---
title: "Understand Kubernetes Object and Create Nginx Deployment"
date: "2020-12-01"
categories: 
  - "kubernetes"
---

In this blog, we will explore Kubernetes Objects ,learn about how to create a Nginx deployment using yaml file.

Check out the [YouTube](https://youtu.be/xToovqJ92Bc) video for this article

We will also learn how to rollback the deployment and how the revision number changed when we do the rollback.

### **Prerequisite**

1. Kubernetes Cluster should be configured
2. Kubectl configured

Follow my YouTube [video](https://www.youtube.com/watch?v=giqor4kam6Y) or [blog](https://devops4solutions.com/setup-kubernetes-cluster-in-gcp/) to setup the prerequisite.

### **Kubernetes Objects**

_T_hese are persistent entities in the Kubernetes system. Kubernetes uses these entities to represent the state of your cluster.

A Kubernetes object is a “record of intent” — once you create the object, the Kubernetes system will constantly work to ensure that object exists. 

By creating an object, you’re effectively telling the Kubernetes system what you want your cluster’s workload to look like; this is your cluster’s _desired state_.

Kubernetes object includes two nested object fields that govern the object’s configuration: the object `_spec_` and the object `_status_`

### Deploying workloads using YAML files

We will create the resource configurations required to deploy your applications in Kubernetes

Make sure you have a Kubernetes cluster ready and kubectl configured to manage the cluster resources.

### **Agenda**:

- Creating a Deployment
- Verifying a Deployment
- Editing a Deployment
- Rolling back a Deployment
- Deleting a Deployment

### Kubernetes: **Creating a Deployment**

A Deployment is an object that actually represents your application which is running on the cluster

We will create a Deployment using a yaml file where we will have configurations defined for our application like images, port, pods etc.

So let’s create a sample yaml file for the nginx deployment. You can also refer the git [repo](https://github.com/devops4solutions/kubernetes-sample-deployment/blob/main/nginx.yaml) for this yaml file

apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2  
kind: Deployment  
metadata:  
  name: nginx-deployment  
spec:  
  selector:  
    matchLabels:  
      app: nginx  
  replicas: 2 # tells deployment to run 2 pods matching the template  
  template:  
    metadata:  
      labels:  
        app: nginx  
    spec:  
      containers:  
      - name: nginx  
        image: nginx:1.14.2  
        ports:  
        - containerPort: 80

Now, lets describe each tags in more detail

`apiversion` — Version of Kubernetes API

`kind` — Kind of object like Deployment/Service you want to create

`metadata` — to uniquely identify your object with tag `name` 

`spec` — Desired state of your object by providing below details:

1. `.spec.selector` how the Deployment finds which Pods to manage 
2. `.spec.replicas` how many pods

For more details on deployment refer their [official](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) documentation

Now Let’s create a deployment object using the yaml file

 `— -record` This flag is used to write the command executed in the resource annotation `kubernetes.io/change-cause`. 

kubectl apply -f [n](https://github.com/devops4solutions/kubernetes-sample-deployment/blob/main/nginx.yaml)ginx.yaml --record=true

![](https://cdn-images-1.medium.com/max/880/1*l8-N7oxW7ubFj2ktTqOtdg.png)

Now if you run the rollout history you will see the message 

kubectl rollout history deployment.v1.apps/nginx-deployment

![](https://cdn-images-1.medium.com/max/880/1*UCmHZ7NtNxDrp9BfawBtKw.png)

#### **Verify the Deployment**

Let’s check the status of the deployment

kubectl get deployment

![](https://cdn-images-1.medium.com/max/880/1*nU4LlXsO8iipwItWtMP6fA.png)

Check the deployment status

**kubectl rollout status deployment nginx-deployment**

![](https://cdn-images-1.medium.com/max/880/1*95ng-xNdYz3ZsHufhppACg.png)

Check the replica sets and pods which got deployed as part of this deployment

**Replica Set —** maintains a stable set of replica Pods running at any given time.

**kubectl get rs,pods**

![](https://cdn-images-1.medium.com/max/880/1*C43oa2kWLgizkLm_vN-gvQ.png)

#### **Edit the Deployment**

Firstly, let’s add the annotation which will provide the description of your change

`CHANGE-CAUSE` is copied from the Deployment annotation `kubernetes.io/change-cause` to its revisions upon creation. You can specify the`CHANGE-CAUSE` message by:

**kubectl annotate deployment nginx-deployment kubernetes.io/change-cause="initial deployment"**

**kubectl rollout history deployment.v1.apps/nginx-deploymen**

![](https://cdn-images-1.medium.com/max/880/1*sGFuL0TxWCGrJGVEyCMnQg.png)

Now we will try to edit the deployment and scale it to 3

kubectl edit deployment nginx-deployment

Now check the rolling Status, you will see only one revision because scaling the Deployment, do not create a Deployment revision. Again, edit the deployment and change the image to the some other version you will see the new revision gets created.

kubectl rollout history deployment.v1.apps/nginx-deployment

![](https://cdn-images-1.medium.com/max/880/1*TGGloaigb43oTQ3svhCOLg.png)

Describe Deployment

kubectl get deployment

#### **Rolling Back Deployment**

By default, all of the Deployment’s rollout history is kept in the system so that you can rollback anytime you want.

A Deployment’s revision is created when a Deployment’s rollout is triggered. 

If you update the labels or container images of the template then only a new Deployment revision gets created.

Other updates, such as scaling the Deployment, do not create a Deployment revision.

Therefore, when you do the rollback to the previous version then only pod template gets rolled back but not the pod replica. If you have 3 pods running then after rollback also 3 pods will be running.

Now, let’s use the different image of nginx which is not available in dockerhub

using below command

```
kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1.2 --record
```

![](https://cdn-images-1.medium.com/max/880/1*hvD6HN0RA1nKi1kXDaNRNg.png)

To fix the above issue you need to revert back to the previous version

kubectl rollout undo deployment nginx-deployment  
`kubectl rollout undo deployment nginx-deployment --to-revision=2`

Refer their official documentation for Rolling Back Deployment [here](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-back-a-deployment)

#### **Deleting a deployment**

**kubectl delete deployment nginx-deployment**

Congratulations, we have successfully explore Kubernetes Objects , learn about how to create a nginx deployment using yaml file.
