---
title: "Continuous Deployment to Amazon S3 using Bitbucket pipeline"
date: "2018-12-16"
categories: 
  - "devops-tools"
---

In this blog, we will setup the Continuous Deployment to Amazon S3 using [Bitbucket pipeline](https://bitbucket.org/product/features/pipelines)

CI/CD Flow:

1. Docker image ( We are using docker customized image which has all the software which is required to run the application)
2. We are using caches feature of bitbucket pipeline. We are doing custom caching for node\_modules folder
3. Automate version using npm version patch of our nodejs application and committing the package.json automatically using pipeline only. To do the auto git commit set the SSH public key setup. Generate the public key of your repository and copy that public key to your account SSH keys.
4. Run the npm install command
5. After build is successful, store the artifacts to S3 bucket with versions
6. For deployment to production — make the trigger as manual.

Prerequisite:

1. S3 bucket on AWS
2. bitbucket-pipeline.yaml file
3. AWS Secret Keys
4. Environment Variables

**How to setup a Continuous Deployment to Amazon S3 using Bitbucket pipeline**

1. Create a bitbucket-pipeline.yaml file at the root of your project
2. For the specific bitbucket repository -> Go to Settings -> Add Repository variables

```
AWS_SECRET_ACCESS_KEYAWS_ACCESS_KEY_IDAWS_REGION
```

 All the above variables are required to connect to s3 bucket from bitbucket pipeline.

3\. We are setting up a Continuous Deployment to Amazon S3 using Bitbucket pipeline for nodejs application

_\# This is a sample build configuration for JavaScript._  
  
  
_\# Check our guides at https://confluence.atlassian.com/x/14UWN for more examples._  
  
  
image:  nikhilnidhi/inventory\_base  
pipelines:  
branches:  
master:  
\- step:  
caches:  
\- node-admin  
\- node-web  
  
  
name: Build and Generate Artifacts  
script:  
\- git config --global user.email "test"  
\- git config --global user.name "build machine"  
\- node -v  
\- ruby -v  
\- VERSION=$(npm version patch  -m "auto verison tick \[skip CI\]")  
\- VERSION=$(echo $VERSION | cut -c 2-)  
\- echo $VERSION  
\- npm install  
\- bower install --allow-root  
\- git add package.json  
\- grunt build  
\- aws s3 sync dist s3://m-test-files/admin/$VERSION  
\- aws s3 cp app/scripts/version.js s3://m-test-files/admin/$VERSION/scripts/version.js  
\- git commit -m "\[skip CI\]"  
\- git push  
  
  
\- step:  
caches:  
\- node-admin  
\- node-web  
name: Deploy to staging  
deployment: staging  
trigger: manual  
script:  
\- cd deploy  
\- ./deploy\_staging.sh  
definitions:       
 caches:            
node-admin: admin/node\_modules            
node-web: web/node\_modules  
  

![Bitbucket pipeline for s3 bucket](https://cdn-images-1.medium.com/max/800/1*R5xgDVq6zgssN5vcxGXxDg.png)

Bitbucket pipeline for s3 bucket

Congratulations, we have successfully setup the Continuous Deployment to Amazon S3 using Bitbucket pipeline.
