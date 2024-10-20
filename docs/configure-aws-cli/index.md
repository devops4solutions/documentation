---
title: "Configure AWS CLI"
date: "2018-06-11"
categories: 
  - "aws"
---

**Configure AWS CLI**

You need to provide the AWS access credential  to do any kind of automation

You can find the security credentials of any IAM user as shown below

![](https://cdn-images-1.medium.com/max/800/1*x1DD6WzE4e8E4mbSJYNMYw.png)

You need python-pip on that machine to install awscli

yum install [https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm](https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm)

sudo yum install python-pip

```
sudo pip install awscli --upgrade pip
```

Run the below command after logging to some user, for eg -ec2-user not as a root user

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

Its best practice to create a profile and use that for any kind of automation as shown below. You can directly edit the file or run the below command to configure it

aws configure --profile profilename

```
[profile user2]
region=us-east-1
output=text
aws_access_key_id=
aws_secret_access_key=
```

You can delete your default configuration as whenever you run anything without providing profile name it will automatically take the default profile name

## **Configure AWS CLI for cross account**

Suppose you have multiple accounts , one is where you put all  your scripting code and other accounts where you want to create resources.

Steps:

1. Create IAM role on sub accounts where resources needs to be created with Admin access and you to put the account number of your scripting account
2.  Create IAM role on  your master account and assume the role.
3. AWS CLI doesn't work directly by just assume the role on your EC2 instance.

\[default\] region = us-west-2

#credential of builduser on pipeline account aws\_access\_key\_id=accesskey aws\_secret\_access\_key=secretkey

\[profile Kinesis\] role\_arn = arn:aws:iam::accountnumber6:role/CrossAccountRole\_Kinesis region = us-west-2 source\_profile = default

 

[Click Her](https://www.terraform.io/docs/providers/aws/)e for other options to configure credentials for terraform.

NOTE: If your user has MFA enabled then that user is not able to use aws cli directly and you will get authentication error
