---
title: "How to use one AWS loadbalancer for multiple services"
date: "2021-05-12"
categories: 
  - "aws"
---

When you have multiple applications which are running inside an ECS cluster or anywhere else then how can you use just one loadbalancer for all the applications.

Using separate loadbalancer for each of the services/applications will be more expensive as each LB cost around $15/month and if you have 5 services then this would be $75/month.

Checkout my [Youtube](https://youtu.be/BNSh2TpkYEg) video on this blog

**Example:**

You have an application which is listening on port `8080` and it requires SSL certificates. 

- Application can listen on same or different port also

**Solution**

- Define the instance port in the Target Group
- configure the Target Group to use port 8080 or whatever your application is listening on.
-  Then point the Application Load Balancer’s port 80 listener to the Target Group. 
- The ALB will listen on port 80, and forward requests to port 8080 on the Target Group instances.

**ALB configuration**

Below is an example of one ALB which is listening on port 80 and 443

![](https://cdn-images-1.medium.com/max/1600/1*1b6UifqCkEIjIiKM-fyMqw.png)

**Add Rules on port 80**

- Create a rule if host header is `example.com` then forward to the target group which you have created for your `example.com` . So this configuration forward requests to port 8080 on the Target Group instances.
- This way you can have as many rules required for each of your applications
- Create one rule for each of the application which redirects to https

![](https://cdn-images-1.medium.com/max/1600/1*LZrl25w_A-09k9QlUTYASQ.png)

**Add Rules on port 443**

For SSL, you can use the certificates which you have created on ACM ( Amazon Certificate Manager)

- Create the same rule as you created for port `80`
- Create one rule on port `80` which will redirect http to https as shown above

![](https://cdn-images-1.medium.com/max/1600/1*aBLcydJblUmZ-k2JjOWo-Q.png)

**How certificate works**

When you create a first rule on port `443` then it will ask for the certificates. At that time you can use one default certificate for any of your application.

Later on, for each of your new application you can add more certificates. 

As per your host header this will automatically take the certificate for your application.

![](https://cdn-images-1.medium.com/max/1600/1*5rCkSD2gK8HLdfNNRWEsGA.png)
