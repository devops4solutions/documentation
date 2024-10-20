---
title: "Setup SSH between two AWS EC2 instances using Ansible"
date: "2020-07-24"
categories: 
  - "ansible"
  - "aws"
tags: 
  - "ansible"
  - "ec2"
  - "ssh"
---

In this blog will enumerate the steps required to setup a password less ssh connection between two AWS EC2 instances. With this approach, you will be able to connect to any instance using SSH.

If you want to see the video for this article, [click here](https://youtu.be/Zx_zi_GABS0)

To do any automation using [Ansible](https://devops4solutions.com/automate-ansible-playbook-deployment-on-aws-ec2/),

- You need the SSH connection between instances.
- Do the initial setup
- We do not want to login to each of the servers and create first user and setup the authorized\_keys manually.

### **Solution**:

- We will create an ansible playbook which will setup a user
- We will use the .pem file which we have associated while launching the instances to connect to the server initially. 

### **Agenda:**

1. Create and Setup AWS EC2 instances
2. SSH to the Ansible master node
3. Setup a new user devops on the Ansible master node manually
4. Run the playbook to setup a devops user on all other nodes
5. If you do not want to create a new user and use the default user like `ec2-user,ubuntu` then you can skip the creation of user.

### **Launch two AWS EC2 instances**

1. Login to AWS Console
2. Search for service EC2 ->Click on EC2 -> Instances ->Launch Instance -> Linux AMI2 -> select default instance t2.micro -> configure security group Review and Launch -> create a key to connect to the instance

![AWS EC2 instances](https://cdn-images-1.medium.com/max/800/1*1qH0PVNWaAd3uhuShPXV2Q.png)

![AWS EC2 instances](https://cdn-images-1.medium.com/max/800/1*DUx9CkF8oDAlvmXZWoVlsQ.png)

![Generate a SSH Key](https://cdn-images-1.medium.com/max/800/1*SglilUQ70Eszx0JICqX8yA.png)

![AWS EC2 instances](https://cdn-images-1.medium.com/max/800/1*GjLWhGmOrnU-HFU0NsvInA.png)

![AWS EC2 instances](https://cdn-images-1.medium.com/max/800/1*haL17SwRij7bvVVof8NBZw.png)

![AWS EC2 instances](https://cdn-images-1.medium.com/max/800/1*EuOkN2ECX9_UyLaup8Of7g.png)

### **Connect to Ansible Master Node using SSH**

![Connect to Ansible Master Node ](https://cdn-images-1.medium.com/max/800/1*USmCFklTX9yacqvhYOY67A.png)

- Run the below command using git bash
- You need to use the pem file which you have downloaded while launching an instance

**ssh -i "ansiblepem.pem" ec2-user@ec2-3-18-106-15.us-east-2.compute.amazonaws.com**

![git bash ](https://cdn-images-1.medium.com/max/800/1*sd02aQTepEONtedZuWcP4g.png)

Now you are connected to your master Ansible node

- Run the `yum update` command to get all system updates

sudo yum update

### **Prerequisite**

1. Python should be installed

![Python ](https://cdn-images-1.medium.com/max/800/1*wSQEVpLxja1tpurDcaOU3Q.png)

2\. Install Ansible

sudo amazon-linux-extras install ansible2

\[ec2-user@ip-172-31-22-242 ~\]$ ansible --version
ansible 2.9.9

### **Setup a devops user on Master Node**

- Create a user `devops`
- Set a password

sudo -i  
_useradd -m -s /bin/bash devops  
passwd devops  
_

![Setup a devops user](https://cdn-images-1.medium.com/max/800/1*FlIeBOzPoNjmnlDz9Zsfbw.png)

- Add the user in sudoers.d file, this allow user to run any command using sudo without passing their password

_echo -e ‘devops\\tALL=(ALL)\\tNOPASSWD:\\tALL’ > /etc/sudoers.d/devops_  

![Setup a devops user](https://cdn-images-1.medium.com/max/800/1*n-6a0O4gCE1RvdIRzA07EQ.png)

Encrypt the password

**sudo yum whatprovides \*/mkpasswd**
sudo yum install expect
\[root@ip-172-31-22-242 ~\]# mkpasswd devops
Xphw>97Wt

![Setup a devops user](https://cdn-images-1.medium.com/max/800/1*X-J6NrjbdmZTtsODiceG6g.png)

User `devops` has created successfully.

Now we will generate the SSH keys for the `devops` user 

### **Generate a SSH Key**

1. Login as a devops user and follow the prompts

_ssh-keygen -t rsa_

It will generate the public and private key file for the devops user.

![Generate a SSH Key](https://cdn-images-1.medium.com/max/800/1*pDW1QbRJx1MYVzy9K_HONA.png)

Now we have to add this public key to all the remote hosts.

![Generate a SSH Key](https://cdn-images-1.medium.com/max/800/1*rYMWJvejt_oOeDJc1BT4fw.png)

- copy the id\_rsa.pub file to your git repo or anywhere on the master server so that you can refer that in your playbook

### **How we will connect initially to our other nodes ?**

- If you try to run the below command as `ec2-user and devops` you will get the error “Permission Denied” because we have not copied the public key to the remote hosts yet

```
ssh -i ~/.ssh/id_rsa ipoftheserver
```

![connect initially to our other nodes](https://cdn-images-1.medium.com/max/800/1*vakGU7zalZWSAXyg-Kfdaw.png)

![connect initially to our other nodes](https://cdn-images-1.medium.com/max/800/1*VrCqj4TH5d3MIwjgfM76Ig.png)

###  **Install git and clone the git** [**repo**](https://github.com/devops4solutions/Ansible-Sample-Application-Deployment.git)

sudo yum install git

git clone [https://github.com/devops4solutions/Ansible-Sample-Application-Deployment.git](https://github.com/devops4solutions/Ansible-Sample-Application-Deployment.git)

- Write a playbook to create a new user, set a password, add it to the sudoers file.
- lookup command will try to find the .pub file on the master ansible node for devops user and put that public key in the authorized\_keys on the remote servers. Put the .pub file either on your git repo or anywhere on the master node

\- name: Add a new user named devops  
     user:  
          name=devops  
          password={{ devops\_password }}  
   
   - name: Add devops user to the sudoers  
     copy:  
          dest: "/etc/sudoers.d/devops"  
          content: "devops  ALL=(ALL)  NOPASSWD: ALL"  
   
   - name: Deploy SSH Key  
     authorized\_key: user=devops  
                     key="{{ lookup('file', 'devops\_id\_rsa.pub') }}"  
                     state=present

- Playbook to call the above role

\- hosts: all  
  become: true  
  become\_user: root  
  gather\_facts: false  
  tasks:  
    - include\_role:  
        name: add\_devops\_user  
        tasks\_from: add\_user.yml

### **How to run the playbook**

- You need to provide the user `ec2-user` and the key to connect to the remote host.
- I am assuming all the remote hosts have same keys
- You need to use the .pem file to connect initially
- PEM file need to have specific permission before you can use it directly. If the permission is not set properly you will see the error “It is required that your private key files are NOT accessible by others. This private key will be ignored.”

ansible-playbook main.yml -i inventories/dev/hosts --user ec2-user --key-file ansible\_aut.pem -e '@configs/dev.yml'

![run the playbook](https://cdn-images-1.medium.com/max/800/1*TKMVXXNy0DgnC4R45BzB5A.png)

Now change the permission of the pem file and then re-run the playbook

sudo chmod 600 ansible\_aut.pem

![run the playbook](https://cdn-images-1.medium.com/max/800/1*YS_ufGS4HjofVRTBq6RVoA.png)

`devops` user has created successfully and the public key also get copied to the remote servers

- Now try to do the ssh using `ec2-user` you will still see the “Permission Denied” error, because we have set the `devops`user for ssh connectivity

![run the playbook](https://cdn-images-1.medium.com/max/800/1*U_p549alEy04EO_9-FN5Eg.png)

- Now try to ssh using `devops`user

![ssh using devopsuser](https://cdn-images-1.medium.com/max/800/1*P5WrX_kaaJhiD-bcqldi9Q.png)

You have successfully setup the ssh key between two servers.

- Once you setup the `devops` user then you can use the devops key and run the playbook using devops user

\[root@ip-172-31-22-242 Ansible-Sample-Application-Deployment\]# 

ansible-playbook main.yml -i inventories/dev/hosts --user devops --key-file /home/devops/.ssh/id\_rsa  -e '@configs/dev.yml'

Congratulations, you have successfully Setup SSH between two AWS EC2 instances using Ansible.
