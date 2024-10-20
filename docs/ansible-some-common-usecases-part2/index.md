---
title: "Ansible Some common usecases - Part2"
date: "2020-07-17"
categories: 
  - "ansible"
tags: 
  - "ansible"
  - "ansible-tutorials"
---

In this blog, we will explore some more common usecases which will help you in the automation.

If you want to see the video for this article, [click here](https://youtu.be/XYI0oPgaYYo)

Checkout my article on some more [Ansible usecases](https://devops4solutions.com/ansible-some-common-usecases/)

### **Agenda:**

1. How to check the connectivity of nodes from the master node ?
2. How to create a system group and user in linux ?
3. How to create a single directory?
4. How to create a multiple directory in linux using loop?
5. How to remove a file ?
6. How to check if files exist and then used `when` and `block` to execute the tasks
7. How to assign permissions to all folders and sub-folders recursively ?
8. How to copy from templates folder ?
9. How to call roles from the main.yml file ?
10. How to change the directory for a specific task ?
11. How to import Java certs ?

Lets explore all the scenarios one by one

### 1\. **How to check the connectivity of nodes from the master node ?**

- **gather\_facts:** This will check whether the ssh connection is setup between master and other nodes.
- By default this command is already set to true
- You should set this to `no` for the subsequent tasks to improve the performance

\- hosts: all
  gather\_facts: True
- hosts: children
gather\_facts: no

### **2\. How to create a system group and user in linux ?**

- We are creating a `test_group` group and `test_user` user on the linux machine
- Setting not to create a home directory of this user
- Setting nologin to this user

\- name: "Create system group"  
  group:  
    name: "test\_group"  
    state: present  
  
\- name: "Create system user"  
  user:  
    name: "test\_user"  
    group: "test\_group"  
    createhome: no  
    shell: "/sbin/nologin"  
    comment: "Test user"  
    state: present

### **3\. How to create a single directory?**

\- name: Create directory  
  file:   
    path: "/opt"  
    state: directory  
    mode: 0777  
  become: true

### **4\. How to create a multiple directory in linux using loop?**

- Creating a directory by setting its owner, group and mode
- Using `with_items` to iterate over multiple folders

\- name: "Creating directories"  
  file:  
    path: "{{ item }}"  
    state: directory  
    owner: "test\_user"  
    group: "test\_group"  
    mode: "u=rwx,g=rx,o="  
  with\_items:  
    - "{{ directory\_1}}"  
    - "{{ directory\_2}}"  
    - "{{ directory\_3}}"  
    - "{{ directory\_4}}"

- Create a variable `FOLDER_CREATE` in your variable file as shown below

FOLDER\_CREATE:  
   - /opt  
   - /temp  
   - /test

- Now in your playbook use this as shown below:

\- name: "Creating directories"  
  file:  
    path: "{{ item }}"  
    state: directory  
    owner: "test\_user"  
    group: "test\_group"  
    mode: "u=rwx,g=rx,o="  
  with\_items:  
    - "{{ FOLDER\_CREATE}}"

### **5\. How to remove a file ?**

\- name: "Remove file"  
    file:  
      path: "/tmp/test.sh"  
      state: absent

### **6\. How to check if files exist and then used** `**when**` **and** `**block**` **to execute the tasks**

- Store the result in variable `binary_stat` if file exist
- Use `block` to execute multiple tasks based on the `when` condition

\- name: "Ensure file exists"
  stat:
    path: "/opt/test.sh"
  register: binary\_stat

- block:
   - name: "Remove file"
     file:
       path: "/opt/test.sh"
       state: absent
when: not binary\_stat.stat.exists

### **7\. How to assign permissions to all folders and sub-folders recursively ?**

\- name: "Set File permission"  
  file:  
    path: "/tmp/test"  
    state: directory  
    recurse: yes  
    owner: "test\_user"  
    group: "test\_group"  
    mode: "go-w"

### **8\. How to copy from templates folder ?**

- Create a file under /templates/test.properties.j2
- Copy it to the destination `test/test.properties`

  
  
\- name: "copy test.properties using template  
  template:  
    src: "test.properties.j2"  
    dest: "test/test.properties"  
    owner: "test\_user"  
    group: "test\_group"  
    mode: "u=rw,g=r,o="

### **9 . How to call roles from the main.yml file ?**

- Below command will run the main.yml file which is present in the roles/install\_java folder

\- hosts: host  
  gather\_facts: no  
  become: true  
  tasks:  
    - include\_role:  
        name: install\_java  
  

- If you want to run the specific yaml file then do as shown below:

\- hosts: host  
  gather\_facts: no  
  become: true  
  tasks:  
    - include\_role:  
        name: installl\_java  
        tasks\_from: test.yml

### 10\. **How to change the directory for a specific task ?**

- Find the specific file and provide the path from where playbook should run using `args`

\- name: "Ensure file exists"  
  stat:  
    path: "test.sh"  
  register: binary\_stat  
  args:  
     chdir: "/opt"

### **11\. How to import java certs ?**

\- name: Import certs using keytool  
  java\_cert:  
   cert\_path: "test.jks"  
   keystore\_path: "cacerts"  
   keystore\_pass: "test"  
   cert\_alias: "test"  
   executable: "keytool"  
   state: present  
  become: yes  
  become\_user: root

Congratulations, you have successfully learnt some common usecases which will help you in the automation.
