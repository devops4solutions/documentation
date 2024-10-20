---
title: "Secure Database password using EC2 parameter store"
date: "2018-06-13"
categories: 
  - "devops-tools"
---

### This blog will help you how to manage database password using E2 parameter store

Steps:

 

1. IAM Policy
2. IAM Role
3. Encryption Keys
4. Parameter Store
5. EC2 Instance — Assume IAM role
6. Install PHP SDK on AWS
7. Configured php code to retrieve db credentials

**1\. IAM policy**

Open the AWS Console and navigate to **IAM -> Policies** and click **Create policy**. Next, select **Create Your Own Policy**, and use the following **Policy Document**:

**APPLICATION\_SECRET\_API\_KEY - this is the key which we will create later**

{
 “Version”: “2012–10–17",
 “Statement”: \[
 {
 “Effect”: “Allow”,
 “Action”: \[
 “ssm:DescribeParameters”
 \],
 “Resource”: “\*”
 },
 {
 “Effect”: “Allow”,
 “Action”: \[
 “ssm:GetParameters”
 \],
 “Resource”: \[
 “arn:aws:ssm:us-west-2:accountnumber:parameter/**APPLICATION\_SECRET\_API\_KEY**”
 \]
 }
 \]
}

**2\. IAM Role**

Go to **IAM -> Roles** and click **Create new role**, then choose **Amazon EC2** for the **AWS Service Role**.

![](https://cdn-images-1.medium.com/max/800/0*56IWilPI4KuJfmGF.png)

Attach the policy which you have created above

![](https://cdn-images-1.medium.com/max/800/1*7NhblXUeWvou9iRuW2XyeQ.png)

Role is created with the permissions.

**3\. Create Keys**

Go to IAM ->Encryption Keys -> Select Region where you want to create key

![](https://cdn-images-1.medium.com/max/800/1*KOoFNTphFFGLKejJY60_rQ.png)

Click on Create Key

Next, enter an **Alias** for your key and an optional **Description**. For **Key Material Origin**, leave the default **KMS** selected.

![](https://cdn-images-1.medium.com/max/800/1*OuQ2k23ZZ8i-il6TYHEfgA.png)

You can add Tags if you need.

**Key administrators**

The key administrator is an IAM User or Role who is allowed to make changes to the key itself or grant others access to use and administer it.

Next, we’ll **Define Key Usage Permissions** so that we can actually use our key. Here, I’ve selected the IAM Role as an authorized key user, which is the role I’ll be assigning to the EC2 instances my application is going to run on.

Key is created successfully.

### Encrypting our secret

Go to **EC2 -> Parameter Store**

![](https://cdn-images-1.medium.com/max/800/1*6avsv5qWJVJAwKof4h5IOg.png)

Retrieve Plaintext value

![](https://cdn-images-1.medium.com/max/800/1*lgFS6Mb-ZV1OcK2RI73F_g.png)

Key setup is done successfully.

**How to use it with Application Code**

You need to install php sdk. [Click here for installation details](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/getting-started_installation.html)

Download as a zip

[http://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.zip](http://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.zip)

and unzip the folder and place it in your application folder

Below is the sample code for php

require ‘/var/www/html/openemr/test/aws-autoloader.php’; use Aws\\Ssm\\SsmClient; $client = new SsmClient(\[ ‘version’ => ‘latest’, ‘region’ => ‘us-west-2’, \]); $result = $client->getParameters(\[ ‘Names’ => \[‘APPLICATION\_SECRET\_API\_KEY’\], ‘WithDecryption’ => true \]);

$host = ‘hostname’; $port = ‘3306’; $login = ‘openemr’; $pass = $result\[‘Parameters’\]\[0\]\[‘Value’\]; $dbase = ‘emrdb’;

**EC2 Instance**

Instance has to assume the role which you have created above.
