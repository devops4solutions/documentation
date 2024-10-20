---
title: "CloudFormation- Introduction"
date: "2018-06-11"
categories: 
  - "devops-tools"
---

CLoudFormation is a declarative way of creating your infrastructure on AWS

For example — A sample cloudformation template

1\. Need EC2 instance

2\. Associate security group to these EC2 instance

3\. Create ELB (Load Balancer) in front of EC2 Instances

So for these above tasks, we can create a template in json/yaml that will automatically create this stack in the right order with the exact configuration mentioned in the template.

Benefits of Cloud Formation Template

1. Infrastructure as code -> All resources are created automatically, no manual intervention required to create an infrastructure architecture on any of the AWS account
2. We can commit the code in git repository for version control system.
3. Cost — You can estimate the cost using cloudformation template
4. You can specify in template that delete the stack at the specified time and recreate it again. This saves you a lot of money. For example — you know your development team is working only 8 -5 then at 6 you can terminate all your infrastructure and recreate it again in the morning.
5. Automated generation of diagram of your template
6. Declarative programming ( we dont need to order and orchestration)
7. We can seperate our stacks like one for network,application, iam stacks.

Example:

Lets create a sample cloudformation stack using the below template. Please click on the below link and save the template on your local machine

[Click here](https://console.aws.amazon.com/cloudformation/designer/home?templateUrl=https://s3.amazonaws.com/cloudformation-examples/sample-ec2-vpc.template&region=us-east-1)

Now we will create a stack using this template

Go to AWS Console ->CLoudFormation ->Create Stack -> Upload a template

![](https://cdn-images-1.medium.com/max/800/1*NFMKyQ56-hOaptdeGf0U_w.png)

Click Next, specify name and all the parameters as per your details.

![](https://cdn-images-1.medium.com/max/800/1*0m7O-QBm0JgZLupjRVPIng.png)

Click Next->Next with all default configuration -> Click Create

This will create a stack and you will all the resources created.
