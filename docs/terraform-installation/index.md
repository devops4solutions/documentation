---
title: "Terraform Installation on AWS EC2 Linux Instance"
date: "2020-09-26"
categories: 
  - "terraform"
tags: 
  - "terraform"
---

In this blog we will do the Terraform Installation on AWS EC2 Linux Instance. We will learn basics of Terraform and its basic commands to launch an Ec2 instance.

Check out [youtube video](https://www.youtube.com/watch?v=MLDFcN3IhB0) for this article

Before starting the installation, lets explore what is terraform.

### Introduction

**Terraform** is a cross-platform tool, which means that it does not only interact with AWS — it can also interact with a multitude of other platforms, such as GCE, VMware, OpenStack, and Azure.

The main benefit of it is that rerunning a declarative definition will never do the same job twice, whereas executing the same shell script will most likely break something on the second run.

- Terraform provides special configuration language to declare your infrastructure in simple text templates.
- Terraform also implements a complex graph logic, which allows you to resolve dependencies, intelligibility and reliability.
- When it comes to servers, Terraform has multiple ways of configuring and wiring them up with existing configuration management tools.
- Terraform keeps track of the current state of the infrastructure it created and applies delta changes when something needs to be updated, added, or deleted. It also provides a way to import existing resources and target only specific resources.
- Terraform is easily extendable with plugins, which should be written in the Go programming language.

### **Install Terraform on Linux**

To get the latest version , check out official  [terraform site](https://www.terraform.io/downloads.html)

sudo yum install unzip (if not installed)

sudo yum install wget (if not installed)

wget [https://releases.hashicorp.com/terraform/0.13.3/terraform\_0.13.3\_linux\_amd64.zip](https://releases.hashicorp.com/terraform/0.13.3/terraform_0.13.3_linux_amd64.zip)

**sudo unzip** [terraform\_0.13.3\_linux\_amd64.zip](https://releases.hashicorp.com/terraform/0.11.6/terraform_0.11.6_linux_amd64.zip) **-d /home/ec2-user/terraform\_install**
vi ~/.bashrc
`export PATH=$PATH:`**/home/ec2-user/terraform\_install**
source ~/.bashrc
**terraform -v**

![](https://cdn-images-1.medium.com/max/800/1*VH7K_Q-9ulggIZeK4yJN-Q.png)

Finally, terraform Installation is completed successfully.

### **Test the installation**

**Write your first terraform template**

**mkdir test && cd test
touch template.tf**

To apply the template, you need to run the terraform apply command. In Terraform, when you run apply, it will read your templates and it will try to create an infrastructure exactly as it’s defined in your templates

**terraform apply
Apply complete! Resources: 0 added, 0 changed, 0 destroyed.**

After each run is finished, you get the number of resources that you’ve added, changed, and destroyed. In this case, it did nothing, as we just have an empty file instead of a real template.

![](https://cdn-images-1.medium.com/max/800/1*uRnl8TxF1ge4wI9UB-LqcQ.png)

We have successfully tested terraform installation and how it works.

Now we will try to edit template.tf with some content as shown below:

vi template.tf

provider "aws" {
      region = "us-west-2"
    }

Now run this template file using below command:

terraform apply

You will see the below error

**Plugin reinitialization required. Please run “terraform init”.  
Reason: Could not satisfy plugin requirements.**  
  
**Plugins are external binaries that Terraform uses to access and manipulate  
resources. The configuration provided requires plugins which can’t be located,  
don’t satisfy the version constraints, or are otherwise incompatible.**  
  
**1 error(s) occurred:**  
  
**\* provider.aws: no suitable version installed  
version requirements: “(any version)”  
versions installed: none**  
  
**Terraform automatically discovers provider requirements from your  
configuration, including providers used in child modules. To see the  
requirements and constraints from each module, run “terraform providers”.**  
  
**Error: error satisfying plugin requirements**

You always have to run the init for the initialization before you can apply the changes

terraform init  
terraform apply

![](https://cdn-images-1.medium.com/max/800/1*XQkRK06r2NmfHQADSjrpuQ.png)

### **Terraform Example**

#### Configure AWS CLI

You need to provide the AWS access credential  to do any kind of automation

You can find the security credentials of any IAM user as shown below

![](https://cdn-images-1.medium.com/max/800/1*x1DD6WzE4e8E4mbSJYNMYw.png)

```
aws configure
```

Once you configured your keys using aws configure, everything is stored under

cd ~/.aws/
pwd
/home/ec2-user/.aws
cat credentials

By default, a default profile is created as show below

```
[default]
region=us-west-2
output=json
aws_access_key_id=
aws_secret_access_key=
```

#### **How to create EC2 instance using terraform**

Terraform file for creating EC2 instance. You need to find a “ami-id” on AWS site according to your instance type.

provider "aws" {
 region = "us-west-2"
 }
# Resource configuration
resource "aws\_instance" "test-instance" {
 ami = "ami-223f945a"
 instance\_type = "t2.micro"
 tags {
 Name = "test"
 }
}

Run the below command

terraform apply
Do you want to perform these actions?
 Terraform will perform the actions described above.
 Only ‘yes’ will be accepted to approve.
Enter a value: yes
For autoapprove, run below command
terraform apply -auto-approve

This is how EC2 instance get created. You can check on AWS console

Congratulations, you have successfully installed Terraform on AWS EC2 instance and test the installation
