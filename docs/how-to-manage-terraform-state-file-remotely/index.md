---
title: "How to manage terraform state file remotely"
date: "2020-11-17"
categories: 
  - "terraform"
tags: 
  - "terraform"
---

Check my [YouTube](https://www.youtube.com/watch?v=MLDFcN3IhB0) video for this article

In this blog, we will learn How to manage terraform state file remotely on S3 bucket. By default, our terraform state file is stored locally. Now we will explore to store it in S3 bucket

If you are working on a team, then its best to store the terraform state file remotely so that many people can access it

If the state file is stored remotely so that many people can access it, then you risk multiple people attempting to make changes to the same file at the exact same time. So we need to provide a mechanism that will “lock” the state if its currently in-use by another user. We can accomplish this by creating a dynamoDB table for terraform to use.

So you need 3 things 

1. s3 bucket to store the state file in

2\. Terraform s3 backend resource.

3\. DynamoDB

4\. [Git repo](https://github.com/devops4solutions/terraform-example-aws/tree/master/create_s3)

### **Example to create S3 bucket and Dynamodb table**

Clone this [git repo](https://github.com/devops4solutions/terraform-example-aws/tree/master/create_s3)

provider "aws" {  
region = var.region  
}

\# terraform state file setup  
\# create an S3 bucket for vpc to store the state file in

resource "aws\_s3\_bucket" "terraform-state-storage-s3-devops4solutions" {  
   bucket = var.bucket\_devops4solutions  
   
    versioning {  
      enabled = true  
    }  
   
  #  lifecycle {  
   #   prevent\_destroy = true  
  #  }  
   
    tags = {  
      Name = "S3 Remote Terraform State Store for devops4solutions"  
    }        
}

\# create a dynamodb table for locking the state file  
resource "aws\_dynamodb\_table" "dynamodb-terraform-state-lock-devops4solutions" {  
  name = "terraform-state-lock-dynamo-devops4solutions"  
  hash\_key = "LockID"  
  read\_capacity = 20  
  write\_capacity = 20  
   
  attribute {  
    name = "LockID"  
    type = "S"  
  }  
   
  tags = {  
    Name = "DynamoDB Terraform State Lock Table for devops4solutions"  
  }  
}

Now in your `main.tf` include the below code:

terraform {

backend "s3" {  
 bucket = "terraform-remote-state-devops4solutions-bucket-s3"  
 dynamodb\_table = "terraform-state-lock-dynamo-devops4solutions"  
 region = "us-east-2"  
 key = "terraform.tfstate"   
 }  
}

Your `backend` configuration cannot contain interpolated variables, because this configuration is initialized prior to Terraform parsing these variables. So we are hardcoding the bucket and dynmodb details

### **How to Download terraform state file from S3**

sh ‘AWS\_ACCESS\_KEY\_ID=”yourkey” AWS\_SECRET\_ACCESS\_KEY=”yourkey” aws s3 sync s3://yourbucketname

### **How to access remote state files in other modules**

We might have a requirement where we want to have separate terraform state file for network components which will build our vpc, subnets etc and other for EC2 instances. So how we will access the vpc id in EC2 instance ?

Only the root level outputs from the remote state are accessible. Outputs from modules within the state cannot be accessed. If you want a module output to be accessible via a remote state, you must thread the output through to a root output.

In your vpc.tf, you will create a output as we have discussed above , now we have to thread that output through to a root output, so we will create a root output.tf file at the root of the project hierarchy like this

output "vpc\_id\_root" {  
  value = "${module.vpc.vpc\_id}"  
}

Now in ec2-instance.tf

data "terraform\_remote\_state" "network\_state" {  
  backend = "s3"

config {  
    bucket = "bucketname"  
    key    = "terraform.tfstate"  
    region = "us-west-2"  
access\_key = "youracesskey"  
 secret\_key = "secretkey"  
  }  
  }

### **How to access it**

subnet\_id = "${data.terraform\_remote\_state.network\_state.public\_subnet\_id\_root}"

if everything is at root you don’t need to create outputs at multiple place, but module configuration is recommended .

Congratulation, we have understood How to manage terraform state file remotely on S3 bucket and how to pass module outputs to another module.
