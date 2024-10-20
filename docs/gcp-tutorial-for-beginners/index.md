---
title: "GCP Tutorial for Beginners"
date: "2020-11-23"
categories: 
  - "google-cloud"
tags: 
  - "gcp"
coverImage: "GCP.png"
---

In this blog, we will explore how to create an account in GCP and learn creating resources like VPC, subnets, firewalls.

Check the [YouTube](https://youtu.be/FqM2i3XeVls) video for this article

### **Setup a GCP account** 

1. Go to [https://cloud.google.com/](https://cloud.google.com/).
2. Click the “Try Free” button
3. Sign up and follow the prompts
4. GCP will give $300 as a free credit to use for this example

### Creating a GCP project

Let’s create a GCP project → navigate as per the screen shown below and click on the dropdown

![](https://cdn-images-1.medium.com/max/880/1*Uxv0IvzEv-6ACx1pK9ffgg.png)

Here, you will see all the projects which are already present and an option to create a new project

![](https://cdn-images-1.medium.com/max/880/1*r-mwKypSbW1q9S-Esl7Kcw.png)

It will also provide you the information how much quota is available for you.

![](https://cdn-images-1.medium.com/max/880/1*iRnu9GhXXcAXYVGq-LMBMg.png)

Then GCP will generate and assign a project ID ,this projectid is unique across all users.

![](https://cdn-images-1.medium.com/max/880/1*mm2Lm32ErOq3GjPVkaov0Q.png)

This is how your dashboard should look like:

![](https://cdn-images-1.medium.com/max/880/1*fSVtSbj2jfY0Wt0AFbSe6g.png)

Now we will install cloud SDK to create any resources in GCP. It is recommended that we use gcloud command line utility to create any resources.

### Installing Cloud SDK

It supports the Windows, Mac, and Linux platforms. All of these require a Python interpreter version 2.7

1. Download from [here](https://dl.google.com/dl/cloudsdk/channels/rapid/GoogleCloudSDKInstaller.exe)
2. Run the Google cloud SDK exe file
3. Proceed with the installation with the default options.

![](https://cdn-images-1.medium.com/max/880/1*Ymu4nnMgD7ExCJtvaxIx3Q.png)

![](https://cdn-images-1.medium.com/max/880/1*RrVKVViR4Rlxtcnp9FfkUQ.png)

After installation is complete, it will open a command prompt to do the initial configuration. It will open the browser and ask you to login to your google cloud account and setup a default project

![](https://cdn-images-1.medium.com/max/880/1*lkTnskySPcAdzeHN-Zj65g.png)

![](https://cdn-images-1.medium.com/max/880/1*UpseHNhYkyaEIGOT3ulF2w.png)

![](https://cdn-images-1.medium.com/max/880/1*eVV9N-t_9UZ6h2d57LcoKg.png)

Finally, cloud SDK is configured for your newly created project. Now you can create any resources using google cloud sdk.

_If you chose the wrong project or you want to try again, at any time you can reconfigure your setup by using the below command_

gcloud init

### **Setup infrastructure on GCP:**

Now we will setup the basic infrastructure on GCP as shown below:

![](https://cdn-images-1.medium.com/max/880/1*mH4kMVGcyKe6f9HDMrylkQ.png)

1. VPC `test-vpc`
2. Two subnets `subnet-us-central1, subnet-us-east1`
3. Three firewall rules `public-ssh, public-http, private-ssh`
4. Add your ssh public key to a project-wide metadata
5. Two Virtual Machines `vm-public, vm-private`

#### **Creating a VPC**

VPC concept is same as we have in AWS**,** but there’s no need to bind a particular region, and also no need to set the CIDR address range.

Now we will use `gcloud compute network` to create a new VPC

- VPC Name `vpc-test` 
- Subnet Mode `custom`

subnet-mode=custom ( This means that it will not create the subnet automatically and we will add the custom subnet manually)

gcloud compute networks create vpc-test --subnet-mode=custom

You will get the prompt if api is not enabled for this service.

Run the below command to check the vpc list

gcloud compute networks list

![](https://cdn-images-1.medium.com/max/880/1*RoOBhje8fY4cHq5VaeK3ew.png)

#### **Create Subnet**

Let’s create two subnets inside our vpc `test-vpc` 

1. In order to create a subnet, you have to choose the region. Run the below command to check how many regions are available

gcloud compute regions list

![](https://cdn-images-1.medium.com/max/880/1*5F8gz-UyfdUM2KwmWhx4Kw.png)

We will create the subnet configuration as shown below:

![](https://cdn-images-1.medium.com/max/880/1*DDABijwye5u93TPOE998Wg.png)

gcloud compute networks subnets create subnet-us-central1 --network=vpc-test --range=192.168.1.0/24 --region us-central1

gcloud compute networks subnets create subnet-us-east1 --network=vpc-test --range=192.168.2.0/24 --region us-east1

![](https://cdn-images-1.medium.com/max/880/1*KStYe-sV8zWJajsCOd701g.png)

Run the below command to check if subnets are configured properly

gcloud compute networks subnets list --network=vpc-test

![](https://cdn-images-1.medium.com/max/880/1*wFOfuj2pg2o7G8FSjXlYUA.png)

#### **Creating Firewall Rules**

Firewall Rules are same as the security group in AWS where you define incoming and outgoing rules.

They use a network tag for all the rules and we specify the tags in the VM’s. VM’s can have multiple tags applied on them

![](https://cdn-images-1.medium.com/max/880/1*T_-1tGMaByDVer18fgq58A.png)

1. Create a public-ssh rule

- Name of the firewall rules `public-ssh` 
- VPC Name `vpc-test` 
- Target tags `public` 
- source-ranges `0.0.0.0/0` 
- allow `tcp:22`

gcloud compute firewall-rules create public-ssh --network=vpc-test --allow="tcp:22" --source-ranges="0.0.0.0/0" --target-tags="public"

2\. Create a public-http rule

- Name of the firewall rules `public-http`
- VPC Name `vpc-test`
- Target tags `public`
- source-ranges `0.0.0.0/0`
- allow `tcp:80`

gcloud compute firewall-rules create public-http --network=vpc-test --allow="tcp:80" --source-ranges="0.0.0.0/0" --target-tags="public"

3\. Create a private-ssh rule

- Name of the firewall rules `private-ssh`
- VPC Name `vpc-test`
- Target tags `private`
- source-ranges `0.0.0.0/0`
- allow `tcp:80`

gcloud compute firewall-rules create private-ssh --network=vpc-test --allow="tcp:22" --source-tags="public" --target-tags="private"

![](https://cdn-images-1.medium.com/max/880/1*TW75bQIAUOFNgryi6gNOpA.png)

4\. Check all firewall rules

gcloud compute firewall-rules list

![](https://cdn-images-1.medium.com/max/880/1*pEYiEp5BWi1Acg-Drng5sw.png)

#### **Add your ssh public key to GCP**

Before you launch VM instances, you need to upload your ssh public key in order to log on to the VM

If you do not have ssh-key generated for your user then run the below command using git bash on windows

ssh-keygen

Now we will run the below command to add the ssh keys. This is required to run for every new project

eval \`ssh-agent -s\`   
ssh-add ~/.ssh/id\_rsa

![](https://cdn-images-1.medium.com/max/880/1*-PThgzRlyKhnU2xW-QVUmA.png)

Now run the below command and you can see public key is added in your gcloud project

gcloud compute config-ssh --ssh-key-file=~/.ssh/id\_rsa  
gcloud compute project-info describe --format=json

![](https://cdn-images-1.medium.com/max/880/1*38-Vyx9Bs0Dn4IvVOjWu_w.png)

#### Deploy Virtual Machines

We will use below values to create the VM’s

- Name of the VM `vm-public` 
- VPC `vpc-test` 
- Subnet `subnet-us-central1` 
- Zone `us-central1-a` 
- tags `public`
- machine-type `f1-micro`

gcloud compute instances create vm-public --network=vpc-test --subnet=subnet-us-central1 --zone=us-central1-a --tags=public --machine-type=f1-micro

gcloud compute instances create vm-private --network=vpc-test --subnet=subnet-us-east1 --zone=us-east1-b --tags=private --machine-type=f1-micro

gcloud compute instances list

![](https://cdn-images-1.medium.com/max/880/1*j3iIdcA-sXcB12z9-smhow.png)

#### **Connect to VM’s**

1. **Public VM** — ssh from your machine to vm-public using the -A option (forward authentication) and using an external IP address:

ssh -A khand@34.122.211.163

![](https://cdn-images-1.medium.com/max/880/1*lnKeQUviXTsCRlhcjsvKJg.png)

1. **Private VM** — ssh from vm-public to vm-private via the internal IP address:

![](https://cdn-images-1.medium.com/max/880/1*UOiuFTGA8_wAIo9eq4r5KA.png)

#### **Test the installation**

Install nginx on public vm

sudo apt-get -y install nginx  
sudo systemctl start nginx

Access it using external IP on the browser

![](https://cdn-images-1.medium.com/max/880/1*XgJsfrIbgnNXDtUWAPrKIw.png)

Congratulations, you have successfully explore GCP and create the basic infrastructure using gcloud commands.
