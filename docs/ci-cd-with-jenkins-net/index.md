---
title: "CI/CD with Jenkins & .NET"
date: "2018-08-03"
categories: 
  - "jenkins"
---

This blog will help you setup the CI/CD with Jenkins & .NET.

# Tools Used:

- **Platform**: .NET 4.5.2
- **IDE**: Visual Studio 2015 (MSBuild files)
- **Application type**: Windows Service
- **NuGet**: Package Manager used for all references.
- **Source Control**: Git (github.com)

## Install the MSBuild Plugin for Jenkins:

Manage Jenkins -> Manage Plugins -> Msbuild -> Install it

![](https://cdn-images-1.medium.com/max/800/1*_VIWH2r2Dq6j6GDCNP3PoQ.png)

## Configure MSBuild

Manage Jenkins — Global Tool Configuration ->

Path -> C:\\Windows\\Microsoft.NET\\Framework\\v4.0.30319  ( This is by default present if .net framework is installed don your machine). Check the path and version and accordingly put in the jenkins configuration.

![](https://cdn-images-1.medium.com/max/800/1*iR2sYglj0P-G3eXMyxc9Dw.png)

 

MSBuild is successfully configured.

Now we will create a job in Jenkins

1.  Open Jenkins Url -> Create a free style project
2. In Source Control Management - Select Github ->https://github.com/devops4solutions/jenkins-ci-dotnet.git

![](https://cdn-images-1.medium.com/max/800/1*h6_ltwppgj9gvkY3LvHqYw.png)

 

Add a build Step -> Execute Windows Batch Command ->nuget restore src/MyWindowsService/

![](https://cdn-images-1.medium.com/max/800/1*_-E8_OwGElBzDZJEVq8yFA.png)

Add another build step ->  Build Visual Studio project -> src/MyWindowsService/MyWindowsService/Deploy-Windows-Service-Via-MSBuild.proj

![](https://cdn-images-1.medium.com/max/800/1*zAXjHKruUswcB6Zx2kIv-Q.png)

 

Now save the project and trigger the build.

![](https://cdn-images-1.medium.com/max/800/1*-m_Vx9MmfeP1SCFsBrECUA.png)

## Check the Results

 

Open Services -> check whether the services got created as shown in the screenshot

![](https://cdn-images-1.medium.com/max/800/1*sgX-rPhTUoVkm-IF5-TrwA.png)

 

Congratulations, finally you have setup a CI/CD with Jenkins & .NET.
