---
title: "Installation of Tomcat on AWS ec2 linux & integration with Jenkins"
date: "2018-08-13"
categories: 
  - "aws"
  - "jenkins"
tags: 
  - "aws"
  - "ec2"
  - "jenkins"
  - "linux"
  - "tomcat"
---

This blog will deal with the Installation of Tomcat on AWS ec2 linux & integration with Jenkins will furthermore be demonstrated by deploying a simple Java WAR package on to the Tomcat server.

### Prerequisite:

Click on the following links to get into the details of each pre-requisite lab.

1. [AWS account and ec2 Linux (Amazon Linux AMI) installed.](https://devops4solutions.com/aws-ec2-linux-instance-launch/)
2. [Java installation on AWS ec2 linux instance.](https://devops4solutions.com/java-installation-linux/)
3. [Jenkins installation on AWS ec2 linux instance.](https://devops4solutions.com/jenkins-installation-on-aws-ec2-linux-instance/)

### Installation of Tomcat on AWS ec2 linux instance

Following are the step-by-step guide to install Tomcat on AWS ec2 linux instance:

#### Step 1: Download Tomcat package

Go to browser --> https://tomcat.apache.org/download-90.cgi (to download tomcat9) --> Copy **_tar.gz_** from core section.

\[caption id="" align="alignnone" width="1100"\]![https://tomcat.apache.org/download-90.cgi](https://cdn-images-1.medium.com/max/1100/1*ErrKQ5Xys5ou4FfLi1Gtjw.png) Apache Tomcat download page\[/caption\]

#### Step 2: Log in to AWS ec2 instance & load putty session (for window users)

\[caption id="" align="alignnone" width="451"\]![AWS ec2 linux instance](https://cdn-images-1.medium.com/max/1100/1*UKPg2rW4J9GPuel75CQDvg.png) AWS ec2 linux instance\[/caption\]

\[caption id="" align="alignnone" width="606"\]![Loading putty session](https://cdn-images-1.medium.com/max/1100/1*DxQ5qpVxUWQj27jROPdJ8w.png) Loading putty session\[/caption\]

#### Step 3: Install & unzip the tar.gz folder

Go to the terminal & type the following commands:

\[ec2-user@ip-xxx-xx-xx-xx\] sudo -i
\[root@ip-xxx-xx-xx-xx\]$ wget <paste the tar.gz address copied in step1>

Now Tomcat has been downloaded, check with "ls" command. Further Unzip the folder:

\[root@ip-xxx-xx-xx-xx\]$ tar -zvxf apache-tomcat-9.0.10.tar.gz

\*In case wget is not installed then firstly employ this step:

\[root@ip-xxx-xx-xx-xx\]$ yum install wget -y

#### Step 4: Start Tomcat service

Under Apache Tomcat folder, there exists two files, namely; startup.sh and shutdown.sh

- Browse to the bin folder

![](https://cdn-images-1.medium.com/max/1100/1*MFIo88lZBNGnLUSDjJ-2Tw.png)

\[root@ip-xxx-xx-xx-xx bin\]$ ls -ltr
//to check the status of the startup services

\[caption id="" align="alignnone" width="981"\]![*No full permission to execute startup & shutdown services](https://cdn-images-1.medium.com/max/1100/1*dxRVn_CfmuHreK6HEojk3A.png) \*No full permission to execute startup & shutdown services\[/caption\]

\[root@ip-xxx-xx-xx-xx bin\]$ chmod +x startup.sh

\[root@ip-xxx-xx-xx-xx bin\]$ chmod +x shutdown.sh

//For all users to execute this script

//Now lets start tomcat service
\[root@ip-xxx-xx-xx-xx bin\]$ ./startup.sh

![](https://cdn-images-1.medium.com/max/1100/1*vrHO0IVyp-6CJ81oCAwMdw.png)

#### Step 5: Change port number from 8080 to 8090 (as Our Jenkins on AWS is also listening to the port 8080)

Browse to conf sub-directory under Tomcat directory and open server.xml file for editing using 'nano' command (vi command can also be used).

![](https://cdn-images-1.medium.com/max/1100/1*WzR-xU2kHNC226Lvu3Su0A.png)

\[root@ip-xxx-xx-xx-xx conf\]$ nano server.xml

\[caption id="" align="alignnone" width="817"\]![server.xml](https://cdn-images-1.medium.com/max/1100/1*uf2FiPf7I_tyZtZL-0YhRA.png) server.xml\[/caption\]

Restart the tomcat service (browse to the bin folder)

\[root@ip-xxx-xx-xx-xx bin\]$ ./shutdown.sh

\[root@ip-xxx-xx-xx-xx conf\]$ ./startup.sh

#### Step 7.  Allow port no 8090 under security group in AWS

- Go to Your AWS account --> ec2 linux instance

![](https://cdn-images-1.medium.com/max/1100/1*oXYrvX5Ih7npoD60bBErXQ.png)

- Got to the related security group (in this case: Myec2Linux)

![](https://cdn-images-1.medium.com/max/1100/1*5pouHWXHYBl38qbdkJMw7g.png)

- Click Inbound Rules

![](https://cdn-images-1.medium.com/max/1100/1*V3GWaCguzFSkta0bbeldZg.png)

- Edit Inbound rules

![](https://cdn-images-1.medium.com/max/1100/1*C4K7rG8yDKgRZE3XjM8NQA.png)

- Add 8090 port no and allow it to be public

![](https://cdn-images-1.medium.com/max/1100/1*PFjeplrcn4QhcIQN5AHe6g.png)

- Go to Browser and type- http://<ip\_address>:8090

![](https://cdn-images-1.medium.com/max/1100/1*j7dG9Gh8Y0KdXPrgOWIfZA.png)

#### Step 8: Edit the context.xml

By default the manager is only accessible from a browser running on the same machine as Tomcat. Therefore to modify this restriction, go to context.xml file and comment out the default IP address.

Use the 'find' command to find the context.xml

\[root@ip-xxx-xx-xx-xx conf\]$ find / -name context.xml

You will get a list, and edit context.xml within webapp, both under host-manager and manager.

![](https://cdn-images-1.medium.com/max/1100/1*Dg4vg8iaeY9O7SWlzF27Jw.png)

![](https://cdn-images-1.medium.com/max/1100/1*sTBs0rX26I2INPU8g8TEyg.png)

Comment out the value section:

![](https://cdn-images-1.medium.com/max/1100/1*lqS7b-lCXhgwg3MuOUBk_w.png)

#### Step 9. Specify the roles and the users:

Browse to the conf directory and open the tomcat-users.xml for editing.

\[root@ip-xxx-xx-xx-xx conf\]$ find / -name context.xml

![](https://cdn-images-1.medium.com/max/1100/1*yXIOymVsAzFVYFNmrWTBUA.png)

![](https://cdn-images-1.medium.com/max/1100/1*tDqoNrbv9-sHB3HRfcUVxQ.png)

![](https://cdn-images-1.medium.com/max/1100/1*nYWvrgkhtATpSmC8wDB_sQ.png)

![](https://cdn-images-1.medium.com/max/1100/1*LnxHTANNOezPeOY9oGPDqA.png)

#### Step 10. Restart Tomcat service

### Integration with Jenkins

Following are the steps to integrate with Jenkins:

Step 1. Install Jenkins on AWS ec2 Linux ([Clickhere](https://devops4solutions.com/jenkins-installation-on-aws-ec2-linux-instance/)).

Step 2. Install 'Deploy to container' plugin from Manage Jenkins --> Manage Plugins --> Available --> 'Deploy to container'

![](https://cdn-images-1.medium.com/max/1100/1*2VYsnY-JOW1Rfe1TjO-rHw.png)

![](https://cdn-images-1.medium.com/max/1100/1*DRus08kjKemsIItk0wlp8Q.png)

Step 3. Create a freestyle project

- Check out the Java code (that will create WAR) from github.

![](https://cdn-images-1.medium.com/max/1100/1*4nk-QinKFGnuyfzuDAEU-g.png)

- Invoke top-level Maven target

![](https://cdn-images-1.medium.com/max/1100/1*whz7SuuXaavKN4da8AaLxg.png)

- Add post-build action : 'Deploy war/ear to container'

![](https://cdn-images-1.medium.com/max/1100/1*ti0z2o52Mj_LDY72TZySOg.png)

Click Save & build now.

- Go to browser : http://<ip\_address>:8090 and log in manager app

![](https://cdn-images-1.medium.com/max/1100/1*1pjvV0c306y6INX8meesFg.png)

You will see your war file deployed to tomcat (in this case 'web\_ex').

Voila!! Congratulations !!

You have done a great job. All steps meticulously followed.
