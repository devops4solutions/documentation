---
title: "Jenkins Setup for PHP unit testing and selenium framework on AWS"
date: "2018-06-13"
categories: 
  - "jenkins"
---

This blog will setup the unit testing framework and selenium testing framework for php application

Login to your AWS EC2 Linux Instance

I**nstall Java :** [**Click Here**](https://devops4solutions.com/install-java-on-linux/) **for more details**

wget --header "Cookie: oraclelicense=accept-securebackup-cookie" [http://download.oracle.com/otn-pub/java/jdk/8u171-b11/512cd62ec5174c3487ac17c61aaa89e8/jdk-8u171-linux-x64.rpm](http://download.oracle.com/otn-pub/java/jdk/8u171-b11/512cd62ec5174c3487ac17c61aaa89e8/jdk-8u171-linux-x64.rpm)

```
sudo yum localinstall jdk-8u171-linux-x64.rpm
```

```
export JAVA_HOME=/usr/java/jdk1.8.0_171/
export JRE_HOME=/usr/java/jdk1.8.0_171/jre

PATH=$PATH:$HOME/bin:$JAVA_HOME/bin

export PATH
sudo alternatives --config java
```

**Install Jenkins :** [**Click here**](https://devops4solutions.com/jenkins%E2%80%8A-%E2%80%8Ainstallation-and-setup/) **for more details**

sudo wget -O /etc/yum.repos.d/jenkins.repo [http://pkg.jenkins-ci.org/redhat/jenkins.repo](http://pkg.jenkins-ci.org/redhat/jenkins.repo)

sudo rpm --import [http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key](http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key)

sudo yum install jenkins

sudo yum install git

sudo service jenkins start

Open the url in the browser. Default port is 8080

[http://localhost:8080/](http://ec2-52-91-111-146.compute-1.amazonaws.com:8080/)

It will ask for the initial password, please run the below command

sudo cat /var/lib/jenkins/secrets/initialAdminPassword

Jenkins setup with windows agent is completed successfully.

Now to run any job on windows slave, you have to provide the label as “windows” in any Jenkins job as shown below

![](https://cdn-images-1.medium.com/max/800/1*R-VVFGNOi0HuqUyVUbL1gQ.png)

**Integrate Jenkins with Gitlab** 

[Click here](https://devops4solutions.com/integrate-jenkins-with-gitlab/) to do the setup

**Install Plugins in Jenkins**

Manage Jenkins -> Manage Plugins -> Gitlab,gitlab hook,junit,testng results

![](https://cdn-images-1.medium.com/max/800/1*py1dibPnKC_Bl7a8LlV4GA.png)

**Setup SSH connection between Jenkins and Gitlab**

[Click here](https://devops4solutions.com/ssh-jenkins-server-with-gitlab/) to do the setup

**Configure Windows Agent to clone git repo.**

Manage Jenkins -> Manage Nodes -> Windows Node — Configure -> Add Tools Location

Configure Java and Git ( Installed Java and Git manually on windows agent)

![](https://cdn-images-1.medium.com/max/800/1*tgWZVUdNzMpPeQgsnUH3Fw.png)

### Configuring webhooks in GitLab

This will trigger the Jenkins job as an when there is a push in the repository

Go to specific project in gitlab-> click on settings ->integration as shown below

![](https://cdn-images-1.medium.com/max/800/1*oFweEdLszXDnPN2suF0LNw.png)

Put the jenkins job project url and all default setting

[http://34.228.188.96:8080/gitlab/build\_now](http://34.228.188.96:8080/gitlab/build_now)

Click on test and push events

<html>
<head>
<meta http-equiv="Content-Type" content="text/html;charset=utf-8"/>
<title>Error 403 No valid crumb was included in the request</title>
</head>
<body><h2>HTTP ERROR 403</h2>
<p>Problem accessing /job/Test\_php/. Reason:
<pre>    No valid crumb was included in the request</pre></p><hr><a href="http://eclipse.org/jetty">Powered by Jetty:// 9.4.z-SNAPSHOT</a><hr/>

</body>
</html>

Error after comitting in gitlab

No need for this -> disable csrf in security
configure credential — for anonymous access

All configuration is setup successfully.

Now we will create a jenkins job for running selenium test case

**Create a Freestyle job in Jenkins**

Set Label that this job will run only on windows agent

![](https://cdn-images-1.medium.com/max/800/1*CvCPr35s21eQmroGM6XIFA.png)

![](https://cdn-images-1.medium.com/max/800/1*XVDt4Ja0SFj9FX_yfG0vAQ.png)

![](https://cdn-images-1.medium.com/max/800/1*B7R9G7FvFZt6aoJoi58OQA.png)

Job Successfully completed. Click on Build now and see the results.

**Setup for PHP Unit test cases**

**Install php and phpunit on linux jenkins server**

sudo yum install php56

wget [https://phar.phpunit.de/phpunit-5.6.1.phar](https://phar.phpunit.de/phpunit-5.6.1.phar)
php phpunit-5.6.1.phar — version
chmod +x phpunit-5.6.1.phar
sudo mv phpunit-5.6.1.phar /usr/local/bin/phpunit

Manage Plugins -> Junit (if not already installed)

Create a freestyle project

/usr/local/bin/phpunit — log-junit test.xml -c phpunit.xml .

![](https://cdn-images-1.medium.com/max/800/1*WuyPHAz6UhV8Oi9HHz73QQ.png)

![](https://cdn-images-1.medium.com/max/800/1*RIWI6DbArQ2es00gq6Ofog.png)

Job is successfully setup.
