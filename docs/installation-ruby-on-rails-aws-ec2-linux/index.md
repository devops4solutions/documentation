---
title: "Installation of Ruby on Rails on AWS EC2 Linux"
date: "2018-08-07"
categories: 
  - "devops-tools"
tags: 
  - "bundle"
  - "gem"
  - "ruby-on-rails"
  - "rvm"
---

In this blog we will install all dependencies/ software which are required for the Installation of Ruby on Rails application.

### Installation of Ruby on Rails

1. #### **Install dependencies**
    

sudo yum install -y curl gpg gcc gcc-c++ make

![](https://cdn-images-1.medium.com/max/800/1*nWifUD-YeVOHz1fZ-gmOvA.png)

### 2\. Install RVM

RVM supports most UNIX like systems and Windows (with [**Cygwin**](https://cygwin.com/) or [**Bash on Ubuntu on Windows**](https://msdn.microsoft.com/en-us/commandline/wsl/about)). The basic requirements are `**bash**`, `**curl**`, `**gpg2**`and overall GNU version of tools - but RVM tries to autodetect it and install anything that is needed.

### Install GPG keys

Firstly we need to install [**mpapis**](https://rvm.io/authors/mpapis/) [**public key**](https://keybase.io/mpapis) used to verify installation package to ensure [**security**](https://rvm.io/rvm/security/).

sudo gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
$ curl -sSL https://get.rvm.io | sudo bash -s stable
$ sudo usermod -a -G rvm \`whoami\`

![](https://cdn-images-1.medium.com/max/800/1*8ZKKikus44qCy6MpSvku_Q.png)

![](https://cdn-images-1.medium.com/max/800/1*u8-us1qhmX3F2HMyPiTJWw.png)

On systems where sudo is configured with `secure_path`, the shell environment needs to be modified to set `rvmsudo_secure_path=1`. `secure_path` is set on most Linux systems, but not on macOS. The following command tries to autodetect whether it is necessary to install `rvmsudo_secure_path=1`, and only installs the environment variable if it is the code.

**if** sudo grep -q secure\_path /etc/sudoers; **then** sudo sh -c "echo export rvmsudo\_secure\_path=1 >> /etc/profile.d/rvm\_secure\_path.sh" && echo Environment variable installed; **fi**

![](https://cdn-images-1.medium.com/max/800/1*zkGIs6WrEBINN47Xo2JlIg.png)

Run rvm command and you will receive command not found. You have to relogin on the server

![](https://cdn-images-1.medium.com/max/800/1*U5gJ-p4gUtS26in8WpPSiw.png)

After relogin , run rvm — version

![](https://cdn-images-1.medium.com/max/800/1*7ogLyCQdoYJ6cyEqu91UQg.png)

**Install the latest version of Ruby**

$ rvm install ruby
$ rvm --default use ruby

\[caption id="" align="alignnone" width="800"\]![Ruby Installation](https://cdn-images-1.medium.com/max/800/1*QT5gofpdRwIuDmePcjWVBg.png) Ruby Installation\[/caption\]

Check ruby version and sqlite3 version. You can use other databases as per your need

\[caption id="" align="alignnone" width="554"\]![Ruby Version](https://cdn-images-1.medium.com/max/800/1*HItdO9AzwiD01bv6CmlHQQ.png) Ruby Version\[/caption\]

### Install Bundler

Bundler is a popular tool for managing application gem dependencies. We will use Bundler in this tutorial, so let us install it:

gem install bundler --no-rdoc --no-ri

![](https://cdn-images-1.medium.com/max/800/1*uYjI1lf98Z2zYAuPU7QPCA.png)

Bundle Installation

### Install Node.js

If you are using Rails, then you must install Node.js. This is because Rails’s asset pipeline compiler requires a Javascript runtime.

```
curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -
```

sudo yum install -y nodejs

**Install Rails**

Before you install Rails, you should check to make sure that your system has the proper prerequisites installed. These include Ruby and SQLite3.

```
$ gem install rails
$ rails --version
```

\[caption id="" align="alignnone" width="522"\]![Rail Version](https://cdn-images-1.medium.com/max/800/1*xezH6iol39osMgO4FmchGw.png) Rail Version\[/caption\]

Finally congratulations!! You have successfully followed each and every step for Installation of Ruby on Rails. All dependencies which are required for Ruby on Rails application is successfully installed.

References:

[**Installing Ruby with RVM - deployment walkthrough with Ruby, Passenger, Nginx and Linux/Unix …** _There are other ways to install Ruby, e.g. through yum, apt-get, source tarball, rbenv and chruby. You can use one of…_www.phusionpassenger.com](https://www.phusionpassenger.com/library/walkthroughs/deploy/ruby/ownserver/nginx/oss/install_language_runtime.html "https://www.phusionpassenger.com/library/walkthroughs/deploy/ruby/ownserver/nginx/oss/install_language_runtime.html")
