---
title: "Docker- Difference between ARG and ENV"
date: "2020-07-18"
categories: 
  - "docker"
tags: 
  - "arg"
  - "docker"
  - "env"
---

In this blog, we will explore what is the Difference between ARG and ENV and the use of both the instructions.

If you want to see the video for this article, [click here](https://youtu.be/Pjhe9nLnVFI)

Checkout this link to understand [what is Docke](https://devops4solutions.com/what-is-docker/)r ?

### **Agenda**:

1. How to pass CMD line arguments to [Dockerfile](https://docs.docker.com/engine/reference/builder/) ?
2. How to pass CMD line arguments to docker-compose.yml file ?

### **What is ARG ?**

- **ARG** instruction defines a variable that can be used to build a Docker image.
- ARG values are not available after the image is built.
- In the running container you can’t access the ARG variables.
- Once ARG variable is defined in the Dockerfile, you can always override the values by passing command line argument as shown below
- If you need to pass two variables using CMD line then you need to use `--build-arg` two times

docker build -t test --build-arg VARIABLE\_1=7 --build-arg VAR\_2=8 .

- We can have multiple **ARG** instruction in the **Dockerfile**. 
- **ARG** is the only instruction that can precede the FROM instruction in the **Dockerfile**.

#### **Example of using ARG argument in the Dockerfile**

FROM alpine:3.7  
ARG VARIABLE\_1=5  
RUN echo $VARIABLE\_1

#### **Run the Dockerfile**

docker build .

Output: We will see the value which is set in the Dockerfile

Sending build context to Docker daemon 2.048 kB
Step 1/3 : FROM alpine:3.7
 ---> 6d1ef012b567
Step 2/3 : ARG VARIABLE\_1=5
 ---> Using cache
 ---> 02b0388547b1
Step 3/3 : RUN echo "print variable value:" $VARIABLE\_1
 ---> Running in 77671e6ff726
**print variable value: 5**
---> 59e00ed8f143
Removing intermediate container 77671e6ff726
Successfully built 59e00ed8f143

#### **Run the Dockerfile by passing arguments**

docker build --build-arg VARIABLE\_1=7 .

**Output**: We can see the value is override with the command line argument

Sending build context to Docker daemon 2.048 kB
Step 1/3 : FROM alpine:3.7
 ---> 6d1ef012b567
Step 2/3 : ARG VARIABLE\_1=5
 ---> Using cache
 ---> 02b0388547b1
Step 3/3 : RUN echo "print variable value:" $VARIABLE\_1
 ---> Running in ecc5f5891b37
**print variable value: 7**
---> 1a44ebca2814
Removing intermediate container ecc5f5891b37
Successfully built 1a44ebca2814

### **What is ENV?**

- ENV is to provide default values for your future environment variables inside the container
- We can’t change the ENV variable using command line argument directly
- If we need to change the ENV variable using CMD line then we have to use ARG and place ARG variable in ENV variable
- In the below Dockerfile, we have created two variables , one as ARG and one as ENV
- Using ARG Value in ENV Variable

FROM alpine:3.7
ARG VARIABLE\_1=5
ENV VARIABLE\_2=$VARIABLE\_1
RUN echo "print variable value:" $VARIABLE\_1
RUN echo " print ENV variable : " $VARIABLE\_2

#### **Build the docker image directly**

- You will both variable having same values as defined for ARG variable

docker build .

Sending build context to Docker daemon 2.048 kB
Step 1/5 : FROM alpine:3.7
---> 6d1ef012b567
Step 2/5 : ARG VARIABLE\_1=5
---> Using cache
---> 02b0388547b1
Step 3/5 : ENV VARIABLE\_2 $VARIABLE\_1
Step 4/5 : RUN echo "print variable value:" $VARIABLE\_1
---> Running in 9d650a1bc85d
**print variable value: 5**
Step 5/5 : RUN echo " print ENV variable : " $VARIABLE\_2
**print ENV variable : 5**
Successfully built 2a3bf7db012e

#### **Build the docker image using CMD line argument**

- `--build-arg VARIABLE_1=7` 
- Output will show 7 for both the variables

docker build -t test --build-arg VARIABLE\_1=7 .

Sending build context to Docker daemon 2.048 kB
Step 1/5 : FROM alpine:3.7
---> 6d1ef012b567
Step 2/5 : ARG VARIABLE\_1=5
---> Using cache
---> 02b0388547b1
Step 3/5 : ENV VARIABLE\_2 $VARIABLE\_1
---> Using cache
---> 663cb87b7a1a
Step 4/5 : RUN echo "print variable value:" $VARIABLE\_1
---> Running in fcb9e45e94ce

**print variable value: 7**
---> 6c74a45a9687
Removing intermediate container fcb9e45e94ce
Step 5/5 : RUN echo " print ENV variable : " $VARIABLE\_2
---> Running in c90ef900ada2

**print ENV variable : 7**
---> 1b35e794624d
Removing intermediate container c90ef900ada2
Successfully built 1b35e794624d

#### **Build image by using ENV variable as CMD line argument**

- Build will fail and you will see the error “**\[Warning\] One or more build-args \[VARIABLE\_2\] were not consumed**

docker build -t test --build-arg VARIABLE\_2=7 .

Sending build context to Docker daemon 2.048 kB
Step 1/5 : FROM alpine:3.7
---> 6d1ef012b567
Step 2/5 : ARG VARIABLE\_1=5
---> Using cache
---> 02b0388547b1
Step 3/5 : ENV VARIABLE\_2 $VARIABLE\_1
---> Using cache
---> 243b8cb9e272
Step 4/5 : RUN echo "print variable value:" $VARIABLE\_1
---> Using cache
---> e797761367a0
Step 5/5 : RUN echo " print ENV variable : " $VARIABLE\_2
---> Using cache
---> 2a3bf7db012e
**\[Warning\] One or more build-args \[VARIABLE\_2\] were not consumed**

#### **Run the container using this image**

- We are using -it for the interactive terminal session
- We will try to echo the variables which we were using in our Dockerfile
- As mentioned already, ARG argument is not available inside the Docker containers and ENV argument is accessible inside the container

$ docker run -it test
/ # echo $VARIABLE\_1
/ # echo $VARIABLE\_2
5

#### **How to pass parameters to docker-compose file**

- In docker-compose file you use the mariadb and don’t want to hardcode the version directly in the file
- Below is the sample code how you use the variable `MARIADB_VERSION`

version: '2'  
services:  
  mariadb:  
    image: 'bitnami/mariadb:**${MARIADB\_VERSION}**'

#### **How to pass the value of the variable in this docker-compose file**

- While running the command manually you can pass the parameter as shown below

MARIADB\_VERSION=1.2 docker-compose up -d

- In automation, you can declare your variable in your property file and use the variable name as shown below:

MARIADB\_VERSION=$(MARIADB\_VERSION) docker-compose up -d
