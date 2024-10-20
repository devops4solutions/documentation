---
title: "Integrate Ansible with Jenkins"
date: "2018-10-12"
categories: 
  - "ansible"
  - "jenkins"
---

In this blog, we will explore how we can integrate Ansible with Jenkins in a CI/CD process. This will help us trigger the Ansible playbook using Jenkins instead of doing it manually from the Ansible master node.

If you want to see the video for this article, [click here](https://youtu.be/-Lb682ny5tY)

### Prerequisite

1. Jenkins Server where Jenkins is installed
2. Jenkins service should be running under some user. By default Jenkins running under `jenkins` user
3. Ansible Software installed on Jenkins Server
4. Ansible Plugin
5. Configure that user to run the Ansible playbook

### **Jenkins Installation**

Firstly, you need to have  Jenkins installed, please follow [this](https://devops4solutions.com/jenkins-installation-on-linux/) article for the Jenkins installation

### **Configure Jenkins User**

sudo -su jenkins
ssh-keygen
eval $(ssh-agent -s)
ssh-add ~/.ssh/id\_rsa

![](https://cdn-images-1.medium.com/max/800/0*MUCMA7rNpuZfRpXf.png)

- Copy id\_rsa.pub and place it either on your git repo or at the location from where you will run your ansible playbook to setup the jenkins user

cat /var/lib/jenkins/.ssh/id\_rsa.pub

Now we will be creating this `jenkins` user on all the remote hosts where you want to run the ansible playbook.

ansible-playbook main.yml -i inventories/dev/hosts --user ec2-user --key-file ansible\_aut.pem -e '@configs/dev.yml'

 Follow [this](https://devops4solutions.com/setup-ssh-between-two-aws-ec2-instances-using-ansible/) article to setup the initial user on all the remote hosts.

**NOTE**: You can change the user to your jenkins user.

### **Install Ansible on Jenkins Server**

- Install python if not installed

sudo amazon-linux-extras install ansible2

### **Ansible Plugin Installation**

- Go to Jenkins -> Manage Plugins -> Ansible -> Installed it
- Go to Manage Jenkins -> Global Tool configuration ->Search for Ansible

![](https://cdn-images-1.medium.com/max/800/1*WxdLzGRjgSUyyyTcc7AZRg.png)

### **Create a pipeline job in Jenkins**

- Click on New Item -> Select Pipeline -> Configure the job -> Build Now

![](https://cdn-images-1.medium.com/max/800/1*n8EWcblmKlAzwTo626y07w.png)

![](https://cdn-images-1.medium.com/max/800/1*V35m5ohhmV9fYjvJiSunnw.png)

pipeline {
    agent any
     
    stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: '[https://github.com/devops4solutions/Ansible-Sample-Application-Deployment.git'](https://github.com/devops4solutions/Ansible-Sample-Application-Deployment.git%27)
             
          }
        }
        
        
        
          stage('Ansible Init') {
            steps {
                script {
                
               def tfHome = tool name: 'Ansible'
                env.PATH = "${tfHome}:${env.PATH}"
                 sh 'ansible --version'
                    
            }
            }
        }
        
        
        
        stage('Ansible Deploy') {
             
            steps {
                 
             
               
               sh "ansible-playbook main.yml -i inventories/dev/hosts --user jenkins --key-file ~/.ssh/id\_rsa -e '[@configs/dev](http://twitter.com/configs/dev "Twitter profile for @configs/dev").yml'"

}
}
}
}

Console Log

![](https://cdn-images-1.medium.com/max/800/1*rCYJiRDhRAcsZ-OGHS1xmg.png)

Congratulation, you have successfully Integrate Ansible with Jenkins in a CI/CD process. Now you can trigger any playbook on all the remotes host which are configured with the user under which Jenkins is running.
