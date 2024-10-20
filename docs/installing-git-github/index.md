---
title: "Working with Git & GitHub"
date: "2018-06-25"
categories: 
  - "devops-tools"
---

### **Installation &  incorporation of Git to your project**

On Windows:

Step 1. Check if Git is already installed. Go to command prompt and type:

git --version

If not install then go to git-scm.com and download 64 bit .exe file for Windows 10.

Step 2. Add your project to Git

a). Go to command prompt and browse to your project folder which you want to add to Git.

git init

// this command initialize tracking of your folder and files.

// a hidden .git folder will be generated & added in your folder

// Git will start tracking your folder

b). Show status

git status

//This command will show the untracked files, current branch, files modified from the last commit, etc.

c). Add file to the staging area, so that these changes can be recorded in the next commit.

git add <file\_name>

git add .

\*Use git status to find out which files are in the index area, i.e. the files which have not been staged

d). Remove files from staging area.

git rm --cached <file\_to\_be\_remove>

e). Commit: When you use commit command, Git compresses the files that have been staged/indexed, gives them a unique name, a hash, and stores them in the object directory. Thus, commit creates a type of snapshot of your working directory.

When you commit, git does the following two tasks:

1). If the file is unmodified, git will just add the name/the hash of the compressed file into the snapshot.

2). If the file has been modified, git will compress it, will store it in the object folder and will finally add the hash of the compressed file into the snapshot.

git commit -m"your\_commit\_message"

Step 3. Adding files to the Remote Repository (GitHub):

In order to share code with others, we require to set up a remote repository.

a). Go to https://github.com/ and create a free account.

"GitHub" is a website which provides us remote remote repository, so you can put all your work on the cloud, thereby it is backed up & interactive through its visual interface.

b). Start a new project/Create a new repository (public) and copy the https url (say '**x**') of the repository created.

c). On the command prompt, in the project folder, where git has been initialized, type the following command:

git remote add origin **x**

// where x is the URL of the remote repository

d). Push the files to the remote repository

git push -u origin master

// We are pushing/uploading the default branch (master) to origin.

Congratulations !! You have successfully installed Git and has saved the different versions of your project in the local as well as remote repository.

Note: Whenever you will push your repository, you will be asked to enter email & user name. In order to evade this step, use the following commands after installing GitHub:

git config --global user.email <your\_github\_account>

git config --global user.name <your\_github\_username>

//Whatever commit will be done, will be reflected by the username.
