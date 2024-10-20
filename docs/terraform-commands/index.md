---
title: "Terraform Examples and Commands"
date: "2018-06-09"
categories: 
  - "ansible"
  - "aws"
  - "terraform"
---

This blog will help you with the basic terraform commands and will give you an idea of how to user variables,map, attributes.

Some useful terraform commands

**Terraform plan** — this command shows you what applying do by checking the template, state file, and actual state of the resource. It is recommended to use this before running apply command to ensure accidental deletion of any resources

**terraform validate** — to check the syntax of the file

**terraform fmt**\- to do the formatting of the file

**terraform taint** -> marks a single resource for recreation. The resource will be destroyed and then created again.

**terraform taint aws\_vpc.my\_vpc** 
    **The resource aws\_vpc.my\_vpc in the module root has been marked as tainted!**

**Destroy the template**

terraform destroy

**Resource Dependencies and Modules**

Creating a VPC (Virtual Private Cloud)

Create a new file vpc.tf

provider “aws” {
 region = “us-west-2”
}
resource “aws\_vpc” “my\_vpc” {
 cidr\_block = “10.0.0.0/16”
}

We need subnet to put instance in a network. This subnet belongs to a previously created VPC. This means that we have to pass a VPC ID when we create it. We don’t have to hardcode it though. Terraform, via interpolation syntax, allows us to reference any other resource it manages using the following syntax: ${RESOURCE\_TYPE.RESOURCE\_NAME.ATTRIBUTE\_NAME}.

**Subnet creation with VPC**

provider “aws” {
 region = “us-west-2”
}
resource “aws\_vpc” “my\_vpc” {
 cidr\_block = “10.0.0.0/16”
}

resource "aws\_subnet" "public" { 
    vpc\_id = "${aws\_vpc.my\_vpc.id}" 
    cidr\_block = "10.0.1.0/24" 
}

**Dependency Graph**

A dependency graph allows us, for example, to properly order the creation or destruction of nodes or to order a set of commands. It’s all about ordering, actually.

There are just three types of nodes in a Terraform graph:

- Resource node
- Provider configuration node
- Resource meta-node

What the resource node and provider configuration node are responsible for is clear: the provider node configures a provider (AWS, in our examples) and the resource node manages an entity of this provider (EC2, VPC, and so on, in the case of AWS). A resource meta-node doesn’t really do anything special; it is used for convenience and makes a graph more pretty. It is applicable only if you specify a **count** parameter greater than one.

terraform graph

\[ec2-user@ip-172–31–22–171 packt-terraform\]$ terraform graph digraph { compound = “true” newrank = “true” subgraph “root” { “\[root\] aws\_subnet.public” \[label = “aws\_subnet.public”, shape = “box”\] “\[root\] aws\_vpc.my\_vpc” \[label = “aws\_vpc.my\_vpc”, shape = “box”\] “\[root\] provider.aws” \[label = “provider.aws”, shape = “diamond”\] “\[root\] aws\_subnet.public” -> “\[root\] aws\_vpc.my\_vpc” “\[root\] aws\_vpc.my\_vpc” -> “\[root\] provider.aws” “\[root\] meta.count-boundary (count boundary fixup)” -> “\[root\] aws\_subnet.public” “\[root\] provider.aws (close)” -> “\[root\] aws\_subnet.public” “\[root\] root” -> “\[root\] meta.count-boundary (count boundary fixup)” “\[root\] root” -> “\[root\] provider.aws (close)” } }

you could install graphiz, [click here](http://www.graphviz.org/download/)

terraform graph | dot -Tpng > graph.png

![](https://cdn-images-1.medium.com/max/800/1*5Q5K78glIDgP8kkPE-q4gw.png)

### Controlling dependencies with depends\_on and ignore\_changes

For each resource, you can specify the depends\_on parameter, which accepts a list of resources that this resource depends on. As a result, this resource won’t be created until the ones listed inside this parameter are created.

resource "aws\_instance" "master-instance" {
      ami = "ami-9bf712f4"
      instance\_type = "t2.micro"
      subnet\_id = "${aws\_subnet.public.id}"
    }
    resource "aws\_instance" "slave-instance" {
      ami = "ami-9bf712f4"
      instance\_type = "t2.micro"
      subnet\_id = "${aws\_subnet.public.id}"
      depends\_on = \["aws\_instance.master-instance"\]
    }

With depends\_on, all resources would be created sequentially. Without it, both EC2 instances will be created in parallel

Now, let’s say we want to include a private hostname of master in the list of tags of the slave, but we don’t want to update it if master was recreated. To achieve this, we will use the ignore\_changes parameter. This parameter is part of lifecycle block, responsible for a few other create/destroy-related parameters. The ignore\_changes parameter accepts the list of parameters to ignore when updating, in our case -tags:

resource "aws\_instance" "slave-instance" { 
  ami = "ami-9bf712f4" 
  instance\_type = "t2.micro" 
  subnet\_id = "${aws\_subnet.public.id}" 
  tags { 
    master\_hostname = "${aws\_instance.master-instance.private\_dns}" 
  } 
  lifecycle { 
   ignore\_changes = \["tags"\] 
  } 
}

_The most common use case for_ ignore\_changes _is, perhaps,_ user\_data _for cloud instances. For most providers, if you change_ user\_data _(the script to be executed on instance creation by the_ cloud-init _utility), Terraform will try to recreate the instance. It is often unwanted behavior because, most likely, you use the same_user\_data _string for multiple instances and you want changes to be applied only for new instances, while keeping the others running (or by recreating them one by one yourself)._

- **The create\_before\_destroy** Boolean parameter allows us to tell Terraform to first create a new resource and then destroy the previous one in the case of recreation.
- **The prevent\_destroy parameter**, also Boolean, marks a resource as indestructible and can save you some nerves. One example of a resource that can benefit from this option is an Elastic IP — a dedicated IP address inside AWS that you can attach to an EC2 instance.

**Create EC2 instance with security group**

provider "aws" { 
  region = "us-west-2" 
} 
resource "aws\_vpc" "my\_vpc" { 
  cidr\_block = "10.0.0.0/16" 
 tags {
    Name = "my-vpc"
  }
}
resource "aws\_subnet" "public" { 
    vpc\_id = "${aws\_vpc.my\_vpc.id}" 
    cidr\_block = "10.0.1.0/24" 
 tags {
    Name = "my-subnet"
  }
}

resource "aws\_security\_group" "allow\_http" { 
  name = "allow\_http" 
  description = "Allow HTTP traffic" 
  vpc\_id = "${aws\_vpc.my\_vpc.id}" 
 
  ingress { 
    from\_port = 80 
    to\_port = 80 
    protocol = "tcp" 
    cidr\_blocks = \["0.0.0.0/0"\] 
  } 
 
  egress { 
    from\_port = 0 
    to\_port = 0 
    protocol = "-1" 
    cidr\_blocks = \["0.0.0.0/0"\] 
  } 
} 
 
resource "aws\_instance" "mighty-trousers" { 
  ami = "ami-223f945a" 
  instance\_type = "t2.micro" 
  subnet\_id = "${aws\_subnet.public.id}" 
  vpc\_security\_group\_ids = \["${aws\_security\_group.allow\_http.id}"\] 
}

**Configuration in Terraform ( How to use variables)**

A template with only hardcoded data in it is a bad template. You can’t reuse it in other projects without modifying it. You will always have to update it by hand if some value changes. And you have to store a lot of information that doesn’t really belong to the infrastructure template. how to make Terraform templates more configurable.

With the default value in place, Terraform won’t ask for the value interactively anymore. It will pick default value unless other sources of variables are present.

Variables.tf

Define a variable

variable "region" {  
  description = "AWS region. Changing it will lead to loss of complete stack." 
  default = "eu-central-1" 
}

template.tf

provider "aws" { 
  region = "${var.region}" 
}

There are three types of variables you can set:

- the string variables (default ones)
- the map variables
- the list variables

**Map Variables**

Map is a lookuptable, where you specify multiple keys with different values. You can then pick the value depending on the key.

For the development and testing purpose, its ok to use t2.micro instance but when it comes to production we need high level instances so what we want, actually, is a way to use different instance types depending on the environment stack is deployed to. Let’s assume that we have only three environments: dev, prod, and test.

Lets define two variable environment and instance\_type

variable "environment" { default = "dev" } 
variable "instance\_type" { 
   type = "map" 
   default = { 
     dev = "t2.micro" 
     test = "t2.medium" 
     prod = "t2.large" 
   } 
}

**Configure DataSource**

Create one VPC manually and then use it as a VPC data source. Add it to your template

data "aws\_vpc" "management\_layer" { 
  id = "vpc-c36cbdab"  
}

Create VPC peering connection between manually created VPC and terraform created VPC

data "aws\_vpc" "management\_layer" { 
  id = "vpc-c36cbdab" 
} 
 
resource "aws\_vpc" "my\_vpc" { 
  cidr\_block = "${var.vpc\_cidr}" 
} 
 
resource "aws\_vpc\_peering\_connection" "my\_vpc-management" { 
  peer\_vpc\_id = "${data.aws\_vpc.management\_layer.id}" 
  vpc\_id = "${aws\_vpc.my\_vpc.id}" 
  auto\_accept = true 
}

 

![](https://cdn-images-1.medium.com/max/800/1*qqahU1XKYnOoaNXcFhcxPg.png)

terraform init

![](https://cdn-images-1.medium.com/max/800/1*hne5Y1Gzs4TzLCWDBZwlUg.png)

terraform plan ( to check what will get installed before running it)

![](https://cdn-images-1.medium.com/max/800/1*_MYXi-bb53QOLPeHQ7sGXg.png)

terraform apply

+![](https://cdn-images-1.medium.com/max/800/1*mDEyCTET77aKZ88am9Y18w.png)

References

[https://github.com/express42/terraform-ansible-example](https://github.com/express42/terraform-ansible-example)
