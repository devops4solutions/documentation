---
title: "Basics of Git & Github"
date: "2018-06-25"
categories: 
  - "devops-tools"
---

**Git is a Distributed Version Control system** to track collaborative work of developers on projects through a Distributed Graph Theory Tree model.

## **What is Version Control System (VCS)?**

It is a software that helps developers to work simultaneously while maintaining complete history of their work, without overwriting each other's work.

For instance, if you have a file to work on and you want to track all the changes you made to that file. You can use VCS which will track all the changes in your file and will maintain all the version history.

![](https://devops4solutions.com/wp-content/uploads/2018/06/Untitled5-300x109.png)

**CVCS :** it makes use of a central server to store all files, thus there is a single central copy of project and developers will commit their changes to this central copy.

**DVCS :** Every developer has a mirror/copy of the entire codebase/repository including its full history on their hard drive.

## **Advantages of DVCS:**

1. In case, if a server goes down, then any of the client's repository can be copied back to the server to restore it.
2. Various operations (other than pulling and pushing) can be done offline as it does not rely on central server.
3. Actions (other than pulling and pushing) can be performed extremely fast, as the tool need to access the hard drive and not a remote server.
4. Committing changes can be done locally and once a group of change-sets ready, then all can be push at once.

## **Why VCS?**

Reason : _Frequency of code change is high_

As developers write code, it is subjected to changes. Example, some new features are required to be added, bugs needed to be fixed, and content may get change.

Most of the code is stored as text files and code is changed by editing these files. When changes are saved, old version is overwritten with the new one.

Unfortunately, no developer is perfect, humans can make mistakes. When a change is made to a file, after saving and compiling, if something went wrong. Then it's helpful to be able to get back to the old version or to find out what actually changed, in order to focus on what developer might have done wrong.

Here comes the use of Version Control System!!

## Basics of Git & Github:

**Git** is a content-addressable file system. When any kind of content is inserted into git repository, it will return a unique key, which can be used later to retrieve that content.

- It is a free and open source DVCS, designed to handle everything from small to very large projects with speed and efficiency.
- In case of the event of a crash or disk corruption, chances of loosing data is rare (as repository is mirrored by various clients).
- Git uses secure hash function (SHA1), in order to name and identify objects within its database.
- There is no need to be online, one can work on the local copy of repository, as Internet connection is required only to pull or push changes.

## **Workflow of Git**

1. You can do a clone or a pull of remote repository on to your own local system.
2. You can check-out any branch, it will create a working copy  and now you can do all your work on this working copy while being offline.
3. Once you are done with changes, you can add all changes to the staging area.
4. Then commit all changes to the local repository.
5. Push the changes to the remote repository (at this step internet is required).

**Github :** It is a cloud based hosting service for Git repositories.

**It is a website to upload your repositories online.**

It helps a fast, flexible and collaborative development process that lets developers work on their own as well as with others.

## **WHY GitHub ?**

1. It provides a backup for repositories on cloud.
2. It provides visual interface to the repositories.
3. It makes collaboration easier.

Git != GitHub

GitHub can be used with any VCS, like, Git, Mercurial etc. Similarly, Git can used with any online repository management system like GitHub, BitBucket etc.
