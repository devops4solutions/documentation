---
title: "Ansible vault password in CI/CD pipeline"
date: "2020-06-28"
categories: 
  - "ansible"
tags: 
  - "ansible"
  - "ansible-vault"
  - "jenkinsfile"
---

In this blog, we will explore how we can use the [ansible vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html#ansible-vault) to encrypt the password in CI/CD pipeline

If you want to see the video for this article, [click here](https://youtu.be/Ry3rCje93zA)

### **Problem Statement:**

- We need to use the database password to get connected to the database from the ansible playbook. How we can encrypt the password and store in git repository.

### **Solution**:

Here, we will use the concept of ansible vault in your playbook

1. Let’s create a yaml file using ansible-vault command

ansible-vault create dev\_vault.yml  

2\. This will prompt you to provide a password for the vault

3\. A file will open — insert the below entry in the file 

db\_password=password

4\. dev\_vault.yml will get created as shown below

$ANSIBLE\_VAULT;1.1;AES256  
66663639343966663237373666396135303237396639616538313461613366326561393339636530  
6363326564336364636163623464353466656532623039660a636537613434383465373337643534  
39643162373436373536353061366137313530656535656132383964316163346231326461323166  
6166623837653839320a336330366464653865306238643433303936346361393833346237656161  
39623539333232616164393933356236623734643238393131306237316463366437

5\. Now you can push this file in your git repo

### **How to use in ansible playbook**

- We have created a variable “db\_password” in vault. You can use this variable in your playbook like this in your template file or wherever you are using in your playbook

jdbc://{{db\_user}}{{db\_password}}

### **How to run the playbook**

- You have to provide the vault password and the path of the file where password is stored as shown below:

ansible-playbook main.yml `--ask-vault-pass -e '@configs/dev-vault.yml'`

- When you run this playbook it will prompt for the vault password and decrypt the password automatically and place it wherever the variable is defined.

This is good when you are running the playbook manually as it prompt for the password and you will provide it but how you can use it in your automation script ?

### **How to automate this ?**

There are only two option to provide the vault password either prompt or use a file.There is no other option to pass the variable for the vault password.

 1. — ask-vault-password

2. — vault-password-file

### **How to use a -vault-password-file option** ?

For using a file option from Jenkins, you can create a credential in Jenkins and use that variable in you Jenkinsfile.

- Create a credential in Jenkins named as “VAULT\_ID”
- Create a variable “FILE”
- echo your password in your text file

VAULT\_CREDS=  credentials("${VAULT\_ID}")  
FILE = 'secret.txt'

```
sh "echo '${
```

Now how will you call this in playbook

ansible-playbook main.yml ` --vault-password-file secret.txt -e '@configs/dev-vault.yml'`

Delete the secret.txt file after using it 

post {  
                always {  
                    //check for secret.txt and remove if it exists  
                    sh '''#!/bin/sh  
                        if \[ -f $FILE \] ; then  
                        rm $FILE  
                        fi  
                    '''  
                }
