---
title: "Run Simple Mean Application using Docker"
date: "2018-06-13"
categories: 
  - "docker"
---

Commands to run:

1\. Docker should be installed on that machine

2\. docker pull nikhilnidhi/nodeapp

3\. docker pull mongo ( This is official docker image of mongo)

4\. docker run -p 27017:27017 -d — name db mongo ( This one I created on top of one docker image which will install the required software like nodejs)

5\. docker run -p 3000:3000 -p 35729:35729 — name nodeapp — link db:db nikhilnidhi/nodeapp
