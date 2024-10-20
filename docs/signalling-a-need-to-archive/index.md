---
title: "Archiving in Jenkins"
date: "2018-08-07"
categories: 
  - "jenkins"
tags: 
  - "archiving"
  - "groovy"
  - "jenkins"
  - "script"
---

This blog will discuss a way to signal a need to archive in Jenkins using groovy script. You can uses script console link to write groovy script that checks for the last successful run of any job; if the year is different to the current year, then a warning is set at the beginning of the job’s description. Thus, it is hinting to you that it’s time to perform an action, such as archiving and then deleting.

##### Archiving:

Following are the steps to signal the need to archive in the description of a project in Jenkins.

Step 1. Firstly, log into Jenkins as an Administrator.

Step 2. Go to Manage Jenkins -> Script Console link

\[caption id="" align="alignnone" width="1276"\]![Manage Jenkins window](https://cdn-images-1.medium.com/max/1375/1*k5FlQAEfX00NcpctBx6aOg.png) Manage Jenkins window\[/caption\]

 

\[caption id="" align="alignnone" width="1375"\]![Script console link](https://cdn-images-1.medium.com/max/1375/1*agfk3oSHpzztF38ztJ737A.png) Script console link\[/caption\]

Step 3. Type the following Groovy script and run it.

def warning=’\[You can ARCHIVE this Job\]’

def now=new Date()

for (job in hudson.model.Hudson.instance.items) {

println “\\nName: ${job.name}”

Run lastSuccessfulBuild = job.getLastSuccessfulBuild()

if (lastSuccessfulBuild != null)

{ def time = lastSuccessfulBuild.getTimestamp().getTime()

if (now.month.equals(time.month)){

println(“Project has same month as build”);

}else

{ if (job.description.startsWith(warning)){ println(“Description has already been changed”); }

else{

job.setDescription(“${warning}”) } } } }

Step 4. Successful execution of the script will give output based on the build jobs available in your Jenkins.

Step 5. Any project that had its last successful build in another month than this will have the description you can archive this job added to its description.

Step 6. Click on Configure to check the description as well.

\[caption id="" align="alignnone" width="1067"\]![Archiving signal](https://cdn-images-1.medium.com/max/1375/1*i_rUdO243fUoqmlSsSvDQw.png) Archiving signal\[/caption\]

To write your CI/CD pipeline in Jenkins click [here](https://devops4solutions.com/jenkins-pipeline-code/).
