---
title: "Manage different values of variable against different host in ansible"
date: "2020-06-25"
categories: 
  - "ansible"
tags: 
  - "ansible"
---

In this blog we will explore how we can use the different values of variable against different host in [ansible](https://devops4solutions.com/automate-ansible-playbook-deployment-on-aws-ec2/)

If you want to see the video for this article, [click here](https://youtu.be/3bRtmJqLngI)

### **Problem Statement:**

- Two servers who have different username and password to connect with

### **Solution:**

In your hosts file you can pass the variables and the different values as shown below:

- We have two hosts which we named as application-0 and application-1
- In your inventory/host file like dev/hosts, we will put details like this 

application-0 ansible\_host=1.1.1.1 ansible\_user="{{ app0\_user}}" ansible\_sudo\_pass="{{ app0\_pass}}" ansible\_password="{{ app0\_pass}}"

application-1 ansible\_host=12.1.1.1 ansible\_user="{{ app1\_username}}" ansible\_sudo\_pass="{{ app1\_pass}}" ansible\_password="{{ app1\_pass}}"

### **How will you pass the variable**

Below is the example of how you can run the playbook using all the variables

'ansible-playbook main.yml -i inventories/${ENV}/hosts  -e "app0\_user=abc" -e "app0\_pass=password" -e "app1\_user=abc" -e "app1\_pass=password"
