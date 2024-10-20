---
title: "Configure webhooks in GitLab"
date: "2018-06-14"
categories: 
  - "devops-tools"
---

This blog will help you how to Configure webhooks in GitLab.

This is required to trigger the Jenkins job as an when there is a push in the repository

## Steps

1. Go to specific project in gitlab-> click on settings ->integration as shown below

![](https://cdn-images-1.medium.com/max/800/1*oFweEdLszXDnPN2suF0LNw.png)

Put the jenkins job project url and all default setting

[http://localhost:8080/gitlab/build\_now](http://34.228.188.96:8080/gitlab/build_now)

Click on test and push events

All configuration is setup successfully.

You may face the below issue :

<html>
<head>
<meta http-equiv="Content-Type" content="text/html;charset=utf-8"/>
<title>Error 403 No valid crumb was included in the request</title>
</head>
<body><h2>HTTP ERROR 403</h2>
<p>Problem accessing /job/Test\_php/. Reason:
<pre>    No valid crumb was included in the request</pre></p><hr><a href="http://eclipse.org/jetty">Powered by Jetty:// 9.4.z-SNAPSHOT</a><hr/>

</body>
</html>

Error after comitting in gitlab

No need for this -> disable csrf in security
configure credential — for anonymous access
