---
title: "What is Sonarqube"
date: "2020-08-16"
categories: 
  - "sonarqube"
tags: 
  - "sonarqube"
  - "static-code-analysis"
---

Sonarqube is a tool to check the code quality and provides a platform to write a cleaner and safer code for the developers. It provides the dashboard for a user to show all the issues related to their code like security issues,vulnerability issues, bugs,code smells etc.

If you want to see the video for this article, [click here](https://youtu.be/UlyMd2Dqcbw)

### Agenda:

1. Introduction of Sonarqube
2. How it works?
3. What is Quality Profile?
4. What is Quality Gates?
5. Analyze your first project

### **Why Sonarqube is so popular?**

- It provides lot of rules to scan multiple languages of your projects

![sonarqube rules](https://cdn-images-1.medium.com/max/800/1*2Ezz9NUpKGPOJ2Pc9shStQ.png)

- It provides a way to scan all the languages together which are present in one git repo.
- For example: In this [project](https://github.com/devops4solutions/CI-usingAnsible), we have jsp and xml file, sonarqube can auto detects all the languages and applied the rules accordingly as shown below:

![](https://cdn-images-1.medium.com/max/800/1*Xdt3tTF7PkiIwCSLJMxY_w.png)

- It provides one place to measure the Maintainability,Reliability and Security of all the languages in your project

![](https://cdn-images-1.medium.com/max/800/1*MdYkdkT6cQvhz8AwkVYt6A.png)

- It provides vast list of rules for each language which helps in checking the quality of code

![Languages](https://cdn-images-1.medium.com/max/800/1*qhLVdX1JclZYr_fEwHOHsQ.png)

Language

### **Installation**:

Follow my article on [installation of Sonarqube](https://devops4solutions.com/sonarqube-installation-windows/) if not already installed.

### **How Sonarqube works?**

- Check out their official documentation on [sonarqube architecture and integration](https://docs.sonarqube.org/latest/architecture/architecture-integration/)
- It uses scanners to scan the code and there is different type of [scanners](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/) as per the language.

### **Quality Profiles**

- Collection of rules that gets applied while scanning the project
- Sonarqube provides default set of rules for each language that you can use directly and customized it as per your project need
- Sonarqube uses set of plugins for each of the quality profiles. It provides default plugins as part of the installation and others that you can download as per your project needs

![sonarqube plugins](https://cdn-images-1.medium.com/max/800/1*B8RlSGYSaGWrOZqhHJkW1g.png)

- You can see all the quality profiles in your instance as shown below:

![](https://cdn-images-1.medium.com/max/800/1*MZL5bUXCz2r2hru1hpjvQw.png)

### **Quality Gates**

- A Quality Gate is a set of measure-based, Boolean conditions. You can create a quality gates as per your project needs and decide what rating is acceptable for your application
-  It helps to identify whether your code is ready to get deployed in production . 
- You will see the project status on the dashboard whether it passes the quality gate conditions or not.
- You can also failed your Jenkins build based on the quality gate status

![](https://cdn-images-1.medium.com/max/800/1*cXM9EfiFhqcQKi6Ne7uJiw.png)

### **Install Sonarqube Scanner for Java**

- Download [software](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/) as per your operation system. 
- I have installed for windows OS and extract it on your local drive
- Add the path in the environment variable

C:\\sonar-scanner-cli-4.4.0.2170-windows\\sonar-scanner-4.4.0.2170-windows\\bin

Sonarqube Scanner installation and configuration is completed successfully.

### **Create a Sonarqube project**

- Its very simple to create a project directly from your project directory
- Create a file `sonar-project.properties` at the root of your project with below details

sonar.projectKey=com.teama  
sonar.projectName= Team-A-project  
sonar.sourceEncoding=UTF-8  
sonar.sources=.  
sonar.host.url=[http://localhost:9000](http://localhost:9000)   
sonar.login=admin   
sonar.password=admin

- Open the command prompt, navigate to your directory and run sonar scanner

cd /CI-examples
sonar-scanner

![](https://cdn-images-1.medium.com/max/800/1*0pgoaHQCZ1taNbLXWOhr0g.png)

- If you need to pass some values using command prompt then you can pass as below:

sonar-scanner -Dsonar.projectKey=com.teamb -Dsonar.projectName=teamb-project1 -Dsonar.host.url=[http://localhost:9000](http://localhost:9000)  -Dsonar.login=admin -Dsonar.password=admin

### **Create a sonarqube project using Maven**

- Navigate to your directory and run the below command. In this case you don’t need separate sonar scanner to be installed on your system

mvn sonar:sonar -Dsonar.projectKey=com.teamb -Dsonar.projectName=teamb-project -Dsonar.host.url=[http://localhost:9000](http://localhost:9000)  -Dsonar.login=admin -Dsonar.password=admin  

All projects are created successfully and you can see all the issues related to your project on the dashboard screen

![](https://cdn-images-1.medium.com/max/800/1*R6mphU-Rxk62_ajnnk5xOQ.png)

![](https://cdn-images-1.medium.com/max/800/1*4N0z3nenLBzKWNemPlTsNA.png)

Congratulations, you have successfully installed Sonarqube and created the project directly using properties file and also using maven command.
