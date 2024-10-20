---
title: "Reusable infrastructure with Terraform Modules"
date: "2020-11-17"
categories: 
  - "terraform"
tags: 
  - "terraform"
---

As development, staging and production environment are isolated from each other, so does the terraform code required to. In order to built a re-usable terraform code for both staging and production environment, without conducting copy and paste, one must follow the modules strategy. In this blog we will learn how to create Reusable infrastructure with Terraform Modules

Check out my [YouTube video](https://www.youtube.com/watch?v=MLDFcN3IhB0) for this article

### Terraform modules:

Think of them just like functions in a programming language, which we define once and then call them by passing parameters from anywhere in the code. Just like functions, put the infrastructure code inside a Terraform module and then reuse it in multiple places throughout the code.

Thus, both our staging and production environment (which are almost identical) can use the same modules without the requirement to copy and paste.

Let’s build a very basic and simple AWS network, comprising of a VPC and subnet 

**Prerequisite**:

1. Install [Terraform](https://devops4solutions.com/terraform-installation/) and configure AWS secret keys
2. Clone this [git repo](https://github.com/devops4solutions/terraform-example-aws)

### Build AWS network via Terraform modules:

Below is our directory structure for the code in git repo

![](https://cdn-images-1.medium.com/max/800/1*YIWXphvmzbhHtBZ2nD8Qug.png)

 **Create VPC**

resource "aws\_vpc" "terraform\_vpc" {   
  cidr\_block = var.vpc-fullcidr   
 #### this 2 true values are for use the internal vpc dns resolution  
enable\_dns\_support = true  
    enable\_dns\_hostnames = true  
 tags = {  
    Name = "terraform\_vpc"  
 }  
}

**Declare output variables for vpc\_id**

output "vpc\_id" {  
  value = "${aws\_vpc.terraform\_vpc.id}"  
}

 **Create subnet**

 Here we are first declaring the variable `vpc_id` and then using the value

variable vpc\_id {}

resource "aws\_subnet" "PublicAZA" {  
  vpc\_id = var.vpc\_id  
  cidr\_block = var.Subnet-Public-AzA-CIDR  
  tags = {  
        Name = "PublicSubnetAZA"  
  }  
   
}

**How to call the modules from the main.tf file**

provider "aws" {  
  region = var.region  
}

module "vpc" {

**source = "./modules/vpc"**

}

module "subnet" {  
 ** source = "./modules/subnets"  
  vpc\_id = "${module.vpc.vpc\_id}"**  
}

\# this is how you refer the bucket and dynamobdb which you have created in the previous step

terraform {

backend "s3" {  
 bucket = "terraform-remote-state-devops4solutions-bucket-s3"  
 dynamodb\_table = "terraform-state-lock-dynamo-devops4solutions"  
 region = "us-east-2"  
 key = "terraform.tfstate"   
 }  
}

Now run the below command

git clone [https://github.com/devops4solutions/terraform-example-aws.git](https://github.com/devops4solutions/terraform-example-aws.git)

cd vpc-subnet-example  
terraform init  
terraform plan  
terraform apply

Check your AWS console, to see the successful creation of VPC, subnet.

Congratulation, you have successfully learnt how to create a reusable infrastructure using terraform modules.
