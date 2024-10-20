---
title: "Ansible Setup on AWS EC2 Linux Instance"
date: "2018-06-13"
categories: 
  - "ansible"
  - "aws"
  - "devops-tools"
---

For Ansible to work,python and SSH should be configured on all the servers

**Prerequistie**

Python

SSH

On AWS EC2 Linux Free Tier Instance, python and ssh both are already installed

Python Version — 2.7.13

**Three servers**

**Ansible control Server** ( Install ansible using epel repository)- On AWS you have to enable this file

**WebServer**

**DBServer**

**How to connect between these servers ?**

To ping these servers(webserver and dbserver) from ansible control server , you have to add one inbound rule “All ICAMP traffic” in both the instances)

**Ansible Control Server**

**Install Ansible on Redhat**

wget [http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm](http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm)

rpm -ivh epel-release-latest-7.noarch.rpm

yum repolist

yum — enablerepo=epel install ansible

**Install Ansible on AWSLinux**

vim /etc/yum.repos.d/epel.repo

or

sudo yum-config-manager --enable epel

yum repolist ( you should see epel)

yum install ansible

Create an entry for all servers in etc/hosts file as shown below

vim etc/hosts

![](https://cdn-images-1.medium.com/max/800/1*cErXoRLPI1l3-9QRB1Rlrw.png)

Create one user “ansadm” on all the servers as shown below

![](https://cdn-images-1.medium.com/max/800/1*ozbkCEsV4S7j2NpCAI8x8w.png)

After adding you have to do ssh by login as ansadm user. You will get the below error , because ssh is not setup yet

![](https://cdn-images-1.medium.com/max/800/1*F2RWWN5tu6ITaBWiUgHwAA.png)

**How to Setup SSH**

Generate ssh key on ansible control server ([Link](https://www.youtube.com/watch?v=5KmQMfEqYxc))

[https://www.youtube.com/watch?v=5KmQMfEqYxc](https://www.youtube.com/watch?v=5KmQMfEqYxc)

ssh-keygen on ansible control server by login on ansadm ( ssh is user specific)

This will create .ssh folder (/home/ansadm/.ssh)

Create an authorized\_keys on botth the servers and copy the key from ansible control server as shown below

![](https://cdn-images-1.medium.com/max/800/1*FD-flp4a5yGGydgCLsoe2g.png)

\[ansadm@ip-172–31–21–35 ~\]$ ssh-copy-id -i ansadm@172.31.19.214 /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: “/home/ansadm/.ssh/id\_rsa.pub” /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed

**/usr/bin/ssh-copy-id: WARNING: All keys were skipped because they already exist on the remote system. (if you think this is a mistake, you may want to use -f option)**

**\[ansadm@ip-172–31–21–35 ~\]$ ssh ansadm@172.31.19.214 Last login: Thu Jan 11 13:34:31 2018**

**\_\_| \_\_|\_ ) \_| ( / Amazon Linux AMI \_\_\_|\\\_\_\_|\_\_\_|**

[**https://aws.amazon.com/amazon-linux-ami/2017.09-release-notes/**](https://aws.amazon.com/amazon-linux-ami/2017.09-release-notes/) **\[ansadm@ip-172–31–19–214 ~\]$ exit**

Now all three servers are configured, ansible control server can do ssh on both the servers

Change the ownsership of etc/ansible folder to ansadm

chown -R ansadm:ansadm /etc/ansible

vim etc/ansible/hosts

\[webserver\] 172.31.19.214 \[dbserver\] 172.31.26.66

ansible.cfg file ( This is an inventory file)

Ansible commands ( We can run all commands only on control server and all other servers are managed by it)

![](https://cdn-images-1.medium.com/max/800/1*GmJIhL3Oqy5THAfRIVMaAA.png)

![](https://cdn-images-1.medium.com/max/800/1*H-E5sbxZsn8QG-epHjFxxQ.png)

![](https://cdn-images-1.medium.com/max/800/1*3Exn52hinxAe5tY2Tu6r7g.png)

To install any package you have to be root. So we are making ansadm of controller as a root user on all machines (except controller)

vi /etc/sudoers

\## ANSIBLE ADMIN USER ansadm ALL=NOPASSWD: ALL

Now run the same command with -s option

![](https://cdn-images-1.medium.com/max/800/1*bC1JBpscKkrqpm8hL8aA_Q.png)

![](https://cdn-images-1.medium.com/max/800/1*wKAEAXHz3Jt8olpXxUZsLQ.png)

**Ansible Roles**

Roles are the next level of abstraction of ansible playbook. Roles are the list of commands that ansible will execute on target machines in given order

Playbook — decides which role is for which target machine

![](https://cdn-images-1.medium.com/max/800/1*oKVPT8z56-iCv7EhOId5jw.png)

**\[ansadm@ip-172–31–21–35 ansible\]$ mkdir roles/basic \[ansadm@ip-172–31–21–35 ansible\]$ mkdir roles/basic/tasks \[ansadm@ip-172–31–21–35 ansible\]$ cd roles/basic/tasks \[ansadm@ip-172–31–21–35 tasks\]$ vi main.yml**

**\[ansadm@ip-172–31–21–35 ansible\]$ cat /etc/ansible/roles/basic/tasks/main.yml**

**\- name: Install ntp yum: name=ntp state=present tags: ntp**

**\[ansadm@ip-172–31–21–35 ansible\]$ vi playbook.yml \[ansadm@ip-172–31–21–35 ansible\]$ ansible-playbook -K playbook.yml**

**\[ansadm@ip-172–31–21–35 ansible\]$ cat playbook.yml - hosts: all roles: — role: basic**

![](https://cdn-images-1.medium.com/max/800/1*NtXmZqVDtH-YznPxXzVguA.png)

![](https://cdn-images-1.medium.com/max/800/1*7N-fD7Us6mipv4FiOT5gSg.png)

ansible-playbook <playbook> — list-hosts

To check if HTTPd is installed, the easiest way is to ask `rpm`:

**rpm -qa | grep httpd**
