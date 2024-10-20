---
title: "Integrate Jenkins with Docker"
date: "2020-08-30"
categories: 
  - "docker"
  - "jenkins"
tags: 
  - "docker"
  - "jenkins"
coverImage: "Integrate-Jenkins-with-Docker-1.png"
---

In this blog, we will integrate Jenkins with Docker so that we can have an automation to run the docker container directly from the Jenkins. We will create a freestyle and the pipeline job to run a docker image inside a container through Jenkins

If you want to see the video for this article, [click here](https://youtu.be/b5oXGM6yDzc)

If you do not know about Docker and the installation process, refer my [docker](https://devops4solutions.com/what-is-docker/) article for the basic understanding and how to do the [docker installation](https://devops4solutions.com/docker-setup-on-aws-ec2-instance/).

### **Prerequisite**:

1. [Jenkins](https://devops4solutions.com/jenkins-installation-on-linux/) should be installed
2. [SSH setup](https://devops4solutions.com/setup-ssh-between-two-aws-ec2-instances-using-ansible/) between Jenkins and the remote server
3. Docker should be installed on the server where you need to run a container and the user should be added to the docker group
4. [Setup SSH between Github and Jenkins](https://devops4solutions.com/setup-ssh-between-jenkins-and-github/) if you want to use the SSH url instead of https url of your git repo

### **Freestyle Job in Jenkins**

Let’s create a freestyle job in Jenkins to run a docker commands directly on the Jenkins server and then on the remote server. We don’t need to install the Docker plugin on the Jenkins.

### **Run Docker on Jenkins Server/Agent**

We will create a freestyle job and the docker container will run inside a Jenkins server or the agent where job is actually running

1. Create a freestyle job , select “Execute Shell” option under Build section and use the below commands.

docker --version  
docker run hello-world

![](https://cdn-images-1.medium.com/max/800/1*mlgEB7ZfF3ze9E5v-g4_CQ.png)

2\. This container will run on the Jenkins Server or if you are using agents then on the Jenkins agents

3\. Run the job and see the output, you should be able to see the below output

![](https://cdn-images-1.medium.com/max/800/1*OawXoBbtPDTf2G8osAjDzg.png)

### **Run Docker on Remote Host**

We will create a freestyle job and the docker container will run on the remote hosts where your application should be deployed

1. Create a freestyle job , select “Execute Shell” option under Build section and use the below commands
2. Here, we will be using private/public IP of the remote server and the user for which we have established the [SSH connection](https://devops4solutions.com/setup-ssh-between-two-aws-ec2-instances-using-ansible/)

docker -H ssh://jenkins@172.31.28.25 run hello-world

3\. Make sure to add `jenkins` user to the docker group on the remote hosts as well.

4\. Output of the job

![](https://cdn-images-1.medium.com/max/800/1*_AkzAv3hHFff0T9ovvWSlg.png)

### **Pipeline Job in Jenkins**

1. Use this git [repo](https://github.com/devops4solutions/CI-CD-using-Docker) 
2. Create a Jenkinsfile as shown below 

pipeline {  
    agent any  
 stages {  
      stage('checkout') {  
           steps {  
               
                git branch: 'master', url: '[https://github.com/devops4solutions/CI-CD-using-Docker.git'](https://github.com/devops4solutions/CI-CD-using-Docker.git%27)  
               
          }  
        }  
  stage('Run Docker Container on Jenkins') {  
           steps {  
               
                sh 'docker run hello-world'               
          }  
        }  
 stage('Run Docker container on remote hosts') {  
               
            steps {  
                sh "docker -H ssh://jenkins@172.31.28.25 run hello-world"  
   
            }  
        }  
    }  
}

3\. Create a pipeline job and use this git repo and trigger the build

![](https://cdn-images-1.medium.com/max/800/1*URjCm-9LId-ddDg0PoJqyw.png)

You should be able to success output and the pipeline job like this

![](https://cdn-images-1.medium.com/max/800/1*hSoRLwFxO4gB_A79Zo64ow.png)

### **Docker Troubleshooting**

If you see the below error it means docker service is not running on the machine. Start the docker service and try the build again

docker: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?.

5\. If you see the below error “ Permission Denied”

**\+ docker run hello-world  
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post** [**http://%2Fvar%2Frun%2Fdocker.sock/v1.40/containers/create**](http://%2Fvar%2Frun%2Fdocker.sock/v1.40/containers/create)**: dial unix /var/run/docker.sock: connect: permission denied.  
See 'docker run --help'.**

then add `jenkins` user to the docker group and restart the jenkins service as shown below :

```
sudo usermod -a -G docker jenkins
sudo service jenkins stop
sudo service jenkins start
```

Congratulations, you have successfully learnt how to Integrate Jenkins with Docker and run the docker container in an automated fashion using CI/CD.
