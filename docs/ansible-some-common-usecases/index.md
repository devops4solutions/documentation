---
title: "Ansible some common usecases"
date: "2020-07-11"
categories: 
  - "ansible"
tags: 
  - "ansible"
---

In this blog, we will explore some common usecases which will help you in the automation.

If you want to see the video for this article, [click here](https://youtu.be/Kwf-B2RBqxA)

### **Agenda**:

1. Find applications which are installed on a system
2. Configure bashprofile — add a new line to set java home
3. Run playbook if variable is defined
4. Run playbook if folder is exist

#### **Scenario 1:** 

- You want to check list of packages installed on your system.
- **no\_log**: We have set this variable as **true.** If you save Ansible output to a log, you expose any secret data in your Ansible output, such as passwords and user names. To keep sensitive values out of your logs, mark tasks that expose them with the `no_log: True` attribute.Check [this](https://docs.ansible.com/ansible/latest/reference_appendices/logging.html) link for more details on ansible logging
- Install the package if not installed else skip the playbook
- Parsing the json output to find a particular application and using **set\_fact** for setting the output in a new variable
- **fail**: This module fails the progress with a custom message using when condition
- Set a variable if application is not installed

\- name: List of all installed packages
  yum:
    list: installed
  register: installed\_packages

- set\_fact:
test\_installed\_packages: "{{ installed\_packages.results | json\_query(query) }}"
vars:
query: "\[?name=='vim-minimal'\]"

- debug:
msg: "{{test\_installed\_packages}}"

- fail:
msg: Package installed already
when: test\_installed\_packages| length > 0

- set\_fact:
application\_installed: False
when: not test\_installed\_packages

- debug:
msg: "application not installed"
when: not application\_installed

Ansible Output if application is found

"msg": \[  
        {  
            "arch": "x86\_64",  
            "envra": "2:vim-minimal-7.4.160-4.el7.x86\_64",  
            "epoch": "2",  
            "name": "vim-minimal",  
            "release": "4.el7",  
            "repo": "installed",  
            "version": "7.4.160",  
            "yumstate": "installed"  
        }

#### **Scenario 2:**

- Configure bash profile for java\_home path
- **lineinfile**: This module ensures a particular line is in a file, or replace an existing line using a back-referenced regular expression.
- This is primarily useful when you want to change a single line in a file only.

\- name: Configure bash\_profile  
  lineinfile:  
    dest: ~/.bash\_profile  
    regexp: '^JAVA\_HOME='  
    line: "JAVA\_HOME=/usr/lib/jvm"  
  become\_user: root  
  become: true  
  ignore\_errors: true  
  
\- name: Configure bashrc  
  lineinfile:  
    regexp: '^JAVA\_HOME='  
    line: "JAVA\_HOME=/usr/lib/jvm"  
    dest:  ~/.bashrc  
  become\_user: root  
  become: true  
  ignore\_errors: true

Check out [this](https://docs.ansible.com/ansible/latest/modules/lineinfile_module.html) link for more details on lineinfile module in ansible.

#### **Scenario 3:**

- You want to run a playbook only when some variable is defined
- Use it as if/else condition based on the variable defined

\- include: main.yml   
  when: VAR\_MAIN is defined

\- include: test.yml   
  when: VAR\_TEST is defined

- Below is the command to run the playbook

ansible-playbook main.yml  -e "VAR\_TEST=abc"

#### **Scenario 4:**

- Run the task only when some folder is exist

\- name: Check folder exist or not.  
  stat:  
   path: "{{ folder\_path }}"  
  register: folder\_path\_exist  
      
\- debug:  
    msg: "Directory is present"  
  when: folder\_path\_exist.stat.exists  
    
\- name: Create {{location }} directory  
  file:   
    path: "{{location}}"  
    state: directory  
    mode: 0777  
  become: true  
  when: folder\_path\_exist.stat.exists ==false

Congratulations, you have successfully learnt some common usecases which will help you in the automation. To know more about the Ansible common usecases visit [here](https://devops4solutions.com/ansible-some-common-usecases-part2/).
