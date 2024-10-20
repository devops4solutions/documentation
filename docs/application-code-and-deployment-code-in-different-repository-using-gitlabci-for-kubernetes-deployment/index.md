---
title: "Application and deployment Code in different repository using GitlabCI for Kubernetes deployment"
date: "2020-06-04"
categories: 
  - "kubernetes"
tags: 
  - "build"
  - "deployment"
  - "gitlab"
  - "gitlab-runner"
  - "kubernetes"
---

In this blog, we will be exploring options how to store the application and the deployment code in different repository for the kubernetes deployment using Gitlab CI.

If you want to see the video for this article, [click here](https://youtu.be/1BIC0sbhfqU)

### **Problem Statement:**

- When you have multiple applications that uses the same deployment code to build that then how will you accomplish that ?
- Storing a deployment code in each application repository could be a major issue for the deployment team. If they need to update any changes in their code then they have to do it in all the application repository.

### **Solution:**

To resolve this type of issue we will be exploring the concept of git submodule.

### **Git Submodules**

- It is used when you want to use the code of the different repository in your repository as a third party library/dependencies etc.
- Using submodule you can store your deployment code also in different repository

#### **How to setup**

1\. Deployment Code Repository( [https://github.com/devops4solutions/submodule-config-repo.git](https://github.com/devops4solutions/submodule-config-repo.git))

2\. Application Code repository ( [https://github.com/devops4solutions/application-code-using-submodule.git](https://github.com/devops4solutions/application-code-using-submodule.git))

3\. Place all the deployment code in first repository and commit the changes

4\. In app code, run the below command to add the submodule. You can provide folder name so that all code will go inside the folder. If you will not provide the folder, folder will get created automatically as per the repository name, in this case build-configs-wp. I have placed . sign before configs to make it as a hidden directory

git submodule add [https://github.com/devops4solutions/submodule-config-repo.git](https://github.com/devops4solutions/submodule-config-repo.git) .configs

![git submodule add](https://miro.medium.com/max/1328/1*kqYqV_jItEmqKOYxV6fc1g.png)

5\. After this, you need to run the below command to get all the changes of the submodule

git submodule update --init  
git submodule update --recursive --remote  
git status

![git submodule update](https://miro.medium.com/max/801/1*aFK5Z0tpPOa8r2kEJNcQKw.png)

6\. Now if you have requirement that all build code should be present at the root of the project then you can run the below command

cp -rf configs/\* .

This way you can have all your changes in the application repository at the root of the project and you will not see any build failures.

#### **Benefits:**

- Easy to maintain deployment code
- Changes need to be done only at one place instead of multiple places

#### **How to Setup a .gitlab-ci.yml file**

1. Declare the variable and run the command in before\_script section, this will ensure that your CI job will take the latest code from deployment repository

before\_script:
        - git submodule sync --recursive       
        - git submodule update --init --remote
        - cp -rf .configs/\* .

Congratulations, we have successfully setup how to separate Application Code and Deployment Code in different repository using GitlabCI for Kubernetes deployment
