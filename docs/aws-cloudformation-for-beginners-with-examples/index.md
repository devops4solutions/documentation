---
title: "AWS CloudFormation For Beginners with Examples"
date: "2021-02-08"
categories: 
  - "aws"
---

In this blog, we will explore AWS CloudFormation with examples.

Check out my [Youtube](https://youtu.be/43rkqY5wuQA) video on this article.

### Prerequisite

1. Clone [this](https://github.com/devops4solutions/AWSCloudFormation) git repo
2. AWS Account Setup

### **Agenda:**

1. CloudFormation Concepts (Stacks & ChangeSet)
2. Parameters
3. Mappings
4. Conditions
5. Outputs

_AWS CloudFormation is used to describe and provision all of the infrastructure resources in your cloud environment._

1. This is an “**Infrastructure as Code**” which means that you are writing a code to create your infrastructure
2. it is repeatable and also version-able because you can store your code in your git repo.
3. So to create an infrastructure on AWS you actually create a CloudFormation template.

### **AWS CloudFormation Concepts**

1. Stack

To create a resources we actually create a stack

![](https://cdn-images-1.medium.com/max/1600/1*ttERmNrCn-kMDrnQcpRrBw.png)

In the stack, you actually provide a template( Cloudformation template) which contains all the code to provision your resources like AWS EC2, VPC etc.

![](https://cdn-images-1.medium.com/max/1600/1*8P8nLRVGVPAlalJ4qW26Wg.png)

2\. **ChangeSet**

You can see what all changes will be made when you run this template. You can say like a dry run before actually creating a resources

![](https://cdn-images-1.medium.com/max/1600/1*W6jM_DCm0SZgMPEi4AFWcQ.png)

Here you can see that what all changes will be deploying with this template

![](https://cdn-images-1.medium.com/max/1600/1*fVRRG3TxUMY4dNrMKwhl3Q.png)

**NOTE**:

> There are several properties in Amazon Relational Database Service (Amazon RDS) instances that AWS CloudFormation modifies and requires replacement in the underlying database instance resource. If backups are not being taken, data loss will occur. You use a change set to preview the replacement event, make the necessary backups, and take the required precautions before you update the resources.

**Permissions**

You can use an IAM user or a role to create a template. IAM user or role should have the permission to create whatever you are provisioning from your cloudformation template.

If you submit the template as a local file, it uploads to Amazon S3 on your behalf. Because of this, you must add these permissions to create a stack:

- cloudformation:CreateUploadBucket
- s3:PutObject
- s3:ListBucket
- s3:GetObject
- s3:CreateBucket

**AWS Template Structure**

```
{
```

_AWS CloudFormation requires only the_ **_Resources_** _section._

If you do not define Resource section then you will see the below error

**Template format error: At least one Resources member must be defined.**

### **Parameters**

[Parameters](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html) — You can provide the input parameters while creating/updating a stack

Each parameter must be given a logical name (also called logical ID like **InstanceTypeParameter**), which must be alphanumeric and unique among all logical names within the template.

For the sensitive parameter like password, you can add the **NoEcho** property. When this is set, the parameter value displays as asterisks (\*\*\*) for any cloudformation:Describe\* calls. 

AWS CloudFormation supports the following parameter types:

- String
- Number
- List of numbers
- Comma-delimited list
- **AWS parameter types** — Like Amazon EC2 key pair names, IDs of resources, AWS regions/availability zones which are already exist in your account so instead of user provides these value manually , cloudformation template automatically get these properties from your AWS account
- **AWS System Manager parameter types** _AWS Systems Manager parameter types_ can reference parameters that exist in the AWS Systems Manager Parameter Store. If you specify a parameter key, AWS CloudFormation will search your Systems Manager Parameter Store for the correct value and input this into the stack

In the below example we will provide following parameters

- what type of instance you want to create 
- create an ec2 instance for that particular type only.
- Ask the DB password 
- All keys from your AWS account will come in the dropdown
- How to refer the parameters in a template

AWSTemplateFormatVersion: 2010-09-09  
Resources:  
 Ec2Instance:  
  Type: AWS::EC2::Instance  
  Properties:  
    InstanceType:  
      Ref: InstanceTypeParameter1  
    ImageId: ami-01aab85a5e4a5a0fe  
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

### **Mappings**

Mappings used to matches a key to a corresponding set of named values.

Create a mapping that will look for Amazon EC2 instance AMI IDs based on the region and architecture type

The below example -

- Creating a mapping of EC2 AMI’s based on the region and type
- Creating an EC2 instance ( finding the region automatically and based on that it will find the AMI ID from the mapping)
- The **AWS::Region** reference is a pseudoparameter that means it’s a parameter that AWS defines automatically on your behalf.

AWSTemplateFormatVersion: "2010-09-09"  
Mappings:   
  RegionMap:   
    us-east-2:  
      HVM64: ami-01aab85a5e4a5a0fe  
      HVMG2: ami-0a584ac55a7631c0c  
    us-west-1:  
      HVM64: ami-0bdb828fd58c52235  
      HVMG2: ami-066ee5fd4a9ef77f1  
    eu-west-1:  
      HVM64: ami-047bb4163c506cd98  
      HVMG2: ami-0a7c483d527806435  
    ap-northeast-1:  
      HVM64: ami-06cd52961ce9f0d85  
      HVMG2: ami-053cdd503598e4a9d  
    ap-southeast-1:  
      HVM64: ami-08569b978cc4dfa10  
      HVMG2: ami-0be9df32ae9f92309  
Resources:   
  myEC2Instance:   
    Type: "AWS::EC2::Instance"  
    Properties:   
      ImageId: !FindInMap \[RegionMap, !Ref "AWS::Region", HVM64\]  
      InstanceType: t2.micro

### **Conditions**

We define conditions when to create a resource. We use intrinsic functions to evaluate multiple inputs against one another

For Ex-

 Create a larger EC2 instance when environment is production

Create a volume only when the environment is production

AWSTemplateFormatVersion: 2010-09-09  
Parameters:  
  EnvType:  
    Description: Environment type.  
    Default: test  
    Type: String  
    AllowedValues:  
      - prod  
      - test  
    ConstraintDescription: must specify prod or test.  
Conditions:  
  CreateProdResources: !Equals   
    - !Ref EnvType  
    - prod  
Resources:  
  EC2Instance:  
    Type: 'AWS::EC2::Instance'  
    Properties:  
      ImageId: ami-01aab85a5e4a5a0fe  
      InstanceType: t2.micro  
  MountPoint:  
    Type: 'AWS::EC2::VolumeAttachment'  
    Condition: CreateProdResources  
    Properties:  
      InstanceId: !Ref EC2Instance  
      VolumeId: !Ref NewVolume  
      Device: /dev/sdh  
  NewVolume:  
    Type: 'AWS::EC2::Volume'  
    Condition: CreateProdResources  
    Properties:  
      Size: 100  
      AvailabilityZone: !GetAtt   
        - EC2Instance  
        - AvailabilityZone

### **Outputs**

_Outputs_ are used to refer some values outside a single stack.

For Ex -

- cross-stack references
- nested stacks
- describe-stack API calls,
- AWS CloudFormation console.

In this example-

- the output named **StackEC2** returns the ID of a EC2, and then exports the value for cross-stack referencing with the name `EC2` appended to the stack's name. (Cross stack reference)
- the output named **InstanceID** returns the ID of EC2

AWSTemplateFormatVersion: "2010-09-09"  
Mappings:   
  RegionMap:   
    us-east-2:  
      HVM64: ami-01aab85a5e4a5a0fe  
      HVMG2: ami-0a584ac55a7631c0c  
Resources:   
  myEC2Instance:   
    Type: "AWS::EC2::Instance"  
    Properties:   
      ImageId: !FindInMap \[RegionMap, !Ref "AWS::Region", HVM64\]  
      InstanceType: t2.micro  
**Outputs:  
   InstanceID:  
    Description: The Instance ID  
    Value: !Ref myEC2Instance  
   StackEC2:  
    Description: The ID of the EC2 Instance  
    Value: !Ref myEC2Instance  
    Export:  
      Name: !Sub "${AWS::StackName}-EC2"**

![](https://cdn-images-1.medium.com/max/1600/1*qC5wDsuRELpdRD1vpNRYWw.png)
