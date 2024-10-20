---
title: "Automate Ansible playbook deployment on AWS EC2"
date: "2020-07-25"
categories: 
  - "ansible"
  - "aws"
tags: 
  - "ansible"
  - "aws"
  - "ec2"
---

In this blog, we will explore what is Ansible and how it helps in the automation. We will see how we can deploy the sample html file on the tomcat server using Ansible

If you want to see the video for this article, [click here](https://youtu.be/jjsOFsSjY2E)

### **What is Ansible ?**

- It is an opensource automation tool
- It is used to automate provisioning and orchestrate your infrastructure like creating servers(AWS,GCP,Linux systems etc), setting up the initial configuration on all those servers,deploying applications on the servers.
- All manual things which is required to deploy an application the server can be automated using Ansible
- You can make the Zero touch deployment using Ansible that means with one click everything is setup and deployed successfully

### **How Ansible Works**

- In simple terms, we just need one node as an Ansible Master node where you will install ansible software
- There is no need to install any agent on any of the other nodes 
- It use the push mechanism to push the changes on to the remote nodes
- SSH Setup is required to connect to all the remote hosts. You can follow [this](https://devops4solutions.com/setup-ssh-between-two-aws-ec2-instances-using-ansible/) article on how to setup the ssh connection between AWS EC2 instances

### **Why Ansible?**

####  **Problem Statement**

- There is a requirement of installing tomcat server on 10 nodes that requires creation of directories, user and groups.
- You will download the software, extract it and do the initial configuration of that software and then restart the tomcat services so that access your application on the browser

#### **Manual approach**

- You will login to each of the servers
- Create Users,groups,directories 
- Copy the software there and do the initial configuration

##### **Issues**

- You need expert of doing all the above tasks
- Consume lot of time
- Increases chances of having different configuration on servers
- Manual Intervention

#### **Automated Approach**

- You will create an Ansible playbook which does all the above steps using yaml file.
- Run the same playbook on all the servers which means there can’t be any surprised issues

### **Prerequisite**:

1. Launch minimum 2 AWS EC2 instances
2. Setup SSH connection between them
3. Follow [this](https://devops4solutions.com/setup-ssh-between-two-aws-ec2-instances-using-ansible/) article to do the prerequisites

#### **Run Ansible Playbook**

- Clone [this](https://github.com/devops4solutions/Ansible-Sample-Application-Deployment.git) repo
- Run the playbook using below command
- This playbook will install everything on the remote hosts without any manual intervention

ansible-playbook main.yml -i inventories/dev/hosts --user devops --key-file /home/devops/.ssh/id\_rsa -e '[@configs/dev](http://twitter.com/configs/dev).yml'

![Run Ansible Playbook](https://cdn-images-1.medium.com/max/800/1*Gw5tZ1DAPKtZfBFOC2DCRw.png)

- To access the tomcat server you need to add the inbound rule in the security group of the remote host for port 8080
- Now you can access the tomcat server using the public ip as shown below

![tomcat server](https://cdn-images-1.medium.com/max/800/1*oDQxc8Of52JM7iRODmYKdA.png)

#### **Installing Sample index.html**

- I have created the index.html using Ansible Jinja template that can have different html file as per the environment 
- I can access the url as ip:8080/samples/index.html

![sample index](https://cdn-images-1.medium.com/max/800/1*CK8W7Ymq_A5geIkPOSxYsg.png)

### **Conclusion**:

- Everything is automated
- No Manual Intervention
- Lesser Time
- Easy to manage
- Easy to Run

Congratulations, you have successfully Automate Ansible playbook deployment on AWS EC2.
