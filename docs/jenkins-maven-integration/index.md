---
title: "Jenkins integration with Maven"
date: "2018-06-19"
categories: 
  - "devops-tools"
tags: 
  - "jenkins"
  - "maven"
coverImage: "Untitled3-1.png"
---

**Maven**  is a build automation tool used primarily for Java projects, though can also be used to build and manage projects written in C#, Ruby, Scala, and other languages. Jenkins integration with Maven through plugins aids to automate the complete build.

Maven is a program that automates the creation of executable applications from source code. It incorporates compiling, linking and packaging the code into a usable/ executable form, generation of reports and dependency management.

## Jenkins integration with Maven

**I. Configure Maven in Jenkins**

1\. Go to Jenkins Dashboard ->Manage Jenkins ->Manage plugins ->Available ->Maven Integration ->Install

2\. Go to Manage Jenkins->Global tool configuration->Maven -> Add Maven\_home variable value (i.e. path of the maven file on your system). if manually installing the maven

![](https://cdn-images-1.medium.com/max/800/1*5ZB3dVWboUoj3z_TqYvwcw.png)

3\. Go to Jenkins Dashboard -> New Item -> Maven Project option will be available

**II. Build a Maven project**

1. Go to Jenkins Dashboard -> New Item -> Choose name for the Maven Project (e.g. MyFirstMavenExample)

\[caption id="" align="alignnone" width="1071"\]![](https://cdn-images-1.medium.com/max/1100/1*5IVpAQUOtxuUqdjUr1QLow.png) Creating a new job in Jenkins\[/caption\]

2\. On Configure Page, set the following:

a). Discard Old builds

Days to keep builds: 1

Max of builds to keep: 5

\[caption id="" align="alignnone" width="1082"\]![](https://cdn-images-1.medium.com/max/1100/1*bKubng5lUegPe-EZ7_VWbA.png) Configuration page of the Jenkins job\[/caption\]

b). JDK (to be used for this project) Java-1.8

c). Build -> adavanced -> enable the following

\- Resolve Dependencies during Pom parsing

\- Use custom workspace (add path of the folder containing pom.xml)

\-Goals & Options = Clean Compile Test

\[caption id="" align="alignnone" width="1078"\]![](https://cdn-images-1.medium.com/max/1100/1*dNOfvG2Eenr7d3JWkl0z4g.png) Configuration page of the Jenkins job\[/caption\]

3). Apply & Save

4). Go to Project Window and Click ‘Build Now’.

**Troubleshoot:**

\*Error : Compilation error - No compiler is provided in this environment. Perhaps you are running on a JRE rather than a JDK?

Probable cause: not able to connect to/find jdk.

Solution:

1\.    Go to Jenkins dashboard -> Manage Jenkins -> Configure System -> Global properties -> Add Environment Variable : JAVA\_HOME and value

2\.    Also check in the system of your windows :
System -> advanced settings -> environment variables
Set the PATH : Append with the folder path of the jdk
If, user variable is pointing towards JRE, then update it to point towards java path.
