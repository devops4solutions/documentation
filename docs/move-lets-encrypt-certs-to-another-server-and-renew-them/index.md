---
title: "Move Let's Encrypt certs to another server and renew them"
date: "2021-05-26"
categories: 
  - "devops-tools"
---

In this blog, we will see how to Move Let’s Encrypt certs to another server and renew it.

Checkout my [Youtube](https://youtu.be/3PUXMg2J5ik) video on this blog.

To move the certs from one server to another -

- Make a zip file of `/etc/letsencrypt` folder 
- Install the certbot on the new server 
- Extract it on the new server

This process will work without any issues but when you will try to renew the certs then you might see the below error

**Attempting to parse the version 1.9.0 renewal configuration file found at /etc/letsencrypt/renewal/abc.com.conf with version 0.40.0 of Certbot. This might not work.  
Renewal configuration file /etc/letsencrypt/renewal/abc.com.conf produced an unexpected error: expected /etc/letsencrypt/live/abc.com/cert.pem to be a symlink. Skipping.**

So, to fix the above issue after moving the certs from one server to another certs , run the below command

certbot certificates

This command will fail with the same error as shown above.

So basically, when we move certs from one server to another, symlink didn’t get created automatically you have to create it manually

- Rename the existing certs from the live folder
- Create a symlink from archive folder. Change the `fullchain10.pem` to whatever you have in your archive folder

sudo mv /etc/letsencrypt/live/abc.com/fullchain.pem /etc/letsencrypt/live/abc.com/fullchain.pem.old

sudo ln -s /etc/letsencrypt/archive/abc.com/fullchain10.pem /etc/letsencrypt/live/abc.com/fullchain.pem

sudo mv /etc/letsencrypt/live/abc.com/cert.pem /etc/letsencrypt/live/abc.com/cert.pem.old  
sudo ln -s /etc/letsencrypt/archive/abc.com/cert10.pem /etc/letsencrypt/live/abc.com/cert.pem

sudo mv /etc/letsencrypt/live/abc.com/chain.pem /etc/letsencrypt/live/abc.com/chain.pem.old  
sudo ln -s /etc/letsencrypt/archive/abc.com/chain10.pem /etc/letsencrypt/live/abc.com/chain.pem

sudo mv /etc/letsencrypt/live/abc.com/privkey.pem /etc/letsencrypt/live/abc.com/privkey.pem.old  
sudo ln -s /etc/letsencrypt/archive/abc.com/privkey10.pem /etc/letsencrypt/live/abc.com/privkey.pem

After this, when you run this command you should see the certification information without any warning message

certbot certificates

**Renew the certs**

Run the below command and follow the prompts

certbot certonly -d abc.com --manual --preferred-challenges dns
