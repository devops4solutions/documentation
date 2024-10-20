---
title: "Heroku — What is dynos, workers, and scaling"
date: "2018-08-16"
categories: 
  - "devops-tools"
tags: 
  - "dynos"
  - "heroku"
---

In this blog, we will understand  What is dynos, workers, and scaling

Heroku’s killer feature has always been its ability to easily scale up and scale out our applications as our user base grows. This frees us from the pains of setting up and managing load balancers and additional servers on our own.

**Scaling up** and **scaling out** are two common terms used when growing web applications:

- **Scaling up** (vertical scaling) means that we are making our servers more powerful by adding more CPU/RAM
- **Scaling out** (horizontal scaling) means that we are adding more servers to our application

### What’s a dyno?

**Dyno** is the term Heroku uses for its web servers. A dyno is simply a virtual private server that runs our application and responds to web requests.

### What’s a worker?

Heroku has an additional class of servers known as **workers**. These are identical to dynos, with the exception that they do not serve web requests.

### Process sizes

Both dynos and workers are available in three different sizes: 1X, 2X, and PX. The default size is 1X; this is a small virtual server with 512 MB of RAM. These are large enough to run most web applications.

If our application has only a single 1X dyno running, it will shut down after an hour of inactivity. To avoid this, we need to have at least two dynos running or use a single 2X dyno.

1. To view our currently running processes, we can use the `ps` command. It will show the type, the size, and exactly what's running:

**$ heroku ps**

![](https://cdn-images-1.medium.com/max/800/1*SRvvm-HV-Elq8bdFq7lMRQ.png)

2\. We currently have only one dyno running for this application. Let’s scale it up to two; this will effectively double our application’s capacity. Scaling processes are done with the `ps:scale` command:

**$ heroku ps:scale web=2**

The `scale` command is very flexible. If we want, we can scale both dynos and workers at the same time:

**$ heroku ps:scale web=2 worker=1**

### DYNOS

A dyno is simply a web server. When we create our application’s Procfile, the web process that we define is what runs on our dynos. When a user visits our web application, their requests get sent to our dynos via Heroku’s routing layer. The routing layer acts like a load balancer. It distributes our users’ requests and monitors the health of our dynos. To handle more users, we can scale out our application by increasing the number of running dynos. This allows us to serve requests from more concurrent users. If we are currently running one dyno and adding another, we have theoretically doubled the amount of web requests that our application can respond do.

### WORKERS

In our Procfile, any process other than `web` will run on a worker. To process background tasks such as sending out e-mails or generating PDFs we use workers.  Any background job (such as Resque or Sidekiq) on Ruby on Rails will need to be run on a worker dyno. Workers can be scaled in exactly the same way as dynos. If our application has a large backlog of tasks that need to be completed, we can add additional workers to increase the number of tasks we can complete simultaneously.

Finally, we have successfully understand What is dynos, workers, and scaling
