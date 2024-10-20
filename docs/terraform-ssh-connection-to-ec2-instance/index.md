---
title: "Terraform-SSH connection to EC2 instance"
date: "2018-06-14"
categories: 
  - "devops-tools"
  - "terraform"
---

This blog will help you if you need to enable the ssh connection to EC2 instance.

Path to the SSH public key to be used for authentication. Ensure this keypair is added to your local SSH agent so provisioners can connect.

ssh-keygen (Generate keys if not exist already)

### Adding your SSH key to the ssh-agent

Ensure ssh-agent is enabled:

### start the ssh-agent in the background

`eval "$(ssh-agent -s)"`

_Agent pid 59566_

#### Add your SSH key to the ssh-agent. If you used an existing SSH key rather than generating a new SSH key, you’ll need to replace id\_rsa in the command with the name of your existing private key file.

`$ ssh-add ~/.ssh/id_rsa`

**How to make ssh connection to host**

variable public\_subnet\_id {}
variable private\_subnet\_id {}
variable FrontEnd\_SG\_id {}
variable Database\_SG\_id {}
variable vpc\_id {}
resource "aws\_key\_pair" "auth" {
  key\_name   = "${var.key\_name}"
  public\_key = "${file(var.public\_key\_path)}"
}
resource "aws\_instance" "webserver" {
  ami           = "${lookup(var.ami, var.region)}"
 connection {
    # The default username for our AMI
    user = "ec2-user"
host = "${aws\_instance.webserver.public\_ip}"
# The connection will use the local SSH agent for authentication.
}instance\_type = "t2.micro"
associate\_public\_ip\_address = "true"
subnet\_id = "${var.public\_subnet\_id}"
vpc\_security\_group\_ids = \["${var.FrontEnd\_SG\_id}"\]
key\_name = "${aws\_key\_pair.auth.id}"
tags {
Name = "webserver"
}
}

 

SSH connection to EC2 Instance is completed successfully.
