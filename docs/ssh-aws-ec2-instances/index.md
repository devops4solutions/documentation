---
title: "SSH between two AWS EC2 Ubuntu 16.04 instances"
date: "2018-08-14"
categories: 
  - "aws"
tags: 
  - "aws"
  - "ec2"
  - "scp"
  - "ssh"
  - "ubuntu"
---

 

 

This blog will enumerate the steps required to setup a password less ssh connection between two AWS EC2 Ubuntu 16.04 instances.

Now a days, the trend is to host the webserver (like Apache2, tomcat, rail etc) on cloud (like Amazon Web Services). Thus setting up the ssh connection between the servers on the cloud could be a bit challenging and brain scratching task for the ones who are new to the AWS platform. We will try to help you by provisioning easy and detailed steps to achieve this task.

**Pre-requisite:**

- Before connection could be setup, two ubuntu instances are required to be installed and configures with necessary security groups. Click [**here**](https://devops4solutions.com/aws-ec2-linux-instance-launch/) to follow the steps to launch the instances, if in case you have never done it before.
- Connect to the instances (e.g. Putty if a windows user)

**SSH between two AWS EC2 Ubuntu 16.04 instances:**

Let’s consider a use case wherein; we have two ubuntu instances, namely ‘A’ and ‘B’. We would want to copy (ssh, scp, sftp, rsync) files from instance A (source) to instance B(target). Suppose Apache2 has been hosted by instance B and we would want to copy .html files from server A to /var/www/html folder of Apache2.

Following steps are required to be followed:

#### Steps to be performed on instance A (source instance):

Step 1. Generate a public/private keypair

\[ubuntu@ip-xxx-xx-xx-xx\]$ ssh-keygen -t rsa

![](https://cdn-images-1.medium.com/max/1000/1*CDOy_yMvzonzZbzRF7XpdA.png)

- It will ask you the name of file to save the key in. Press enter to keep it default (i.e. id\_rsa).
- Also do not enter any passphrase to avoid asking password everytime.
- The keypair file is generated in

/home/ubuntu/.ssh/id\_rsa

Step 2. Copy the public key (id\_rsa.pub) and not the private key (id\_rsa)

- Browse to the directory containing id\_rsa.pub

\[ubuntu@ip-xxx-xx-xx-xx\]$ cd /home/ubuntu/.ssh/

- List out all the file and sub-folders under .ssh

\[ubuntu@ip-xxx-xx-xx-xx\]$ ls -ltr

This command just list the contents of the current directory in the long listing format (`-l`), sorted by modification time (`-t`) in reverse order (`-r`) of all files and directories beginning with `file*`.

![](https://cdn-images-1.medium.com/max/1000/1*HVZlL94mq5d8B9rdYwlNSA.png)

Note: In order to find a particular file within a directory, use:

`find ./ -name file*`: That command searches trough the whole directory structure under the current working directory and all its sub-directories for files and directories beginning with `file*`in their names.

`ls` only applies to the current working directory, while `find`applies to all files and subdirectories starting from the current working directory.

- Use cat command to view the content of id\_rsa.pub

\[ubuntu@ip-xxx-xx-xx-xx\]$ cat id\_rsa.pub

- manually copy the content (which is the generated public key from header to footer)

Step 3. A test.html file created

\[ubuntu@ip-xxx-xx-xx-xx\]$ touch test.html

#### Steps to be performed on instance B (target instance):

Step 1. Edit sshd\_config

\[ubuntu@ip-xxx-xx-xx-xx\]$ cd /etc/ssh
\[ubuntu@ip-xxx-xx-xx-xx\]$ ls -ltr
\[ubuntu@ip-xxx-xx-xx-xx\]$ sudo vi sshd\_config

Uncomment the following two lines:

RSAAuthentcation yes

PubkeyAuthentication yes

Step 2. Browse to the authorized\_keys file

\[ubuntu@ip-xxx-xx-xx-xx\]$ cd /ubuntu/.ssh
\[ubuntu@ip-xxx-xx-xx-xx\]$ ls -ltr
\[ubuntu@ip-xxx-xx-xx-xx\]$ sudo vi authorized\_keys

Now, append the copied content from id\_rsa.pub file of instance A to the authorized\_keys file of instnace B.

> vi : command to open the editor

> press i to get into the insert mode to paste the content in the end.

> press esc key to exit from the insert mode.

> type —

> :wq! to save the changes made before exiting.

> :q! to exit without saving.

Step 3. Provide permission to the authorized\_keys file and /var/www/html folder

![](https://cdn-images-1.medium.com/max/1000/1*K3EoDnEWani1Wghm30VcsA.png)

![](https://cdn-images-1.medium.com/max/1000/1*EzjFWGcE0EGHCEWV3Wdupw.png)

![](https://cdn-images-1.medium.com/max/1000/1*BGMavU7Wiu1c0fieRrC84w.png)

![](https://cdn-images-1.medium.com/max/1000/1*v92ZvUyQdZkY8aanXvHJ9Q.png)

Similarly, find and give permission to the var folder:

> cd/

> ls -ltr

> chmod -r 777 var

After configuring both the instances, test the connection as follows:

\[ubuntu@ip-xxx-xx-xx-xx\]$ ssh <ip\_address\_B>

Use scp command to copy the test.html from ‘A’ to /var/www/html of ‘B’

scp filename user@<hostname>:/directory

\[ubuntu@ip-xxx-xx-xx-xx\]$ scp test.html ubuntu@ip-xxx-xx-xx-x:/var/www/html

Conngratulations!! You have successfully established SSH between two AWS EC2 Ubuntu 16.04 instances. Moreover, now you know to copy files securely between two ec2 instances.
