---
title: "Terraform installation on ec2 ubuntu instance"
date: "2018-09-14"
categories: 
  - "terraform"
---

Terraform has emerged as a revolution in automation sphere. Earlier talk of the town was to automate the CI/CD workflow through scripts. Now, the new hot topic is to automate the infrastructure building process. What is terraform? How it is helpful? Why it is called cloud agnostic? All these questions can be easily addressed via following [https://www.terraform.io/](https://www.terraform.io/) . In this blog we will follow the facile steps for Terraform installation on ec2 ubuntu instance. Also, we will install and configure AWS CLI, in order to provide access and secret key (AWS IAM user credentials) which is a pre-requisite to do any kind of automation.

Our story line would be:

(i). Installing Terraform on ubuntu.

(ii). Installing & configuring AWS CLI.

(iii). Writing a terraform file (.tf) to :

- Create a ec2 instance within default VPC.
- Specifying the attached security group.
- Deploying a simple web server on that instance.
- Varaible declaration to satisfy DRY (Don’t repeat yourself) principle.

Let’s cover the first two points of this story line.

### Terraform installation on ec2 Ubuntu instance:

Step 1. Install unzip:

sudo apt-get install unzip

Step 2. Download the latest version of Terraform:

```
wget https://releases.hashicorp.com/terraform/0.11.8/terraform_0.11.8_linux_amd64.zip
```

Step 3. Extract the file archive:

```
unzip terraform_0.11.8_linux_amd64.zip
```

Step 4. Move the executables to /usr/local/bin/

```
sudo mv terraform /usr/local/bin/
```

Step 5. Check the version installed:

```
terraform --version
```

![](https://cdn-images-1.medium.com/max/1000/1*vfp3xEb2PFDCOOLAPGkdEQ.png)

Since the folder /usr/local/bin is already set to PATH environment variable, we don’t need to set it again.

#### **Installing AWS CLI:**

sudo apt-get update

sudo apt-get install python pip

pip install awscli

aws version

![](https://cdn-images-1.medium.com/max/1000/1*ZsVu503QOl8Q851uOU1w7g.png)

#### **Configuring AWS CLI:**

aws configure

![](https://cdn-images-1.medium.com/max/1000/1*RbtMyxdmA0qXo3ajBR-o-Q.png)

It will prompt you to provide AWS Access Key ID and AWS Secret Access key, default region and default output format.

Provide the details and you are good to proceed to start your infrastructure automation.

Congratulations!! Finally you have installed terraform and AWS CLI.

To proceed towards the next half of the story (writing .tf file), click [here](https://devops4solutions.com/deploying-ec2-instance-with-terraform/).
