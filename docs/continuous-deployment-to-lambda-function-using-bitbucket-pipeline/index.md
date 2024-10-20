---
title: "Continuous Deployment to Lambda function using Bitbucket pipeline"
date: "2018-12-16"
categories: 
  - "devops-tools"
---

In this blog, we will setup the Continuous Deployment to Lambda function using [Bitbucket pipeline](https://bitbucket.org/product/features/pipelines)

### CI/CD Flow:

1. Docker image 
2. We are using caches feature of bitbucket pipeline. We are doing custom caching for node\_modules folder
3. Run the npm install command
4. Creating a zip file
5.  Upload zip file to S3 bucket 
6.  Update the lambda function that will take the new artifacts from S3 buckets

### Prerequisite:

1. S3 bucket on AWS
2. Lambda function
3. IAM Role
4. bitbucket-pipeline.yaml file
5. AWS Secret Keys
6. Environment Variables

**How to setup a Continuous Deployment to Lambda function using Bitbucket pipeline**

1. Create a bitbucket-pipeline.yaml file at the root of your project
2. For the specific bitbucket repository -> Go to Settings -> Add Repository variables

```
AWS_SECRET_ACCESS_KEYAWS_ACCESS_KEY_IDAWS_REGION
```

All the above variables are required to connect to s3 bucket and the lambda function from bitbucket pipeline.

3\. We are setting up a Continuous Deployment to Lambda function using Bitbucket pipeline for nodejs application

image: lambci/lambda:build-nodejs6.10  
pipelines:  
default:  
\- step:  
deployment: production  
script:  
\- npm install  
\- npm install node-lambda -g  
\- zip test.zip ./node\_modules index.js test.js  
\- aws s3 cp test.zip s3://test-bucket-carnival  
\- aws lambda update-function-code --function-name lambdatest --s3-bucket test-bucket-carnival --s3-key test.zip  
_#- node-lambda deploy -a $AWS\_ACCESS\_KEY\_ID -s $AWS\_SECRET\_ACCESS\_KEY -o $AWS\_ROLE -r $AWS\_REGION -n $AWS\_LAMBDA\_FUNCTION\_NAME --excludeGlobs "bitbucket-pipelines.yml"_  
  
  
caches:  
\- node  
  
  

Logs of a bitbucket-pipeline for lambda function

![](https://cdn-images-1.medium.com/max/800/1*ZaISBvjx5mPsuYZK6a6gIQ.png)

  
Bitbucket Pipeline for Lambda function

Congratulations, we have successfully setup the Continuous Deployment to Lambda function using Bitbucket pipeline
