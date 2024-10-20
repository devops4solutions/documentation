---
title: "Cloud Custodian IAM Role"
date: "2018-06-15"
categories: 
  - "devops-tools"
---

# IAM Role

This blog will help you to setup an IAM role for cloud custodian.

##  **Create IAM Role**

Go to AWS Console -> IAM ->Create Role -> Select Lambda Access -> AWSLambdaFullAccess -> Create Policy (cloud-custodian-role) and make sure to have lambda services as Trust Relationship

![](https://cdn-images-1.medium.com/max/800/1*sOBqlaor7X2CtJqA1xcwyw.png)

![](https://cdn-images-1.medium.com/max/800/1*Ehj_5zXDSgnNSjtRzoyRyA.png)
