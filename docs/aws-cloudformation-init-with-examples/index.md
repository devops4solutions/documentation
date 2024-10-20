---
title: "AWS CloudFormation Init with Examples"
date: "2021-02-12"
categories: 
  - "aws"
---

In this blog, we will explore AWS CloudFormation Init and will create an example of using all the cfn-init concepts

You can check my [Youtube](https://youtu.be/mkdR2fgg_as) video on this article.

### **Agenda**

1. Basics of CloudFormation::Init
2. cfn-init
3. cfn-signal
4. cfn-hup

### **AWS::CloudFormation::Init**

Use the AWS::CloudFormation::Init type to include metadata on an Amazon EC2 instance 

![](https://cdn-images-1.medium.com/max/1600/1*PeZ91uEGleK2K1uL1EtqRg.png)

![](https://cdn-images-1.medium.com/max/1600/1*lAt3AMo-oTtkk_ic0Zcz5A.png)

#### **AWS::CLOUDFORMATION::INIT: RESOURCE METADATA**

```
"Resources": {
```

**Packages**\- Used to install the software on your instance like tomcat using (yum, apt, python, and others)

**Groups**\- To created groups on the linux system

**Users**\- The users key allows you to create Linux/UNIX users on your instance.

**Sources-**use the sources key to download files from remote locations

**Files**\- The files key creates files based on either inline content in the template or content from a remote location (URL).

**Commands-** Execute any custom script or a file

**Services-** The services key defines which services are enabled or disabled on the instance being configured. Linux systems utilize sysvinit to support the services key

```
"services" : {
```

**ConfigSets**

By default, cfn-init helper script processes these configuration sections in the following order: 

- packages, 
- groups,
-  users, 
- sources,
-  files, 
- commands,
-  and then services.

If you require a different order, then you can use the concept of **ConfigSet**.

![](https://cdn-images-1.medium.com/max/1600/1*2a9E25JsOktAejfIf8qtjQ.png)

1. Defined two config Keys and then use a configset that specifies the order in which these config keys should be processed.

#### Install

![](https://cdn-images-1.medium.com/max/1600/1*1ifRfA3Bwts0MkqzZdoyGw.png)

#### Configure

![](https://cdn-images-1.medium.com/max/1600/1*8HaseihrVHOWQRBMaNPACA.png)

#### What is cfn-init

How we can use that metadata which we have created using **AWS::CloudFormation::Init**

So for that we use `cfn-init` helper script it reads template metadata from the AWS::CloudFormation::Init key and acts accordingly to:

- Fetch and parse metadata from AWS CloudFormation
- Install packages
- Write files to disk
- Enable/disable and start/stop services

#### How to call cfn-init template

You can specify a configset when you call cfn-init in your template. If you don’t specify a [configset](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-init.html), cfn-init looks for a single config key named _config_.

From the UserData section you can call the `cfn-init`

> You must pass UserData to instances in Base64 format. Thus, you call the Fn::Base64 function to convert the text-based script to a Base64 encoding.

![](https://cdn-images-1.medium.com/max/1600/1*fg910tCX9DO5aCjJv7l9kA.png)

### **cfn-signal**

The cfn-signal helper script signals AWS CloudFormation to indicate whether Amazon EC2 instances have been successfully created or updated.

If you install and configure software applications on instances, you can signal AWS CloudFormation when those software applications are ready.

You use the cfn-signal script in conjunction with a [CreationPolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-attribute-creationpolicy.html) or an Auto Scaling group with a [WaitOnResourceSignals](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-attribute-updatepolicy.html) update policy.

![](https://cdn-images-1.medium.com/max/1600/1*DZH3W9YEdWnwOj5Pcrmzxg.png)

### **cfn-hup**

The cfn-hup helper is a daemon that detects changes in resource metadata and runs user-specified actions when a change is detected. This allows you to make configuration updates on your running Amazon EC2 instances through the UpdateStack API action.

### **Let’s create a sample LAMP Stack**

- Create a LAMP stack using a single EC2 instance and a local MySQL database for storage. 
- This template demonstrates using the AWS CloudFormation bootstrap scripts to install the packages and files necessary to deploy the Apache web server, PHP and MySQL at instance launch time. 
- Clone [this](https://github.com/devops4solutions/AWSCloudFormation/blob/main/cf-init-LAMP.yaml) yaml file and create a stack

Once the stack is created successfully you should be able to see all the events and access the url directly

![](https://cdn-images-1.medium.com/max/1600/1*a7Ytpen-Msn9p09fo3-pmw.png)

![](https://cdn-images-1.medium.com/max/1600/1*n7JwKTX628sUNcLKVb_1VQ.png)

**References**

https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/deploying.applications.html
