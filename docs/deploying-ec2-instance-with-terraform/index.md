---
title: "Deploying ec2 instance with Terraform"
date: "2018-09-14"
categories: 
  - "terraform"
---

Terraform is cloud agnostic, that means it can be use to automate the infrastructure building process for different cloud providers including public (like: AWS, Azure, DigitalOcean, Google Cloud) and private cloud and virtualization platforms (like: OpenStack and VMWare). With this blog we will learn deploying ec2 instance with Terraform and we will also deploy a simple web server.

So let’s start writing your .tf file:

Terraform code can be written in any text editor and save the file with “.tf” extension. Terraform code is written in HCL (HashiCorp COnfiguration Language), which is a declarative language. In the .tf file you need to write script (which is very easy and readable) specifying the infrastructure you wish to create, and Terraform will deduce how to create it.

Pre-requisite:

1.  AWS account.
2. EC2 Ubuntu instance.
3. [Terraform installed](https://devops4solutions.com/terraform-installation/).
4. [AWS CLI installed & configured](https://devops4solutions.com/terraform-installation/).

Step 1. Putty/SSH into your ec2 Ubuntu instance. (In order to learn how to create one, click [here](https://devops4solutions.com/aws-ec2-linux-instance-launch/)).

Step 2. Create a folder (e.g. terraform-projects) and create a file (e.g. main.tf).

$ mkdir terraform-projects

$ cd terraform-projects

$ touch main.tf

Step 3. Open the main.tf with a text editor (e.g. vi)

$ vi main.tf

Step 4. Write the following script in main.tf and let’s understand it block by block.

resource “aws\_security\_group” “websg” {
 name =”terraform-webserver-websg”

ingress{
 from\_port = “${var.server\_port}”
 to\_port = “${var.server\_port}”
 protocol =”tcp”
 cidr\_blocks = \[“0.0.0.0/0”\]
 }
}

resource “aws\_instance” “webserver”{
 ami = “ami-40d28157”
 instance\_type = “t2.micro”
 vpc\_security\_group\_ids = \[“${aws\_security\_group.websg.id}”\]

tags{
 Name = “terraform-first-example”
 }

user\_data = <<-EOF
 #!/bin/bash
 echo “Hello,World” > index.html
 nohup busybox httpd -f -p “${var.server\_port}” &
 EOF
}

variable “server\_port” {
 description = “The port the server will use for HTTP requests”

default = 8080
}

#### Script Interpretation:

\[I\]. Input Variable

variable “server\_port” {
 description = “The port the server will use for HTTP requests”
 default = 8080
}

In order to implement DRY (Don’t Repeat Yourself) principle, that is rather than repeating same data again and again at different places, it is the best practice to declare a variable for the same. This makes it easier and unambiguous. Also, when an update is required, you need to do modification only at the source (i.e. in variable definition).

In Terraform, one can define input variable with the syntax:

variable “Variable\_Name” {
 description = “example of list type”
 type = "list"
 default = \[1,2,3\]
}

variable “Variable\_Name” {
 description = “example of map type”
 type = "map"

default = {
    key1 = "value1"
    key2 = "value2"
}
}

\*Note: If no type is mentioned then terraform will assume the type that resembles the default value type. However, if default value is also not specified then String type is considered.

\*In our case Variable Name is : server\_port and default value is 8080.

\[II\]. Security Group creation

As, shortly we will create an EC2 instance and deploy a web server on it. AWS on its own will not allow incoming or outgoing traffic from its EC2 instance. It has to be within a security group which defines the incoming and outgoing port range.

In our case we want to allow our EC2 instance to receive traffic from port number 8080. Following is the script to do so.

resource “aws\_security\_group” “websg” {
 name =”terraform-webserver-websg”

ingress{
 from\_port = “${var.server\_port}”
 to\_port = “${var.server\_port}”
 protocol =”tcp”
 cidr\_blocks = \[“0.0.0.0/0”\]
 }
}

**“aws\_security\_group”** : type of AWS resource

**“websg”** : name by which terraform will recognize this resource

**“terraform-webserver-websg”** : name of the security group that will be reflected on your AWS console.

\*Note: Rather then using value 8080, we have used interpolation : “${var.server\_port}” , this is a way to extract value of variable server\_port defined earlier.

\[III\]. AWS instance creation

resource “aws\_instance” “webserver”{
 ami = “ami-40d28157”
 instance\_type = “t2.micro”
 vpc\_security\_group\_ids = \[“${aws\_security\_group.websg.id}”\]

tags{
 Name = “terraform-first-example”
 }

**“aws\_instance”** : type of resource

**“webserver”** : name of the resource identified by terraform

**“terraform-first-example”** : name/tag reflected on AWS console

**ami** : type of amazon machine image

**instance\_type** provided by AWS

**vpc\_security\_group\_ids** : As we have created security group earlier, but we need to tell ec2 instance to use this security specifications. In order to do it, we have to pass the value of _ID attribute of security group_ into the _vpc\_security\_group\_ids_ parameter of created _aws\_instance_.

To fetch the security group id, use the following interpolation syntax:

“${aws\_security\_group.websg.id}”

\[IV\]. Deploying a single web server

user\_data = <<-EOF
 #!/bin/bash
 echo “Hello,World” > index.html
 nohup busybox httpd -f -p “${var.server\_port}” &
 EOF
}

This is a simple bash script which is writing “Hello, World” into index.html. It runs a busybox (which is by default installed on Ubuntu) tool , in order to fire up a web server on port 8080 (as defined by interpolation syntax using server\_port variable). It is enclosed between nohup and & , to run web server permanently in the background.

\*Note: <<-EOF and EOF are Terraform’s heredoc syntax, allows multiline string creation without inserting newline characters.

Step 5. Run the following commands within the project

$ terraform init

$ terraform plan

$ terraform apply

![](https://cdn-images-1.medium.com/max/1000/1*ZshuIUf5U__k9f30S-ZbZg.png)

Your infrastructure is build, you can check on your AWS console. Also use, the IP address of your created EC2 instance and on web browser type : <ip\_address>:8080

You can see

![](https://cdn-images-1.medium.com/max/1000/1*gxbWlitgtbag4Vajn6qMdg.png)
