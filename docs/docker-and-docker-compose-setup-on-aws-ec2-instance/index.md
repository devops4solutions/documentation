---
title: "Docker and Docker-Compose Setup on AWS EC2 Instance"
date: "2018-06-13"
categories: 
  - "docker"
---

This blog will help you to setup a docker and docker-compose on AWS EC2 Instance

Install Docker on AWS

sudo yum update -y

sudo yum install -y docker

sudo service docker start

sudo usermod -a -G docker ec2-user

Docker version 17.09.1-ce, build

Docker installed successfully.

Install Docker-Compose. Get the latest one from here [https://github.com/docker/compose/releases](https://github.com/docker/compose/releases)

```
curl -L https://github.com/docker/compose/releases/download/1.20.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

**Test Docker installation**

Run hello-world image

docker run hello-world

![](https://cdn-images-1.medium.com/max/800/1*PwPd_42BnIs_boIE7ERzYg.png)

Build a Image

Create a Dockerfile,requirements.txt,app.py

```
docker build -t friendlyhello .
```

```
docker image ls
docker run -p 4000:80 friendlyhello
```

![](https://cdn-images-1.medium.com/max/800/1*Ev-mRj9Jv5QQpoA5EkSQTQ.png)

RUn the app in detached mode

```
docker run -d -p 4000:80 friendlyhello
docker ps
docker images
docker stop containerid

```

Push image in Docker Hub

![](https://cdn-images-1.medium.com/max/800/1*hXkSclbWuJftlMkRuwxE2w.png)

Now you can run your image from anywhere

```
docker run -p 4000:80 username/repository:tag
```

Till now, we have created an image using Dockerfile,push it to DOckerhub so that anyone can use it now.

Some useful commands

Create DockerImage with commit option

1. Run a container from the ubuntu and connect it to its command line:

docker run -i -t ubuntu /bin/bash

2\. Install the Git toolkit:

**root@dee2cb192c6c:/# apt-get update**
**root@dee2cb192c6c:/# apt-get install -y git**

1. Check if the Git toolkit is installed:

**root@dee2cb192c6c:/# which git**
 **/usr/bin/git**

1. Exit the container:

**root@dee2cb192c6c:/# exit**

1. Check what has changed in the container comparing it to the ubuntu image:

**$ docker diff dee2cb192c6c**

The command should print a list of all files changed in the container.

1. Commit the container to the image:

**$ docker commit dee2cb192c6c ubuntu\_with\_git**

Using the exact same method, we can build ubuntu\_with\_git\_and\_jdk on top of the ubuntu\_with\_git image:

**$ docker run -i -t ubuntu\_with\_git /bin/bash**
**root@6ee6401ed8b8:/# apt-get install -y openjdk-8-jdk**
**root@6ee6401ed8b8:/# exit**
**$ docker commit 6ee6401ed8b8 ubuntu\_with\_git\_and\_jdk**

Create Image directly using Dockerfile

Create a Docker file with below contents

FROM ubuntu:16.04
MAINTAINER Rafal Leszko
RUN apt-get update && \\
apt-get install -y python
COPY hello.py .
ENTRYPOINT \[“python”, “hello.py”\]

Creata a hello.py

print “Hello World from Python!”

**docker build -t hello\_world\_python .
$ docker run hello\_world\_python**

**Docker Volumes**

Let’s start with an example and specify the volume with the -v <host\_path>:<container\_path> option and connect to the container:

**$ docker run -i -t -v ~/docker\_ubuntu:/host\_directory ubuntu:16.04 /bin/bash**

Now, we can create an empty file in host\_directory in the container:

**root@01bf73826624:/# touch host\_directory/file.txt**

Let’s check if the file was created in the Docker host’s filesystem:

**root@01bf73826624:/# exit**
**exit**

**$ ls ~/docker\_ubuntu/**
**file.txt**

We can see that the filesystem was shared and the data was therefore persisted permanently. We can now stop the container and run a new one to see that our file will still be there:

**$ docker stop 01bf73826624**

**$ docker run -i -t -v ~/docker\_ubuntu:/host\_directory ubuntu:16.04 /bin/bash**
**root@a9e0df194f1f:/# ls host\_directory/**
**file.txt**

**root@a9e0df194f1f:/# exit**

Instead of specifying the volume with the -v flag, it’s possible to specify the volume as an instruction in the Dockerfile, for example:

VOLUME /host\_directory

Some useful commands

docker ps ( to show all running containers)

docker ps -a ( to show all containers(stopped and running)

docker images

docker exec -it 4a53d243816e bash ( To go inside a container)

Docker setup has completed succesfully with some basic knowledge.

**Create a Docker-compose.yml/scale up application**

is a YAML file that defines how Docker containers should behave in production.

This `docker-compose.yml` file tells Docker to do the following:

- Pull [the image we uploaded in step 2](https://docs.docker.com/get-started/part2/) from the registry.
- Run 5 instances of that image as a service called `web`, limiting each one to use, at most, 10% of the CPU (across all cores), and 50MB of RAM.
- Immediately restart containers if one fails.
- Map port 80 on the host to `web`’s port 80.
- Instruct `web`’s containers to share port 80 via a load-balanced network called `webnet`. (Internally, the containers themselves publish to `web`’s port 80 at an ephemeral port.)
- Define the `webnet` network with the default settings (which is a load-balanced overlay network).

```
docker swarm init
```

```
docker stack deploy -c docker-compose.yml getstartedlab
```

![](https://cdn-images-1.medium.com/max/800/1*YN4Gi7WHS0mjdefbune_6Q.png)

Our single service stack is running 5 container instances of our deployed image on one host.

```
docker service ps getstartedlab_web
```

```
docker container ls -q
```

![](https://cdn-images-1.medium.com/max/800/1*_w08bruZiwXabaZWj8s7mQ.png)

You can run `curl -4 http://localhost` several times in a row and you will get different hostnames

![](https://cdn-images-1.medium.com/max/800/1*x177XU6bN_pY3b0jNX8fnQ.png)

You can update docker-compose.yml file and re-run the stack command .Docker performs an in-place update, no need to tear the stack down first or kill any containers.

`docker stack rm getstartedlab`

`docker swarm leave --force`

We have learnt how it should run in production by turning it into a service, scaling it up 5x in the process.

**CLuster in Docker**

Now we will deploy this application onto a cluster, running it on multiple machines. Multi-container, multi-machine applications are made possible by joining multiple machines into a “Dockerized” cluster called a **swarm**.

Swarm — group of machines that are running Docker and joined into a cluster.

Swarm managers can use several strategies to run containers, such as “emptiest node” — which fills the least utilized machines with containers. Or “global”, which ensures that each machine gets exactly one instance of the specified container. You instruct the swarm manager to use these strategies in the Compose file, just like the one you have already been using.

Swarm managers are the only machines in a swarm that can execute your commands, or authorize other machines to join the swarm as **workers**. Workers are just there to provide capacity and do not have the authority to tell any other machine what it can and cannot do.

`docker swarm init` to enable swarm mode and make your current machine a swarm manager then run `docker swarm join` on other machines to have them join the swarm as workers

**Install Docker-machine on AWS EC2**

```
curl -L https://github.com/docker/machine/releases/download/v0.14.0/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine && \
sudo install /tmp/docker-machine /usr/local/bin/docker-machine
```
