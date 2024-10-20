---
title: "Maven installation and configuration -- Windows"
date: "2018-06-19"
categories: 
  - "devops-tools"
tags: 
  - "java"
  - "maven"
---

### Maven

**Maven** is a project management & comprehension tool that provides a complete _build lifecycle framework_ to assist developers. It is a build automation tool used primarily for Java projects, though can also be used to build and manage projects written in C#, Ruby, Scala, and other languages. Maven addresses two aspects of building software: first, it describes how software is built, and second, it describes its dependencies. In this blog, we will consider Maven installation and configuration on Windows.

#### **How does Maven helps ?**

Maven addresses the following common problems:

- **Multiple Jars** **:** For example in order to use the hibernate framework in your application, all required jars need to be included. Also need to make sure those jars are available at compile time, bundle them up during distribution when deploying it. Sometimes we miss out jars and it is a tedious job to download them and make sure the required jars are available on your system.
- **Dependencies :** Say you have a particular jar & that has dependency on another jar. Make sure all dependencies are closed.
- To set up a **Project Structure** : need to have proper directories, libraries etc.
- Building, publishing & deploying.

Maven helps in creating project structure, managing project dependencies, downloading dependencies etc. This all comes from repositories. For this, Maven talks to those online repositories.Hence, internet connectivity is imperative.

### Maven Installation and configuration -- Windows

Employ the following steps-

**Step 1: Download and install JDK (if previously not downloaded)**

Maven is a Java based tool, therefore JDK (JDK 1.7 or above, preferably JDK 1.8) installation on your system is a pre-requisite.

- **JAVA\_HOME** environment variable is set to point to the base directory location where Java is installed on your machine.
- Set the environment variable JAVA\_HOME to C:\\ProgramFiles\\Java\\jdk1.7.0\_60
- Append the string “;C:\\Program Files\\Java\\jdk1.7.0.60\\bin” to the end of the system variable, Path.

Verify JAVA installation on your windows system:

c:\\> java -version

###### **Step 2: Download Maven Archive**

**Download** [apache-maven-3.5.3-bin.zip](http://www-eu.apache.org/dist/maven/maven-3/3.5.3/binaries/apache-maven-3.5.3-bin.zip)        (binary zip archive file) and extract it into your system (e.g. program files).

###### **Step 3: Set Maven Environment variables**

Go to Control Panel -> System ->  Advanced System settings ->Environment variables -> System variable -> new

M2\_HOME=  Location of Maven folder ( C:\\Program Files (x86)\\apache-maven-3.5.3)
MAVEN\_HOME= Location of Maven folder (C:\\Program Files (x86)\\apache-maven-3.5.3)
PATH= Location of Maven bin folder (append using ; to the existing path)(———————– ; C:\\Program Files (x86)\\apache-maven-3.5.3\\bin)\*

\*\* C:\\Program Files (x86)\\apache-maven-3.5.3\\bin == %M2\_HOME%\\bin and we set this variable, so that we can trigger maven executable from command prompt and it points to the right executable file which is in the bin folder\*Although M2\_HOME suffice your requirement, but sometimes some program do search for MAVEN\_HOME variable too. Therefore, set it also.

**Check the installation :**

> mvn –version

###### **Step 4: Define the project structure:**

Go to cmd à go to directory/folder where u want to build the project

D:\\DevOps\\Maven\\My-maven-first-app>**mvn archetype:generate**

 **\***this command will start downloading all the plugins that are required by the maven to run this mvn archetype:generate command. Before running any command, maven requires a set of plugins that are stored in a repository on internet. These plugins do not come as a part of maven installation.

\*When we try running any command for the first time on a system, maven will try to look for the required plugins to run that command on the following location on your system: C:\\Users…\\.m2\\repository/. If it will not find it here, then it will download it from the internet and will store at this location, so that next time before running this command it does not have to download the associated plugins.

Choose a number: 1 (you can type : maven-archetype-quickstart)

Normally, we just use the following two templates:

- **maven-archetype-webapp** : Java Web Project (WAR)
- **maven-archetype-quickstart** : Java Project (JAR)

Choose a version: 4

Define value for property ‘groupId’: com.anushasoftware

Define the value for property ‘artifactId’: maven-demo

Define the value for property ‘version’: 1.0-SNAPSHOT

Define the value for property ‘package’: com.anushasoftware.demos

###### **Step 5: Build project –**

**To compile :** Go to command prompt -> go to the location/folder containing pom.xml

c:\\>mvn compile

All dependencies are specified in pom.xml. Maven downloads them for you. Even more, it does the compilation of Java classes in application directory.

**Firstly, Test :  executes the test cases**

 \>mvn test

**Secondly, Packaging :  Package it into a jar file (artifact) in the target folder.**

 \>mvn package

**Lastly, Install : install the artifact in the local repository**

 **>**mvn install

Finally Congratulations!! You have completed Maven installation and configuration on Windows.

Furthermore, click [here](https://devops4solutions.com/jenkins-pipeline-code/) to use Maven projects with Jenkins.
