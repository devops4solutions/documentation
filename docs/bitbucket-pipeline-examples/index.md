---
title: "Bitbucket Pipeline Examples"
date: "2018-11-30"
categories: 
  - "devops-tools"
---

**This articles demonstrate a quick example to create Bitbucket pipeline.**

\# This is a sample build configuration for JavaScript.

\# Refer https://confluence.atlassian.com/x/14UWN for more examples.

\# Only use spaces to indent your .yml configuration.

\# -----

\# You can specify a custom docker image from Docker Hub as your build environment.

image: nikhilnidhi/inventory\_base

pipelines:

branches:

codeship:

\- step:

caches:

\- node-admin

\- node-web

name: Build and Generate Artifacts

script:

\- git config --global user.email "tname"

\- git config --global user.name "build machine"

\- node -v

\- ruby -v

\- cd admin

\- VERSION=$(npm version patch -m "auto verison tick \[skip CI\]")

\- VERSION=$(echo $VERSION | cut -c 2-)

\- echo $VERSION

\- npm install

\- bower install --allow-root

\- git add package.json

\- grunt build

\- aws s3 sync dist s3:/bucnetname/$VERSION

\- aws s3 cp app/scripts/version.js s3://bucketnames/admin/$VERSION/scripts/version.js

\- cd ../web

\- npm install

\- bower install --allow-root

\- npm version patch -m "auto verison tick \[skip CI\]"

\- git add package.json

\- grunt build

\- aws s3 sync dist s3://bu/web/$VERSION

\- aws s3 cp app/scripts/version.js s3://bucketname/web/$VERSION/scripts/version.js

\- git commit -m "\[skip CI\]"

\- git push

\- step:

caches:

\- node-admin

\- node-web

name: Deploy to m-test

script:

\- cd deploy

\- ./deploy\_test.sh

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

\- step:

caches:

\- node-admin

\- node-web

name: Deploy to Production

deployment: production

trigger: manual

script:

\- cd deploy

\- ./deploy\_prod.sh

definitions:

caches:

node-admin: admin/node\_modules

node-web: web/node\_modules

Sample for lambda function

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

#- node-lambda deploy -a $AWS\_ACCESS\_KEY\_ID -s $AWS\_SECRET\_ACCESS\_KEY -o $AWS\_ROLE -r $AWS\_REGION -n $AWS\_LAMBDA\_FUNCTION\_NAME --excludeGlobs "bitbucket-pipelines.yml"

caches:

\- node
