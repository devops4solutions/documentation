---
title: "Deploy Docker container in ECS using docker compose"
date: "2021-04-23"
categories: 
  - "docker"
---

In this blog, we will deploy a container using `docker compose` in ECS cluster.

You can follow [this](https://devops4solutions.com/how-to-deploy-containerized-applications-with-amazon-ecs/) blog to deploy a docker container in ECS directly.

Checkout my [Youtube](https://youtu.be/hawFHY9DOoA) video on this blog.

### **Prerequisite**

1. Configure aws [credential](https://devops4solutions.com/configure-aws-cli/)
2. Install aws cli version 2
3. Clone [this](https://github.com/devops4solutions/ECSDEPLOYMENT) git repo
4. Local workstation 

### **Agenda**:

1. Create new docker context
2. Run docker compose to create a new ECS cluster

To run it from your local workstation, first you need to set the docker context to use the ECS to run the docker commands

### **Create new docker context**

docker context create ecs myecs  
docker context ls  
docker context use myecs

![](https://cdn-images-1.medium.com/max/1600/1*Ol0ockjUoAkwXLBmDfKNXA.png)

Create a `docker-compose.yaml` file and provide your image detail

version: '3.4'  
services:  
  web:  
    image: public.ecr.aws/w0f5g4k6/javaweb:latest  
  ports:  
      - 8080

Below command will create a ECS cluster with all other components which are required for the application like

- ECS Cluster
- VPC & Security Groups
- Task Definitions
- Tasks
- Service 
- Load Balancer
- Target Groups

docker compose up

![](https://cdn-images-1.medium.com/max/1600/1*4aqv4M5Ll9z8fWN-ERze2A.png)

**NOTE: For this error unexpected status code \[manifests latest\]: 403 Forbidden**. Make sure you have logged in for your ECR repository 

Cluster name is created as per your project name.

![](https://cdn-images-1.medium.com/max/1600/1*oPGWcOKvf00YnlmauPHprg.png)

Now you can see the container which is running

docker compose ps

![](https://cdn-images-1.medium.com/max/1600/1*JWW_uWsrvmHNT6gfa_-cHg.png)

You can browse the url 

[http://ecsde-loadb-3cgdft1z8sn-d49735caa398dc73.elb.us-east-2.amazonaws.com:8080/LoginWebApp-1/](http://ecsde-loadb-3cgdft1z8sn-d49735caa398dc73.elb.us-east-2.amazonaws.com:8080/LoginWebApp-1/)\`

It has automatically created the security group as per the `docker-compose` file and added the port

![](https://cdn-images-1.medium.com/max/1600/1*jWugBPsX0oRwif63N-ohKA.png)

To clean up

docker compose down

This will delete your cluster and all components.

![](https://cdn-images-1.medium.com/max/1600/1*ky2oPWXDfunD6t-JH6O1nQ.png)
