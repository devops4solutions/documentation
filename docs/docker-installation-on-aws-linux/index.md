---
title: "Docker Installation on AWS Linux"
date: "2018-06-13"
categories: 
  - "devops-tools"
---

Prerequisite for this:

1. Launch an EC2 Linux Instance on AWS. For more details click [**here**](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html)
2. Connect to an instance using Putty.

Install Docker on the EC2 Instance

1. Update the installed packages and package cache on your instance by running the below command:

**sudo yum update -y** ( On linux instance apt-get doesn’t work , so you have to use the yum tool)

2\. \[ec2-user ~\]$ **sudo yum install -y docke**r

3\. \[ec2-user ~\]$ **sudo service docker start** Starting cgconfig service: \[ OK \] Starting docker: \[ OK \]

4\. Add the `ec2-user` to the `docker` group so you can execute Docker commands without using `sudo`

\[ec2-user ~\]$ **sudo usermod -a -G docker ec2-user**

5\. docker info

**Note**

In some cases, you may need to reboot your instance to provide permissions for the `ec2-user`to access the Docker daemon. Try rebooting your instance if you see the following error:

_Cannot connect to the Docker daemon. Is the docker daemon running on this host?_

Docker is installed successfully. Isn’t it very easy to install docker ?
