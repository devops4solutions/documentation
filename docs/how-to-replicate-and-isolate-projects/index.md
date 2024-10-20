---
title: "How to Replicate and Isolate Projects"
date: "2018-09-04"
categories: 
  - "docker"
tags: 
  - "docker"
---

In this blog, we will explore How to Replicate and Isolate Projects

Let’s take an example, we are managing multiple projects and for that we managed many servers that had different versions of PHP, MySQL, and even different operating systems. My local machine was built differently to my colleagues’. It ran different hardware and different software. Stuff that worked on my machine didn’t always work on my colleague’s machine, and vice versa. Like all developers, I thought that everything seemed fragmented, and I was left asking the question, “how can I replicate and isolate each of these environments?”

During the life of a web developer, we often work on multiple projects. Some of these projects will have different versions of components and services as shown below

![](https://cdn-images-1.medium.com/max/800/1*Mn6rtaPU9mqpCTn_7jBRqw.png)

 For example, Project A may have a different version of PHP to that of Project B. Also, things like settings and configurations will be different amongst each of the projects, and it’s very difficult to manage each project individually in isolation. Ideally, what we would like to do is isolate each of these projects within their own environments.

So there are various methods to isolate the development environment.

1. Change setting on your system for each project. Its a bad idea and prone to human error and also its not very scalable
2. Virtual Box/VMWare

Using multiple virtual machines is not a good idea either. Virtual machines require lots and lots of resources. Virtual machines cannot be reused very easily either. So, for example, it’s very difficult to create a base snapshot of, say, some base packages, and then extend that to different package versions and interchange the images and snapshots. Even though a virtual machine is saved to a file or series of files, it’s not easily shared amongst team members. So, let’s say the virtual machine instance was being shared and used at the same time by other team members. It’s very difficult to replicate changes made by one team member in that virtual machine instance to the other instances for the other team members. Virtual machines cannot be easily ran in parallel. If you need to work on multiple projects at the same time, you’re going to need to run multiple virtual machines at once. And, as I’ve mentioned before, virtual machines are resource hungry, so you’re going to need a monster of a host machine in order to do this.

3\. Docker

Docker is extremely lightweight. An application can be split up into separate containers to form micro services. This means that a container is an independently deployable and modular service. Docker is very extendable. We can create base images that hold generic and global configuration for things such as security and logging. These base images can then be extended to other images, which are used within our projects. So, for example, you could have an image that is tailored to a specific version of MySQL. We can then store the make up of an image in a Dockerfile. We can create these images in such a way that allows us to inject settings and configuration on a per-project basis. This means that we can decouple the image from the project and inject project configuration when the image is used. This also means that we can reuse an image in multiple projects. Not only can the Docker files be stored in source control, but the images themselves can also be stored in a public or private repository. This makes it very easy to share our images amongst each project, each team member, and also to the general public.
