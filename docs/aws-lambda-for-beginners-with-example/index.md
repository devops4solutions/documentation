---
title: "AWS Lambda for beginners with example"
date: "2021-02-27"
categories: 
  - "aws"
---

In this blog we will learn about AWS Lambda and create a lambda function.

Check out my [Youtube](https://youtu.be/rV9akF3-0qw) video on this article.

### **What is Serverless Compute ?**

> _Serverless compute_ is a cloud computing execution model in which the AWS Cloud acts as the server and dynamically manages the allocation of machine resources.

- _AWS Lambda_ is the _AWS serverless compute_ platform that enables you to run code without provisioning or managing servers.
- With AWS Lambda, you can run code for nearly any type of application or backend service — with zero administration
- Only upload your code, and AWS Lambda performs all the tasks you require to run and scale your code with high availability.

**AWS Lambda is sometimes referred to as a _function-as-a-service_ (FaaS)**

AWS Lambda executes code whenever the function is triggered, and no _Amazon Elastic Compute Cloud_ (Amazon EC2) instances need to be spun up in your infrastructure.

### **Benefits of Lambda over EC2 Instances**

- No Servers to manage
- Automatically scales your application by running code in response to each trigger.
- when you run Amazon EC2 instances, you are responsible for costs associated with the instance runtime. It does not matter whether your site receives little to no traffic — if the server is running, there are costs. With AWS Lambda, if no one executes the function or if the function is not triggered, no charges are incurred.

### **How AWS Lambda works**

- AWS Lambda uses containerization to run your code
- When your function is triggered, it creates a _container that will execute your code and returns your application as a result_
- **Cold Start: Container is created on the first invocation**
- Once the container starts to run, it remains active for several minutes before it terminates. If an invocation runs on a container that is already available, that invocation runs on a **_warm containe_**_r_.

![](https://cdn-images-1.medium.com/max/1600/1*38ruq-KjbBYVNOxRUJVY_g.png)

### LANGUAGES AWS LAMBDA SUPPORTS

AWS Lambda functions currently support the following languages:

- C# (.NET Core 1.0)
- C# (.NET Core 2.0)
- Go 1._x_
- Java 8
- Node.js 4.3
- Node.js 6.10
- Node.js 8.10
- Python 2.7
- Python 3.6

When you create an AWS Lambda function, there are four options:

![](https://cdn-images-1.medium.com/max/1600/1*40HxeBT_G3NcQ4wF4IPSwA.png)

- **Author from scratch** Manually create all settings and options.
- **Blueprints** Select a preconfigured template that you can modify.
- **Container Image**
- **Serverless application repository** Deploy a publicly shared application with the AWS Serverless Application Model (AWS SAM).

### EXECUTION METHODS/INVOCATION MODELS

- **Non-streaming event source ( Push Model)**

**Ex**\- Amazon Echo, Amazon Simple Storage Service (Amazon S3), Amazon Simple Notification Service (Amazon SNS), and Amazon Cognito

- **Streaming event source ( Pull Model)**

Ex- Amazon Kinesis or Amazon DynamoDB stream

We can execute an AWS Lambda function **synchronously** or **asynchronously**.

The **InvocationType** parameter determines when to invoke an AWS Lambda function. 

This parameter has three possible values:

- **RequestResponse** — Execute synchronously.
- **Event** — Execute asynchronously.
- **DryRun** — Test that the caller permits the invocation but does not execute the function.

With an event source (_push model_), a service such as Amazon S3 invokes the AWS Lambda function each time an event occurs with the bucket you specify.

**Push Model Flow**

![](https://cdn-images-1.medium.com/max/1600/1*9unMcEkuxDPxuqZ7MTRYwQ.png)

1. You create an object in a bucket.
2. Amazon S3 detects the object-created event.
3. Amazon S3 invokes your AWS Lambda function according to the event source mapping in the bucket notification configuration.
4. AWS Lambda verifies the permissions policy attached to the AWS Lambda function to ensure that Amazon S3 has the necessary permissions.
5. AWS Lambda executes the AWS Lambda function, and the AWS Lambda function receives the event as a parameter.

**Pull Model**

With a _pull model_ invocation, AWS Lambda polls a stream and invokes the function upon detection of a new record on the stream. Amazon Kinesis uses the pull model.

![](https://cdn-images-1.medium.com/max/1600/1*zQW9lXrDL87LfgXaxIiYBA.png)

1. A custom application writes records to an Amazon Kinesis stream.
2. AWS Lambda continuously polls the stream and invokes the AWS Lambda function when the service detects new records on the stream. AWS Lambda knows which stream to poll and which AWS Lambda function to invoke based on the event source mapping you create in AWS Lambda.
3. Assuming that the attached permissions policy, which allows AWS Lambda to poll the stream, is verified, then AWS Lambda executes the function.

The final way to invoke an AWS Lambda function applies to custom applications with the RequestReponse invocation type. Using this invocation method, AWS Lambda executes the function synchronously, returns the response immediately to the calling application, and alerts you to whether the invocation occurs.

Your application creates an HTTP POST request to pass the necessary parameters and invoke the function. To use this type of invocation model, you must set the RequestResponse in the X-Amz–Invocation–Type HTTP header.

### **Security in Lambda Function**

There are two types of policies with Lambda: 

- a function policy and 
- an execution policy, or AWS role.

 A function policy defines which AWS resources are allowed to invoke your function. 

The execution role defines which AWS resources your function can access.

![](https://cdn-images-1.medium.com/max/1600/1*H3cY-Ce0xFPafj8jspG3CA.png)

AWS Lambda provides the following AWS permissions policies:

1. **LambdaBasicExecutionRole (**Grants permissions only for the Amazon CloudWatch logactions to write logs.)
2. **LambdaKinesisExecutionRole**
3. **LambdaDynamoDBExecutionRole**
4. **LambdaVPCAccessExecutionRole**

### **AWS Lambda Function Components**

1. Function Package

The function code package contains everything you need to be available locally when your function is executed. At minimum, it contains your code for the function itself, but it may also contain other assets or files that your code references upon execution. This includes binaries, imports, or configuration files that your code/function needs. The maximum size of a function code package is 50 MB compressed and 250 MB extracted/decompressed.

2\. Function Handler

When the AWS Lambda function is invoked, the code execution begins at the handler. The handler is a method inside the AWS Lambda function that you create and include in your package.

```
def aws lambda_handler(event, context):
```

**Event object**

The event includes all the data and metadata that your AWS Lambda function needs to implement the logic.

For Ex — For Amazon API Gateway service with the AWS Lambda function, it contains details of the HTTPS request that was made by the API client. 

Values, such as 

- the path, 
- query string, 
- and the request body, are within the event object.

**Context object**

The context object contains data about the AWS Lambda function invocation itself. The context and structure of the object vary based on the AWS Lambda function language.

There are three primary data points that the context object contains.

1. **AWS Requestid** Tracks specific invocations of an AWS Lambda function, and it is important for error reports or when you need to contact AWS Support.

**2\. Remaining time** Amount of time in milliseconds that remain before your function timeout occurs. AWS Lambda functions can run a maximum of 300 seconds (5 minutes) but you can configure a shorter timeout.

**3\. Logging** Each language runtime provides the ability to stream log statements to Amazon CloudWatch Logs. The context object contains information about which Amazon CloudWatch Log stream your log statements are sent to.

### **Configure AWS Lambda Functions**

1. Provide Description/Tags
2. Memory — You can configure how much memory your function requires
3. TimeOut- The default timeout value is 3 seconds; however, you can specify a maximum of 300 seconds (5 minutes)

With the default network configuration, your AWS Lambda function communicates from inside an Amazon VPC that AWS Lambda manages. 

The AWS Lambda function can connect to the internet, but not to any privately deployed resources that run within your own VPCs, such as Amazon EC2 servers.

### **Versioning & Aliases**

- You can create multiple versions of your function without affecting your the function which is currently deployed in production environment
- Each AWS Lambda function has a unique ARN

**Publish a Version**

- You can publish a version of a function
- After you publish a version, it is immutable, and you cannot change it.
- After this you can use this version number in your ARN to refer that exact function

**Problem with the version**

Your function is on version 1 and you updated ARN in your application to refer to that version. 

Now when you will do the update to your function then you need to update the ARN at all the places to reflect this new function

**Create an Alias**

Now we will create an alias to fix this issue

- assign an alias to a particular version (Version 1) and use that alias in the application.

> Version 1 -> Assign alias of name `production` to this version

- Update the ARN to use this alias `production` instead of the version number

Now for the next version you can make the changes without affecting your production environment

- Create a new version `Version 2` and do some testing.
- At the time if version update in production you just reassign the alias to the new version number. In this case, you do no need to update any ARN reference in your application

### **Monitoring**

AWS Lambda enables these metrics: 

- invocation count, 
- invocation duration,
- invocation errors, 
- throttled invocations, 
- iterator age, 
- and DLQ errors.

![](https://cdn-images-1.medium.com/max/1600/1*xhue6O7OVXFzcVRWx5cWPA.png)

![](https://cdn-images-1.medium.com/max/1600/1*3tHd-MAmTrer-Q_W3z5WIg.png)

![](https://cdn-images-1.medium.com/max/1600/1*MTc9I49t-l0NG3jv0_f-FQ.png)

![](https://cdn-images-1.medium.com/max/1600/1*Ca6AwCNKkwy837HJxAeWiw.png)

### **Concurrency**

![](https://cdn-images-1.medium.com/max/1600/1*KxEhjvpWPUuO3MUbAGSEPA.png)

![](https://cdn-images-1.medium.com/max/1600/1*JZaj0XlCQ19y1A-u6wyOIQ.png)

Here you can see that Function is throttled

![](https://cdn-images-1.medium.com/max/1600/1*zULS2Zu1-M1-pfKKBGniIg.png)

### **Dead Letter Queues/ Asynchronous Configuration**

When you invoke your function asynchronously, AWS Lambda handles retries. Incoming events are placed in a queue before being sent to the function. If the function returns an error, Lambda retries up to two times. If the function is throttled, or Lambda returns an error, the event is kept in the queue for up to six hours. To change the default retry behavior, adjust the number of retry attempts and the maximum amount of time an item can be kept in the queue.

When an event fails all attempts or stays in the asynchronous invocation queue for too long, Lambda discards it. Configure a dead-letter queue to send discarded events to an Amazon SQS queue or Amazon SNS topic. Your function’s execution role requires permission to write to the queue or topic.

![](https://cdn-images-1.medium.com/max/1600/1*fhlpaFq5izshPYzKG81-pg.png)

**References**

1. [Safari](https://learning.oreilly.com/library/view/aws-certified-developer/9781119508199/c12.xhtml) Book
