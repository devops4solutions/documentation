---
title: "Cloud Custodian -Configure email"
date: "2018-06-18"
categories: 
  - "devops-tools"
---

 

This blog will help you to configure email in cloud custodian. Using email notification you can notify specific users for the specific tasks.

I am configuring email notification for multiple accounts using only one account (i.e security account in my case where my whole step of cloud custodian is present).

This is the best practice to manage all email communication using a centralized account only.

So in this blog, SQS queue , IAM Role everything is created on the centralized account only.

Prerequisite:

1. SQS queue
2. IAM Role
3. SES - Email Verified

 

For Ex-

1. If RDS instance is  not encrypted then notify users before terminating it.
2. Notify if any security group is open to the world

## Install cloud custodian emailer

Create a directory

```
$ mkdir mailer
```

Clone the repository:

```
$ git clone https://github.com/capitalone/cloud-custodian
```

Install dependencies (with virtualenv):

```
$ virtualenv c7n_mailer
$ source c7n_mailer/bin/activate
$ cd 
```

Install the extensions:

```
python setup.py develop
```

## Create SQS queue

Go to AWS Console -> SQS -> Create QuickQueue  -> Add specif permissions to your queue -> for my testing purpose i gave permission to everything on this queue.

Create IAM role with lambda as a trusted entity which has read access to this queue ( I have given administrative access also you can refine as per your need )

![](https://cdn-images-1.medium.com/max/800/1*2_88F8reOnU1knTCkfBEXg.png)

You should have lambda as a trusted entity

![](https://cdn-images-1.medium.com/max/800/1*Ha05l25T8LeqmPfH4LkPSg.png)

 

 

## Verify Email Address

You have to verify your email address also, go to AWS Console ->SES -> Identity Management -> Email Address -> Verify.

You have to verify all email addresses (all recipients list) and this is region specific.

Once you verify your email address , send a test email.

Go to AWS Console -> SES -> Email Address -> Send Test Email

Create a mailer.yml and put the below content

queue\_url: [https://sqs.us-east-1.amazonaws.com/1234567890/c7n-mailer-test](https://sqs.us-east-1.amazonaws.com/1234567890/c7n-mailer-test)
role: arn:aws:iam::123456790:role/c7n-mailer-test
from\_address: you@example.com

Now let’s make a Custodian policy to populate your mailer queue. Create a `test-policy.yml`: Change the emailid and queue name

policies:
  - name: c7n-mailer-test
    resource: sqs
    filters:
     - "tag:MailerTest": absent
    actions:
      - type: notify
        template: default
        priority\_header: '2'
        subject: testing the c7n mailer
        to:
          - you@example.com
        transport:
          type: sqs
          queue: https://sqs.us-east-1.amazonaws.com/1234567890/c7n-mailer-test

## Run the command

```
source c7n_mailer/bin/activate
```

## Output:

2018-06-18 15:13:15,738: custodian.policy:INFO policy: c7n-mailer-test resource:sqs region:us-west-2 count:1 time:0.00
2018-06-18 15:13:16,232: custodian.actions:INFO sent message:a7ff12b2-3625-4c56-9df5-79930cf5c7c1 policy:c7n-mailer-test template:default count:1
2018-06-18 15:13:16,232: custodian.policy:INFO policy: c7n-mailer-test action: notify resources: 1 execution\_time: 0.49

Below command is to send any email which are in queue
c7n-mailer --run -c mailer.yml

## Output:

(c7n\_mailer) \[ec2-user@ip-172-31-24-18 mailer\]$ c7n-mailer --run -c mailer.yml
2018-06-18 15:13:57,748 - custodian-mailer - INFO - Downloading messages from the SQS queue.
2018-06-18 15:13:58,159 - custodian-mailer - INFO - Sending account:cydaptivlabs-secops policy:c7n-mailer-test sqs:1 email:default to \[[u'khandelwal12nidhi@gmail.com](mailto:u%27khandelwal12nidhi@gmail.com)'\]
2018-06-18 15:13:58,291 - custodian-mailer - INFO - Sending account:cydaptivlabs-secops policy:c7n-mailer-test sqs:1 email:default to \[[u'khandelwal12nidhi@gmail.com](mailto:u%27khandelwal12nidhi@gmail.com)'\]
2018-06-18 15:13:58,510 - custodian-mailer - INFO - Sending account:cydaptivlabs-secops policy:c7n-mailer-test sqs:1 email:default to \[[u'khandelwal12nidhi@gmail.com](mailto:u%27khandelwal12nidhi@gmail.com)'\]
2018-06-18 15:13:58,768 - custodian-mailer - INFO - Sending account:cydaptivlabs-secops policy:c7n-mailer-test sqs:1 email:default to \[[u'khandelwal12nidhi@gmail.com](mailto:u%27khandelwal12nidhi@gmail.com)'\]
2018-06-18 15:14:08,783 - custodian-mailer - INFO - No sqs\_messages left on the queue, exiting c7n\_mailer.

If there are no messages in the queue, the following output will get displayed

(c7n\_mailer) \[ec2-user@ip-172-31-24-18 mailer\]$ c7n-mailer --run -c mailer.yml
2018-06-18 15:16:02,243 - custodian-mailer - INFO - Downloading messages from the SQS queue.
2018-06-18 15:16:12,333 - custodian-mailer - INFO - No sqs\_messages left on the queue, exiting c7n\_mailer.

## Example of security group

policies:
- name: high-risk-security-groups-remediate
resource: security-group
description: |
Remove any rule from a security group that allows 0.0.0.0/0 ingress
and notify the user who added the violating rule.
filters:
- type: ingress
Cidr:
value: "0.0.0.0/0"
actions:
- type: notify
template: default.html
priority\_header: 1
subject: "Open Security Group Rule Created-\[custodian {{ account }} - {{ region }}\]"
violation\_desc: "Security Group(s) Which Had Rules Open To The World:"
action\_desc: |
"Actions Taken: The Violating Security Group Rule Needs to be Removed As It
Violates Our Company's Cloud Policy. Please Refer To The Cloud FAQ."
to:
- khandelwal12nidhi@gmail.com
- curt@cydaptiv.com
transport:
type: sqs
queue: queueurl
region: us-west-2

Run this and you will get the email if any of the security ingress group is open to the world

Conclusion:

This is how we can Configure email in cloud custodian and that will help to identify issues very quickly.

References

1. [https://github.com/capitalone/cloud-custodian/tree/master/tools/c7n\_mailer](https://github.com/capitalone/cloud-custodian/tree/master/tools/c7n_mailer)
