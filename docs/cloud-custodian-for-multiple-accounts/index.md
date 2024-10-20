---
title: "Cloud Custodian for multiple accounts"
date: "2018-06-15"
categories: 
  - "devops-tools"
---

## Manage multiple accounts in Cloud Custodian

This blog will help you to manage multiple accounts and regions from one centralized location . In our case we have a security account which we are using to setup all configuration

## Prerequisite

1. One centralized account( Security Account)
2. Other Sub account
3.  Create an IAM user( build\_user) on security account and configure it on your EC2 instance.

## Installation

**c7n-org** is a tool to run custodian against multiple AWS accounts, Azure subscriptions, or GCP projects in parallel.

pip install c7n-org

It will get installed at the location /home/ec2-user/custodian/bin

In order to run c7n-org against multiple accounts, a config file must first be created containing information about all the accounts with regions and you need to setup an cross account  IAM role before proceeding further. [Click here](https://devops4solutions.com/cross-account-iam-role-cc/) to setup.

Create a new file called accounts.yaml, you can add multiple accounts with multiple regions and you will put the role which you have created on this particular account

## Accounts.yaml

accounts:
- account\_id: '123123123123'
  name: account-1
  regions:
  - us-east-1
  - us-west-2
  role: arn:aws:iam::123123123123:role/CloudCustodian

## Sample policy

This policy will fidn out all EC2 instances which are in running state across accounts and regions

policies:
- name: my-first-policy
resource: ec2
filters:
- "State.Name": running

## Run the below command

source custodian/bin/activate
c7n-org run -c policy/accounts.yml -s output -u policy/custodian.yml --dryrun
c7n-org run -c policy/accounts.yml -s output -u policy/custodian.yml

After running above command, you will get the following output structure of your policy

![](https://cdn-images-1.medium.com/max/800/1*pg8YZMX04b3JQV-CReGc6A.png)

 

References

[https://github.com/capitalone/cloud-custodian/tree/master/tools/c7n\_org](https://github.com/capitalone/cloud-custodian/tree/master/tools/c7n_org)
