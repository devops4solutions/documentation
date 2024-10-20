---
title: "Heroku in brief"
date: "2018-08-25"
categories: 
  - "devops-tools"
tags: 
  - "heroku"
---

With this blog we will try to understand Heroku in brief. What is heroku, what are its features, why and who should use heroku are some of the points discussed.

##### Why Heroku?

In Modern day world, web infrastructure requirement for applications, has grown immensely, making it complicated day by day. As a developer, you would crave to have a ‘Magic box’, which can handle all tasks related to infrastructure, deployment, patching, upgrading, 24/7 operations and security and let you throw all your efforts in coding for application development.

“Heroku lets us focus on our app & deliver more value to customers”

\-Adron Peckham, Urban Dicionary

##### Traditional way to define and describe Heroku:

**“Heroku is an elastic, multi-language, multi-framework PaaS”**

A bit history doesn’t hurt:

- Founded in 2007
- To provide cloud platform service for developers
- Acquired by Salesforce in 2010

##### Heroku is a largest PaaS (Platform as a service):

PaaS : provides a platform allowing customers to develop, run and manage applications without the complexity of building and maintaining the infrastructure, typically associated with developing and launching an application.                                                                                                                                                                                                                        -Wikki

It handles over 5 billion requests per day. Provides everything you require to build, run and scale custom and customer facing apps.

With PaaS it has made far simpler to host applications on internet. Heroku handls most of the details in the background required to release a line application.

**Story time:** Few developers have created an app called ‘Level up’ for mobile payment during the hurricane Sandy, enabling people to buy essentials with any mode of payment. They have used Heroku to deploy the app quickly with zero downtime and can easily scale to manage thousands of payment partners and to process over 3 million orders.

##### Heroku key points:

- It is a far simpler tool which can be used by beginners to deploy web applications, as Heroku platform supplies the read runtime environment.
- It is directly based on AWS.
- No need to worry about developing or managing the infrastructure. Just choose the plan and alter it to scale according to requirement.
- Heroku also provides certain amount of processing power/time to try our apps. Every month you can run an app for free (31 days = 744 days). Once you will get popular then you might need to buy some resources (scale up dynos to handle HTTP traffic) to support additional user/customers hitting your website.
- You only worry about creating an app and push it to Heroku using Git. Afterwards, all management will be done by Heroku CLI or dashboard.
- Initially Heroku was designed to deploy Ruby on Rails, but now it supports other languages like Python, PHP, Clojure, Go, Java, and Node.js. Also any other language can be supported through a feature called “BuildPack”.
- Heroku encourages a very modular approach to scale your app. Just a slide on the dashboard, you can scale up web, clock and qc worker along with the amount chargeable.

##### Heroku gives you:

1. Developer driven deployment.

You just assemble your code in git (version control) and a following simple command will push it to Heroku.

$git push heroku master

 

Now Heroku take cares of everything else. After which, all you need to take care about is scaling your application.

2. Heroku status

It also let you monitor the current status of the Heroku and will instantly report any error. Thus, in case of any error, you can check whether it is because of your app or Heroku service has gone wrong.

3. Logging

In Heroku, it is referred to as “Logplex” which is a compiled view to understand a root cause of any problem. Because of the display of all relevant information, diagnosis becomes faster.

$heroku logs

4. Faster Rollbacks

In case of any problem with the current released version, you can easily restore the previous version while analyzing the root cause for error. Both database and application rollback is possible.

5. Add-ons

Heroku provides large collection of add-ons, covering whole range of services from databases, logging, management etc.

6. App Health monitoring
7. GitHub Integration
8. Manual Vertical and Horizontal Scaling

##### Instant & Continuous deployment with Heroku:

Deploy changes at the speed of business change.

![](https://cdn-images-1.medium.com/max/1000/1*E0vazyXdfJsIDo0_nTxBEQ.png)

 

##### Heroku should be used when:

- To deploy and test the minimum viable product (MVP).
- Have the constant requirement to improve app based on users reviews.
- Cannot afford to hire DevOps engineer.
- Your app doesn’t need high computing power.

Conclusion: Heroku is a great platform as a service provider which is quite a good option to opt for if your project is not too big and if you would like to analyse the success rate of your application. However, for bigger target audience and high HTTP traffic input you might need to consider cost variable as AWS EC2 and Elastic Beanstalk are strong competitors for heroku.

References:

- https://hackernoon.com/
- https://dzone.com/
- https://smashingboxes.com
- https://rubygarage.org/
- https://medium.com/
