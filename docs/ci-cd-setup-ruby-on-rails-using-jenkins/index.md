---
title: "CI/CD Setup Ruby On Rails using Jenkins"
date: "2018-08-07"
categories: 
  - "jenkins"
tags: 
  - "ci-cd"
  - "github"
  - "jenkins"
  - "junit-test-report"
  - "ruby-on-rails"
---

In this blog, we will  do the CI/CD Setup Ruby On Rails using Jenkins.

**Prerequisite**

1. Java — [Install Java from here](https://devops4solutions.com/java-installation-linux/)
2. Jenkins — [Install Jenkins from here](https://devops4solutions.com/jenkins-installation-on-aws-ec2-linux-instance/)
3. Git — sudo yum install git

**Install Plugins in Jenkins**

1. Go to Manage Jenkins -> Manage Plugins -> RubyMetrics, Rake, Rvm
2. Go To Manage Jenkins -> Configure System -> Search for Rake

Find the RVM Installation and Ruby Installation Path , run the below command:

which rvm
which ruby

![](https://cdn-images-1.medium.com/max/800/1*LIhzu2TLRGlf5Ktug4n9BQ.png)

RVM and Ruby Path

Please put the below values of these installations

RVM Path -> /usr/local/rvm/bin

Ruby Path -> /usr/local/rvm/rubies/ruby-2.5.1/

![](https://cdn-images-1.medium.com/max/800/1*_4roAfykUW-HKhwQwkGmuA.png)

rvm home path

**Setup Environment for jenkins user**

1. Change password for jenkins user

sudo passwd jenkins

2\. Edit the below file

vi /etc/sudoers

3.Put the below content

jenkins ALL=NOPASSWD: ALL

4\. Login as jenkins user

sudo -su jenkins

4\. Run the below command

gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

curl -sSL [https://get.rvm.io](https://get.rvm.io/) | bash -s stable --ruby

Next, we need to add rvm to our shell profile — in this case, we’ll add the following to the end of our ~/.bashrc:

\[\[ -s "$HOME/.rvm/scripts/rvm" \]\] && source "$HOME/.rvm/scripts/rvm"

Now re login to jenkins user and run the rvm command , it will work.

**Configure Jenkins Job**

1. Create a freestyle Project -> Under SCM -> Select Git ->[https://github.com/devops4solutions/CI\_Jenkins\_RubyonRails.git](https://github.com/devops4solutions/CI_Jenkins_RubyonRails.git)

![](https://cdn-images-1.medium.com/max/800/1*lVoILJCtktx89ww6VHhy4g.png)

2\. Build Environment -> Select Run the build in a RVM-managed environment -> Value is . (dot)

![](https://cdn-images-1.medium.com/max/800/1*nKwAu3X9M1tbYGMVP2D4cg.png)

3\. Add build step -> Execute Shell

Make sure your Gemfile has an enrty for RspecJunitformatter like this

gem ‘rspec’, :require => false, :group => :testgem ‘rspec\_junit\_formatter’, :require => false, :group => :test

bundle install
RAILS\_ENV=test bundle exec rake db:migrate
RAILS\_ENV=test bundle exec rspec — format RspecJunitFormatter — out results.xml

![](https://cdn-images-1.medium.com/max/800/1*G5Nv9iM41MQbnRmp-Bx45A.png)

4\. Add Post Build Action ->Publish Junit Test Result Report

![](https://cdn-images-1.medium.com/max/800/1*wdaJ0dTo_yHnvKVtYYEXDg.png)

5\. Build Results

![](https://cdn-images-1.medium.com/max/800/1*wBdgJ8LipkC9MsN8-kkbiQ.png)

![](https://cdn-images-1.medium.com/max/800/1*FJqLb4q2YzumP8KeK-gSyg.png)

6\. Check Test Results

![](https://cdn-images-1.medium.com/max/800/1*-cvmR-U4lrP3acuZRd_Yeg.png)

Unit Test Report

Finally congratulations!! You have successfully followed each and every step for setting up CI/CD of Ruby On Rails using Jenkins.

References:

1. [https://github.com/sj26/rspec\_junit\_formatter](https://github.com/sj26/rspec_junit_formatter)
