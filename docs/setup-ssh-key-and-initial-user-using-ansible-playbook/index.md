---
title: "Setup SSH Key and initial user using Ansible Playbook"
date: "2018-10-12"
categories: 
  - "ansible"
  - "aws"
  - "devops-tools"
---

In this blog we will Setup SSH Key and initial user using Ansible Playbook

To create new user on ubuntu system, you need the following things:

1. Username/Password
2. Public Key of the user
3. You will first create a user on one machine. Machine can be your local workstation also
4. Generate ssh-key for this
5. Put the public key of that user to the remote hosts.
6. Add that user to the sudoers.d file

Now we want to disable the Password Authentication on all the remote hosts.This means no user/root user can login to the system by using password. They have to use the SSH keys only.

Steps:

1. Login as root . Do sudo -su
2. _useradd -m -s /bin/bash devops passwd devops_
3. _echo -e ‘devops\\tALL=(ALL)\\tNOPASSWD:\\tALL’ > /etc/sudoers.d/devops_
4. Encrypt your password
5. _sudo apt install whois -y_
6. _mkpasswd — method=SHA-512 TYPE THE PASSWORD ‘devops’_

![](https://cdn-images-1.medium.com/max/800/1*d3fspoQao1ntxgvDMRlLQA.png)

**Generate a new SSH-key**

1. Login as a devops user
2. _ssh-keygen -t rsa_

It will generate the public and private key file for the devops user.

![](https://cdn-images-1.medium.com/max/800/1*TEGm8jDeUt7WqbyT5TWMJw.png)

Now we have to add this public key to all the remote hosts.

Create a ansible playbook “add-user-ssh.yml”

\---
 - hosts: all
   vars:
     - devops\_password: 'abcddefsfdfdfdfdfdfdfdfdfdfd'
   gather\_facts: no
   remote\_user: ubuntu
   become: true

tasks:

\- name: Add a new user named devops
     user:
          name: devops
          shell: /bin/bash
          password: "{{ devops\_password }}"

\- name: Add devops user to the sudoers
     copy:
          dest: "/etc/sudoers.d/devops"
          content: "devops  ALL=(ALL)  NOPASSWD: ALL"

\- name: Deploy SSH Key
     authorized\_key: user=devops
                     key="{{ lookup('file', '/home/devops/.ssh/id\_rsa.pub') }}"
                     state=present

\- name: Disable Password Authentication
     lineinfile:
           dest=/etc/ssh/sshd\_config
           regexp='^PasswordAuthentication'
           line="PasswordAuthentication no"
           state=present
           backup=yes

_\- name: Disable Root Login
     lineinfile:
           dest=/etc/ssh/sshd\_config
           regexp='^PermitRootLogin'
           line="PermitRootLogin no"
           state=present
           backup=yes
     notify:
       - restart ssh_

handlers:
   - name: restart ssh
     service:
       name=sshd
       state=restarted

**Run the playbook**

ansible-playbook add-devops-user-ssh.yml -i hosts

**Validate Disable Password Authentication**

$ ssh servername -o PubkeyAuthentication=no

You will get the “Permission Denied(public key)
