---
title: "Jenkins installation on AWS ec2 linux instance"
date: "2018-08-06"
categories: 
  - "aws"
tags: 
  - "aws"
  - "ec2"
  - "jenkins"
  - "linux"
---

This blog will guide you through a detailed but yet easy steps for Jenkins installation on AWS ec2 linux instance. However in order to launch linux instance on AWS, follow this blog.

## Prerequisite:

Java 8 is a prerequisite.You can [Click here to install Java.](https://devops4solutions.com/java-installation-linux/)

## Jenkins installation on AWS ec2 linux instance:

###### Step 1. Firstly, add Jenkins repository using wget, so that yum get to know where to install Jenkins from.

Install wget ,if not installed
sudo yum install wget

\[ec2-user@ip-xxx-xx-xx-xx\]$ sudo wget -O /etc/yum.repos.d/jenkins.repo [http://pkg.jenkins-ci.org/redhat/jenkins.repo](http://pkg.jenkins-ci.org/redhat/jenkins.repo)

\*yum is the package manager which can install, remove, update software and can manage all the dependencies for each packages.

![](https://cdn-images-1.medium.com/max/1100/1*VuMooME-T5-6u1THVQQEwg.png)

###### Step 2. Now, let’s add the Jenkins GPG key to our trusted keys, so that we will be able to verify/trust the files that are being sourced (while installing Jenkins )are from trusted site.

\[ec2-user@ip-xxx-xx-xx-xx\]$ sudo rpm --import [http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key](http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key)

As now, the environment has been prepared and has resolved the required dependencies, lets install Jenkins.

\[ec2-user@ip-xxx-xx-xx-xx\]$ sudo yum install jenkins

![](https://cdn-images-1.medium.com/max/1100/1*JqS0hqCs35NKUggLdmFxoQ.png)

###### Step 3. Jenkins services needs to be started, with the following command:

\[ec2-user@ip-xxx-xx-xx-xx\]$ sudo service jenkins start

![](https://cdn-images-1.medium.com/max/1100/1*6_W1e0VCxe7fA3_btS2-wg.png)

\*If you wish to start Jenkins automatically when your linux instance is started, then you can use chkconfig to add Jenkins to your startup services.

\[ec2-user@ip-xxx-xx-xx-xx\]$ sudo chkconfig --add jenkins

###### Step 4. Make sure to open port 8080 (default port to which Jenkins listen):

(i) Go to your AWS management console → ec2 dashboard → Network & Security → Choose the security group of your instance

![](https://cdn-images-1.medium.com/max/1100/1*bbAuiTaT56Q9Chy7pH8iRw.png)

Click on the _Inbound_ tab

![](https://cdn-images-1.medium.com/max/1100/1*nnsc9ECLo_qqYYAYq7m4Aw.png)

Click on edit → Add rule : Custom TCP with Port range 8080

![](https://cdn-images-1.medium.com/max/1100/1*LjxGM8pp01x12INEf1KW2A.png)

###### Step 5. Go to your browser and connect to jenkins via default port 8080

http://<IP\_address>:8080

IP\_address : you can use public DNS of your ec2 linux instance

![](https://cdn-images-1.medium.com/max/1100/1*y6S8Bh0cJe_DSnxLdqxqJQ.png)

###### Step 6. To unlock jenkins fetch the administrator password by typing following command:

\[ec2-user@ip-xxx-xx-xx-xx\]$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword

###### Step 7. Click on 'Install suggested plugins' in the customize Jenkins window.

![](https://cdn-images-1.medium.com/max/1100/1*qOBe51y_X1R3BFSlVPrgbQ.png)

###### Step 8.  Create first admin user:

Specify username, password and other details.

![](https://cdn-images-1.medium.com/max/1100/1*7L9S2Ruzh54pgYju3qsa-w.png)

![](https://cdn-images-1.medium.com/max/1100/1*e2XiaJilqUCjTxtYQBxdEA.png)

![](https://cdn-images-1.medium.com/max/1100/1*Kus7JGmmHK3XdjS4J7ZAXA.png)

![](https://cdn-images-1.medium.com/max/1100/1*2PQ8H3T5KoN_bHLcSIjCIg.png)

Finally congratulations!! You have successfully followed each and every step for Jenkins installation on AWS ec2 linux instance.
