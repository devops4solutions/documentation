---
title: "Ansible Setup on AWS EC2 Instance with windows Nodes"
date: "2018-06-13"
categories: 
  - "ansible"
  - "aws"
  - "devops-tools"
---

In this blog, we will do the Ansible Setup on AWS EC2 Instance with windows Nodes

### **Prerequisite of Ansible Setup**

1. Python

2\. SSH

### **Three servers**

**Ansible control Server** ( Install ansible using epel repository)- On AWS you have to enable this file

**WebServer**

**DBServer**

**How to connect between these servers ?**

To ping these servers(webserver and dbserver) from ansible control server , you have to add one inbound rule “All ICAMP traffic” in both the instances)

**Step 1**

**Ansible Control Server**

**Install Ansible on AWSLinux**

sudo yum update
vim /etc/yum.repos.d/epel.repo

or

sudo yum-config-manager --enable epel

yum repolist ( you should see epel)

yum install ansible

**Second Method of installing ansible**

```
curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
```

```
if getting sudo pip command not found
sudo env "PATH=$PATH" pip install pytz
sudo pip install ansible
```

![](https://cdn-images-1.medium.com/max/800/1*vn9gkKZYIVC_vDi2wXYsCg.png)

Ansible Version

Ansible installed successfully.

**Step 2:**

**Create a Windows EC2 Instance**

Check ping command

Next make sure the Amazon network rules allow _Echo Requests_. Go to the **Security Group** for the EC2.

- right click, select **inbound rules**
- A: select **Add Rule**
- B: Select **Custom ICMP Rule — IPv4**
- C: Select **Echo Request**
- D: Select either **Anywhere** or **My IP**
- E: Select **Save**
- Added All traffic also

If ping doesn’t work, do the below step also

- Next, Windows firewall blocks inbound Echo requests by default. Allow Echo requests by creating a windows firewall exception…
- Go to **Start** and type **Windows Firewall with Advanced Security**
- Select **inbound rules**

Prerequisite

1. Powershell 3.0 or higher should be installed. (Version 5 is present on AWS EC2 Windows instance). Check version using below command

```
$PSVersionTable.PSVersion
```

2\. WinRM setup on windows machine

[script](https://github.com/ansible/ansible/blob/devel/examples/scripts/ConfigureRemotingForAnsible.ps1) on the remote machine and then execute it in PowerShell console as an administrator.

powershell.exe -File ConfigureRemotingForAnsible.ps1

powershell.exe -File ConfigureRemotingForAnsible.ps1

**Ansible Control Server**

create folder named windowsplaybook using below command

_mkdir windowsplaybook_

_cd windowsplaybook/_

### create file named inventory by using command

vi inventory

put in below content

_\[web\]_

_Ip address of machine_

Create File Named all in group\_vars folder

Create folder named group\_vars using mkdir group\_vars

Create files named all using vi group\_vars/all and put below contents

ansible\_user: windows\_username

ansible\_password: SecretPasswordGoesHere

ansible\_port: 5986

ansible\_connection: winrm

_\# The following is necessary for Python 2.7.9+ (or any older Python that has backported SSLContext, eg, Python 2.7.5 on RHEL7) when using default WinRM self-signed certificates:_

ansible\_winrm\_server\_cert\_validation: ignore

after this done please run below command to test if you are able to ping windows machine

_ansible web -i inventory -m win\_ping
ansible web -i_ \-i /home/ec2-user/windowsplaybook/inventory _\-m win\_feature_

Ansible control server connection with windows nodes is completed.

Now run some playbooks

Create a directory with file main.yml

mkdir /home/ec2-user/windowsplaybook/roles/basic/tasks

vi main.yml

ansible-playbook -i /home/ec2-user/windowsplaybook/inventory main.yml

\[root@ip-10–0–0–20 tasks\]# cat main.yml
— -
# YAML documents begin with the document separator — -

# The minus in YAML this indicates a list item. The playbook contains a list
# of plays, with each play being a dictionary
-

# Target: where our play will run and options it will run with
hosts: all

# Task: the list of tasks that will be executed within the play, this section
# can also be used for pre and post tasks
tasks:
— name: Set a fact
set\_fact:
our\_fact: Ansible Rocks!

- name: Install IIs WebServer
win\_feature:
name: Web-Server
state: present
— name: Install IIS
win\_feature:
name: Web-Mgmt-Tools,
Web-Mgmt-Console,
Web-Scripting-Tools,
Web-Mgmt-Service
state: present
include\_sub\_features: no

# Three dots indicate the end of a YAML document
…
