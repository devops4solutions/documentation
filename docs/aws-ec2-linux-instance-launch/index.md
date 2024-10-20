---
title: "Launching Linux Instance on EC2"
date: "2018-06-20"
categories: 
  - "aws"
tags: 
  - "aws"
  - "ec2"
  - "linux"
  - "putty"
  - "puttygen"
---

### **Amazon EC2**

It is an AWS primary web service that provides resizable compute capacity in the cloud. It can be used to launch instances (e.g. Linux, Ubuntu, Windows, Databases) on cloud. This blog will guide with easy steps for Launching Linux Instance on EC2 (Elastic Compute).

### _**What is Elastic Compute?**_

**Compute** refers to the amount of computational power required to fulfill workload. Amount of compute that you require may change unexpectedly over time. Amazon EC2 provides the compute capability through launching virtual servers called _instances_.

Therefore, one has to pay per hour while instance is running and once the instance is stopped, billing amount also halts. It is similar to book a ride on rickshaw, the meter starts once your journey embarks. However, once you have reached your destination, the meter stops calculating your bill. As a result you have to pay for only how much you use !!

Let’s start this story by launching a linux instance on an AWS account.

### Launching  Linux instance on EC2

Following are the steps used:

##### **Step 1.**

Go to [AWS site](https://aws.amazon.com/free/?sc_channel=PS&sc_campaign=acquisition_IN&sc_publisher=google&sc_medium=cloud_computing_hv_b&sc_content=aws_core_e&sc_detail=aws&sc_category=cloud_computing&sc_segment=188903753461&sc_matchtype=e&sc_country=IN&s_kwcid=AL!4422!3!188903753461!e!!g!!aws&ef_id=WEPo0QAAANPsYKGf:20180615064104:s) and create a free tier account (subscription is free for a year, voila !!)

\[caption id="" align="alignnone" width="1100"\]![AWS services](https://cdn-images-1.medium.com/max/1100/1*qkV6poHk3YXQDW2I8llO1w.png) Amazon Web Service Home Page\[/caption\]

##### **Step 2**.

Sign in as a root user → Click “_My Account_” → Choose “_Management Console_”

Choose the nearest region to launch your instance. For instance, choose N.Virginia if you are in India.

\[caption id="" align="alignnone" width="1100"\]![AWS services](https://cdn-images-1.medium.com/max/1100/1*rKCygb95c_6LKitR_FmORg.png) Choose Compute service\[/caption\]

##### **Step 3.**

Click on _Services_ and choose _EC2_ in compute from the plethora of services provided by AWS.

\[caption id="" align="alignnone" width="1100"\]![AWS services](https://cdn-images-1.medium.com/max/1100/1*uNSWuXesTCYZK4wAVjiUrw.png) AWS services\[/caption\]

You would be directed towards EC2 dashboard. Hit _Launch Instance_ button.

\[caption id="" align="alignnone" width="1100"\]![ec2 launch](https://cdn-images-1.medium.com/max/1100/1*HAOHf2eQ6S82MKfRKqP4dA.png) AWS EC2 launch\[/caption\]

##### **Step 4.**

Choose **AMI** (Amazon Machine Images). Select free tier Amazon Linux AMI (Reason: as it comes with whole bunch of pre-building tools including python, ruby, java and most importantly AWS commad line tools)

\*The **AMI** defines the initial software that will be on an instance when it is launched. It defines the every aspect of the software state at instance launch, including: OS & its configuration, initial state of patches, Application or system software. All AMIs are based on x86 OSs, either Linux or Windows.

\[caption id="" align="alignnone" width="1100"\]![ec2 launch](https://cdn-images-1.medium.com/max/1100/1*q5nH1vS7s_ujC7X-YADLNw.png) AWS EC2 Launch\[/caption\]

##### **Step 5.**

Choose an **instance type** (by default you are eligible for t2 micro type) Click one Next.

**_\*Instance type_** in EC2 defines the virtual hardware dedicated to/supported on an Amazon EC2 instance. There are multitude of instance types available, varying in dimensions, including : Virtual CPUs, Memory, Storage (size an type) and Network Performance.

\[caption id="" align="alignnone" width="1100"\]![ec2 launch](https://cdn-images-1.medium.com/max/1100/1*2QUV11LAhElB54w7t2Mr5Q.png) Choose instance type\[/caption\]

##### **Step 6.**

Leave the instance details as default and click Next.

![](https://cdn-images-1.medium.com/max/1100/1*Hp-UK-hJxaSxxkP2584jjg.png)

\*Note: As a free tier account holder, do not create more than one instance, as then it will be chargeable.

##### **Step 7.**

Keep the storage details as default and click next.

![](https://cdn-images-1.medium.com/max/1100/1*Ll-arJbTakQAjFKr3SSNGg.png)

\*Note: Volume here represents virtual hardware from where you can boot your operating system (Linux in this case).

##### **Step 8.** **Managing Instances**:

Tags can be added to Amazon EC2 instances and also to AWS Cloud services.

Tags are very helpful to manage instances, as when the number of instances in the account starts increasing, it becomes difficult to keep track of them. Also it helps to manage the costs. Currently one can add upto 50 tags.

Tags are case-sensitive key/value pairs that can be associated with instance or other services.

![](https://cdn-images-1.medium.com/max/1100/1*NsuWh4_NuuAXZg2TjNhlZA.png)

![](https://cdn-images-1.medium.com/max/1100/1*HafV7Ge074gp12hu1xEulg.png)

##### **Step 9.** **Configure Security Groups:**

To set firewall rules to control the kind of traffic to reach the instance.

Either select from the available security groups or can create a new security group.

![](https://cdn-images-1.medium.com/max/1100/1*6FkFtvF63f78m_SToxxidA.png)

Add HTTP and HTTPS security groups id you want to view websites / set up a web server.

![](https://cdn-images-1.medium.com/max/1100/1*U8iFTL4--NZMxmaKN4a3kg.png)

##### **Step 10.**

After clicking **Review and Launch,** it will ask to create a key pair, give a name to it, e.g. MyPuttyKey and download the .pem file.

![](https://cdn-images-1.medium.com/max/1100/1*CCx3acE08bVsOEjWWEUhkA.png)

Click on Launch instances and after a while your linux instance would be launched with the above specifications.

![](https://cdn-images-1.medium.com/max/1100/1*oXYrvX5Ih7npoD60bBErXQ.png)

\*Several ways to address a launched Instance:

1. Public Domain Name System (DNS) — It is generated automatically and cannot be specified by the customer. It persists only while the instance is running and cannot be transferred to another instance.
2. Public IP — This IP address is assigned from the addresses reserved by AWS and cannot be specified. It is unique on the internet and persists only while the instanc is running, and cannot be transferred to another instance.
3. Elastic IP — It is a unique address on Internet that is reserved independently by customers and associated with an Amazon EC2. In contrast to public IP, this IP address persists until the customer releases it. Plus it is not tied to the lifetime or state of an individual instance. In case of an instance failure, the Elastic IP address can be transferred to a replacement instance.

### How to use Putty (for Windows users only) -

As written in wiki, Putty is a free and open-source terminal emulator, serial console and network file transfer application.

**Need Putty :** In order to SSH in your EC2 instance

**Need PuttyKeyGen :** In order to convert your pem file which is generated when EC2 instance is created. However putty doesn’t read .pem (private key file), therefore PuttyKeyGen converts .pem file to .ppk file.

**Step 1.** Go to the web browser and type putty and select [https://www.chiark.greenend.org.uk/~sgtatham/putty/](https://www.chiark.greenend.org.uk/~sgtatham/putty/)

Download and save : (a) putty.exe (b) puttygen.exe

\*Respective items could be saved onto the desktop.

![](https://cdn-images-1.medium.com/max/1100/1*bSlesufMRNbpwRVqMaPVVg.png)

putty.exe and puttygen.exe on desktop

**Step 2.** Open up puttygen.exe → Click on generate

![](https://cdn-images-1.medium.com/max/1100/1*QVcXZzSRp50E8H3UbilR-Q.png)

**Step 3.** Choose all files → .pem file → open

![](https://cdn-images-1.medium.com/max/1100/1*GnHem7ExEwMkCdSWFmrDuQ.png)

![](https://cdn-images-1.medium.com/max/1100/1*1JTc19uo32r8-4HQX8Br1g.png)

**Step 4.** Click Save private key and save the file with .ppk extension

**Step 5.** Open up putty

![](https://cdn-images-1.medium.com/max/1100/1*87NxzsAtQnWWGzClXzj_fA.png)

Go to EC2 instance that has been launched and copy public IP address and specify the host name in putty :

> ec2-user@public-IP-address

e.g. ec2-user@52.51.120.70

Also copy the same in the saved session in order to set the default session.

Step 6. Got o SSH → Auth → Browse → Select the .ppk file

![](https://cdn-images-1.medium.com/max/1100/1*debpw2mD1bQBV9ePorwDuQ.png)

![](https://cdn-images-1.medium.com/max/1100/1*S9GibihSWPQpd2gMMS_bwQ.png)

**Step 7.** Go to session again → click on save → click load → click open

![](https://cdn-images-1.medium.com/max/1100/1*m7mYqvf-RkBOXiAgne1Jpw.png)

And there we go !! EC2 instance have got connected.

![](https://cdn-images-1.medium.com/max/1100/1*eelAT0f1nlPsXTbh_PXYuQ.png)

**Conclusion:** With this blog, we were successful in launching Linux instance on EC2.  Also, as a window users, we have learnt to use putty in order to SSH in our EC2 instance.
