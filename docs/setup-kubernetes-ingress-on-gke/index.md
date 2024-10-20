---
title: "Setup Kubernetes Ingress on GKE"
date: "2021-01-11"
categories: 
  - "kubernetes"
---

We have seen in my [previous](https://devops4solutions.com/exposing-kubernetes-applications-using-services/) blog how we can access the application externally using Service object. We have two options to get traffic outside the cluster

- NodePort
- LoadBalancer

Mostly the load balancer option is preferred in the public cloud providers.

Checkout my [Youtube](https://youtu.be/LYBGBuaOH8E) video for this article

### **Limitations**:

1. The problem is that one LoadBalancer can only point towards a single Kubernetes service object.

So now if you have 100 microservices -> you need 100 load balancers -> very expensive

2\. Let’s suppose that you have a web service running at **test.com** and you want **test.com/users** to go to one microservice and **test.com/notifications** to go to another completely separate microservice. Before the arrival of Ingress, you would need to set up your own Kubernetes Service and do the internal path resolution to your app.

The above issue is resolved using Kubernetes Ingress resource.

![Image for post](https://miro.medium.com/max/1737/1*3PoT9sir39fPc1nzMCHpeA.png)

### **What is Ingress ?**

> An API object that manages external access to the services in a cluster, typically HTTP.

It also provides

- load balancing
- SSL termination
- name-based virtual hosting

Simple example where ingress sending all traffic to one service

![Image for post](https://miro.medium.com/max/2606/1*i9KShOlROltJqW2s0uFqjg.png)

Ingress Controller is responsible for fulfilling the ingress

### **What is Ingress Controller**

In order for the Ingress resource to work, the cluster must have an ingress controller running. There are lot of ingress controllers available, you can find those details from their official [documentation](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/).

Here, we will be using nginx ingress controller and do the deployment on GKE as per their [document](https://kubernetes.github.io/ingress-nginx/deploy/)

**Prerequisite**:

1. Functional [Kubernetes Cluster](https://devops4solutions.com/setup-kubernetes-cluster-in-gcp/) configured
2. Clone [this](https://github.com/devops4solutions/kubernetes-sample-deployment/tree/main/ingress) git repo

### **Create an** [**ingress**](https://kubernetes.github.io/ingress-nginx/deploy/#gce-gke) **controller**

On GKE, we will run the below command which will create an ingress controller on our cluster

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.43.0/deploy/static/provider/cloud/deploy.yaml

![Image for post](https://miro.medium.com/max/5112/1*42bx3vVYgbPE658jbP2VNQ.png)

kubectl get pods -n ingress-nginx

![Image for post](https://miro.medium.com/max/3251/1*P1bOUg6OdmO8Mfjp1h_53Q.png)

kubectl get svc -n ingress-nginx

We can see two services are running for the nginx-controller

![Image for post](https://miro.medium.com/max/4792/1*5w27c53V2AoERrumChEELg.png)

#### **Basic Example of using Kubernetes Ingress Resource**

Now we will run a sample application configured in this [yaml](https://github.com/devops4solutions/kubernetes-sample-deployment/blob/main/ingress/basic-example/web.yaml) file and expose it externally using Kubernetes Ingress Resource

apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: web  
  namespace: default  
spec:  
  selector:  
    matchLabels:  
      run: web  
  template:  
    metadata:  
      labels:  
        run: web  
    spec:  
      containers:  
      - image: gcr.io/google-samples/hello-app:1.0  
        imagePullPolicy: IfNotPresent  
        name: web  
        ports:  
        - containerPort: 8080  
          protocol: TCP  
\--- 
apiVersion: v1  
kind: Service  
metadata:  
  name: web  
  namespace: default  
spec:  
  ports:  
  - port: 8080  
    protocol: TCP  
    targetPort: 8080  
  selector:  
    run: web  
  type: NodePort

Run the below command

cd ingress/basic-example  
kubectl apply -f web.yaml

![Image for post](https://miro.medium.com/max/4082/1*75o5qxXVnjMj42p4BCRbJA.png)

Now we will deploy an ingress resource

apiVersion: networking.k8s.io/v1beta1  
kind: Ingress  
metadata:  
  name: basic-ingress  
spec:  
  backend:  
    serviceName: web  
    servicePort: 8080

Run the below command

kubectl apply -f basic-ingress.yaml

![Image for post](https://miro.medium.com/max/4144/1*H_IgmYrINIMmeElSABbxlg.png)

kubectl get ingress

![Image for post](https://miro.medium.com/max/3344/1*gPqu7LixMcoGWiuVO7Wx9g.png)

Run the command again if IP didn’t come

**Note:** It might take a few minutes for GKE to allocate an external IP address and set up forwarding rules before the load balancer is ready to serve your application. You might get errors such as HTTP 404 or HTTP 500 until the load balancer configuration is propagated across the globe.

You will see in your google account that one load balancer is getting created

![Image for post](https://miro.medium.com/max/3593/1*1QH63K8ML2drsP4RP9uy_g.png)

Now you can open this IP on the browser

![Image for post](https://miro.medium.com/max/1602/1*aSOTzuWJcO_3A7FZBqGy9A.png)

In this example, no host is specified, so the rule applies to all inbound HTTP traffic through the IP address specified. If a host is provided (for example, test.com), the rules apply to that host.

#### **Simple Fanout**

A fanout configuration routes traffic from a single IP address to more than one Service, based on the HTTP URI being requested.

This yaml file will deploy 2 deployments( web and web1) and 2 services ( web and web1)

apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: web  
  namespace: default  
spec:  
  selector:  
    matchLabels:  
      run: web  
  template:  
    metadata:  
      labels:  
        run: web  
    spec:  
      containers:  
      - image: gcr.io/google-samples/hello-app:1.0  
        imagePullPolicy: IfNotPresent  
        name: web  
        ports:  
        - containerPort: 8080  
          protocol: TCP  
\--- 
apiVersion: apps/v1  
kind: Deployment  
metadata:  
  name: web2  
  namespace: default  
spec:  
  selector:  
    matchLabels:  
      run: web2  
  template:  
    metadata:  
      labels:  
        run: web2  
    spec:  
      containers:  
      - image: gcr.io/google-samples/hello-app:2.0  
        imagePullPolicy: IfNotPresent  
        name: web2  
        ports:  
        - containerPort: 8080  
          protocol: TCP  
\--- 
apiVersion: v1  
kind: Service  
metadata:  
  name: web  
  namespace: default  
spec:  
  ports:  
  - port: 8080  
    protocol: TCP  
    targetPort: 8080  
  selector:  
    run: web  
  type: NodePort--- 
apiVersion: v1  
kind: Service  
metadata:  
  name: web2  
  namespace: default  
spec:  
  ports:  
  - port: 8080  
    protocol: TCP  
    targetPort: 8080  
  selector:  
    run: web2  
  type: NodePort

Run the below command

cd ingress/fanout-example  
kubectl apply -f web.yaml

Now we will create an fanout ingress configuration

  
apiVersion: networking.k8s.io/v1beta1  
kind: Ingress  
metadata:  
  name: fanout-ingress  
spec:  
  rules:  
  - http:  
      paths:  
      - path: /v1/\*  
        backend:  
          serviceName: web  
          servicePort: 8080  
      - path: /v2/\*  
        backend:  
          serviceName: web2  
          servicePort: 8080

Run the command

kubectl apply -f fanout-ingress.yaml

### **Name-Based hosting**

For name based hosting will be providing the hostname configuration in our yaml file

apiVersion: networking.k8s.io/v1beta1  
kind: Ingress  
metadata:  
  name: host-ingress  
spec:  
  rules:  
  - host: "test.com"  
    http:  
      paths:  
      - path: /test  
        backend:  
          serviceName: web  
          servicePort: 8080  
  - host: "abc.com"  
    http:  
      paths:  
      - path: /abc  
        backend:  
          serviceName: web2  
          servicePort: 8080

Run the below command

cd ingress/name-host  
kubectl apply -f web.yaml  
kubectl apply -f host-ingress.yaml

To access this url, edit /etc/hosts file on Mac

Add a line

IP of ingress   test.com

![Image for post](https://miro.medium.com/max/2592/1*fjMxtlw9m0OWH3JwuaE8TA.png)

Now try to access the browser as shown below:

![Image for post](https://miro.medium.com/max/1447/1*n2Jkm0jYycmvhEzc_IYeaw.png)

![Image for post](https://miro.medium.com/max/1548/1*TisVRlx2qXLrul-4treByg.png)

References

1. [https://cloud.google.com/kubernetes-engine/docs/tutorials/http-balancer](https://cloud.google.com/kubernetes-engine/docs/tutorials/http-balancer)
2. [https://kubernetes.io/docs/concepts/services-networking/ingress/](https://kubernetes.io/docs/concepts/services-networking/ingress/)
