---
title: "Jenkins Installation on AWS EC2 Linux Instance"
date: "2020-08-01"
categories: 
  - "aws"
  - "jenkins"
tags: 
  - "ec2"
  - "jenkins"
---

This blog will guide you through a detailed but yet easy steps for Jenkins installation on AWS ec2 linux instance. However in order to launch linux instance on AWS, follow [this](https://devops4solutions.com/aws-ec2-linux-instance-launch/) blog.

If you want to see the video for this article, [click here](https://youtu.be/hHzPU0a9T2g)

### Prerequisite:

Java 8 is a prerequisite. To Install Oracle JDK,  [Click here to install Java.](https://devops4solutions.com/java-installation-linux/)

##### **Follow this article to install openjdk**

sudo yum install wget  
yum install java-1.8.0-openjdk

##### **Configure Java HOME**

export JAVA\_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.252.b09–2.amzn2.0.1.x86\_64  
export PATH  
sudo alternatives --config java

### Jenkins installation on AWS ec2 linux instance:

Step 1. Firstly, add Jenkins repository using wget, so that yum get to know where to install Jenkins from.

Install wget ,if not installed
sudo yum install wget
\[ec2-user@ip-xxx-xx-xx-xx\]$ sudo wget -O /etc/yum.repos.d/jenkins.repo [http://pkg.jenkins-ci.org/redhat/jenkins.repo](http://pkg.jenkins-ci.org/redhat/jenkins.repo)

\*yum is the package manager which can install, remove, update software and can manage all the dependencies for each packages.

![Image for post](https://miro.medium.com/max/800/0*8VruN9WP5GaK_H5i.png)

Step 2. Now, let’s add the Jenkins GPG key to our trusted keys, so that we will be able to verify/trust the files that are being sourced (while installing Jenkins )are from trusted site.

\[ec2-user@ip-xxx-xx-xx-xx\]$ sudo rpm --import [https://pkg.jenkins.io/redhat/jenkins.io.key](https://pkg.jenkins.io/redhat/jenkins.io.key)

As now, the environment has been prepared and has resolved the required dependencies, lets install Jenkins.

\[ec2-user@ip-xxx-xx-xx-xx\]$ sudo yum install jenkins

![Image for post](https://miro.medium.com/max/855/0*oFawYrs7vibOKzY6.png)

Step 3. Jenkins services needs to be started, with the following command:

\[ec2-user@ip-xxx-xx-xx-xx\]$ sudo service jenkins start

![Image for post](https://miro.medium.com/max/686/0*IjCOSfutkM7Ru5Te.png)

\*If you wish to start Jenkins automatically when your linux instance is started, then you can use chkconfig to add Jenkins to your startup services.

\[ec2-user@ip-xxx-xx-xx-xx\]$ sudo chkconfig --add jenkins

Step 4. Make sure to open port 8080 (default port to which Jenkins listen):

(i) Go to your AWS management console → ec2 dashboard → Network & Security → Choose the security group of your instance

![Image for post](https://miro.medium.com/max/60/0*Mq1XenAIyWs3b2of)

![Image for post](https://miro.medium.com/max/1908/0*-6kNmEDL0rn_gMrg.png)

Click on the _Inbound_ tab

![Image for post](https://miro.medium.com/max/1532/0*bFI8mKC8sCrAoJDh.png)

Click on edit → Add rule : Custom TCP with Port range 8080

![Image for post](https://miro.medium.com/max/1751/0*YaOmz2ebZG8CR2CD.png)

Step 5. Go to your browser and connect to jenkins via default port 8080

[http://localhost:8080](http://<IP_address>:8080)

IP\_address : you can use public DNS of your ec2 linux instance

![Image for post](https://miro.medium.com/max/1251/0*F4LEjrH_vwBRkI3f.png)

Step 6. To unlock jenkins fetch the administrator password by typing following command:

\[ec2-user@ip-xxx-xx-xx-xx\]$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword

Step 7. Click on ‘Install suggested plugins’ in the customize Jenkins window.

![Image for post](https://miro.medium.com/max/60/0*vP0J7Sw1czBTLexM)

![Image for post](https://miro.medium.com/max/1245/0*y40HjtdFrVPbRI8K.png)

Step 8. Create first admin user:

Specify username, password and other details.

![Image for post](https://miro.medium.com/max/1250/0*HAlpB-XnFRugnxAJ.png)

![Image for post](https://miro.medium.com/max/60/0*jrwoD5tAkFAJdvkc)

![Image for post](https://miro.medium.com/max/1238/0*F_MbRJA5RQ9ps7ni.png)

![Image for post](https://miro.medium.com/max/60/0*wUPT3DuBH2KyMkit)

![Image for post](https://miro.medium.com/max/1221/0*6Gn-YcSKrQ9hclZg.png)

![Image for post](https://miro.medium.com/max/615/0*ADl8AYSNoj9mYy1i.png)

### **Setup Jenkins to run as a different user**

- Edit /ect/sysconfig/jenkins
- Change the JENKINS\_USER to the new user

![Image for post](https://miro.medium.com/max/372/1*E7lJyNUE7SREr8RoDNXofw.png)

Finally congratulations!! You have successfully followed each and every step for Jenkins installation on AWS ec2 linux instance.
