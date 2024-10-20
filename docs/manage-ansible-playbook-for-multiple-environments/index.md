---
title: "Manage Ansible Playbook for multiple environments"
date: "2018-10-23"
categories: 
  - "ansible"
tags: 
  - "ansible"
  - "environments"
---

In this blog , we will see how we can manage [Ansible Playbook](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html) for multiple environments.

**Problem Statement:**

- How we can write the role in ansible which has to refer different values for each environment (dev,pvs,production) ? Example — database details

**Solution:**

- To resolve this issue,we will be using different files for each environment and pass that as a variable while running the ansible playbook.

**How to do:**

1. Install Ansible if not already installed
2. Use Ansible Roles

Create the directory structure as shown below:

![directory structure](https://miro.medium.com/max/682/1*44QPMCvV-p1OAetX4VN9cA.png)

1. **configs** — This folder will have separate file for each environment which stores all the environment specific values.
2. **group\_vars** — This will have one file which is used to store all the variables which are common across environments.
3. **inventories** — This folder will have host file for each environment
4. **roles** — Task which you will have for your application like install java
5. **ansible.cfg** — You can use this file if you have to override any default configuration. Example:

host\_key\_checking = False ( You will not see this error **"****The authenticity of host “hostname” can’t be established."** if you specify this variable. 

Now in your main.yml file call the roles as mentioned below:

**gather\_facts: True (**This will check the connectivity between master and other nodes)

\---
- hosts: all
  gather\_facts: True  ( This will check the connectivity between   master and other nodes)
  become: true

  vars:
    java\_version: "{{ java\_version}}"

  roles:
    - role: install-java

Run ansible playbook as shown below :

ansible-playbook main.yml -i inventories/dev/hosts -k -K -e '@configs/dev.yml' -e 'java\_version=1.8'

Finally congratulations!! You have successfully followed each and every step for **Managing Ansible Playbook for multiple environments.**
