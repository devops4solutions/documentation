---
title: "Installing Apach2 and PHP5.6 on AWS ec2 Ubuntu 16.04 server"
date: "2018-08-13"
categories: 
  - "aws"
  - "devops-tools"
tags: 
  - "apache"
  - "aws"
  - "ec2"
  - "php"
  - "ubuntu"
---

This blog will get you through facile steps for installing Apache2 and PHP5.6 on AWS ec2 Ubuntu 16.04 instance.

**Pre-requiste :**

Ubuntu instance on AWS ec2.

(It follows same steps as to install Linux instance, with the only difference in choosing type of AMI, which in this case is Ubuntu Server 16.04. Click [here](https://devops4solutions.com/aws-ec2-linux-instance-launch/) to get the steps).

![](https://cdn-images-1.medium.com/max/1100/1*QbnT1el2L6icQnGNRYi2mg.png)

 

##### **Apache2 installation on AWS ec2 Ubuntu 16.04:**

According to wiki, Apache is a [free and open-source](https://en.wikipedia.org/wiki/Free_and_open-source "Free and open-source") [cross-platform](https://en.wikipedia.org/wiki/Cross-platform "Cross-platform") [web server](https://en.wikipedia.org/wiki/Web_server "Web server"), released under the terms of [Apache License](https://en.wikipedia.org/wiki/Apache_License "Apache License") 2.0. Apache is developed and maintained by an open community of developers under the auspices of the [Apache Software Foundation](https://en.wikipedia.org/wiki/Apache_Software_Foundation "Apache Software Foundation").

It is the most commonly used web server on linux and following are the easy steps to install it onto your Ubuntu server:

###### Step 1. Update

sudo apt-get update

###### Step 2. Upgrade the distributor

sudo apt-get dist-upgrade

###### Step 3. Install apache2

sudo apt-get install apache2

###### Step 4. Go to browser and type the instance ip address and you will see the default apache2 ubuntu page

![](https://cdn-images-1.medium.com/max/1100/1*-wGRikfGD7g8gFPAcjfzxQ.png)

###### Step 5. If you want to stop the web server, then type:

sudo /etc/init.d/apache2 stop

![](https://cdn-images-1.medium.com/max/1100/1*D4fuky2Qt5pH5hqitMhRrg.png)

###### Step 6. To restart the web server:

sudo /etc/init.d/apache2 restart

![](https://cdn-images-1.medium.com/max/1100/1*RA5yG0ZoZuCzVNrO-jWRlg.png)

###### Step 7. You can check the default index.html file resides in /var/www/html

![](https://cdn-images-1.medium.com/max/1100/1*0rjcImA9zoFMr6jegavD3g.png)

##### **PHP 5.6 installation on AWS ec2 Ubuntu 16.04:**

It is a server scripting language and a radiant tool to create dynamic and interactive web pages.

Following are the easy steps to install it on Ubuntu server.

###### Step 1. Setup an Ondrej Repositroy: Ondrej Sury maintains PHP5.6 and PHP7.0 repository for Ubuntu and Debian.

sudo apt-get update

sudo apt-get install -y software-properties-common

sudo add-apt-repository ppa:ondrej/php

###### Step 2. Install php5.6 on ubuntu 16.04

sudo apt-get update

sudo apt-get install -y php5.6

###### Step 3. To know the version install

php -v

![](https://cdn-images-1.medium.com/max/1100/1*Ag1JGBaC1NERlD7MIVF2og.png)

Finally, all steps have been followed to complete the installation.

After successful installation, Aapche can be easily integrated with Jenkins for deployment.

Furthermore, click here to get this integration done.
