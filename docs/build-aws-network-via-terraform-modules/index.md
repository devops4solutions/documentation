---
title: "Build AWS network via Terraform modules"
date: "2018-09-27"
categories: 
  - "terraform"
tags: 
  - "automation"
  - "aws"
  - "modules"
  - "terraform"
---

As development, staging and production environment are isolated from each other, so does the terraform code required to. In order to built a re-usable terraform code for both staging and production environment, without conducting copy and paste, one must follow the modules strategy. In this blog we will learn to Build AWS network via Terraform modules.

### **Terraform modules:**

Think of them just like functions in a programming language, which we define once and then call them by passing parameters from anywhere in the code. Just like functions, put the infrastructure code inside a Terraform module and then reuse it in multiple places throughout the code.

Thus, both our staging and production environment (which are almost identical) can use the same modules without the requirement to copy and paste.

Let's build a very basic and simple AWS network, comprising of a VPC, subnet and an EC2 instance using terraform modules.

### Build AWS network via Terraform modules:

Step 1. Login to AWS console and putty into one of your instance (here we are using ubuntu instance).

Step 2. Install [Terraform](https://devops4solutions.com/terraform-installation/)

Step 3. [Install and configure AWS CLI](https://devops4solutions.com/configure-aws-cli/).

Step 4. Create the following directory structure:

![](https://cdn-images-1.medium.com/max/1000/1*YIWXphvmzbhHtBZ2nD8Qug.png)

Step 5. Let's start with writing code:

open the network.tf in an editor and write the following code:

![](https://cdn-images-1.medium.com/max/1000/1*ojw8u1uDz7H5B9lStJaXMw.png)

//creating AWS network including VPC and subnet

resource "aws\_vpc" "terraform\_vpc" { 
cidr\_block = var.vpc-fullcidr 
#### this 2 true values are for use the internal vpc dns resolution
enable\_dns\_support = true
enable\_dns\_hostnames = true
tags = {
Name = "terraform\_vpc"
}
}

resource “aws\_subnet” “subnet1” {

                      vpc\_id =”${var.vpc\_id}” cidr\_block = “${var.subnet\_cidr}”

                     tags { Name = “main” }

output "vpc\_id"{

                  value = "${aws\_vpc.main.id}"

}

output "subnet\_id" {

value = "${aws\_subnet.subnet1.id}"

}

}

Step 6. Define variables:

In Step5 we have written the code to create vpc and subnet. However, rather hard coding, we have make use of variables. So lets define those variables inside modules --> vpc --> var.tf

//defining variables required by vpc module

variable "vpc\_cidr" { default = "10.0.0.0/16" }

variable "tenancy" { default = "dedicated" }

variable "vpc\_id" { }

variable "subnet\_cidr" { default = "10.0.1.0/24" }

Step 7. Create ec2 module:

Edit instances.tf :

![](https://cdn-images-1.medium.com/max/1000/1*88a-gzTUnpgTY79qcQV9fg.png)

resource "aws\_instance" "web-ec2" {

ami = "${var.ami\_id}" instance\_type = "${var.instance\_type}"

//put this ec2 instance inside a subnet which is inside a vpc

subnet\_id = "${var.subnet\_id}" }

Step 8. Define variables for instances:

Edit var.tf inside modules --> ec2 --> var.tf

variable "ami\_id" {}

variable "instance\_type" { default = "t2.micro" }

variable "subnet\_id" {}

variable "ec2\_count" { default = "1" }

Step 9. Write code for dev environment

Edit main.tf

provider "aws" {

region = "${var.region}"

}

//Pass the parameters to the vpc module

module "my\_vpc" {

source = "../modules/vpc" vpc\_cidr = "192.168.0.0/16" tenancy = "default" **vpc\_id = "${module.my\_vpc.vpc\_id}"** subnet\_cidr = "192.168.1.0/24"

}

module "my\_ec2" {

source = "../modules/ec2" ec2\_count = 1 ami\_id = "ami-759bc50a" instance\_type ="t2.micro"

}

\*Note:

**vpc\_id = "${module.my\_vpc.vpc\_id}"** 

Value of vpc\_id has to be fetched from the module. To do so, define this value as output of the module, so that module can return the value of the output variable. And the above is the syntax to access it in the user/dev environment.

Step 10. edit var.tf of dev folder

variable "region" { default ="us-east-1" }

Step 11. Following commands are used to create the infrastructure via terraform:

terraform-example/dev$   terraform init

terraform-example/dev$   terraform apply

Check your AWS console, to see the successful creation of VPC, subnet and ec2 instance.
