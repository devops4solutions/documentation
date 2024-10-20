---
title: "Jenkins integration with JFrog Artifactory"
date: "2018-06-19"
categories: 
  - "jenkins"
tags: 
  - "jenkins"
  - "jfrog-artifactory"
coverImage: "Untitled4-1.png"
---

 

This integration allows your build jobs to deploy artifacts and resolve dependencies to and from Artifactory, and then have them linked to the build job that created them.

**STEP I. Download and configure JFrog Artifactory**

Following are the steps to download and create repositories and configure permissions to certain users in JFrog Arifactory:

a. Download the JFrog artifactory .zip folder from [https://bintray.com/jfrog/artifactory/jfrog-artifactory-oss-zip/4.15.0](https://bintray.com/jfrog/artifactory/jfrog-artifactory-oss-zip/4.15.0)

b. Extract the .zip folder in your system. Go to the Bin folder and execute _artifactory.bat_

c. Go to the browser and visit localhost:8081 in order to visit to the artifactory in browser.

![](https://cdn-images-1.medium.com/max/1100/1*icyBHkktV09KMIEQCCG6NQ.png)

d. Log in as admin by providing the default credentials:

> Username: admin

> Password: password

e. You can create a Local repository to store package files created by the Jenkins/Maven project:

Go to Admin -> Repositories ->Local -> New

![](https://cdn-images-1.medium.com/max/1100/1*b9VcHccn--aa7K0yLwMq6Q.png)

f. Select Maven

![](https://cdn-images-1.medium.com/max/1100/1*v8AUWL3MBhGLV3bNaCI3Zw.png)

g. Provide key (name: Jenkins-integration) for your repository and check Handle Release and deselect Handle Snapshot.

![](https://cdn-images-1.medium.com/max/1100/1*xOGgyuctrcbEu2xJmnqL3w.png)

h. Similarly create another local repository with key (e.g. Jenkins-snapshot) and check Handle Snapshot while deselecting Handle Release.

i.Create a user that you can utilize from Jenkins to access Artifactory:

Go to admin -> Security -> users -> Click on NEW from Users management window -> Add new user->Save

![](https://cdn-images-1.medium.com/max/1100/1*0AvBNt8VUt9Y3H-eLKZmXw.png)

![](https://cdn-images-1.medium.com/max/1100/1*Q0VtfiWrSXeBrskcN8dzNQ.png)

Verify the list of users.

j. Provide the newly created user with permissions to the repositories:

Go to admin -> security-> users

\- Give the name to the permission

\- Choose the repositories on which you want to set the permission

\-Click save & finish

![](https://cdn-images-1.medium.com/max/1100/1*kanDd6-TzojQ8EJ5SPxuwA.png)

- Check the Permissions Management section in Artifactory for recent changes:

![](https://cdn-images-1.medium.com/max/1100/1*EFiaYL6et1bHP5RvyUq6zg.png)

k. Edit the permission and assign the user:

![](https://cdn-images-1.medium.com/max/1100/1*aGDGK03q9k36b7JrJ90MvQ.png)

Check the Permissions Management section in Artifactory for recent changes:

![](https://cdn-images-1.medium.com/max/1100/1*0dl55f2RH8_mU4wFZShfrQ.png)

Now you are ready to integrate Artifactory with Jenkins.

## Jenkins integration with JFrog Artifactory

**Step II. Artifactory Plugin configuration in Jenkins**

a. Go to Jenkins dashboard -> Manage Jenkins -> Manage Plugins -> Available -> Artifactory -> Install without restart.

![](https://cdn-images-1.medium.com/max/1100/1*MdRfpjGp0MJoFdEHATRnGw.png)

b. Configure Artifactory-related settings in Jenkins:

Go to Jenkins dashboard -> Configure System ->Artifactory section ->Add artifactory server -> provide the details -> Test the connection ->apply & save

![](https://cdn-images-1.medium.com/max/1100/1*gFgfG_jFiKh4CyGsUAqVGg.png)

c. Go to a Jenkins project that creates a package file after compiling all of the source files.

Go to Build Environment section -> Resolve artifacts from artifactory -> Click on refresh Repositories ->select the repository in release and snapshot field from the lists.

![](https://cdn-images-1.medium.com/max/1100/1*pWdKQM9J1MD7QSSxVYMq3A.png)

d. Go to Add post-build section ->select deploy artifacts to artifactory -> click on refresh -> choose the target releases and snapshot repository (repositories created earlier) ->save

![](https://cdn-images-1.medium.com/max/1100/1*weOHtpFGjcz-MEGja85W5A.png)

e. Click on Build now and verify logs in the Console Output. Jar files are resolved from the local repository or Artifactory.

f. Once the package is created, it is stored in artifactory too. Go in the artifactory and check the package.
