---
title: "Docker volumes - How to manage data in docker"
date: "2020-09-15"
categories: 
  - "docker"
tags: 
  - "docker"
  - "docker-compose"
---

In this blog, we will see what is docker volumes and why do we need to use volumes. We will explore options for the data storage in docker. How we can persist the data using docker volumes?

If you do not know about Docker, refer my [docker](https://devops4solutions.com/what-is-docker/) article for the basic understanding

Check out [youtube video](https://youtu.be/evkgrnPy3HY) for this article

### **Three types of storage is present in docker**

1. bind mount
2. tmpfs mount
3. volume — is the most preferred mechanism for persist the data

In the below diagram, we have

1. **Host machine** where docker is installed
2. **bind mount** — which uses the filesystem of your local docker host machine to persist/store your data
3. **tmpfs mount —** This works only for Docker on linux.This is temporary and only persist in the host memory.
4. **volumes** — Volumes are stored in the Docker area and managed by Docker only

![](https://cdn-images-1.medium.com/max/800/1*bo6IOrBjaHbtkPgTKT08NA.png)

### **Docker volumes**

- Volumes are the preferred mechanism for data persist
- Managed by Docker
- Shared across multiple containers
- They are often a better choice than persisting data in a container’s writable layer, because a volume does not increase the size of the containers using it, and the volume’s contents exist outside the lifecycle of a given container.

#### **Types of Volumes**

- **Anonymous volume**\- If you do not provide any name for your volume then a random name is generated

volumes:  
      ** - /var/lib/mysql**

- **Named Volume —** This will create a volume with the name you provided in your file. This is the one which people uses in their production environment

volumes:  
      ** - db\_vol:/var/lib/mysql**

### **Let’s create a mysql container which uses Anonymous volume to store the data**

We will launch a mysql and phpmyAdmin container which uses a docker volume so that all data will persist even if we remove the container

Clone this [repo](https://github.com/devops4solutions/docker-volume-example/tree/master/Anonymous-Volume) for trying the below example

version: '3.3'  
services:  
   db:  
     image: mysql:5.7  
     volumes:  
      ** - /var/lib/mysql**  
     environment:  
       MYSQL\_ROOT\_PASSWORD: root  
       MYSQL\_DATABASE: testdb1  
       MYSQL\_USER: testuser  
       MYSQL\_PASSWORD: root  
     ports:  
       - 3306:3306  
   phpmyadmin:  
    depends\_on:  
      - db  
    image: phpmyadmin/phpmyadmin  
    ports:  
      - '8081:80'  
    environment:  
      PMA\_HOST: db  
      MYSQL\_ROOT\_PASSWORD: root

Run the command

cd Anonymous-Volume/
docker-compose up -d

You will see a random volume name is generated as shown below

![](https://cdn-images-1.medium.com/max/800/1*wizzzqYZ4mGoXooL4Oktew.png)

### **Now Let’s create a mysql container which uses Named volume to store the data**

version: '3.3'  
services:  
   db:  
     image: mysql:5.7  
     volumes:  
      ** - db\_vol:/var/lib/mysql**  
     environment:  
       MYSQL\_ROOT\_PASSWORD: root  
       MYSQL\_DATABASE: testdb1  
       MYSQL\_USER: testuser  
       MYSQL\_PASSWORD: root  
     ports:  
       - 3306:3306  
   phpmyadmin:  
    depends\_on:  
      - db  
    image: phpmyadmin/phpmyadmin  
    ports:  
      - '8081:80'  
    environment:  
      PMA\_HOST: db  
      MYSQL\_ROOT\_PASSWORD: root  
**volumes:  
     db\_vol:**

If you don’t declare your variable at the end then you will see the below error 

**ERROR: Named volume “db\_vol:/var/lib/mysql:rw” is used in service “db” but no declaration was found in the volumes section. we always need to declare our volume**

Now you can run this file and can see the volume is created with the name

```
cd Named-Volume/
docker-compose up -d
```

![](https://cdn-images-1.medium.com/max/800/1*6PhCsfESwqPW07Prj3yGKA.png)

### **How data will Persist?**

Now we will see how data will persist even if we remove the container and launch it again.

- You can access the database on the browser [http://yourip:8081](http://yourip:8081) and you can see `testdb1` database has been created.

![](https://cdn-images-1.medium.com/max/800/1*Fa03o5pPfvXr0NQcuqnRDQ.png)

-  Create some tables and then delete the container

![](https://cdn-images-1.medium.com/max/800/1*qIr8yu367u-YZgCAZdXvww.png)

docker-compose down

- Now when you run it again you should be able to see all the tables which you have created

docker-compose up -d

- But if you delete the volume then data will go away. Volume deletion is a separate process if you will not defined explicitly then volume will not get deleted
- You can see same data is present inside the container and in the volume

![](https://cdn-images-1.medium.com/max/800/1*qG6LEjOY8HOIpTa7K8-OKQ.png)

- To delete the volume you can use the below command

docker-compose down --volumes

docker volume ls  
get the volume name  
docker volume rm nameofthevolume

### Populate a volume using a container

If you start a container and map it to the folder inside a container which has already some default values then all the data will get copied to the volume.

```
docker run -d \  --name=nginxtest \  -v nginx-vol:/usr/share/nginx/html \  nginx:latest
```

As you can see the container data is automatically get copied to the new volume which got created after running the above command

![](https://cdn-images-1.medium.com/max/800/1*OlnjPtKrQgujXkp6G_G4dg.png)

### **Bind Mounts**

- A file or directory on the host machine is mounted into a container
- Docker will create the file/directory if doesn’t exist already but that can’t be used directly you need to update the content of the file
- Bind mount is good when developer wants to use his local files in the docker container for their testing.
- They want to deploy the artifacts which are present locally
- In the below example, we are copying index.html file from our local machine. So in this way easily you can actually test your application using bind mount.

version: '3'  
services:  
  web:  
    image: tomcat  
    ports:  
      - "8081:8080"  
    volumes:  
      - ./index.html:/usr/local/tomcat/webapps/test/index.html

### **tmpfs**

- tmpfs mount is temporary and only persist in the host memory
- When container stops then this mount also get removed
- This is useful to store sensitive data that you do not want to store either in host or container writable layer.

Congratulations, you have successfully learnt what is volumes in docker and how we can use those in our production environment. Also, we have learnt use of bind mount which is useful for the developers.
