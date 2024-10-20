---
title: "AWS Secret Key using EC2 Instance Role"
date: "2018-06-13"
categories: 
  - "aws"
---

This blog will help you to setup AWS Secret Key using EC2 Instance Role

## How to maintain aws secret key using EC2 Instance role

An instance profile is a container for an IAM role that you can use to pass role information to an EC2 instance when the instance starts.

On Sandbox2 account

Created one role Build\_Infrastructure\_Terraform\_Role with Administrative access.

On pipeline account

Create a policy like this which has resource arn from sandbox account

{ “Version”: “2012–10–17”, “Statement”: \[ { “Effect”: “Allow”, “Action”: “sts:AssumeRole”, “Resource”: \[ “arole “ \] } \] }

Create a role and attach this policy to that role
