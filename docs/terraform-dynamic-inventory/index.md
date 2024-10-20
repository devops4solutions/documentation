---
title: "Terraform-Dynamic Inventory"
date: "2018-06-13"
categories: 
  - "ansible"
  - "aws"
  - "terraform"
tags: 
  - "ansible"
  - "terraform"
---

# How to get the Terraform-Dynamic Inventory for your ansible script

This blog will help you to find out the Terraform-Dynamic Inventory like EC2 instance which is created automatically using Terraform. Now to do the deployment on this EC2 instance through ansible you need the details.

Here, we will find out how we can put the host entry in the ansible hosts file automatically.

## Requirements

1. Install Terraform Inventory

### Installation

Download the software from here as per your operating system.  [_Click Her_](https://github.com/adammck/terraform-inventory)e

wget [https://github.com/adammck/terraform-inventory/releases/download/v0.7-pre/terraform-inventory\_v0.7-pre\_linux\_amd64.zip](https://github.com/adammck/terraform-inventory/releases/download/v0.7-pre/terraform-inventory_v0.7-pre_linux_amd64.zip)

unzip the folder and then cd to terraform-inventory .

Now we have to run the ansible playbook from the location here terraform state file is present.

In main.yml we have defined which group of EC2 instances we need to run

ansible-playbook --inventory-file=/home/ec2-user/opt/terraform-inventory/terraform-inventory /home/ec2-user/opt/main.yml

This command will read the ip address of Ec2 instance from the terraform state file and execute the playbook on that particular instance.
