---
title: "CI/CD using Jenkins and Ansible"
date: "2020-08-15"
categories: 
  - "jenkins"
tags: 
  - "ci-cd"
  - "jenkins"
---

In this blog, we will explore the CI/CD process using Jenkins and Ansible. We will deploy the sample java application using Ansible on the tomcat servers. At last,this process will help us in achieving continuous integration and continuous deployment for your application.

If you want to see the video for this article, [click here](https://youtu.be/B1sjiq1wD_Y)

### **Prerequisite:**

1. Two AWS EC2 instance ( one is Jenkins Server and another is tomcat server)
2. Follow [Jenkins](https://devops4solutions.com/jenkins-installation-on-linux/) Installation article if not already installed
3. Integration between [Jenkins and Ansible](https://devops4solutions.com/integrate-ansible-with-jenkins/)
4. Make sure ssh connection is already setup between for jenkins user between two AWS EC2 Instances.

### **Agenda:**

1. Create a sample application in java
2. Compile the project using maven
3. Package the project in war file
4. Create a role in Ansible to install java and tomcat
5. Deploy the war file on tomcat server using ansible playbook
6. Push the code in the [github](https://github.com/devops4solutions/CI-usingAnsible)
7. Create a pipeline job in Jenkins and trigger the build

### **Sample Application in Java**

Firstly, create a sample registration and login page in [jsp](https://github.com/devops4solutions/CI-usingAnsible/tree/master/src/main/webapp). 

### **Compile and package the project using Maven**

Created a pom.xml file to compile and package the project in war file. Clone the git repo and run the mvn command, this will generate a war file under target folder

git clone [https://github.com/devops4solutions/CI-usingAnsible.git](https://github.com/devops4solutions/CI-usingAnsible.git)  
mvn package

Now we have generated the war file and we can deploy it on our tomcat server. Previously, we used to install tomcat and java manually on the server and then copy the war file under webapps folder and restart the service.

Now, we will write the ansible roles to do all the tasks and configure it to run inside a Jenkins job

### **Ansible Playbook**

In the git repo, we have created first `ansible.cfg` file with the below entries

\[defaults\]  
host\_key\_checking = False

This is required to bypass the hostkey checking while making a ssh connection with the tomcat servers. If you will not use this file, then when you run your playbook using jenkins it will fail with the error `Host key verification is falied`

Now we will create `main.yml` file which will call our roles to install tomcat and java

\- hosts: all  
  become: true  
  become\_user: root  
  gather\_facts: false  
  tasks:  
    - include\_role:  
        name: tomcat

I have following folder structure to manage ansible playbook for inventories and the variables as shown below

![](https://cdn-images-1.medium.com/max/800/1*FQDk2nvkFU_nFwgcSXKpAg.png)

[Role](https://github.com/devops4solutions/CI-usingAnsible/blob/master/roles/tomcat/tasks/main.yml) to install java, configure tomcat and deploy the war file on the tomcat server

\- name: Install Java 1.7
  yum: name=java-1.7.0-openjdk state=present

- name: add group "tomcat"
group: name={{tomcat\_group}}

- name: add user "tomcat"
user: name={{tomcat\_user}} group={{tomcat\_group}} home=/usr/share/tomcat createhome=no
become: True
become\_method: sudo

- name: Download Tomcat
get\_url: url=[http://archive.apache.org/dist/tomcat/tomcat-7/v7.0.61/bin/apache-tomcat-7.0.61.tar.gz](http://archive.apache.org/dist/tomcat/tomcat-7/v7.0.61/bin/apache-tomcat-7.0.61.tar.gz) dest=/opt/apache-tomcat-7.0.61.tar.gz

- name: Extract archive
command: chdir=/usr/share /bin/tar xvf /opt/apache-tomcat-7.0.61.tar.gz -C /opt/ creates=/opt/apache-tomcat-7.0.61

- name: Symlink install directory
file: src=/opt/apache-tomcat-7.0.61 path=/usr/share/tomcat state=link
- name: Change ownership of Tomcat installation
file: path=/usr/share/tomcat/ owner=tomcat group=tomcat state=directory recurse=yes

- name: Configure Tomcat server
template: src=server.xml dest=/usr/share/tomcat/conf/
- name: Create sample directory
file:
path: "/opt/apache-tomcat-7.0.61/webapps/samples"
state: directory
mode: 0777
become: true
- name: copy war file
copy: src=./target/LoginWebApp-1.war dest=/opt/apache-tomcat-7.0.61/webapps/
notify: restart tomcat
- name: Install Tomcat init script
copy: src=tomcat-initscript.sh dest=/etc/init.d/tomcat mode=0755
- name: Start Tomcat
service: name=tomcat state=started enabled=yes
- name: wait for tomcat to start
wait\_for: port={{http\_port}}

Also,you can check more [articles](https://devops4solutions.com/category/ansible/) on how to manage ansible playbook/inventories/multiple environments

### **Jenkins Pipeline**

Now we will write [Jenkinsfile](https://github.com/devops4solutions/CI-usingAnsible/blob/master/Jenkinsfile) to run all the tasks together as mentioned above

- Configure agent
- Configure tool [maven](https://devops4solutions.com/jenkins-maven-integration/) and ansible which you have configured in your Jenkins. Name should match as per your jenkins configuration
- Checkout the code, its not required if you directly take the Jenkinsfile from SCM.
- Set the environment path
- Execute the `mvn package`
- Lastly, run the ansible playbook

pipeline {
    agent any
    
    tools
    {
       maven "Maven"
    }
     
    stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: '[https://github.com/devops4solutions/CI-example.git'](https://github.com/devops4solutions/CI-example.git%27)
             
          }
        }
         stage('Tools Init') {
            steps {
                script {
                    echo "PATH = ${PATH}"
                    echo "M2\_HOME = ${M2\_HOME}"
               def tfHome = tool name: 'Ansible'
                env.PATH = "${tfHome}:${env.PATH}"
                 sh 'ansible --version'
                    
            }
            }
        }
     
        
         stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        
     stage('Ansible Deploy') {
             
            steps {
                 
           sh "ansible-playbook main.yml -i inventories/dev/hosts -- user jenkins --key-file ~/.ssh/id\_rsa"
}
}
}
}

### **Configure a Jenkins Pipeline project**

- Create a pipeline job in Jenkins

![](https://cdn-images-1.medium.com/max/800/1*XPVts4hlalaMEEuXBpQ35A.png)

- Configure gitrepo. I have used ssh url because my jenkins is already [integrated with github](https://devops4solutions.com/setup-ssh-between-jenkins-and-github/)

![](https://cdn-images-1.medium.com/max/800/1*V1HvyEK_FzWHu-vC1nkc_g.png)

![](https://cdn-images-1.medium.com/max/800/1*foumIIwE3Qy668aucy6RXw.png)

Click on Save and trigger the pipeline

![](https://cdn-images-1.medium.com/max/800/1*ybk52pdpxV-SA_ZxP_NX-g.png)

![](https://cdn-images-1.medium.com/max/800/1*C_YRrn7XQdC7uMYK1nEXdg.png)

![](https://cdn-images-1.medium.com/max/800/1*cmqzsk1kxuJH25CSy6AJzQ.png)

Now, you can browse your application by using the url [http://yourip:8080/LoginWebApp-1/](http://18.222.127.218:8080/LoginWebApp-1/) and you will see the login page as shown below:

![](https://devops4solutions.com/wp-content/uploads/2020/08/image-3.png)

Congratulations,you have successfully setup the CI/CD process using Jenkins and Ansible. You can also integrate your job with [sonarqube](https://devops4solutions.com/jenkins-sonarqube-integration/) to check the code quality and code coverage.
