---
title: "DevOps culture/agility for Infrastructure as code (Iac)"
date: "2018-09-07"
categories: 
  - "devops-tools"
---

# DevOps culture/agility for Infrastructure as code (Iac)

“The basic principle is that operators should not log in to a new machine and configure it from documentation.” -- **_Boyd Hemphill_ **                 Director of evangelism – StackEngine

Manually configuring, provisioning and changing the infrastructure components (VM, Networks, Firewalls, Load Balancer etc) is often convoyed with the associated errors, risks and technical debts, as:

- Changes become difficult to track and manage
- Versioning and consistency missing
- Centralized management is difficult to administer
- Rollbacks can become a real troublesome
- Tedious manual job to manipulate existing infrastructure is subjected to inefficiency and human errors
- Deploying applications is prone to human errors

**Iac** is a notion that identifies almost every underlying infrastructure component (servers, databases, network, apps, logs, cloud provider settings etc) of your application as software and thereby can implement code to manage the automation and configuration of infrastructure provisioning.

There exists multitude of tools and cloud providers to automate the infrastructure building process. At **xvz.com** we use AWS as an Iaas, **Terraform** as a provisioning tool to build, change and version the infrastructure and **Ansible** to configure the infrastructure and to deploy our applications. Everything is tightly bolted together with **JIRA** (as a tracking tool), Slack (for communication & collaboration), **GitHub** (as source code repository) and **Jenkins** (for CI/CD) to employ the orchestration with pipeline, ensuring DevOps amalgamation and agile framework. Any change in the Terraform script or Ansible playbook is committed and versioned in GitHub, triggering the Jenkins job to execute Terraform first and Ansible afterwards to successfully create and configure reliable infrastructure.

![](https://cdn-images-1.medium.com/max/1000/1*nVqMGifuAenkjuJoxo04jg.png)

“xyz manages your infrastructure like a software product using agile development processes.”

![](https://cdn-images-1.medium.com/max/1000/1*Mucpwi0dsJi4no9JWMpcEA.png)

References:

- [https://aws.amazon.com](https://aws.amazon.com)
- [https://www.terraform.io](https://www.terraform.io)
- [https://www.zaizi.com](https://www.zaizi.com)
- [http://d0.awsstatic.com](http://d0.awsstatic.com)
- [https://www.cloudreach.com](https://www.cloudreach.com)
- [https://dzone.com](https://dzone.com)
