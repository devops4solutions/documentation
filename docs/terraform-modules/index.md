---
title: "Terraform Modules"
date: "2018-06-13"
categories: 
  - "terraform"
---

**Modules**

This blog will help you to work with Terraform modules which helps in grouping multiple resources and how to use the module outputs across other modules

**Modules** in Terraform are used to group multiple resource

create a directory structure as shown below. We are creating separate folder structure for all the modules.

Package structure as shown below

![](https://cdn-images-1.medium.com/max/800/1*Pl2d7r9IfZqmRMNJYMxDdQ.png)

Inside a modules folder, for now we will use only vpc and ec2-instance

Now in main.tf, this show how to access the modules and also the variable like vpc\_id for ec2-instance

provider "aws" {
alias="sandbox2"
region = "${var.region}"
access\_key = "${var.aws\_access\_key\_sandbox2}"
secret\_key = "${var.aws\_secret\_key\_sandbox2}"

}

module "vpc" {
providers = {
    "aws" = "aws.sandbox2"
  }
source = "./modules/vpc"
}

1111111112

module "ec2\_instance" {
 providers = {
    "aws" = "aws.sandbox2"
  }
  source = "./modules/ec2-instance"
  vpc\_id = "${module.vpc.vpc\_id}"

}
module "ec2\_instance" {
 providers = {
 "aws" = "aws.sandbox2" }
source = "./modules/ec2-instance"
vpc\_id = "${module.vpc.vpc\_id}"
}
}

Now in VPC folder, you need to create a vpc\_id as output so that it can be used with other modules like this
resource “aws\_vpc” “terraform\_vpc” { 
 cidr\_block = “${var.vpc-fullcidr}” 
 #### this 2 true values are for use the internal vpc dns resolution
enable\_dns\_support = true
 enable\_dns\_hostnames = true
 tags {
 Name = “terraform\_vpc”
 }
}
output “vpc\_id” { 
 value = “${aws\_vpc.terraform\_vpc.id}” 
}

Now how to use that vpc\_id, you need to create a variable and then use it as shown below

variable vpc\_id {}
resource "aws\_key\_pair" "auth" {
  key\_name   = "${var.key\_name}"
  public\_key = "${file(var.public\_key\_path)}"
}
resource "aws\_instance" "webserver" {
  ami           = "${lookup(var.ami, var.region)}"
 connection {
    # The default username for our AMI
    user = "ec2-user"
host = "${aws\_instance.webserver.public\_ip}"

\# The connection will use the local SSH agent for authentication.
  }

instance\_type = "t2.micro"
  associate\_public\_ip\_address = "true"
  subnet\_id = "${var.public\_subnet\_id}"
  vpc\_security\_group\_ids = \["${var.FrontEnd\_SG\_id}"\]
 key\_name = "${aws\_key\_pair.auth.id}" 
 tags {
        Name = "webserver"
  }
  
}

### Retrieving module data with outputs — one more example

In an output, you define which data you want to be returned by the module

You have 2 folders (VPC,Network), now VPC is created inside VPC folder so the vpc\_id is not directly accessible on network folder. You will add below line on the vpc.tf file

output “vpc\_id” {
 value = “${aws\_vpc.demo\_vpc.id}”
}

module

Now on main.tf

module "network" {
  source = "./modules/network"
  vpc\_id = "${module.vpc.vpc\_id}"
}

on route-network.tf where you want to use vpc\_id. You have to declare variable , name of the variable should be same as we have declared in output and then access it using var.

variable “vpc\_id” {}
resource “aws\_internet\_gateway” “gw” {
 vpc\_id = “${var.vpc\_id}”
 tags {
 Name = “internet gw terraform generated”
 }
}

Now for modules first you need to run

terraform get

this command will first load all the modules

![](https://cdn-images-1.medium.com/max/800/1*qqahU1XKYnOoaNXcFhcxPg.png)

terraform init

![](https://cdn-images-1.medium.com/max/800/1*hne5Y1Gzs4TzLCWDBZwlUg.png)

terraform plan ( to check what will get installed before running it)

![](https://cdn-images-1.medium.com/max/800/1*_MYXi-bb53QOLPeHQ7sGXg.png)

terraform apply

![](https://cdn-images-1.medium.com/max/800/1*mDEyCTET77aKZ88am9Y18w.png)
