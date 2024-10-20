---
title: "Demo of creating an AWS Lambda Function"
date: "2021-02-27"
categories: 
  - "aws"
---

In this blog, we will run a Demo of creating an AWS Lambda Function which will convert a csv file into a json file.

### **Prerequisite**

1. Clone this git [repo](https://github.com/devops4solutions/AWSCodeSamples/tree/main/Lambda)
2. Refer this blog for basic understanding of AWS Lambda

For this we will use S3 bucket as a source where we will store the csv file that will trigger the lambda function and convert the file in json and store it in another bucket.

We will be creating below resources. You can refer the git repo

1. Create source S3 bucket
2. Create target S3 bucket
3. Create a lambda function in python
4. Zip the file
5. Upload a zip file in source S3 bucket
6. Create a lambda function
7. Create an IAM role and add permission
8. Create a trigger
9. Test it

### **Create source and target bucket**

cd Lambda  
python create\_s3\_bkt1.py  
python create\_s3\_bkt2.py

![](https://cdn-images-1.medium.com/max/1600/1*xReEIJZ7G-Rf6n6NXuT3rQ.png)

Now we will create a Lambda function which will do the parsing and store the new file in the target bucket

zip lamda\_function.zip lambda\_function.py  
`aws s3 cp lamda_function.zip s3://`devops4solutions-source

![](https://cdn-images-1.medium.com/max/1600/1*98jLGogmCZrnluBfirm_TQ.png)

We have successfully uploaded the zip file in our source bucket

### **Create an IAM role**

- which gives access to the lambda function with policy `AWSLambdaExecute`

aws iam create-role --role-name PayrollProcessingLambdaRole1 --description "Provides AWS Lambda with access to s3 and cloudwatch to execute the PayrollProcessing function" --assume-role-policy-document file://lambda-trust-policy.json

```
aws iam attach-role-policy --role-name PayrollProcessingLambdaRole1 --policy-arn arn:aws:iam::aws:policy/AWSLambdaExecute
```

### **Create a Lambda function**

You can use the aws-cli or can create directly from GUI also

```
aws lambda create-function --function-name PayrollProcessing --runtime python3.7 --role 
```

![](https://cdn-images-1.medium.com/max/1600/1*I1xGNLzBFEMZXAZpzfxQDg.png)

![](https://cdn-images-1.medium.com/max/1600/1*BebrERJz6OlHXR8MpIfOsg.png)

### **Add permission**

```
aws lambda add-permission --function-name PayrollProcessing --statement-id lambdas3permission --action lambda:InvokeFunction --principal s3.amazonaws.com --source-arn arn:aws:s3:::
```

![](https://cdn-images-1.medium.com/max/1600/1*w2qQ9zS_OPdtN1RPKsZUtA.png)

Now add trigger whenever an object is put in the S3 bucket

```
aws s3api put-bucket-notification-configuration ––bucket 
```

![](https://cdn-images-1.medium.com/max/1600/1*A103eanOgUXDHwjpDTcxDw.png)

Sometimes you see this error when you are trying to use the same bucket. To fix this issue, go to S3 bucket -> Properties -> Event -> Delete Event Notification

![](https://cdn-images-1.medium.com/max/1600/1*LFEf-4xIVeSvWfSktTvLOA.png)

![](https://cdn-images-1.medium.com/max/1600/1*Eap9ZL0cq75kqIGmKmOjeg.png)

### **Test it**

Copy csv file in the source bucket using below command

```
aws s3 cp input-payroll-data.csv s3://
```

Now you can see that in the target ,a new json file is uploaded

![](https://cdn-images-1.medium.com/max/1600/1*a6HQ4S1dEdCvKoZiXPreaA.png)

**References**

1. [Safari](https://learning.oreilly.com/library/view/aws-certified-developer/9781119508199/c12.xhtml) Book
