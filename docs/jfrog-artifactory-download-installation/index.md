---
title: "Download and configure JFrog Artifactory on Windows"
date: "2018-06-19"
categories: 
  - "devops-tools"
tags: 
  - "artifactory"
  - "binary-repository-manager"
  - "jenkins"
  - "jfrog-artifactory"
---

Artifactory is a repository manager that allows you to store and retrieve artifacts, such as dependencies or package files. It is like a local repository in the organization.

_**JFrog Artifactory**_ is the only Universal **_Repository Manager_** supporting all major packaging formats, build tools and CI servers. Software binaries (e.g. files created by the build process, any libraries and static files that application requires) are managed through JFrog Artifactory.

Manual distribution of files across many file system is very in-efficient and error-prone in case of updates, rollbacks and uninstall. In contrast, various advantages are offered by JFrog Artifactory.

JFrog Artifactory provides,

1. Fully-reproducible builds
2. Full visibility of deployed artifacts and used dependencies
3. Information about build environment and build status for full trace-ability
4. Bidirectional connection between build and artifact information inside the CI server and Artifactory.

##  **Download and configure JFrog Artifactory**

**Pre-requisite:** Java 1.8

Following are the steps to download and create repositories and configure permissions to certain users in JFrog Arifactory:

###### a. Firstly, download the JFrog artifactory .zip folder from [https://bintray.com/jfrog/artifactory/jfrog-artifactory-oss-zip/4.15.0](https://bintray.com/jfrog/artifactory/jfrog-artifactory-oss-zip/4.15.0)

###### b. Extract the .zip folder in your system. Go to the Bin folder and execute _artifactory.bat_

###### c. Go to the browser and visit localhost:8081 in order to visit to the artifactory in browser.

![](https://cdn-images-1.medium.com/max/1100/1*icyBHkktV09KMIEQCCG6NQ.png)

###### d. Log in as admin by providing the default credentials:

> Username: admin

> Password: password

###### e. You can create a Local repository to store package files created by the Jenkins/Maven project:

Go to Admin -> Repositories ->Local -> New

![](https://cdn-images-1.medium.com/max/1100/1*b9VcHccn--aa7K0yLwMq6Q.png)

###### f. Select Maven

![](https://cdn-images-1.medium.com/max/1100/1*v8AUWL3MBhGLV3bNaCI3Zw.png)

###### g. Provide key (name: Jenkins-integration) for your repository and check Handle Release and deselect Handle Snapshot.

![](https://cdn-images-1.medium.com/max/1100/1*xOGgyuctrcbEu2xJmnqL3w.png)

###### h. Similarly create another local repository with key (e.g. Jenkins-snapshot) and check Handle Snapshot while deselecting Handle Release.

###### i. Also, create a user that you can utilize from Jenkins to access Artifactory:

Go to admin -> Security -> users -> Click on NEW from Users management window -> Add new user->Save

![](https://cdn-images-1.medium.com/max/1100/1*0AvBNt8VUt9Y3H-eLKZmXw.png)

![](https://cdn-images-1.medium.com/max/1100/1*Q0VtfiWrSXeBrskcN8dzNQ.png)

Verify the list of users.

###### j. Furthermore, provide the newly created user with permissions to the repositories:

Firstly, go to admin -> security-> users

Secondly, give the name to the permission

Lastly, choose the repositories on which you want to set the permission

\-Click save & finish

![](https://cdn-images-1.medium.com/max/1100/1*kanDd6-TzojQ8EJ5SPxuwA.png)

- Check the Permissions Management section in Artifactory for recent changes:

![](https://cdn-images-1.medium.com/max/1100/1*EFiaYL6et1bHP5RvyUq6zg.png)

###### k. Edit the permission and assign the user:

![](https://cdn-images-1.medium.com/max/1100/1*aGDGK03q9k36b7JrJ90MvQ.png)

Also, check the Permissions Management section in Artifactory for recent changes:

![](https://cdn-images-1.medium.com/max/1100/1*0dl55f2RH8_mU4wFZShfrQ.png)

Congratulations !! Finally you have configured JFrog Artifactory.

Seems like, now you are ready to integrate  Artifactory with CI server (e.g. Jenkins).

Also, click [here](https://devops4solutions.com/jenkins-jfrog-artifactory-integration/) to integrate Jenkins with JFrog Artifactory !!
