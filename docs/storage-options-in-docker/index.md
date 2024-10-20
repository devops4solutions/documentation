---
title: "Storage Options in Docker"
date: "2018-09-04"
categories: 
  - "docker"
tags: 
  - "docker"
  - "storage"
---

In this blog we will explore the Storage Options in Docker.

# Introduction

A Docker container has a writeable layer, and this is where the data is stored by default. However, this has several drawbacks. Because the container can be replaced and removed at any time, the data is also lost. So, data is not persisted when the container is removed. Data stored in this writeable layer is also tightly coupled to the container. This means that it’s very difficult to share this data amongst other containers if needed. So, storing data inside a container is not always recommended.

Every host machine will have its own filesystem, and inside this, there is an area specifically for Docker. The host also has its own memory.

**Three ways to mount data inside a Docker container**

\[caption id="" align="alignnone" width="604"\]![Mount in Docker](https://cdn-images-1.medium.com/max/800/1*sUWmf6i3l3NCGVBtHqIN5w.png) Mount in Docker\[/caption\]

1. ### Docker volumes
    

A Docker volume sits inside the Docker area, within the host’s filesystem. This can be shared amongst other containers. Docker volume is a Docker object with its own ID, and it’s decoupled from the container. Docker volumes can be attached and shared across multiple containers, meaning that you can have different types of data running against different containers. For example, you could have an Apache web server, and you could have logs going into one container, and you could have another container that actually handles the HTTP requests

\[caption id="" align="alignnone" width="603"\]![volumes](https://cdn-images-1.medium.com/max/800/1*6qZV-IzsC6_VMRn9hFaAFg.png) volumes\[/caption\]

### 2\. Bind mounting

Data is directly coming from the host’s file system. This is great for developers who are building applications stored in the host’s filesystem within a local development environment. Bind mounts are great when developing applications in your local development environment. You can mount your working directory into the container and write code within your host machine, and see changes appear within the container.  bind mounts cannot be shared across containers.

\[caption id="" align="alignnone" width="537"\]![bind mount](https://cdn-images-1.medium.com/max/800/1*9si8FmAlWePMwze2Xdqkrg.png) bind mount\[/caption\]

### 3\. using the tmpfs

tmpfs is stored in the host’s memory. Storing data in the host’s memory is useful when the container generates data which is to be considered a throwaway or non-persistent data. You gain better performance when storing this non-persistent data in the host’s memory instead of the container’s writeable layer.

\[caption id="" align="alignnone" width="496"\]![tmpfs](https://cdn-images-1.medium.com/max/800/1*87G1PAFMuf2Pe4A7QlePmw.png) tmpfs\[/caption\]
