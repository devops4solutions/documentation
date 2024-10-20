---
title: "Setup Nginx as a Reverse proxy for Apache Server"
date: "2018-10-16"
categories: 
  - "devops-tools"
tags: 
  - "nginx"
  - "reverse-proxy"
---

In this blog, we will Setup Nginx as a Reverse proxy for Apache Server running on the same server

Requirement:

1. One Ubuntu Server
2. Apache installed
3. Nginx installed

Install Apache2

sudo apt-get install apache2

sudo service apache2 start

By default, it is running on port 80, open your browser [http://yourip](http://yourip/)

You will see the default page of apache ubuntu server.

Now to check this url is running on which server

1. Right click on your page -> Inspect -> Click on Network -> make sure All is selected as shown below

![](https://cdn-images-1.medium.com/max/800/1*ieK1r_4xtpj8s2zWoiX3_A.png)

2\. Reload the page -> you will see your ip -> click on your ip ->Click Headers

You will see that is running on apache server

![](https://cdn-images-1.medium.com/max/800/1*-EOS1rT4WKqwo8Fr5FAeBg.png)

Edit Apache default configuration

1. sudo vi /ect/apache2/ports.conf and make your configuration like this or you can use the port as per your requirement

\# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

Listen 8080

<IfModule ssl\_module>
 Listen 8443
</IfModule>

<IfModule mod\_gnutls.c>
 Listen 8443
</IfModule>

\# vim: syntax=apache ts=4 sw=4 sts=4 sr noet

2\. sudo vi /etc/apache2/sites-enabled/000-default.conf

Change the VirtualHost to run on port 8080. All other setting remains same

<VirtualHost 127.0.0.1:8080>

3\. Restart Apache Server

**NGINX Configuration**

1. sudo apt-get install nginx
2. sudo service nginx start
3. Open your browser and you will see the default NGinx home page
4. NGinx also runs on default port 80

**Configure it to use as a proxy for apache server**

1. cd /etc/nginx/conf.d
2. sudo vi proxy.conf and make sur eto delete the default file

server {
listen 80;
root /usr/share/nginx/html;
index index.html;
location / {
 proxy\_pass [http://127.0.0.1:8080/](http://127.0.0.1:8080/);
}
}

3\. sudo nginx -t

ubuntu@ip-11–0–11–165:/etc/nginx/conf.d$ sudo nginx -t nginx: the configuration file /etc/nginx/nginx.conf syntax is ok nginx: configuration file /etc/nginx/nginx.conf test is successful

4\. Restart or reload nginx

5\. Now browse url — [http://yourip](http://yourip/) — this will redirect to apache home page and do the above steps to check which server it is running on , you will see the nginx server

![](https://cdn-images-1.medium.com/max/800/1*17pllxo54Y5SotuAKTyCcw.png)

Below is my security group setting for this EC2 instance — Make sure port 80 is open

![](https://cdn-images-1.medium.com/max/800/1*-jekU4ja8IcmwpDU1fO4Hw.png)
