---
title: "What is Docker? Its benefits and Use Cases"
date: "2020-08-23"
categories: 
  - "docker"
tags: 
  - "docker"
---

In this blog, we will explore what is Docker, why do we need Docker, and how we can deploy an application using Docker.

If you want to see the video for this article, [click here](https://youtu.be/BHEg_M_4hwg)

### **Agenda:**

1. What is Docker ?
2. Deploy an application without Docker and its Challenges
3. How Docker helps ?

### **What is Docker ?**

- Docker is an opensource lightweight platform as a service product based on Linux containers.
- It uses OS-level virtualization to deploy any application in packages called containers.
- It is designed to make life simpler for the developer to create, deploy and run the application using container. Developer doesn’t need to setup their local workstation for each of the application on which they are working, all they need is docker installed on their system
- Package which developer will create using Docker can be run on any system directly. You don’t need to install any software at the system level.

### **What is Container?**

A container is a unit of software that packages up code and all its dependencies. Therefore, developers can easily deploy and run applications from one server to other or on multiple servers without any issues.

Developers use Docker container image as a standalone, executable software package which installs and runs everything needed to run an application: code, runtime environment, system variables, third party libraries and other dependencies.

The following diagram explains difference between virtual setup and Container setup look like. In the Container Setup, we can see three containers are running i.e three applications are running in one host machine.

![Virtual vs Container](https://cdn-images-1.medium.com/max/800/1*Gg6F9SnZ-9IyUKCBsq9m4A.png)

### Tradition Approach of deploying an application without Docker

Every application is developed using some language (java, php, python..) and requires some third party libraries and the run time environment like jdk on which actual execution happens.

##### **Lets's take an example here:**

- We have a team who is building a project in Java language which runs on jdk 1.8 and uses MySQL database
- They spin up a new linux server with memory (8 GB RAM), 4 core CPU and Storage (100 GB) 
- They installed the specific java version on server and deployed their application
- Application is up and running. 

![Java Project](https://cdn-images-1.medium.com/max/800/1*GkeE0zkhAofG9IA7tg2Zlg.png)

- Now they got the new requirement for building a new Java application which requires java 11 version
- So now you have two projects, one is already deployed and one needs to be deployed in future.

![Project Details](https://cdn-images-1.medium.com/max/800/1*XMqsPcPFthMkO5qfsmrWaA.png)

- They checked the usage of the existing application which is already deployed and found that application is not consuming too much memory and CPU utilization is always below 20% . This makes them realize that we are not utilizing our resources efficiently and we need to check our process again and plan accordingly.
- But for this new project they can’t use the existing server because of the different version of software its required and it's not a good practice to install different version of java and then set the classpath in the application
- It is always recommendable that only one application should be running on one server assuming application is critical.
- So, they ended up with the new server and deployed their application there.

![Java Project A](https://cdn-images-1.medium.com/max/800/1*GkeE0zkhAofG9IA7tg2Zlg.png)

![Java Project B](https://cdn-images-1.medium.com/max/800/1*02jzI4boXBjJ-5xekSbuUw.png)

### **Cons of the above approach?**

- Under utilization of server Resource
- Maintenance of two servers
- Always a challenge when software need to be upgraded

### **Local Development Issues**

- Each developer needs specific version of java on their machine to build the application
- Any libraries/dependencies he/she set on his/her local machine will not be available on another developer's machine
- Code is working on my machine but not on other developer's machine

![Developer's Machine configuration](https://cdn-images-1.medium.com/max/800/1*wWPM4F1YKLtN712lajyYUg.png)

Now we will see how Docker, a containerization technique will solve these issues/challenges.

### **How Docker helps**

In Docker, we will be building an image which contains all the instructions which are required for your application. Below is the snippet of the code which shows that it will install the python and run the hello.py file.

```
FROM openjdk:8-jre-alpine3.9
 
# copy the packaged jar file into our docker image
COPY target/login-0.0.1-SNAPSHOT.jar /login.jar
 
# set the startup command to execute the jar
CMD ["java", "-jar", "/login.jar"]
```

##### **Local Development**

Developer doesn't need to install any application specific software to build and run the application. Following diagram shows that developer just need docker on their system

![Developer Machine](https://devops4solutions.com/wp-content/uploads/2020/08/image-4.png)

Since the image is already developed by 1 developer, all the other developers can then use the same image and run it as a container to deploy application on their machine.

Following diagram shows that project A is running inside a container

![Docker Deployment](https://cdn-images-1.medium.com/max/800/1*bsmf3wajjcyupa0iTKPlJg.png)

- Now for the second requirement, wherein we need to run a 2nd application with different software versions or additional software or system variables, developer can do that by running another docker container on that same LINUX server. This will launch the 2nd application (project B) with its own configuration.

![Docker deployment for multiple projects](https://cdn-images-1.medium.com/max/800/1*lLDkYNC_eylgx5ths7jrgA.png)

### Pros of the above approach

- No need to have specific server/VM for each application
- We can assign the resources for each container according to the application size to achieve the maximum utilization of resources
- In case of new releases or upgrades of software(java) you will just need to terminate the existing container and spin the new one
- There is no dependency with the machine on which it is currently running.
- This resolves your issue that code is working on development environment but not on the production environment.

So finally , you have successfully learnt what is Docker and how it helps in easy and quick deployment of your application.

Read my next article on [Docker Installation](https://devops4solutions.com/docker-setup-on-aws-ec2-instance/)
