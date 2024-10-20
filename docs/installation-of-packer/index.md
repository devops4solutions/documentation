---
title: "Installation of Packer"
date: "2018-08-30"
categories: 
  - "devops-tools"
tags: 
  - "packer"
---

In this blog, we will do the Installation of Packer

Please follow the below steps

1. Download the packer from [here](https://www.packer.io/downloads.html) 

### INSTALLING PACKER ON MICROSOFT WINDOWS

1. Create a directory like C:\\packer and copy the executable file here.
2. Add the packer path in your system path
3. Open the powershell window and run the below command

![](https://cdn-images-1.medium.com/max/800/1*q7GrJhJv03kuL5XQLqHcRA.png)

Packer is successfully installed on your system.

**Packer Components**

Packer calls the process of creating an image a _build_.

_Artifacts_ are the output from builds. One of the more useful aspects of Packer is that you can run multiple builds to produce multiple artifacts.

A _build_ is fed from a _template_. The template is a JSON document that describes the image we want to build — both where we want to build it and what the build needs to contain.

To determine what sort of image to build, Packer uses components called _builders_. A builder produces an image of a specific format — for example, an AMI builder or a Docker image builder. Packer ships with a number of builders and you can also add your own builder in the form of plugins.

**Running Packer**

![](https://cdn-images-1.medium.com/max/800/1*jG2McB8ZWkr3jI-2vKSnzg.png)

Create a Json file to create an image

1. Create a file ami.json

```
{
  "variables": {
    "aws_access_key": "",
    "aws_secret_key": ""
  },
  "builders": [{
    "type": "amazon-ebs",
    "access_key": "{{user `aws_access_key`}}",
    "secret_key": "{{user `aws_secret_key`}}",
    "region": "us-east-1",
    "source_ami": "ami-a025aeb6",
    "instance_type": "t2.micro",
    "ssh_username": "ubuntu",
    "ami_name": "packer-example {{timestamp | clean_ami_name}}"
  }]
}
```

2\. Run the below command to validate the packer

```
$ packer validate ami.json
Template validated successfully.
```

3\. Build the image

```
$  packer build ami.json

```

![](https://cdn-images-1.medium.com/max/800/1*-FiwOYVPyaCx9yP3FaQyBQ.png)

![](https://cdn-images-1.medium.com/max/800/1*Ys4aNgHxkPuyEHGCPgZyqQ.png)

You’ve now created your first Packer image successfully.
