---
title: "Provisioning EC2 key pairs with terraform"
date: "2018-09-26"
categories: 
  - "terraform"
tags: 
  - "ssh-key"
---

In this blog, we will be Provisioning EC2 key pairs with terraform.

There are two ways to Provisioning EC2 key pairs with terraform

1. Use Existing Key
2. Create New Key

**Use Existing Key**

You can create one key using AWS console and use the existing one which is already present as shown below

![](https://cdn-images-1.medium.com/max/800/1*8VuY8mWjLmHUbr9Z78uRuQ.png)

Now in your terraform code, you have to just use this key name in your configuration like this

resource “aws\_instance” “bastion” {

count = “1”

connection {
 user = “ubuntu”
 // private\_key = “${base64decode(var.ssh\_private\_key)}”
 }

instance\_type = “${var.instance\_type}”

ami = “${var.aws\_amis}”

key\_name = “${local.ssh\_key\_name}”

**Create New Key**

You can create a new key, you have to pass the public key as shown, you can use the path or hardcoded the public key .

resource "aws\_key\_pair" "deploy" {
  key\_name   = "Terraform-test"
  public\_key = "ssh-rsa "

}
