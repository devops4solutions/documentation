---
title: "Manage inventory file for ansible playbook"
date: "2020-06-25"
categories: 
  - "ansible"
tags: 
  - "ansible"
---

In this blog we will explore how can we manage inventory file for the ansible playbook.

If you want to see the video for this article, [click here](https://youtu.be/ayMgxv54cTE)

### **Problem Statement:**

- There is a clustered application ( 2 application nodes and 1 database node), now how will you run your different playbooks for different nodes. For ex- you will be having different task to be done like install java on application node but that same task is not required on the database node

### **Solution:**

We need to maintain the inventory file as shown below:

- We have created a tag \[all\] and provide all the host values
- We have created two other tags, one for application and one for database
- We have created one more tag to exclude the second application node

\[all\]
application-0 ansible\_host=1.1.1.1
application-1 ansible\_host=1.2.2.3
database-0    ansile\_host=3.3.2.1
\[application-nodes\]
application-0
application-1

\[database-nodes\]
database-0

\[all-nodes-exclude-second\]
application-0
database-0

### **How to call in main.yml file ?**

- We will provide the tag name as per our inventory in the playbook where particular role needs to be executed

#### **Scenarios**

- Check SSH connectivity on all the nodes

\- hosts: all  
  gather\_facts: yes

- Install java on all the application nodes

\- hosts: application-nodes\[0:\]   
  gather\_facts: yes  
  tasks:  
    - include\_role:  
        name: install\_java  
       

- Install java only on first application nodes

\- hosts: application-nodes\[1\]
  gather\_facts: yes
  tasks:
    - include\_role:
        name: install\_java

- Install java only on first application node and database node, exclude the second application node

 - hosts: all-nodes-exclude-second\[0:\]
   gather\_facts: no
   tasks:
     - include\_role:
         name: install\_java
         

Like this you can create your inventory file as per your project need. I have provided some sample scenario that can help you how to Manage inventory files for ansible playbook
