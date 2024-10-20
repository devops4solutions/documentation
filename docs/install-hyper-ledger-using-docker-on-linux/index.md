---
title: "Install Hyper-ledger using Docker on Linux"
date: "2018-06-13"
categories: 
  - "docker"
---

**Prerequisite:**

· [Docker](https://www.docker.com/products/overview) v1.12 or greater is required.

· Node.js — version 6.9.x or greater

· Node.js version 7.x is not supported at this time.

· Docker Compose Version should 1.8 or later installed

**Installation:**

1\. Install Docker

\- sudo yum update –y

\- sudo yum install -y docker

\- sudo service docker start

\- sudo usermod -a -G docker ec2-user

\- docker info

For more details, please refer below link

[http://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html)

2\. Install Docker Compose

\- sudo su

\- curl -L “https://github.com/docker/compose/releases/download/1.8.0/docker-compose-$(uname -s)-$(uname -m)” > /usr/local/bin/docker-compose

\- chmod +x /usr/local/bin/docker-compose

\- exit

For more details, please refer below link

[http://alex-dai.logdown.com/posts/2016/10/19/1012883](http://alex-dai.logdown.com/posts/2016/10/19/1012883)

3\. Install NodeJS

curl -sL [https://deb.nodesource.com/setup\_6.x](https://deb.nodesource.com/setup_6.x) | sudo -E bash –

4\. Install GIT

yum install git

5\. Install build essential

yum groupinstall “Development Tools

Putty Output mentioned below:

**Install Application:**

\- git clone [https://github.com/hyperledger/fabric-sdk-node.git](https://github.com/hyperledger/fabric-sdk-node.git)

\- cd fabric-sdk-node/examples/balance-transfer/

\- docker-compose -f artifacts/docker-compose.yaml pull

\- ./runApp.sh

\- ./testAPIs.sh

For more details, please refer below link

[https://github.com/hyperledger/fabric-sdk-node/tree/master/examples/balance-transfer](https://github.com/hyperledger/fabric-sdk-node/tree/master/examples/balance-transfer)

**Output:**

![](https://cdn-images-1.medium.com/max/400/1*Bi94EXDrJu64SrYMmg8zDw.png)

![](https://cdn-images-1.medium.com/max/400/1*1bpK8L5Xwbw6NttkVbi7Sg.png)

![](https://cdn-images-1.medium.com/max/400/1*sXDWfgLahyKoWF4wp6i9JA.png)
