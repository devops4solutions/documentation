---
title: "CI/CD using Jenkins Pipeline as code"
date: "2018-07-23"
categories: 
  - "jenkins"
tags: 
  - "artifactory"
  - "automation"
  - "jenkins"
  - "pipeline"
  - "sonarqube"
---

#### Jenkins Pipeline

Pipeline annexes a strong set of automation tools onto Jenkins. It assists use cases covering from simple to comprehensive continuous integration/delivery pipelines. This blog will provide easy steps to implement CI/CD using Jenkins Pipeline as code. Furthermore it will integrate Jenkins, Github, SonarQube and JFrog Artifactory.

##### Job --> Pipeline

As Job is a defined process. Build is outcome of that process. Pipeline can be thought of as a series of jobs. It orchestrates various phases (e.g. build, test, deploy) involved in the life-cycle of a software. Above all, using Jenkins pipeline as code, entire process can be automated by writing scripts for each module.

##### Types of DSL syntax

Two types of DSL syntax are available: Scripted Pipeline and Declarative Pipeline. Groovy DSL is used to code scripted pipelines, while Declarative Pipeline provides predefined structures and models. It allows fast, stable & compact pipelines creation by users with or without learning Groovy.

##### 3 ways to create pipeline

As enumerated by [their website](https://jenkins.io/doc/book/pipeline/getting-started/) , there exists 3 ways to create pipeline:

- [Through Blue Ocean](https://jenkins.io/doc/book/pipeline/getting-started/#through-blue-ocean) — after setting up a Pipeline project in Blue Ocean, the Blue Ocean UI helps to write Pipeline’s Jenkinsfile and commit it to source control.
- [Through the classic UI](https://jenkins.io/doc/book/pipeline/getting-started/#through-the-classic-ui) —  a basic Pipeline can be entred directly in Jenkins through the classic UI.
- [In SCM](https://jenkins.io/doc/book/pipeline/getting-started/#defining-a-pipeline-in-scm) —  Jenkins can be written manually and then can be committed to project's source control repository.

\*Note : It is the best practice to define pipeline in Jenkinsfile and store it in source control (Github) along with the other code check-in. Hence allowing Jenkins to load it directly from SCM and execute the scripted stages.

Using Jenkinsfile has the following advantages (as stated [by them](https://jenkins.io/doc/book/pipeline/jenkinsfile/)):

a). Firstly, code review/iteration on the Pipeline

b). Secondly, audit trail for the Pipeline

c). Lastly, it act as a single source of truth for the Pipeline, therefore can be viewed and edited by multiple members of the project.

> Enough with the background theory, let’s get start with the procedure step-by-step :

#### CI/CD using Jenkins Pipeline as code

Following are the steps employing declarative pipeline script to automate the Jenkins job:

##### **Step 1. Start Jenkins :**

a). If downloaded as a .zip file & installed by running jenkins.msi :

- Run the Jenkins on default browser [**_http://localhost:8080/_**](http://localhost:8080/login?from=%2F)
- Supply the login id & password

b). However, if downloaded as a jenkins.war file:

Go to command prompt. Browse to the directory containing jenkins.war. Run the following command:

java -jar jenkins.war

After Jenkins is fully up & running. Go to the default browser. Log in with the credentials

##### **Step 2. Create & configure Jenkins Pipeline Project/Job:**

a). Install the plugin from Manage Jenkins

Go to Jenkins Dashboard. Manage Jenkins. Manage Plugins. Available. search for ‘Build Pipeline’. Click on ‘Install without restart’

\[caption id="" align="alignnone" width="1100"\]![Jenkins plugin management](https://cdn-images-1.medium.com/max/1100/1*TK8ZuPokJJhiGuwWR6QTeA.png) Plugin Installation : Jenkins\[/caption\]

b). Go to Jenkins Dashboard. Click on “New Item”. Type the name of the project/job. Select pipeline. Click OK.

\[caption id="" align="alignnone" width="1100"\]![Jenkins pipeline job](https://cdn-images-1.medium.com/max/1100/1*lxPfoccnZEUDfqPD-vb9Sw.png) Job creation : Jenkins\[/caption\]

c). Edit the Project Configuration Window. You can provide the Description (though optional).

\[caption id="" align="alignnone" width="1100"\]![Jenkins project configuration page](https://cdn-images-1.medium.com/max/1100/1*6de__cZwfOuGfBZXfincyw.png) Job configuration Window : Jenkins\[/caption\]

Go to the Pipeline section :

- Select “Pipeline script from SCM” from definition.
- Select Git as SCM
- Provide the URL of the github repository, where you have checked-in your source code, pom.xml and **_Jenkinsfile (Wait. Don’t panic!! we will create one in a bit)._**

\[caption id="" align="alignnone" width="1100"\]![Jenkins pipeline configuration](https://cdn-images-1.medium.com/max/1100/1*8n9m-SFjsEOt0mOpNdmzNw.png) Configuration Window : Jenkins\[/caption\]

- Provide the script path, i.e. name of your Jenkinsfile (by default its name is Jenkinsfile, without any extension).
- Click Apply & Save.

##### **Step 3. Create a basic maven project (maven-archetype-webapp or maven-archetype-quickstart).**

##### **Step 4. Create a Jenkinsfile:**

Go to the root directory of your maven project. Create a new text file with name: Jenkinsfile  and edit it with the following code snippet:

###### **_a)._** **_Create Artifactory Server Instance :_**

Make sure Artifactory Server is installed and configured ([see here](https://devops4solutions.com/installation-of-jfrog-artifactory/)) in Jenkins under Manage Jenkins → Configure System

![Configure System : Jenkins](https://cdn-images-1.medium.com/max/1100/1*PdfcFhqaD0-UCDnd1j30Pg.png)

![](https://cdn-images-1.medium.com/max/1100/1*XPT9ep6LyacJvtN59ZVC6g.png)

_\*Provide the server-id same as set in the Jenkins Configuration System._

###### **_b). Create Artifactory Maven Build instance._**

![](https://cdn-images-1.medium.com/max/1100/1*m0VRYL5vWs_zr-2sr39Ldg.png)

\*Maven builds can resolve dependencies, deploy artifacts and publish build-info to Artifactory.

\*Let's define buildInfo Object too.

###### **_c). Start Declarative pipeline script (_**[**_get basics here_**](https://jenkins.io/doc/book/pipeline/syntax/)**_) with different stages._**

**Stage (i). ‘Clone sources’ :** to tell Jenkins to checkout source code from a particular repository (github in this case).

![](https://cdn-images-1.medium.com/max/1100/1*vBzISmBifrOGEIeBvL4Pmg.png)

 

**Stage (ii). ‘SonarQube analysis’ :**

![](https://cdn-images-1.medium.com/max/1100/1*rMJkoz-M_G9ERUxFGpr0SA.png)

Line no. **28** : The dedicated stage to run SonarQube analysis.

Line no. **31** : Since version 2.5 of the SonarQube scanner for Jenkins, there is an official support of Jenkins pipeline. They provide ‘withSonarQubeEnv’ block that allow to select the SonarQube server instance you want to interact with.

Line no. **32** : Running & configuring scanner — triggering SonarQube analysis on maven projects, with the help of sonar-maven-plugin, available in [**maven central repository**](https://mvnrepository.com/artifact/org.sonarsource.scanner.maven/sonar-maven-plugin).

**Stage (iii). ‘Artifactory configuration’ : this is the stage responsible for uploading the artifacts to artifactory.**

Maven Builds with Artifactory:

Maven builds can resolve dependencies, deploy artifacts  and can also publish build-info to Artifactory. To run Maven builds with artifactory from your pipeline script,

(1). Create Artifactory server instance (done in the beginning).

stage('Artifactory configuration'){

steps {

script {
rtMaven.tool ='Maven-3.5.3'

rtMaven.deployer releaseRepo: 'libs-release-local', 'libs-snapshot-local', server: server

rtMaven.resolver releaseRepo:'libs-release', snapshotRepo: 'libs-snapshot', server: server

rtMaven deployer.artifactDeploymentPatterns.addExclude("pom.xml")

buildInfo = Artifactory.newBuildInfo()

buildInfo.retention maxBuilds: 10, maxDays: 7, deleteBuildArtifacts: true

buildInfo.env.capture = true
}
}

(2) Create an Artifactory Maven Build instance, as well as define the location to deploy Maven build artifacts (jar, war, ear) into. Also, the location to download dependencies from.

\* For instance, release dependencies to be resolved from the ‘libs-release’ repository & the snapshot dependencies from the ‘libs-snapshot’ repository.

\*Though by default all the build artifacts are deployed to Artifactory, filter can also be applied based on their names, using the ‘addInclude’ & ‘addExclude’ method.

(3) We can also capture the enviornment variables and publish the build information in artifactory. Set build-Info object to automatically capture enviornment variables while downloading & uploadng files.

\*By default enviornment variables, like ‘password’, ‘secret’, or ‘key’ are excluded & will not be published to Artifactory.

**Stage (iv). Executing Maven goals**

![](https://cdn-images-1.medium.com/max/1100/1*6lE0BYmOonqYjJPdkUwvow.png)

**Stage (v). Publishing Build Information:**

![](https://cdn-images-1.medium.com/max/1100/1*HqMdxjZANeuPnr4m1TawCg.png)

###### **Step 5. Commit & push the Jenkinsfile** in the source code repository specified in Stage (i), with the git (Use git add, git commit & git push command).

![](https://cdn-images-1.medium.com/max/1100/1*3kCW7ElcZ8c_yUBJ9b9ZKQ.png)

###### **Step 6. Build Project/job:**

Go to Jenkins dashboard. Select your project (“CD-pipeline”). Click on “Build Now”.

\[caption id="" align="alignnone" width="1375"\]![](https://cdn-images-1.medium.com/max/1375/1*Z-PcyZCGH1HNCj4MAnqwqw.png) Pipeline Execution : Jenkins\[/caption\]

\*Note 1: Each stage defined in the script can be visualized in the stage view, along with the details (time spent & logs).

\*Note 2: Moreover it is easy to visualize and track the build process. It helps to detect the point/stage where & which build failed.

\*Note 3: Further, we can notify the developers regarding the failed build.

Congratulations !! You have finally automated your pipeline in Jenkins.
