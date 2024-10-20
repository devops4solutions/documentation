---
title: "What is Heroku"
date: "2018-08-15"
categories: 
  - "devops-tools"
---

In this blog, we will cover What is Heroku and configuring Heroku on our local system.

**Introduction**

As developers, we work in a world of abstractions. Each piece of technology that we use is built upon layers and layers of other systems. They set up standards and best practices to build web applications. We have all of these amazing tools to build applications, but for a long time, **we still struggled to easily deploy and scale them.**

**What is Heroku**

Heroku is the Platform as a Service that changed how we deploy web applications. Heroku automates the pain points of deploying code and has established best practices to build applications that need to scale. We no longer need to deal with the pain of setting up **load balancers, patching servers, or scrambling to scale up our infrastructure in response to high traffic.**

Prerequisite

1. Git

**2\. Install Heroku CLI from** [**here**](https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up) **for windows**

You use the CLI to manage and scale your applications, provision add-ons, view your application logs, and run your application locally.

**Verify Installation**

1. Open cmd prompt
2. Run heroku -v

![](https://cdn-images-1.medium.com/max/800/1*8o2tgVYKYRERRWQmue0M1w.png)

3\. Run heroku login

![](https://cdn-images-1.medium.com/max/800/1*9aq6W_lMICDhXWiCToZs0w.png)

Authenticating is required to allow both the `heroku` and `git` commands to operate.

We can ensure that we are authenticated with the `auth:whoami` command. If logged in successfully, it will print our e-mail address:

**$ heroku auth:whoami**
**youremail@example.com**

Conngratulations!! You have successfully completed Introduction of Heroku and configuring Heroku on our local system.
