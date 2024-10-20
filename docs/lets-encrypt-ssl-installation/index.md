---
title: "Automate Let's Encrypt SSL Installation with Ansible for multiple domains"
date: "2018-10-23"
categories: 
  - "ansible"
  - "aws"
  - "devops-tools"
tags: 
  - "ansible"
  - "ssl"
---

In this blog, we will Automate Let's Encrypt SSL Installation with [Ansible](https://devops4solutions.com/automate-ansible-playbook-deployment-on-aws-ec2/) for multiple domains.

### What is Let's Encrypt

[Let's Encrypt](https://letsencrypt.org/) is a free, open and automated certificate authority. For any website you need the SSL certificate so that your website is more secure. Let's Encrypt is one of the tool which is being used to generate the SSL certificate for your website.

In general, we generate the SSL certificate manually by running the command which Let's Encrypt provides. In this article, we will see how we can use Let's Encrypt for generating the certificates.

### Prerequisite:

1. 1. Nginx Server . It could be apache or any other server where you want to install Let's Encrypt for generating SSL certificates

## Ansible Setup:

#### Steps:

1. Create a variable file which stores all the variables which are required to run an ansible playbook

\# Variables for Role Certbot certs.
certbot\_create\_if\_missing: false
certbot\_create\_method: standalone
certbot\_admin\_email: [y](mailto:ngupta@kpd-i.com)ouremailid
certbot\_create\_standalone\_stop\_services: - nginx
apache\_vhosts: - {servername: "abcyourdomain.com", documentroot: "/var/www/abcyourdomain.com"} - {servername: "abcyourdomain1.com", documentroot: "/var/www/abcyourdomain1.com"}

2\. Create a main.yml file that will execute the command of installing certbot. This script handles multiple domains using loop concept in ansible. I have used with\_items that will work as a for loop and then based on the result of the first statement it will generate the certificate if not exist

\---
- name: Upgrade System
  apt: upgrade=dist update\_cache=yes
- name: Add certbot repository apt\_repository: repo: 'ppa:certbot/certbot'

- name: Install Certbot's Nginx package apt: name: python-certbot-nginx state: present - name: Check if certificate already exists. stat: path: /etc/letsencrypt/live/{{ item.servername}}/cert.pem register: letsencrypt\_cert with\_items: "{{ apache\_vhosts }}"
- name: Stop services to allow certbot to generate a cert. service: name: "{{ item }}" state: stopped with\_items: - "{{ certbot\_create\_standalone\_stop\_services }}"

- name: Generate new certificate if one doesn't exist. shell: "certbot certonly --standalone --noninteractive --agree-tos --email {{ certbot\_admin\_email }} -d {{ item.item.servername}}" with\_items: "{{ letsencrypt\_cert.results }}" when: item.stat.exists == False

- name: Start services after cert has been generated. service: name: "{{ item }}" state: started with\_items: "{{ certbot\_create\_standalone\_stop\_services }}"

3\. Create a main.yml that will call the above file.

\- hosts: proxy
  become: yes
  gather\_facts: no
  vars\_files:
     - environments/{{ env }}//group\_vars//main.yml
  roles:
      - certbot

4\. Now run the playbook

ansible-playbook main.yml -i environments/dev/inventory/hosts — extra-vars env=dev
