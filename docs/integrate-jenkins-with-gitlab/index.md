---
title: "Integrate Jenkins with Gitlab"
date: "2018-06-14"
categories: 
  - "devops-tools"
---

This blog will help you to Integrate Jenkins with Gitlab.

**Go to GitLab** ->**Profile Settings**

![](https://cdn-images-1.medium.com/max/800/1*T48JNROOgaQab68OiWmslA.png)

Then, click “**Access Token**” and create a **Jenkins API Token**

Then, save and **copy this API Token**. You won’t be able to access it again.

**Configure Jenkins server with Gitlab server**

Go to **Manage Jenkins -> Configure System** and scroll down to the ‘**GitLab**‘ section. Write a custom connection name, enter the **GitLab server URL** in the **‘GitLab host URL**‘ field and click **Add -> Jenkins**button.

![](https://cdn-images-1.medium.com/max/800/0*-nJ6ZguHuyxGi-BQ.png)

Then, fill required fields as shown below with the **Jenkins Access API Token**which we created in GitLab before.

![](https://cdn-images-1.medium.com/max/800/0*Pn7WVRoMQ1Goibp7.png)

Then, select this **API Token credential** and finally click “**Test**” and see the “**Success**” message as shown below

![](https://cdn-images-1.medium.com/max/800/1*wJOdwQYE565bQDXbHf5apQ.png)
