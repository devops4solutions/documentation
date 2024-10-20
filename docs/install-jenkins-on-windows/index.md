---
title: "Install Jenkins On Window"
date: "2018-06-13"
categories: 
  - "jenkins"
---

## **Two methods to install Jenkins on Windows**

1. Download jenkins.war file and and deploy that war file on tomcat .
2. In Command Prompt Run the following command:

java -jar jenkins.war — httpPort=8080

1. Normal download whole software and install it [http://mirrors.jenkins.io/windows/latest](http://mirrors.jenkins.io/windows/latest)
2. [http://localhost:8080](http://localhost:8080/login?from=%2F)
3. Login and default password is present in admin and secrets/initialAdminPassword

**Run as a Window Service**

1. If you have install it as a windows installer then it automatically running as a Windows Service.
2. If not, then Go to Manage Plugin and click on Install as a Window Service

Change the default port

1. Change jenkins.xml file

**Configure Windows Slave Agent**

Manage Jenkins -> Global Tool Configuration ->Agents as shown below

![](https://cdn-images-1.medium.com/max/800/1*IdVBV_mKR27ub_Wdy90dgw.png)

Once this is enabled you will see the option in Launch method “Launch agent via Java Web Start” in the below steps

Now go to Manage Jenkins -> Manage Nodes -> Add Node

![](https://cdn-images-1.medium.com/max/800/1*kyl9rmNMlhzoH9KSrWS99w.png)

Login to your windows machine

Install Java on Windows Machine

Install git on Windows Machine

Set path

C:\\Program Files\\Git\\cmd

%JAVA\_HOME%\\bin

Download the jdk-8u161-windows-x64.exe and install it

Click on agent.jar to download agent.jar

Copy agent.jar to the remote agent

![](https://cdn-images-1.medium.com/max/800/1*zfxecoFlb0haWWJcTtoNHg.png)

Run from agent command line:

java -jar [agent.jar](http://127.0.0.1:8080/jnlpJars/agent.jar) -jnlpUrl [http://127.0.0.1:8080/computer/WindowsSlave/slave-agent.jnlp](http://127.0.0.1:8080/computer/WindowsSlave/slave-agent.jnlp) -secret 40ff8dd0b5e437cdfaa670d3a6844304e9644a78118f645f2b1a0cb3106ede04 -workDir "D:\\WindowsJenkinsSlave"

Once command is successful, you will see the below screenshot

![](https://cdn-images-1.medium.com/max/800/1*K-NYK7pj6h51m9Xu-pZ0Vg.png)

Jenkins Setup with Windows Slave is completed successfully.

**Configure Java\_home,GIT,Maven,Gradle**

1. Manage Plugins -> Global Tool Configuration

![](https://cdn-images-1.medium.com/max/800/1*sezoA4Ujvz10HETMcmNNog.png)

![](https://cdn-images-1.medium.com/max/800/1*B8KZC4f9p9tdHO9YzqdTRA.png)

**Create a freestyle project for ANT and Maven**

1. Create a sample project and commit in github
2. Create build.xml and commit it with the code ( Need to research if i need to use different xml file on different location)

Run Jenkins build from eclipse

1. Download myvln

**Back up and Restore Jenkins**

Install pLugins

1. Dashboard View, Deploy to Container Plugin,Monitoring,Audit Trail,disk-usage plugin ,backup plugin
2. Create back up and then restore

**Jenkins Command Line**

1. Download jar from this location : jenkinsurl/jnlpJars/jenkins-cli.jar

2\. java -jar jenkins-cli.jar -s [http://localhost:8080/](http://localhost:8080/) help

ERROR: You must authenticate to access this Jenkins.  
Jenkins CLI

http://localhost:8080/user/nidhigupta/configure

**Configuration**

1. YOu can change config.xml for like increasing the number of executor

Disk Usage

1. Go to Dashboard | Manage Jenkins | Disk Usage:

Integrate sonar with Jenkins

1. Install plugins : Sonar quality Gates Plugin, SonarQube scanner for Jenkins
2. Get the token from sonarqube to integrate with sonar
3. Configure sonar — Manage Jenkins->Configure System
4. Configure sonar-runner — Manage Jenkins — Global Tool Configuration
5. In job , add build — execute sonar -> and that will do the anaylysis of files.

**JFrog Artifactory**

Download from [https://bintray.com/jfrog/artifactory/jfrog-artifactory-oss-zip/5.9.1](https://bintray.com/jfrog/artifactory/jfrog-artifactory-oss-zip/5.9.1)

Run artifactory.bat for windows

http://localhost:8081/artifactory/webapp/#/home

Default username/password = admin/password

Unit Testing Plugin

TestNG Result Plugin
