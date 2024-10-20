---
title: "AWS::CloudFormation::Interface"
date: "2021-02-12"
categories: 
  - "aws"
---

### **AWS::CloudFormation::Interface**

This helps how to modify the ordering and presentation of parameters in the AWS CloudFormation console.

Check out my [Youtube](https://youtu.be/beA2b8sZbh4) video on this article

By default, parameters display in an alphabetical order by their logical IDs

![](https://cdn-images-1.medium.com/max/1600/1*q6kgz5AFphv-nxebi49r4g.png)

The AWS::CloudFormation::Interface metadata key uses two child keys,

1. ParameterGroups (you could group all EC2-related parameters in one group and all DB-related parameters in another group)

- Each entry in ParameterGroups is defined as an object with a **Label** key and **Parameters** key

![](https://cdn-images-1.medium.com/max/1600/1*B6iybuIh-BJU-F-2nTZuug.png)

2\. ParameterLabels (A label is a friendly name or description that the console displays instead of a parameter’s logical ID.)

The ParameterLabels key takes a list of parameter logical IDs, each of which has a friendly description as a subkey.

![](https://cdn-images-1.medium.com/max/1600/1*IS-bSlPwPmM5CWNKkrBOOA.png)

This is how define in your template

- Create two Parameter groups one for database configuration and one for EC2 configuration
- For Key Pair we are using label that will shown in the console
- Clone [this](https://github.com/devops4solutions/AWSCloudFormation/blob/main/interface.yaml) repo and create a stack

Metadata:   
  AWS::CloudFormation::Interface:   
    ParameterGroups:   
      - 
        Label:   
          default: "DB Configuration"  
        Parameters:   
          - DBPort  
          - DBPwd  
      - 
        Label:   
          default: "Amazon EC2 Configuration"  
        Parameters:   
          - InstanceTypeParameter1  
          - myKeyPair  
    ParameterLabels:   
      myKeyPair:   
        default: "Use the Key Pair"  
Parameters:  
  InstanceTypeParameter1:  
    Type: String  
    Default: t2.micro  
    AllowedValues:  
      - t2.micro  
      - m1.small  
      - m1.large  
    Description: Enter t2.micro, m1.small, or m1.large. Default is t2.micro.  
  DBPort:   
    Default: 3306  
    Description: TCP/IP port for the database  
    Type: Number  
    MinValue: 1150  
    MaxValue: 65535  
  DBPwd:   
    NoEcho: true  
    Description: The database admin account password  
    Type: String  
    MinLength: 1  
    MaxLength: 41  
  myKeyPair:   
    Description: Amazon EC2 Key Pair  
    Type: "AWS::EC2::KeyPair::KeyName"  
Resources:  
 Ec2Instance:  
  Type: AWS::EC2::Instance  
  Properties:  
    InstanceType:  
      Ref: InstanceTypeParameter1  
    ImageId: ami-01aab85a5e4a5a0fe

![](https://cdn-images-1.medium.com/max/1600/1*MrUpTdQbVw2gki4rj8G6RA.png)
