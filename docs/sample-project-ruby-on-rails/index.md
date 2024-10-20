---
title: "Sample Project Ruby On Rails"
date: "2018-08-07"
categories: 
  - "devops-tools"
---

In this blog , we will create a sample project (create and read operation) of Ruby on Rails project.

# **Introduction**

Ruby on Rails is a web development framework . Rails is used by companies as diverse as Airbnb, Basecamp, Disney, GitHub, Hulu, Kickstarter, Shopify, Twitter, and the Yellow Pages.

## **What makes Rails so great?**

Firstly, Ruby on Rails is 100% open-source.Rails effectively creates a domain-specific language for writing web applications. As a consequence, many common web programming tasks — such as generating HTML, making data models, and routing URLs — are easy with Rails, and the resulting application code is concise and readable.

### **Prerequisite**

To develop a web application using Ruby on Rails Framework, you need to install the following softwares −

- Ruby
- The Rails Framework
- A Web Server
- A Database System

Please visit [Ruby on Rail Installation link.](https://devops4solutions.com/installation-ruby-on-rails-aws-ec2-linux/)

### Ruby on Rails MVC Framework

The **M**odel **V**iew **C**ontroller principle divides the work of an application into three separate but closely cooperative subsystems.

#### Model (ActiveRecord )

It maintains the relationship between the objects and the database and handles validation, association, transactions, and more.

#### View ( ActionView )

It is a presentation of data in a particular format, triggered by a controller’s decision to present the data.

#### Controller ( ActionController )

The facility within the application that directs traffic, on the one hand, querying the models for specific data, and on the other hand, organizing that data (searching, sorting, messaging it) into a form that fits the needs of a given view.

Rails application structure has an application directory called **app/**containing three subdirectories: **models**, **views**, and **controllers**. This is a hint that Rails follows the model-view-controller (MVC) architectural pattern, which enforces a separation between the data in the application (such as user information) and the code used to display it, which is a common way of structuring a graphical user interface (GUI).

When interacting with a Rails application, a browser sends a _request_ that is received by a webserver and passed on to a Rails _controller_, which is in charge of what to do next. In some cases, the controller will immediately render a _view_, which is a template that gets converted to HTML and sent back to the browser. More commonly for dynamic sites, the controller interacts with a _model_, which is a Ruby object that represents an element of the site (such as a user) and is in charge of communicating with the database. After invoking the model, the controller then renders the view and returns the complete web page to the browser as HTML.

##### **Create a Sample Project**

This will create a Rails application called Blog in a `blog` directory and install the gem dependencies that are already mentioned in `Gemfile` using `bundle install`.

rails new blog

![](https://cdn-images-1.medium.com/max/800/1*wU_oiMcS0M9biBKI7Pis9g.png)

![](https://cdn-images-1.medium.com/max/800/1*2ioVMLEVQ8JEqTFufRkZ5Q.png)

Directory structure of rails project as shown below:

 

\[caption id="" align="alignnone" width="783"\]![](https://cdn-images-1.medium.com/max/800/1*4hhmiWCvUwq4N5Wyu9q5nA.png) Rails Directory Structure\[/caption\]

**Starting WebServer**

cd blog
bin/rails server

 

\[caption id="" align="alignnone" width="712"\]![Rail Server Started](https://cdn-images-1.medium.com/max/800/1*ckNkETVDlxD3i2dGA99nbA.png) Rail Server Started\[/caption\]

Open web browser : [http://localhost:3000](http://localhost:3000/)

![](https://cdn-images-1.medium.com/max/800/1*y4GhIu13-Tus72AxAWQqhw.png)

The above screen shows that Ruby on Rails Sample project is successfully setup

Now you can customize the code and create any sample application as per your need. I have created a basic (Create and Read Operation) on Ruby On Rails. Please refer the github link for [Ruby on Rails Sample Projec](https://github.com/devops4solutions/CI_Jenkins_RubyonRails)t.

To understand how the sample project is being created, please refer this [link](https://guides.rubyonrails.org/getting_started.html)

Finally congratulations!! You have successfully followed each and every step for creating Sample Project in Ruby On Rails.
