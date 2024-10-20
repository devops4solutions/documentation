---
title: "Integrate Terraform with Jenkins"
date: "2018-09-26"
categories: 
  - "jenkins"
  - "terraform"
---

In this blog, we will Integrate Terraform with Jenkins

First you need to install plugin

Go to Manage Jenkins — Install Plugin — Terraform

Now configure Terraform

Manage Jenkins ->Global Tool Configuration

![](https://cdn-images-1.medium.com/max/800/1*qbordlRBEErODAO03QaroA.png)

To store AWS Secret Key

Manage Jenkins -> Configure System -> Set environment variable

![](https://cdn-images-1.medium.com/max/800/1*S2hxMNryDitoaa5spja0Xw.png)

Now create a pipeline job

pipeline {
 agent any
 
 stages {
 stage(‘checkout’) {
 steps {
 git branch: ‘develop’, url: ‘[git@](mailto:git@git.kpd-i.com)your url’
 
 }
 }
 stage(‘Set Terraform path’) {
 steps {
 script {
 def tfHome = tool name: ‘Terraform’
 env.PATH = “${tfHome}:${env.PATH}”
 }
 sh ‘terraform — version’
 
 
 }
 }
 
 stage(‘Provision infrastructure’) {
 
 steps {
 dir(‘dev’)
 {
 sh ‘terraform init’
 sh ‘terraform plan -out=plan’
 // sh ‘terraform destroy -auto-approve’
 sh ‘terraform apply plan’
 }
 
 
 }
 }
 
 
 
 }
}

![](https://cdn-images-1.medium.com/max/800/1*VQMec-Uvjuq4_5qV493-Uw.png)
