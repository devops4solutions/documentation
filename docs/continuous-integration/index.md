---
title: "What is Continuous Integration?"
date: "2018-06-19"
categories: 
  - "devops-tools"
tags: 
  - "continuous-integration"
  - "jenkins"
coverImage: "Untitled2.png"
---

## **Definition :** 

_**Continuous Integration (CI)**_ is a software development practice that requires developers to integrate their code into a shared repository at regular intervals (usually each person integrates at least daily - leading to multiple integrations per day). Further, e52ach integration/ check-in is verified by an automated build (including test) to detect integration errors  as quickly as possible.

\- Martin Fowler

## **What is the essence of CI?**

At a regular interval (ideally at every commit), the system is:

- Firstly, INTEGRATED:  All changes up until the point of last commit are combined into the project.
- BUILT: The code is compiled into an executable package.
- TESTED: Automated suits are run.
- ARCHIVED: Versioned & stored so it can be distributed as is, if desired.
- Finally, DEPLOYED: Loaded onto a system where developers can interact with it.

"Continuous Integration doesn't get rid of bugs, however it does make them dramatically easier to find and remove"

                      - Martin Fowler

## **Why  CI is required?** 

Earlier, a team of developers used to work in isolation for long. The code/changes in the code are only merged to the master branch of the shared repository, once the work gets finished. Consequently, this resulted into a difficult and time-consuming merging code process, which also lead to bugs accumulation for an extended period without correction. These elements made it tough to deliver updates quickly to end-users.

##### **CI Benefits:**

- Immediate bug detection for remediation
- Improves productivity of developers : it free developers from manual tasks and encourages practices that help in reducing errors released to end-users.
- Faster updates delivery to the end-users: It creates a  deployable system at any given point of time.
- Above all, record of evolution of project.

##### **CI Tool set:**

1. Code Repositories : SVN, Mercurial, Git etc.
2. Continuous Build System : Jenkins, Bamboo, Cruise Control, TeamCity, Apache Gumbp etc. (Click [10 Best DevOps tools for Continuous Integration](https://www.marutitech.com/devops-tools-continuous-integration/) ).
3. Test Frameworks : JUnit, Cucumber, Cpp Unit etc.
4. Artifact Repositories : Nexus, Artifactory, Archiva etc.

Furthermore, click [here](https://devops4solutions.com/jenkins%e2%80%8a-%e2%80%8ainstallation-and-setup/) to start using Jenkins, Git, Artifactory, SonarQube etc.

###### **References:**

- https://aws.amazon.com/devops/continuous-integration/
- https://www.thoughtworks.com/continuous-integration
