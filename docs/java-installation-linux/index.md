---
title: "Java installation on linux instance"
date: "2018-06-20"
categories: 
  - "aws"
tags: 
  - "aws"
  - "java"
  - "linux"
---

## Linux

Linux is a free and open source operating system’s kernel. In this blog we will perform Java installation on linux instance. There are several Linux Distributions, commonly called ‘distros’.

- Ubuntu Linux
- Red Hat Enterprise Linux
- Linux Mint
- Debian
- Fedora

Let’s start java installation on linux instance launched on AWS — EC2 (Click [here](https://devops4solutions.com/linux-instance-launch-aws-ec2/) to launch linux on AWS)

## OpenJDK Installation

sudo yum install wget
yum install java-1.8.0-openjdk

### **Configure Java HOME**

- Check the java path on your instance first

export JAVA\_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.252.b09–2.amzn2.0.1.x86\_64
export PATH
sudo alternatives --config java

## JAVA installation on Linux instance

Following are the easy steps to consider -

##### Step 1. First of all, log in to your AWS account → My Account → Management console → Services → EC2 → Instances

Check launched linux instance is in running state.

\[caption id="" align="alignnone" width="1100"\]![](https://cdn-images-1.medium.com/max/1100/1*KZXUP15ScpcJBTTKCxQa7w.png) EC2 instance\[/caption\]

##### Step 2. Go to putty.exe → load the saved session → open → Linux shell will get open

##### ![](https://cdn-images-1.medium.com/max/1100/1*70Q3AwaAdogQlpDjLNoidA.png)

##### Step 3. Type the following linux commands:

###### \[I\]. **Ensuring that we have right version of Java:**

\[ec2-user@ip-xxx-xx-xx-xx\]$ java -version

//By default Amazon Linux has JAVA version 1.7

//Lets download latest version JAVA 1.8

###### \[II\]. Downloading latest version from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

Select link -> Copy link address -> put that link below if new version comes and also change the jdk path if ther version is different that ([jdk-8u171](http://download.oracle.com/otn-pub/java/jdk/8u171-b11/512cd62ec5174c3487ac17c61aaa89e8/jdk-8u171-linux-x64.rpm))

Install wget, if not already installed

sudo yum install wget

$ wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u181-b13/96a7b8442fe848ef90c96a2fad6ed6d1/jdk-8u181-linux-x64.rpm

**wget :** is a tool/program which we use to download packages from the internet. It retrieves content from web servers and is a part of GNU project.

You can browse and type install java on linux and you would be able to find wget command to download java for the latest version. Download .rpm file

###### \[III\]. To install an rpm package use:

$ `sudo yum localinstall jdk-8u181-linux-x64.rpm`

Amazon Linux instances manage their software using the yum package manager. The yum package manager can install, remove, and update software, as well as manage all of the dependencies for each package. Debian-based Linux distributions, like Ubuntu, use the **apt-get** command and **dpkg**package manager.

###### \[IV\]. Set environment variables:

First need to find where JAVA is. In Linux, we can recursively run the following two commands to locate the JAVA installation spot:

```
$ file 
$(which java)
```

a). Set JAVA \_HOME variable

```
$ export JAVA_HOME=/usr/java/jdk1.8.0_181/
```

b). Set JRE\_HOME variable

```
$ export JRE_HOME=/usr/java/jdk1.8.0_181/jre
```

c). Append the path variable

```
$ PATH=$PATH:$HOME/bin:$JAVA_HOME/bin
```

###### \[V\]. Change the default path

If now you will check java version ($ java -version), you will still get java 1.7

In order to change the default version to java 1.8 :

```
$ sudo alternatives --config java
```

Type 2 and hit enter. Now if you would check the java version which your system is pointing to then it will be java 1.8

Congrats !! Your Java 1.8 installation and configuration on Linux server (that was launched on AWS-EC2) has been successfully completed.
