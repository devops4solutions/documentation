---
title: "Cross Account IAM Role on Cloud Custodian"
date: "2018-06-15"
categories: 
  - "devops-tools"
---

# Cross Account IAM Role

This blog will help you to setup a Cross Account IAM Role on Cloud Custodian

## Accounts Used:

1. Security account where all installation and configuration of cloud custodian will reside
2. Other sub accounts

## Requirement

1. I want to run some scripts from security account to sandbox02 account
2. Need to create one role on sandbox account with permissions(for now i have given administrative permissions) to another account (security account)
3. Security account will assume that role.
4. Attach this role to EC2 Instance
5. Need to remove the AWS default credentials which are present in ~/.aws

### On Sandbox account ->

1. Create a role “cloudcustodian-role”, put the security account number

![](https://cdn-images-1.medium.com/max/800/1*RazM-YikFBiaX90iPPok_w.png)

Trust Relationship

![](https://cdn-images-1.medium.com/max/800/1*ZVEHXhgJRboTxq74V-YQMg.png)

Policy — Administrative access.

 

### **How to assume this role on the security account**

Now on cloudcustodian account ->

Create a new policy as below

{ "Version": "2012-10-17", "Statement": \[ { "Sid": "VisualEditor0", "Effect": "Allow", "Action": "sts:AssumeRole", "Resource": "arn:aws:iam::sandboxaccount:role/cloudcustodian-role" } \] }

Create a new role "cloud-custodian-cross-account-role" and attach this policy and trust relationship should be like this

{ "Version": "2012-10-17", "Statement": \[ { "Effect": "Allow", "Principal": { "Service": \[ "ec2.amazonaws.com", "lambda.amazonaws.com" \] }, "Action": "sts:AssumeRole" } \] }

Attach this role to the EC2 instance
