---
title: "CI/CD using Jenkins job"
date: "2018-06-20"
categories: 
  - "jenkins"
tags: 
  - "jfrog-artifactory"
  - "jnekins"
  - "maven"
  - "sonarqube"
  - "tomcat"
---

## Jenkins

Jenkins is a self-contained (Java-based program), open source automation server. It is used by various organizations to perform DevOps practices, like Continuous Integration and Continuous delivery.  With the help of its extendable architecture through plugins, it can easily be integrated with myriads of tools and can easily implement CI/CD using Jenkins job both manually as well as through script.

However, before configuring a Jenkins job to integrate above mentioned tools, make sure that the corresponding plugins and tools are installed and configured in Jenkins. Following are the links with the detail illustrations of the prerequisites :

Integrating Jenkins with -

1. [SonarQube](https://devops4solutions.com/jenkins-sonarqube-integration/).
2. [JFrog Artifactory](https://devops4solutions.com/jenkins-jfrog-artifactory-integration/).
3. [Maven](https://devops4solutions.com/jenkins-maven-integration/).

Integrating Jenkins WITH myriads of tools (Maven + Git + SonarQube + JFrog Artifactory + tomcat9x) manually

## CI/CD using Jenkins job

Following are the quick steps to follow to integrate various tools and build a pipeline manually:

Step 1. Go to Jenkins Dashboard -> New Item -> Maven Project -> Configuration window of the project will appear.

Step 2. Go to SCM section of the job and provide the details (URL & credentials if any) of the source code repository.

\[caption id="" align="alignnone" width="467"\]![](https://cdn-images-1.medium.com/max/1100/1*RYpZLLzRSvL_mB2Fv-I-Ew.png) Github repository URL specification\[/caption\]

Step 3. Go to the Build section and choose “resolve artifacts from artifactory” and after refresh repository, choose the dedicated repositories (either created one or pre-defined).

\[caption id="" align="alignnone" width="1071"\]![](https://cdn-images-1.medium.com/max/1100/1*9ADRuI9FENznvzdY3-YojQ.png) Artifactory server specifications\[/caption\]

Step 4. Go to pre-steps and configure the SonarQube scanner.

\[caption id="" align="alignnone" width="1072"\]![](https://cdn-images-1.medium.com/max/1100/1*9UAaK-229K34wP9qi01xiw.png) Sonar-project.properties\[/caption\]

> **Following is the analysis property section for a webapp type Maven project.**

\# Required metadata 
 sonar.projectKey=Maven-Sonar-Jfrog-Tomcat-demo
 sonar.projectName= Maven project analyzed with the SonarQub Runner 
 sonar.projectVersion=1.0 

 # Comma-separated paths to directories with sources (required) 
 sonar.sources=src/main/webapp
 
 #Language
 sonar.language=java

 # Encoding of the source files 
 sonar.sourceEncoding=UTF-8

\*Note: Although the project properties can be provided in the project configuration window (as described above), the best practice is to create a file “sonar-project.properties” in the workspace (in the root directory). Finally then provide the path in the “Path to project properties”.

Step 4. In the Build section, specify the maven goals (e.g. clean package)

\[caption id="" align="alignnone" width="1065"\]![](https://cdn-images-1.medium.com/max/1100/1*Td67VFJTxSqPL9UflwWN_g.png) Maven build configuration\[/caption\]

Step 5. Go to the Post-Build section -> Choose “Deploy artifacts to artifactory”

\[caption id="" align="alignnone" width="1068"\]![](https://cdn-images-1.medium.com/max/1100/1*HLdMLacrh0uxTRAeK2UxAg.png) Specifying target repositories in Artifactory\[/caption\]

Step 6. Go to the Post-Build section -> Choose Deploy war/ear to a container & specify tomcat credentials specified in the tomcat-users.xml

\[caption id="" align="alignnone" width="1060"\]![](https://cdn-images-1.medium.com/max/1100/1*6ftKPgIabaALvQmWI1Ietw.png) Deployment container specifications\[/caption\]

Step 7. Go to Project/job window -> Click on Build Now.

Finally Congratulations !! You have successfully build your Jenkins job manually.

Furthermore, click [here](https://devops4solutions.com/jenkins-pipeline-code/) to do the  same through pipeline script.
