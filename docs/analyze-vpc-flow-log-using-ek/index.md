---
title: "Analyze VPC Flow log using EK"
date: "2018-06-08"
categories: 
  - "devops-tools"
---

This blog will help you to Analyze VPC Flow logusing Elastic Search

![](https://cdn-images-1.medium.com/max/800/1*zsRamahrG00qYn6_Go1i0Q.png)

## Tools/Task:

1. Create Flow Log in VPC
2. IAM Role for VPC Flow Log
3. Cloudwatch Log Group
4. ElasticSearch
5. IAM Lambda role

## Create Flow Log on VPC

1. Open VPC
2. Click on Create Flow Log

![](https://cdn-images-1.medium.com/max/800/1*muC16yJKRdZkbA8W0xmshg.png)

3\. For Role — Click on Set up Permissions, it will create a role and then select it

4\. For Destination Log Group — this is the cloud watch log group name

The name of the CloudWatch Logs log group to which the flow log will be published. A log stream will be created in this log group for each network interface being monitored.

![](https://cdn-images-1.medium.com/max/800/1*7jaTWkaYrsVpuOdsd9sa9Q.png)

Flow Log is created successfully.

You can check the flow logs being generated

![](https://cdn-images-1.medium.com/max/800/1*u7t8FrtmNCzluZStwwgBKg.png)

ElasticSearch Service

1. Create domain

![](https://cdn-images-1.medium.com/max/800/1*AHCyu_Z3v0yiafLui--zuA.png)

![](https://cdn-images-1.medium.com/max/800/1*XuYx8v-o8R3xkEfUu0rWXg.png)

All default setting then

![](https://cdn-images-1.medium.com/max/800/1*jWZIFaBx56y6HDIFKuPXxw.png)

![](https://cdn-images-1.medium.com/max/800/1*Q1wcWd0kM2I_xo3dPT5QMQ.png)

Click Confirm

It will take around 10 minutes to create a elasticsearch service

Now go to cloudwatch

![](https://cdn-images-1.medium.com/max/800/1*j7vX4xmVLUIAxMTq94mN5w.png)

Click on Actions -> Stream to Amazon ElasticSearchService.

Select this Account and your ES cluster

![](https://cdn-images-1.medium.com/max/800/1*YUT0L9NLqeGsBZ6kZt9U4w.png)

CloudWatch Logs uses Lambda to deliver log data to Amazon ES. You must specify an IAM role that grants Lambda permission to make calls to Amazon ES. You can choose an existing role or create an IAM role that automatically has the required permissions. To deliver log data to another account, you must specify the Elasticsearch Domain ARN and Elasticsearch Endpoint of other account and ensure permissions are granted to be able to publish to that ARN.

Select — Click on Create new role

![](https://cdn-images-1.medium.com/max/800/1*pH-3Y1VtQkF2L3YmiFzrOw.png)

{ “Version”: “2012–10–17”, “Statement”: \[ { “Effect”: “Allow”, “Action”: \[ “logs:CreateLogGroup”, “logs:CreateLogStream”, “logs:PutLogEvents” \], “Resource”: \[ “arn:aws:logs:\*:\*:\*” \] }, { “Effect”: “Allow”, “Action”: “es:ESHttpPost”, “Resource”: “arn:aws:es:\*:\*:\*” } \] }

![](https://cdn-images-1.medium.com/max/800/1*oixyMeovZ8VfRyz79Lj1Hg.png)

![](https://cdn-images-1.medium.com/max/800/1*o7l_x2crLODV7KAwTLGWAg.png)

![](https://cdn-images-1.medium.com/max/800/1*eH13p5kClhqptdaYv8eZXg.png)

![](https://cdn-images-1.medium.com/max/800/1*cOcpMjB1Rr-FtY8a47V5Gg.png)

![](https://cdn-images-1.medium.com/max/800/1*HBetv-h-QMdgpm8eRJ4snw.png)

You will see all the fields

Click on Discover, you will see the graph based on timestamp

![](https://cdn-images-1.medium.com/max/800/1*cvdulZU9t8xuwo-sjxCAoA.png)

Create a visualization as per your requirement

![](https://cdn-images-1.medium.com/max/800/1*4BVjqAJo1-ocjdLzgcjoug.png)

Create a Dashboard, add the visualization which you have created above to show it on your dashboard.

To do it using script, please follow the below link, it will work as it is.

[**AWS: aws\_flow\_log - Terraform by HashiCorp** _Provides a VPC/Subnet/ENI Flow Log_www.terraform.io](https://www.terraform.io/docs/providers/aws/r/flow_log.html "https://www.terraform.io/docs/providers/aws/r/flow_log.html")

References

[CLick Here](https://www.youtube.com/watch?v=DMpfC3juTGQ)
