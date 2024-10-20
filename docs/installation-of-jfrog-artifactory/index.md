---
title: "Installation of JFrog Artifactory"
date: "2018-06-13"
categories: 
  - "devops-tools"
---

Download artifactory ( Installed on windows)

[**jfrog-artifactory-oss-zip** _Download jfrog-artifactory-oss-zip from Bintray. Modern Software Distribution Service for Docker, Maven, Debian, RPM…_bintray.com](https://bintray.com/jfrog/artifactory/jfrog-artifactory-oss-zip "https://bintray.com/jfrog/artifactory/jfrog-artifactory-oss-zip")

For linux

Copy software from your machine to linux

Java should be present

**Install JAVA 8**

yum install wget ( in case wget is not found)

```
$ wget --header "Cookie: oraclelicense=accept-securebackup-cookie" 
```

```
$ sudo yum localinstall jdk-8u161-linux-x64.rpm
```

```
export JAVA_HOME=/usr/java/jdk1.8.0_161/
export JRE_HOME=/usr/java/jdk1.8.0_161/jre
```

```
PATH=$PATH:$HOME/bin:$JAVA_HOME/bin
```

```
export PATH
sudo alternatives --config java
```

Start artifactory.bat/.sh,present in bin folder

Go to your browser and visit IP\_ADDRESS:8081 to visit Artifactory in the browser:

Login — default username password — admin/password

![](https://cdn-images-1.medium.com/max/800/0*X83NYuaX2vFV0d60.png)

Create a Local repository to store package files created by the Jenkins project:

Click on Admin -> Repositories ->Local

![](https://cdn-images-1.medium.com/max/800/0*zFV-LS9wizGyOIyt.png)

Click on New:

![](https://cdn-images-1.medium.com/max/800/0*YBcnohAKMo5ZkKiq.png)

Select Maven as a Project Type:

![](https://cdn-images-1.medium.com/max/800/0*6IvYKNdhIUkzwPDU.png)

Give a Repository Key “jenkins-release” and click on Save and Finish:

![](https://cdn-images-1.medium.com/max/800/0*S30CjJS8muzNEt1I.png)

Similarly, create a Jenkins-snapshot repository:

![](https://cdn-images-1.medium.com/max/800/0*Zikf5L-D63zGQRjk.png)

Verify all repositories in the list:

![](https://cdn-images-1.medium.com/max/800/0*w3LeAz3yHgD0rQQQ.png)

Create a user that you can utilize from Jenkins to access Artifactory:

![](https://cdn-images-1.medium.com/max/800/0*MtCjKjQy_xzS7b-r.png)

Click on New:

![](https://cdn-images-1.medium.com/max/800/0*kMc72n0P7Mr6E3Zp.png)

Provide user details and Save:

![](https://cdn-images-1.medium.com/max/800/0*TlmBUFrfOSW_yL_a.png)

Verify the list of users:

![](https://cdn-images-1.medium.com/max/800/0*P3VmIj9WwR52A82x.png)

Provide the newly created user with permissions to the repositories:

Security -> Pemissions

![](https://cdn-images-1.medium.com/max/800/0*LynTnTdnfYE5hHNJ.png)

Create a new permission “jenkins-permission”

Select Repositories and click on Save & Finish:

![](https://cdn-images-1.medium.com/max/800/0*vkmtAA3npsV5kFwv.png)

Check the Permissions Management section in Artifactory for recent changes:

![](https://cdn-images-1.medium.com/max/800/0*IXACb9rLjYLt5Ps3.png)

Edit the permissions and assign the user: If user is already admin then it has all permissions

![](https://cdn-images-1.medium.com/max/800/0*vDtjfZP9VDOgUXeI.png)

Click on Save & Finish:

![](https://cdn-images-1.medium.com/max/800/0*oZj4dEgpRCtl6fM0.png)

Check the Permissions Management section in Artifactory for recent changes:

![](https://cdn-images-1.medium.com/max/800/0*njOVYotn2nIttQZ4.png)

Jfrog setup is complete
