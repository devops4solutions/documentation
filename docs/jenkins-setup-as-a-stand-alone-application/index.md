---
title: "Jenkins installation on windows machine"
date: "2018-08-13"
categories: 
  - "jenkins"
tags: 
  - "jenkins"
---

This blog will give basic and simple information regarding Jenkins setup as a stand-alone application on Windows.

To install and run Jenkins on Windows as a stand-alone application (Jenkins.war file comes with an embedded servlet containers, viz. Winstone, jetty ) rather than deploying it as a standard web application on to a Java Servlet container (e.g. Tomcat, JBoss, GlassFish).

### PROS :

- Facile, simple and straightforward way to setup and run Jenkins.
- Provides additional features (as compared to when deployed to conventional application servers), for instance, installing and upgrading plugins on fly, & restart jenkins directly from the administration screens.

### **STEPS**

1. Download **_jenkins.war_** file from the official website for Jenkins ([https://jenkins.io/](https://jenkins.io/)) and after clicking on the download tab on the web interface, choose “ **_Generic Java Package (.war)_**” from the Long-term Support Section.

![](https://cdn-images-1.medium.com/max/1100/1*rW_K-ncG0RI9DhzfMxTA4g.png)

Save the downloaded WAR file in a particular drive (e.g. D:\\ drive).

2\. Open the command prompt and browse to the directory (e.g. d:)and the folder (DevOps) hosting jenkins.war file. e.g.

C:\\Users\\user>cd /d d:\\DevOps

D:\\DevOps> java -jar jenkins.war

3\. Once the above command will run, various task will get executed and if the process completes without any major errors then the following message will appear on the command prompt :

INFO : Jenkins is fully up and running.

OR

Download the .zip file of Jenkins by clicking on the windows (or according to your operating system) and extract the file in to your system. Install the package jenkins.msi

**Note** : I received one error which hindered the installation process, which was “ java.net.BindException”, this can be caused either by another application running on the port (8080) or failed shut down of the confluence.

**Resolution** :

**Run command line as an administrator (right-click on the command prompt & choose _run as administrator_ option) and type the following command : netstat -ano | findstr :8080 (it will give the PID — process identifier) then execute the following command to terminate the process associated with port 8080 : taskkill /PID<write pid>/F**

- In the following image, the yellow encircled no. is the PID.

![](https://cdn-images-1.medium.com/max/1100/1*Slr4p0tHooYIISqTBi89nw.png)

4\. After resolving the error, again write the command : java -jar jenkins.war on the command prompt. After successful completion, note for the default password provided, and use it to log in on the browser ( [**_http://localhost:8080/_**](http://localhost:8080/login?from=%2F)**_) ._**

![](https://cdn-images-1.medium.com/max/1100/1*Bq17YO2bc00VIXcUM2Vc8w.png)

![](https://cdn-images-1.medium.com/max/1100/1*pKQ17rm0prDvr8MvksxDrQ.png)

You can also find the default password for the admin user id on file:///C:/Users/user/.jenkins/secrets/initialAdminPassword

5\. After providing the user and password information, hit log in and it would ask you to create your own id and password for the future reference.
