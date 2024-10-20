---
title: "Cross Account Access on AWS"
date: "2018-06-13"
categories: 
  - "aws"
---

I have two accounts ( sandbox and pipeline)

On Sandbox account, Go IAM -> Roles -> Create ROle-> Select Another AWS acccount as shown below

Put Pipeline account number

![](https://cdn-images-1.medium.com/max/800/1*DRsymL4HuWk-Ds9yptS3-Q.png)

Click Permissions -> Select any policy /custom policy -> Put Role name ->Create Role

Below policy will auto created

{ “Version”: “2012–10–17”, “Statement”: \[ { “Effect”: “Allow”, “Principal”: { “AWS”: “arn:aws:iam::accountnumber of pipeline :root” }, “Action”: “sts:AssumeRole”, “Condition”: {} } \] }

Now on pipeline account

Create a policy like this

{ “Version”: “2012–10–17”, “Statement”: \[ { “Sid”: “VisualEditor0”, “Effect”: “Allow”, “Action”: “sts:AssumeRole”, “Resource”: “arn:aws:iam::account number of sandbox:role/Build\_Infrastructure\_Terraform\_Role” } \] }

Create a role and assign that policy to this role

![](https://cdn-images-1.medium.com/max/800/1*GscFIqpG48cz9wHpFjbYKQ.png)

Attach this role to the EC2 instance

Now run the below command. NOTE if AWS CLI is preconfigured then first delete those and run it.

aws sts assume-role — role-arn “arn:aws:iam::accountnumber of sandbox:role/Build\_Infrastructure\_Terraform\_Role” — role-session-name “EC2FromB”
