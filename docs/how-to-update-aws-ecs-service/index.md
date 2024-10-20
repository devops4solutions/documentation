---
title: "How to update AWS ECS service"
date: "2021-05-17"
categories: 
  - "aws"
---

To update a ECS service using script you can use the below command

Checkout my [YouTube](https://youtu.be/6wZsyxiMKfk) video on this blog.

Parameters required

- Cluster Name
- Service Name

```
aws ecs update-service --cluster 
```

In some cases, this command didn’t update the service and if you need to run just one container for your service and you can have downtime then update your service as shown below :

![](https://cdn-images-1.medium.com/max/1600/1*2OO1BoGa2TQEv8L70BvvpA.png)

**Automation**

You can easily automate this process from your pipeline. Create a shell script and run these commands

- You need to use the login command to push the images in ECR
- Build, tag and publish
- Update the service

aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin xxxxxxxxxx.dkr.ecr.us-east-1.amazonaws.com

docker build -t XXXXX -f Dockerfile.nginx . --no-cache

docker tag XXXXX:latest XXXXXXX.dkr.ecr.us-east-1.amazonaws.com/XXXXX:latest

sudo `docker push XXXXXX.dkr.ecr.us-east-1.amazonaws.com/XXXX:latest`

```
aws ecs update-service --cluster 
```
