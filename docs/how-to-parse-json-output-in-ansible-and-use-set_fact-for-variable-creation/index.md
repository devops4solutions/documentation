---
title: "How to parse json output in Ansible and use set_fact for variable creation"
date: "2020-07-11"
categories: 
  - "ansible"
tags: 
  - "ansible"
  - "json"
  - "set_fact"
---

In this blog, we will explore how to parse json output in [Ansible](https://devops4solutions.com/automate-ansible-playbook-deployment-on-aws-ec2/) and use set\_fact for variable creation based on the json output

If you want to see the video for this article, [click here](https://youtu.be/_ddWgjxssBQ)

### **Problem Statement:**

- You have an application where you want to make an api call to search for a particular group and depending on the result you want to run your playbook or set some variables

### **Solution**:

- Write a playbook to search for the group “test\_group”
- Use “**uri**” module to interact with your webservices (http/https)
- **Registering variable:** When you execute a task and save the return value in a variable for use in later tasks, you create a registered variable.
- **force\_basic\_auth:** Force the sending of the Basic authentication header upon initial request.The library used by the uri module only sends authentication information when a webservice responds to an initial request with a 401 status. Since some basic auth services do not properly send a 401, logins will fail.
- **debug**: prints statements during execution and can be useful for debugging variables or expressions without necessarily halting the playbook.
- **ignore\_errors**: You set this as true if you want playbook to continue run subsequent tasks and doesn’t fail the playbook

### **Ansible Playbook**

#search group if exist   
\- name: Search if it exit  
  uri:  
    url: "http://{{hostname}}/api/user\_groups/search?q=test\_group"  
    method: GET  
    user: "username"  
    password: "password"  
    force\_basic\_auth: yes  
    return\_content: yes  
    headers:  
        Content-Type: "application/json"  
  ignore\_errors: true  
  register: result

\- debug:  
    msg: "{{result}}"

\- debug:   
    msg: "{{ result.json | list }}"

#### **Sample JSON Output**

- This is the sample output from my application when I execute the above playbook

"msg": {  
        "changed": false,  
        "msg": "All items completed",  
        "results": \[  
            {  
                 
                "content": "{\\"paging\\":{\\"pageIndex\\":1,\\"pageSize\\":100,\\"total\\":1},\\"groups\\":\[{\\"id\\":11760,\\"name\\":\\"test\_group\\",\\"membersCount\\":3,\\"default\\":false}\]}",  
                  
                "content\_type": "application/json",  
                  
                "json": {  
                    "groups": \[  
                        {  
                            "default": false,  
                            "id": 11760,  
                            "membersCount": 3,  
                            "name": "test\_group"  
                        }  
                    \],  
                    "paging": {  
                        "pageIndex": 1,  
                        "pageSize": 100,  
                        "total": 1  
                    }  
                }  
                  
        }  
        \]  
    }

- **paging.total** —gives me the count of group
- **set\_fact**: allow setting new variables
- Based on the count you can call other tasks/playbook etc
- Below is the example of how you can write a playbook using set\_fact and filter the json data using ‘**from\_json**’

\- set\_fact: total\_count="{{ result.content | from\_json | json\_query('paging.total')}}"

\- include: sentemail.yml   
  when: total\_groups == '0'  
  ignore\_errors: true

- If group is exist and you want to fetch the group name

\- set\_fact: group\_name="{{ result.content | from\_json | json\_query('groups\[0\].name')}}"

- Check this [link](https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html#filters-for-formatting-data) for more examples of filtering in json

Congratulations, you have successfully learnt how to parse json output in Ansible and use set\_fact for variable creation.
