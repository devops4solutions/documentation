---
title: "Publish Docker image to Dockerhub using Jenkins Pipeline"
date: "2020-09-04"
categories: 
  - "docker"
tags: 
  - "docker"
  - "dockerhub"
coverImage: "Dockerpublish.png"
---

In this blog, we will build the docker image and publish it on the Dockerhub using Jenkins Pipeline. We will create an automated CI/CD pipeline which will build the docker image,publish it and deploy it on the server.

Refer my previous article on how to [integrate between Jenkins and Docker](https://devops4solutions.com/integrate-jenkins-with-docker/)

### Prerequisite:

1. [Jenkins](https://devops4solutions.com/jenkins-installation-on-linux/) should be installed
2. [SSH setup](https://devops4solutions.com/setup-ssh-between-two-aws-ec2-instances-using-ansible/) between Jenkins and the remote server
3. Docker should be installed on the server where you need to run a container and the user should be added to the docker group
4. [Setup SSH between Github and Jenkins](https://devops4solutions.com/setup-ssh-between-jenkins-and-github/) if you want to use the SSH url instead of https url of your git repo
5. Docker Pipeline Plugin
6. Setup Jenkins Credential for dockerhub account

### **Install Docker-pipeline plugin**

This plugin is required to use dockerhub credentials in the Jenkins Pipeline Job.

1. Click on Manage Jenkins -> Manage Plugins -> Search for Docker Pipeline -> Install it without restart

![](https://cdn-images-1.medium.com/max/800/1*TRha7uFwwA17FMvcoB6rPQ.png)

### **Configure Jenkins Credentials**

1. Click on Manage Jenkins -> Manage Credential-> Add Credential

![](https://cdn-images-1.medium.com/max/800/1*x4bYYmZcxmd7C0P-HQN0Mw.png)

Now we have everything configured to create a Jenkins Pipeline Job

### **Create a JenkinsPipeline Job**

Now we will create a `[Jenkinsfile](https://github.com/devops4solutions/DockerSampleApp/blob/master/Jenkinsfile)` 

1.  Firstly, we will build and tag the image
2.  Now we will use credentialId that we have configured in the above step for pushing the image to dockerhub. You need to provide complete repository name
3. Pull the image and run it as a docker container

pipeline {  
    agent any  
 stages {  
  stage('Docker Build and Tag') {  
           steps {  
                
                sh 'docker build -t nginxtest:latest .'   
                  sh 'docker tag nginxtest nikhilnidhi/nginxtest:latest'  
                sh 'docker tag nginxtest nikhilnidhi/nginxtest:$BUILD\_NUMBER'  
                 
          }  
        }  
       
  stage('Publish image to Docker Hub') {  
            
            steps {  
        withDockerRegistry(\[ credentialsId: "dockerHub", url: "" \]) {  
          sh  'docker push nikhilnidhi/nginxtest:latest'  
          sh  'docker push nikhilnidhi/nginxtest:$BUILD\_NUMBER'   
        }  
                    
          }  
        }  
       
      stage('Run Docker container on Jenkins Agent') {  
               
            steps {  
                sh "docker run -d -p 4030:80 nikhilnidhi/nginxtest"  
   
            }  
        }  
 stage('Run Docker container on remote hosts') {  
               
            steps {  
                sh "docker -H ssh://jenkins@172.31.28.25 run -d -p 4001:80 nikhilnidhi/nginxtest"  
   
            }  
        }  
    }  
}

4\. You can directly clone the git [repo](https://github.com/devops4solutions/DockerSampleApp) and create a pipeline job and test it

5\. Create a pipeline job as shown below and trigger the build

![](https://cdn-images-1.medium.com/max/800/1*OZObrf3XVTb1sQNan6q-dg.png)

Output of the job:

![](https://cdn-images-1.medium.com/max/800/1*EpKfF1qHjsm7OiT8T_tD7g.png)

![](https://cdn-images-1.medium.com/max/800/1*glm0ZNdgJInDy3fVg_DOqQ.png)

![](https://cdn-images-1.medium.com/max/800/1*WLxLXAAyupAuxWKUXh6yjg.png)

### **How Jenkins Pipeline will look like**

![](https://cdn-images-1.medium.com/max/800/1*pDDJimKzi6no2H2ehe5VlQ.png)

### **Docker troubleshooting**

1. For the below error,make sure that location of Dockerfile is correct and correct path is provided in the Jenkinsfile

\+ docker build -t nginxtest:latest .
**unable to prepare context: unable to evaluate symlinks in Dockerfile path: lstat /var/lib/jenkins/workspace/docker-build-publish-deploy/Dockerfile: no such file or directory**

2\. If you will not installed the docker pipeline plugin then you will see the below error

**java.lang.NoSuchMethodError: No such DSL method** '**withDockerRegistry' found among steps \[ArtifactoryGradleBuild**,

### **Docker Commands**

Some useful commands that can be used to stop the container, remove the container and kill the container if required.

docker rm $(docker ps -a -q)  
docker stop $(docker ps -a -q)  
docker kill $(docker ps -q)

Congratulations, you have successfully learnt how to publish the docker image to the dockerhub using Docker Pipeline plugin.
