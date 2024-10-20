---
title: "Setup Nginx as a Reverse Proxy for Apache Tomcat running on different servers"
date: "2018-10-17"
categories: 
  - "devops-tools"
tags: 
  - "nginx"
  - "reverse-proxy"
---

In this blog, we will Setup Nginx as a Reverse Proxy for Apache Tomcat running on different servers

Requirement:

1. Two Ubuntu Server
2. Apache installed on one ubuntu server
3. Nginx installed on one ubuntu server

**Install Apache2**

sudo apt-get install apache2

sudo service apache2 start

By default, it is running on port 80, open your browser [http://yourip](http://yourip)

You will see the default page of apache ubuntu server.

Now to check this url is running on which server

1. Right click on your page -> Inspect -> Click on Network -> make sure All is selected as shown below

![](https://cdn-images-1.medium.com/max/800/1*ieK1r_4xtpj8s2zWoiX3_A.png)

2\. Reload the page -> you will see your ip -> click on your ip ->Click Headers

You will see that is running on apache server

![](https://cdn-images-1.medium.com/max/800/1*-EOS1rT4WKqwo8Fr5FAeBg.png)

Edit Apache default configuration

1. sudo vi /ect/apache2/ports.conf and make your configuration like this or you can use the port as per your requirement.
2. If you <VirtualHOst 127.0.0.1:8080> — this will not work if your instance is in private subnet or private IP
3. 127.0.0.1 points to the loopback device
4. 0.0.0.0 is listen on all interfaces (\`lo0\`, \`eth0\`, \`eth1\`, etc)

\# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

<VirtualHost 0.0.0.0:8080>

ServerAdmin test@localhost.local
        DocumentRoot /var/www/html

ErrorLog ${APACHE\_LOG\_DIR}/error.log
        CustomLog ${APACHE\_LOG\_DIR}/access.log combined

</VirtualHost>

2\. sudo vi /etc/apache2/sites-enabled/000-default.conf

Change the VirtualHost to run on port 8080. All other setting remains same

<VirtualHost 0.0.0.0:8080>

3\. Restart Apache Server

**NGINX Configuration**

2. sudo apt-get install nginx

4. sudo service nginx start

6. Open your browser and you will see the default NGinx home page

8. NGinx also runs on default port 80

**Configure it to use as a proxy for apache server**

2. cd /etc/nginx/conf.d

4. sudo vi proxy.conf

6. Configure DNS for your EC2 instance where nginx server is running and put that domain name in tag “server\_name”

upstream testname {
 server ipofapacheserver:8080 fail\_timeout=0;
}

server {
   listen       80;
   server\_name domainname\_of\_nginxserver;

location / {
       proxy\_set\_header Host $host;
       proxy\_set\_header X-Real-IP $remote\_addr;
       proxy\_set\_header X-Forwarded-For $proxy\_add\_x\_forwarded\_for;
       proxy\_set\_header X-Forwarded-Proto $scheme;
       proxy\_pass [http://testname](http://testname);
       proxy\_read\_timeout 90;
       proxy\_http\_version 1.1;
       proxy\_request\_buffering off;
   }
}

Configure proxy.conf file if HTTPS is enabled

upstream testname {
server ipofapacheserver:8080 fail\_timeout=0;
}
server {
    #listen       80;
   server\_name domainname\_of\_nginxserver;
location / {
       proxy\_set\_header Host $host;
       proxy\_set\_header X-Real-IP $remote\_addr;
       proxy\_set\_header X-Forwarded-For $proxy\_add\_x\_forwarded\_for;
       proxy\_set\_header X-Forwarded-Proto $scheme;
       proxy\_pass [http://testname](http://testname);
       proxy\_read\_timeout 90;
       proxy\_http\_version 1.1;
       proxy\_request\_buffering off;
   }

listen 443 ssl; # managed by Certbot
    ssl\_certificate /etc/letsencrypt/live/devopstest1.kpd-i.com/fullchain.pem; # managed by Certbot
    ssl\_certificate\_key /etc/letsencrypt/live/devopstest1.kpd-i.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl\_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}

server {
    if ($host = [www.devopstest1.kpd-i.com](http://www.devopstest1.kpd-i.com)) {
        return 301 [https://$host$request\_uri](https://$host$request_uri);
    } # managed by Certbot

if ($host = devopstest1.kpd-i.com) {
        return 301 [https://$host$request\_uri](https://$host$request_uri);
    } # managed by Certbot

listen       80;
    server\_name  devopstest1.kpd-i.com [www.devopstest1.kpd-i.com](http://www.devopstest1.kpd-i.com);
    return 404; # managed by Certbot

}

3\. sudo nginx -t

ubuntu@ip-11–0–11–165:/etc/nginx/conf.d$ sudo nginx -t nginx: the configuration file /etc/nginx/nginx.conf syntax is ok nginx: configuration file /etc/nginx/nginx.conf test is successful

4\. Restart or reload nginx

5\. Now browse url —  [http://yourip](http://yourip) — this will redirect to apache home page and do the above steps to check which server it is running on , you will see the nginx server

![](https://cdn-images-1.medium.com/max/800/1*17pllxo54Y5SotuAKTyCcw.png)

Below is my security group setting for this EC2 instance — Make sure port 80 is open

![](https://cdn-images-1.medium.com/max/800/1*-jekU4ja8IcmwpDU1fO4Hw.png)
