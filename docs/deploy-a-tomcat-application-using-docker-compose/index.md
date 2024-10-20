---
title: "Deploy a tomcat application using docker-compose"
date: "2020-09-14"
categories: 
  - "docker"
tags: 
  - "docker-compose"
  - "tomcat"
coverImage: "docker-compose-1.png"
---

In this blog, we will learn what is docker-compose and how we can deploy a tomcat application which uses mysql database. We will learn how we can setup a development environment using docker-compose in a single command

If you want to see the video for this article, [click here](https://www.youtube.com/watch?v=siPdgVzLZNY&t=456s)

### **Prerequisite**:

1. [Docker](https://devops4solutions.com/docker-setup-on-aws-ec2-instance/) and [Docker-compose](https://docs.docker.com/compose/install/) installed

### **INTRODUCTION**

- docker-compose is a tool which is used to deploy multi-container application. 
- One single yaml file to deploy your application on the server.
- Best suited for the developers to setup their workstation in a single command without installing any kind of dependencies for the application
- `docker-compose up` to start your application 
- `docker-compose down` to clean up all the docker containers

#### **Let’s take an example here:**

We have a project called user registration which uses mysql for storing the data . In terms of microservices, we can say that we have two services as shown below:

- Web Service
- Database Service

You can clone [this](https://github.com/devops4solutions/CI-CD-using-Docker/) git repo and try the below example

#### **Explanation of docker-compose**

1. **version :** This is the version as per the [docker engine](https://docs.docker.com/compose/compose-file/) you have installed on your machine 
2. **services:** This is the main tag which is used to configure multiple services and under that we have details of all the services

 3. **web**: This is our service name -> using image, ports and volumes

4\. **volumes:** To store the database files

Now we will create a `[docker-compose](https://github.com/devops4solutions/CI-CD-using-Docker/blob/master/docker-compose.yml)` file which will together launch a tomcat, mysql and phpmyadmin container

**Tomcat container** — To run your application

**Database container** — To store the data

**PhpMyAdmin** — Access the database through GUI

So we will have three services

1. **db** — we are using local path to store the data so that when you run `docker-compose down` all your data will retain. If you use the volume then all data will get lost if you run the `docker-compose down`

 Also, we are importing sql file so that our database is ready with the sample data. It is good so that each developer will always have the base or the actual data to run their application on the local machine

2\. **phpmyadmin** — which is used to access the database through GUI and it depends on service `db` 

3\. **web** — which is used to access your web application and it also depends on service `db`

version: '3.3'  
services:  
   db:  
     image: mysql:5.7  
     volumes:  
       - /opt/test:/var/lib/mysql  
       - ./mysql-dump:/docker-entrypoint-initdb.d  
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
web:  
    depends\_on:  
      - db  
    image: tomcat  
    volumes:  
      - ./target/LoginWebApp-1.war:/usr/local/tomcat/webapps/LoginWebApp-1.war  
    ports:  
      - '8082:8080'  
    environment:  
      MYSQL\_ROOT\_PASSWORD: root  
      MYSQL\_DATABASE: testdb1  
      MYSQL\_USER: testuser  
      MYSQL\_PASSWORD: root

Run the command

docker-compose up -d

![](https://cdn-images-1.medium.com/max/800/1*c8pPnv7AAULrGo3_JJO3Ag.png)

All the environment variables which we are passing like mysql database details for the tomcat container can be used in the code.

![](https://cdn-images-1.medium.com/max/800/1*OHKiTRTAJpsorIzPwB4Teg.png)

You can now browse the url

http://yourip:8082/LoginWebApp-1/

![](https://cdn-images-1.medium.com/max/800/1*vk7w25CSjl8LD45chhIBbQ.png)

Click on register and fill the below details and then check the database it has inserted all the values in USER table. This shows that tomcat application is able to make a database connection.

![](https://cdn-images-1.medium.com/max/800/1*sIoCMxKlAqAniZaowdfDUw.png)

![](https://cdn-images-1.medium.com/max/800/1*SJQOOPxIpt1Eq_NxZbm_fw.png)

### Create a tomcat container using docker-compose

version: '3'  
services:  
  web:  
    image: tomcat  
    ports:  
      - "8081:8080"  
    volumes:  
      - ./index.html:/usr/local/tomcat/webapps/test/index.html

Now you can run this compose file in detached mode and can see the output “**Server startup**” at the end:

docker-compose -f docker-compose\_tomcat.yml up -d

![](https://cdn-images-1.medium.com/max/800/1*-VBFwiaLb-H9hh6YQ1nOZg.png)

Now if you try to access it using [http://yourip:8081,](http://yourip:8081,) you will see the below error. You are getting this error because webapps folder is empty . There is no sample example folder is present.

![](https://cdn-images-1.medium.com/max/800/1*RB4suNG8YZPpPomHsiaURg.png)

But as we have put the index.html we can try to access that

http://yourip:8081/test

You should be able to see the content of your index.html.

This shows that we have successfully launched tomcat inside a docker container using docker-compose.

**Clean the docker container**

docker-compose -f docker-compose\_tomcat.yml down

### **Install Mysql**

1. Now we will use the mysql image and launch a container
2. This will create a volume on your docker machine and all data will be removed if you delete the container .
3. You need to declare db\_data1 in volume section at the bottom. 

version: '3.3'

services:  
   db:  
     image: mysql:5.7  
     volumes:  
       - db\_data1:/var/lib/mysql  
     environment:  
       MYSQL\_ROOT\_PASSWORD: root  
       MYSQL\_DATABASE: testdb1  
       MYSQL\_USER: testuser  
       MYSQL\_PASSWORD: root  
     ports:  
       - 3306:3306  
volumes:  
    db\_data1:  
           name: db\_data1

Now run this

docker-compose -f docker-compose\_mysql.yml up -d

![](https://cdn-images-1.medium.com/max/800/1*CgULqdRpWBbWjax2Qnzi7A.png)

As you can see all mysql db details are created inside a container if you use the volume approach

![](https://cdn-images-1.medium.com/max/800/1*TsDdV35wBXxJVmA3E7l96Q.png)

![](https://cdn-images-1.medium.com/max/800/1*Sw8gcc_qtHQsJ4wnEhVxBA.png)

#### **How to connect using command prompt**

mysql -utestuser -proot1 -h 127.0.0.1

Tear down container and volumes

```
# To Tear Down$ 
docker-compose down --volumes
```

### **Install Mysql with phpadmin so that we can access the database through GUI**

- We are using the local path for the database so that data will retain even if container get deleted
- We are creating a new user `testuser` for our database. If you want to use same root user then you do not need to provide the user parameter ,only password parameter is required to set the password

version: '3.3'

services:  
   db:  
     image: mysql:5.7  
     volumes:  
       - /opt/test:/var/lib/mysql  
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

Run the docker-compose

docker-compose -f docker-compose\_mysql\_phpMyAdmin.yml up -d

![](https://cdn-images-1.medium.com/max/800/1*Vwqi02C8jh9MOT5ssqE6ow.png)

Logged on phpmyadmin and create some table to test if it persist or not

http://yourip:8081/

![](https://cdn-images-1.medium.com/max/800/1*xDMcOvMUQGBBGyjk5NBD9w.png)

![](https://cdn-images-1.medium.com/max/800/1*2Cubd7agbDhYqCSWfz2BIg.png)

Now clean your container 

docker-compose -f docker-compose\_mysql\_phpMyAdmin.yml down

Rerun the container 

docker-compose -f docker-compose\_mysql\_phpMyAdmin.yml up -d

You should be able to see the table which you have created 

### **How to import .sql file in docker-compose file**

This is required if you need to import the existing sql file so that every developer who runs the container can have all the database tables and some sample data created 

mysql-dump must be a directory. All the .sql’s in the directory will be imported.

version: '3.3'

services:  
   db:  
     image: mysql:5.7  
     volumes:  
       - /opt/test:/var/lib/mysql  
       - **./mysql-dump:/docker-entrypoint-initdb.d**  
     environment:  
       MYSQL\_ROOT\_PASSWORD: root  
       MYSQL\_DATABASE: testdb1  
       #MYSQL\_USER: testuser  
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

Congratulations, you have successfully installed the docker-compose and ran the tomcat application with mysql database using docker-compose.
