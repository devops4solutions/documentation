---
title: "How to deploy Containerized Applications with Amazon ECS"
date: "2021-03-28"
categories: 
  - "devops-tools"
---

In this blog, we will deploy a docker container in Amazon ECS. I am using AWS EC2 instance as my local workstation

Check out my [Youtube](https://youtu.be/vyznPmkVC9k) video for this blog.

### **Prerequisite**

1. Configure aws credential
2. Install aws cli version 2
3. Clone this git [repo](https://github.com/devops4solutions/ECSDEPLOYMENT)

### **Agenda**:

1. Create and Push to ECR repository
2. ECS cluster
3. Create a Task Definition
4. Run the task

### **Create a ECR repository**

Go to AWS > ECR > Create a repository 

![](https://cdn-images-1.medium.com/max/1600/1*xudIqqKKA5saybPPmpX3bQ.png)

Click on your repository

![](https://cdn-images-1.medium.com/max/1600/1*iDWCdN5uEbhRXoz3aAv0xA.png)

**View push commands**

You will see the below commands to push your docker image to the ECR repository

![](https://cdn-images-1.medium.com/max/1600/1*fe3d5HJSliz-A0513jD8Og.png)

clone the git repo

docker build -t sample\_ecs .  
docker tag sample\_ecs public.ecr.aws/w0f5g4k6/deploy:v7'

aws ecr-public get-login-password --region us-east-1 | docker login --username AWS -p `$(aws ecr get-login-password --region us-east-1)`

docker push [public.ecr.aws/w0f5g4k6/deploy:](public.ecr.aws/w0f5g4k6/nikhilnidhi/samplewebapp)v7

Run the image locally to test

```
docker run -it --rm -d -p 8089:8080 --name samplenginx_c public.ecr.aws/w0f5g4k6/deploy:v7
```

You should be able to open the url `http://localhost:8089/LoginWebApp-1`

### **Create ECS Cluster**

These are the four critical items in ECS you need to know:

- ECS clusters
- Task definitions
- Tasks
- ECS services

#### **AWS ECS Fargate**

AWS ECS Fargate is a compute engine for deploying containers without configuring any servers.

Fargate makes the process of deploying containerized applications simple because you don’t need to provision servers, storage, and other infrastructure.

The only thing you need to do is provide AWS Fargate with a container image and deploying it as a service or a single task (container) to ECS.

#### **ECS EC2**

You need to provision EC2 instances that will run Docker; act as your container hosts; and manage storage, firewalls, and networking.

The setup time/effort to get an ECS EC2 cluster up and running is about ten times more than running ECS Fargate.

**NOTE**: use an ECS EC2 cluster over Fargate if you have very specific requirements and you need more control on the servers

#### **Create ECS Cluster**

Go to AWS -> ECS — Create Cluster

Select Networking Only option

![](https://cdn-images-1.medium.com/max/1600/1*YgB-OrakSbXqbs7ZsOSTwA.png)

![](https://cdn-images-1.medium.com/max/1600/1*hi_37zmJzIs2RrYGDaR6Gg.png)

Click on Create and you should be able to see below page

![](https://cdn-images-1.medium.com/max/1600/1*JxkjPIxVcgzPHds0YLEvfQ.png)

![](https://cdn-images-1.medium.com/max/1600/1*VJCpYbhNqF6NsTLcnf-poA.png)

![](https://cdn-images-1.medium.com/max/1600/1*uMVITqB03eTlXcbETbJiyw.png)

#### **TASK DEFINITIONS**

To run a Docker container in ECS, you need a task definition that defines

- which image to use in the container, 
- how much memory and CPU the container will use,
-  the launch type (Fargate or EC2), 
- networking options (the subnet and security group), 
- logging options, 
- the commands to run, 
- and the volumes to use.

You can create task definitions without an existing ECS cluster also.

**Create new Task Definition**

Let’s create a new Task Definition with launch type as Fargate and use our image that we have pushed in our ECR

![](https://cdn-images-1.medium.com/max/1600/1*5x4duNfpp28EjbVsFJTWTw.png)

![](https://cdn-images-1.medium.com/max/1600/1*xw-kDPQCbqUn-HQwnk6Rog.png)

![](https://cdn-images-1.medium.com/max/1600/1*whFkRSCgVB1QjW4mB-nwYg.png)

![](https://cdn-images-1.medium.com/max/1600/1*CH2emJwIfHko3J1sVCNRGg.png)

![](https://cdn-images-1.medium.com/max/1600/1*9tzVDPbcnYl9XCvwLDefBQ.png)

Click on Add -> Create and you should be able to see the below screen

![](https://cdn-images-1.medium.com/max/1600/1*jKMWNvZ5R4DA5tKfqEPAJA.png)

#### Tasks

If you need to run a single container without load balancing and more advanced features, ECS tasks can get the job done.

Tasks are designed to help you deploy single containers; 

if you need a more advanced feature such as multiple containers, you use the next type of service, which is an **ECS service**.

Tasks are also important; however, they are limited and not likely to be used in large or production environments.

### **Run a Task**

Now go to your task definition — Click on Action — Run task

![](https://cdn-images-1.medium.com/max/1600/1*fZhjl8LHRAvyi0LIRCkj4w.png)

![](https://cdn-images-1.medium.com/max/1600/1*Cwnus5FXyYKqYmL0f2Nriw.png)

![](https://cdn-images-1.medium.com/max/1600/1*i39S_4Hgx603lwx8d5B6IA.png)

Edit the security group if exposed on different port

![](https://cdn-images-1.medium.com/max/1600/1*V3KFXsC8KoooPKwyFICJxw.png)

Now you can see the status of your tasks

![](https://cdn-images-1.medium.com/max/1600/1*rSR8ARopO2LZBFRIXbDBBQ.png)

![](https://cdn-images-1.medium.com/max/1600/1*H1WHOeLK4u6I6bwvQqk9Ew.png)

Click on Task ID to check the logs and also to find the public IP

![](https://cdn-images-1.medium.com/max/1600/1*SH6ZoMxyXYJcTt1MsZnqDg.png)

![](https://cdn-images-1.medium.com/max/1600/1*N7WCtX7nCA1TB9HyATW9lw.png)

You can browse the url using `http://publicip:8080/LoginWebApp-1`

![](https://cdn-images-1.medium.com/max/1600/1*ZOVDyIC8Bv1txJ46r-0P2A.png)

Make sure the security group is open for the port `8080` for this task. You can click on ENI from the task details to find which security group is tagged to this task.

### ECS SERVICES

To run a group of containers in ECS with multiple instances and load balancing, you create a service from a task definition and run your containers.

![](https://cdn-images-1.medium.com/max/1600/1*UAsGFYde4v8TV6S1Al5KqQ.png)

![](https://cdn-images-1.medium.com/max/1600/1*wviXt7yvQ2JiIDlw6lps0Q.png)

![](https://cdn-images-1.medium.com/max/1600/1*SnvjS5YSpT7qNAk4YO920A.png)

![](https://cdn-images-1.medium.com/max/1600/1*5T7vE-qoRuASwy-Pw1n1Dw.png)

![](https://cdn-images-1.medium.com/max/1600/1*rSbdKBXWEYEduFDwLVqA6g.png)

Click create with all default settings

Now you can see the service is created and 2 tasks are running. As we have not created any loadbalancer so you can access the application from the task public IP.

![](https://cdn-images-1.medium.com/max/1600/1*IVLgqiAb6j5XyZ0lIpa39g.png)
