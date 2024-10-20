---
title: "Installation of Ruby on Rails on AWS EC2 Ubuntu 16.04"
date: "2018-08-20"
categories: 
  - "devops-tools"
tags: 
  - "aws"
  - "ruby"
  - "ruby-on-rails"
  - "ubuntu"
---

In this blog we will go through the steps for Installation of Ruby on Rails on AWS EC2 Ubuntu 16.04.

##### Ruby installation on Ubuntu:

Following are the steps to be employed to download and install latest version of Ruby onto your AWS EC2 Ubuntu instance:

###### Step 1. rvm installation

- rvm **(Ruby Version Manager):** rvm is needed to be install, in order to download, compile and install new ruby version.
- However, before installing rvm, we require to install “curl” program.

ubuntu@ip-xxx-xx-xx-xxx:~$ sudo apt-get update
ubuntu@ip-xxx-xx-xx-xxx:~$ sudo apt-get install curl
ubuntu@ip-xxx-xx-xx-xxx:~$ \\curl -L https://get.rvm.io | bash -s stable --ruby

\*Note: You may require to close and reopen your terminal for rvm command to work.

###### Step 2. Install Ruby

ubuntu@ip-xxx-xx-xx-xxx:~$ rvm get stable --autolibs=enable
ubuntu@ip-xxx-xx-xx-xxx:~$ rvm install ruby

![](https://cdn-images-1.medium.com/max/1100/1*hOEyI10QSuc80rpC76g9vA.png)

###### Step 3. Check the version of ruby and the default database ‘sqlite3’ installed:

ubuntu@ip-xxx-xx-xx-xxx:~$ ruby -v
ubuntu@ip-xxx-xx-xx-xxx:~$ sqlite3 --version

![](https://cdn-images-1.medium.com/max/1100/1*0HyJ1Jdpg0NafLuXl4SjHQ.png)

![](https://cdn-images-1.medium.com/max/1100/1*sV1hG_GabiLYemmVBcD9FA.png)

###### Step 4. Node.js installation

Few libraries on which Rails depends upo, require JavaScript runtime. For Ubuntu Linux, Node.js server-side JavaScript environment is best to handle those libraries.

ubuntu@ip-xxx-xx-xx-xxx:~$ `curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -`

ubuntu@ip-xxx-xx-xx-xxx:~$ `sudo apt-get install -y nodejs`

![](https://cdn-images-1.medium.com/max/1100/1*OMrKwGTNopxx2CoxrhStIA.png)

###### Step 5. Bundler installation

![](https://cdn-images-1.medium.com/max/1100/1*HzORiwT5CH9B8B2bgmGzQA.png)

![](https://cdn-images-1.medium.com/max/1100/1*JyYb9c72btozoiJUe7txog.png)

The [Bundler](https://rubygems.org/gems/bundler) gem is an essential tool for managing gems when developing and running Rails applications. RVM used to install Bundler automatically.

![](https://cdn-images-1.medium.com/max/1100/1*8LwhIHbg7WDJ-f8D4zup_Q.png)

#### Ruby on Rails installation on AWC EC2 Ubuntu 16.04:

ubuntu@ip-xxx-xx-xx-xxx:~$ gem install rails

check the version installed

ubuntu@ip-xxx-xx-xx-xxx:~$ rails -v

![](https://cdn-images-1.medium.com/max/1100/1*A0HgTy5tcG8ZFG0btJTobg.png)

Congrats!! You have successfully installed Ruby on Rails on Ubuntu.

Furthermore, if you would like to learn the installation on other platforms, visit the link  https://devops4solutions.com/category/language/

Also, in order to learn to deploy rails app on Heroku, click **here**.
