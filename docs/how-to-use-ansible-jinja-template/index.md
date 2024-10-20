---
title: "How to use Ansible Jinja template"
date: "2020-07-03"
categories: 
  - "ansible"
tags: 
  - "ansible"
  - "ansible-jinja-template"
  - "automation"
---

In this blog, we will see how we can use the [Ansible Jinja template](https://docs.ansible.com/ansible/latest/user_guide/playbooks_templating.html) and how it helps in the automation.

If you want to see the video for this article, [click here](https://youtu.be/kn_5cCdg800)

### **Problem Statement:**

- In many applications, you always have some property file which stores all the variables like database details,memory setting, JAVA VM arguments etc.
- And that property file can have different values across environments.

### **Solution:**

- Create a file test.properties.j2 under templates folder in your ansible playbook
- Create all the variables which are required in your template file

Option 1 —  How to use the variable directly

test.jdbc.maxActive={{ test\_jdbc\_maxactive | default('60') }}

Option 2 —How to use if/else condition in your template file and then use variables

{% if test\_db\_embedded %}
test.embeddedDatabase.port={{ test\_db\_embedded\_port | default('8080') }}
{% else %}
test.jdbc.username={{ test\_db\_user }}
test.jdbc.password={{ test\_db\_pass }}
test.jdbc.url={{ test\_jdbc\_url }}
{% endif %}

**How to use it in playbook**

\- name: "copy properties file"  
  template:  
    src: "test.properties.j2"  
    dest: "test/test.properties"
