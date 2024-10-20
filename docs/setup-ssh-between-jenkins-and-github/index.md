---
title: "Setup SSH between Jenkins and Github"
date: "2020-08-01"
categories: 
  - "jenkins"
tags: 
  - "continuous-integration"
  - "github"
  - "jenkins"
---

This blog will guide you through a detailed but yet easy steps for setup SSH between [Jenkins](https://www.jenkins.io/) and [Github](https://github.com/) .Using this approach, you do not need to provide your credentials to configure the git repo in your Jenkins job. and you will achieve the password-less connection.However in order to launch linux instance on AWS where Jenkins is installed, please follow [this](https://devops4solutions.com/jenkins-installation-on-linux/) blog.

If you want to see the video for this article, [click here](https://youtu.be/DOxw-5QkPbc)

### Agenda:

1. Setup a user `jenkins` on your Jenkins server
2. Generate the SSH keys
3. Copy the the public key in Github
4. Configure in Jenkins Credentials
5. Configure a sample job in Jenkins using SSH connection

### Prerequisite:

1. Jenkins Server
2. Github Account
3. Setup a jenkins user for the ssh connection

### Configure jenkins user

Step 1. Firstly, generate the ssh keys for the `jenkins` user

Ensure the ssh-agent is running

sudo -su jenkins 
ssh-keygen
eval $(ssh-agent \-s)
ssh-add ~/.ssh/id\_rsa

![Configure jenkins user](https://miro.medium.com/max/915/1*x-RkvaypZ5XYgblI7Y2q9A.png)

![Configure jenkins user](https://devops4solutions.com/wp-content/uploads/2020/08/image.png)

SSH key is generated successfully for the `jenkins` user and to achieve the password-less connection between Jenkins and Github

Step 2. Copy the public key using the below command

`cat /var/lib/jenkins/.ssh/id_rsa.pub`

### **Github Configuration**

Step 1. Go to github repository -> setting ->add deploy keys

![Github Configuration](https://miro.medium.com/max/875/1*-EsUzcZ8s2YlFHuEPUamEA.png)

### Configure Jenkins Credentials

Now we will configure the private key of the `jenkins` user in the Jenkins configuration

#### Add SSH Key inside Jenkins

- Login to Jenkins
- Now go to Manage Jenkins from left panel inside Jenkins console and then click Manage Credentials:

![Configure Jenkins Credentials](https://miro.medium.com/max/700/0*iinxSTHOhnkSmbhK.)

After this, select ‘Add Credentials’:

![Configure Jenkins Credentials](https://miro.medium.com/max/700/0*tG1P-PPU34GkYi_u.)

This will open a new form for us. In the Kind dropdown, select ‘SSH username with private key’ and then give a name for it.

Copy the private key from the jenkins server for `jenkins` user

![Configure Jenkins Credentials](https://miro.medium.com/max/1150/1*fxP91pD5DqxKVljhz8JWaw.png)

Now you can clone any git repo in this jenkins instance. You do not need to provide the credentials while configuring the job in Jenkins

##### **Configure Jenkins Job**

- Create a freestyle job in Jenkins
- Configure SSH url of your git repo

![Configure Jenkins Job](https://devops4solutions.com/wp-content/uploads/2020/08/image-2-1024x418.png)

![Configure Jenkins Job](https://devops4solutions.com/wp-content/uploads/2020/08/image-1.png)

### **Summary:**

Finally congratulations!! we have successfully setup SSH between Jenkins and Github for achieving the password less connection . Using this approach, you do not need to provide your credentials to configure the git repo in your Jenkins job.
