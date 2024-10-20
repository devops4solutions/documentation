---
title: "Cloud Custodian Installation"
date: "2018-06-15"
categories: 
  - "devops-tools"
---

 

Cloud Custodian is an open source framework which is used to monitor your AWS infrastructure and run the policies which are written in yaml against your whole AWS account/regions. The policies is like a rule which is put in the yaml file to find out if something is compliant or not.

We create so many resources on AWS, we need some kind of mechanism to find out for any new or existing violations.

For ex —

1. I need to see if my whole EC2 instances are properly tagged or not.
2. All RDS instances should not be publicly available
3. If any EC2 instance is idle for 10 days then terminate it.
4. Find out security group which are open to the world

Basic Concepts:

1. Policy — `[**c7n.policy**](http://capitalone.github.io/cloud-custodian/docs/generated/aws/c7n.html#module-c7n.policy "c7n.policy")`Defined in yaml, specifies a set of filters and actions to take on a given AWS resource type.
2. Resource — Any resource on AWS which you want to monitor
3. Mode — `[**c7n.policy**](http://capitalone.github.io/cloud-custodian/docs/generated/aws/c7n.html#module-c7n.policy "c7n.policy")` (yes, `policy`)Defines how the policy will execute (lambda, config rule, poll, etc). Policies run in `pull` mode by default.Modes are responding to cloudwatch events and that can be the reason of running any policy locally or manually.If you want to pull all resources of a specific type based on your filter, you can remove the mode section to run it manually.

**TIPS:**

Generally, think of the **mode**: cloudtrail policies as real-time remediation policies, if you have less urgent/more resource intensive policies you should run those on a cron outside of lambda

**Installation**

Prerequisite:

Python

Check which python version is installed, if not then install python first

$ virtualenv --python=python2.7 custodian
$ source custodian/bin/activate
(custodian) $ pip install c7n

Installation of cloud custodian is completed successfully.

**Configure AWS CLI ( [Click here](https://devops4solutions.com/configure-aws-cli/) for more detail)**

You need python-pip on that machine to install awscli

yum install [https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm](https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm)

sudo yum install python-pip

```
sudo pip install awscli --upgrade pip
```

Run the below command after logging to some user, for eg -ec2-user not as a root user

```
aws configure
```

Once you configured your keys using aws configure, everything is stored under

cd ~/.aws/
pwd
/home/ec2-user/.aws
cat credentials

### Write your first policy:

Create a file custodian.yml

vi custodian.yml

Put the below content in your file. This will fetch all the instances which are in running state.

policies:
  - name: my-first-policy
    resource: ec2
    filters:
      - "State.Name": running

Validate your policy

custodian validate custodian.yml

Dry run : This will show you what your policy will do before actually running it

custodian run --dryrun -s . custodian.yml

Now run your yaml file

custodian run --output-dir=. custodian.yml

![](https://cdn-images-1.medium.com/max/800/1*hVlgEyj2VRRcW3sEc-iN_g.png)

Now we will modify the policy and rerun the same command and you will see in output that count is 0.

policies:
  - name: my-first-policy
    resource: ec2
    filters:
      - "State.Name": running
      - "tag:Custodian":present

![](https://cdn-images-1.medium.com/max/800/1*Wsc8hf7lldt-x7A1wNswTA.png)

This is how we can write the rules as per your need.
