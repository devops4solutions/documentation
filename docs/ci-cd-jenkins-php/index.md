---
title: "CI/CD for PHP using Jenkins as CI server and Apache2 as deployment server"
date: "2018-08-15"
categories: 
  - "jenkins"
tags: 
  - "apach2"
  - "aws"
  - "jenkins"
  - "php"
---

This blog will specify the steps to conduct CI/CD for PHP using Jenkins as CI server and Apache2 as deployment server. In this use case, both are installed on separate AWS ec2 Ubuntu 16.04 instances.

### **General Workflow:**

Our PHP code resides on a SCM (Source code Management) server, which is in this case is Github. Jenkins (CI server) will fetch the code from the github account automatically upon check-in. Jenkins will run the build and will deploy the code in the /var/www/html directory of the Apach2 web server. To accomplish this Jenkins instance has to connect to the Apache2 instance via SSH to trigger the deployment process.

![](https://cdn-images-1.medium.com/max/1100/1*8IeSB0_e4ZQmXJMskHp9Cw.png)

### Pre-requisite:

1. Two AWS ec2 Ubuntu 16.04 instances.
2. Jenkins and Java installation on Ubuntu instance one (Click [**here**](https://devops4solutions.com/installing-java-jenkins-on-aws-ec2-ubuntu-16-04/)).
3. Apache2 and PHP5.6 installation on Ubuntu instance second(Click [**here**](https://devops4solutions.com/apache2-php5-6-aws-ec2-ubuntu-16-04-installation/)).

Following steps have to be employed:

#### Configure Jenkins:

Step 1. Go to browser and start Jenkins server at default port 8080

![](https://cdn-images-1.medium.com/max/1100/1*3vByyITZuSlVPCjH9TZ-IQ.png)

Step 2. After login, Go to Manage Jenkins → Manage Plugins → Available → search for “Publish over SSH” plugin → install without restart.

Step 3. Go to Manage Jenkins → Configure System → Publish over SSH

![](https://cdn-images-1.medium.com/max/1100/1*JLt9NLb4w5kCbRe2ibNthQ.png)

Either provide the path to the generated ssh key or paste it directly. It is important to paste everything including header and footer as shown in the above snip from my lab experiment.

If wondering how to generate a SSH key to establish connection between two servers, click [**here**](https://devops4solutions.com/ssh-aws-ec2-instances/) to get more details and steps.

Then click on ADD button in order to add a server to SSH with/ connect with.

![](https://cdn-images-1.medium.com/max/1100/1*w-zqEhTCXMpOgyln6m3V8g.png)

Fill in the details, like;

- Name: Provide any logical name which can be used later on to configure job.
- Hostname: ip address of the server to connect to (in this case it is the instance second hosting Apache2)
- Username: name of the user to login to (in this case it is ubuntu)
- Remote Directory: path of any directory you want to deploy to (or can be leaved blank for later configuration within the job)

Step 4. Click ‘Test Configuration’ to confirm the connection and ‘Save’ at the bottom of the page.

Note: Both servers must be up and running to test the configuration.

#### Create a Jenkins Job:

Step 1. Go to Jenkins Dashboard → Click on ‘New Item’ → Provide name of the project (e.g. CI-CD-PHP) → choose ‘Freestyle Job’ → Click ‘OK’.

In the configuration window of the job:

Step 2. Provide the Git URL from where code has to be pulled from.

![](https://cdn-images-1.medium.com/max/1100/1*-KsiW-hSaewTAKIsB2XZyA.png)

Step 3. In the Build Environment section choose:

a). Delete workspace before build starts.

b). Send files or execute commands over SSH after the build runs

![](https://cdn-images-1.medium.com/max/1100/1*PLCFenhXPqo6RpI5HGMZ_Q.png)

Provide the Name of the server, source files and remote directory.

OR

Another strategy is to archive artifacts and then send build artifacts over SSH

![](https://cdn-images-1.medium.com/max/1100/1*UupLUuZmGtvydgvCAePm_A.png)

Also specify the remote directory (Although, if already specified in the configuration system then not necessary to specify it here).

Step 4. Save the job and build it.

Step 5. Check the Apache2 server for the successful transfer of your files.

Finally, we have successfully completed all the steps required to setup a CI/CD workflow for a PHP code.
