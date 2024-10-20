---
title: "JenkinsPipeline for Terraform deployment"
date: "2018-06-14"
categories: 
  - "devops-tools"
---

This blog will help you to use the JenkinsPipeline for Terraform deployment

## Tools Used:

1. Jenkins
2. Github
3. Terraform

## JenkinsPipeline for Terraform deployment

pipeline {
    agent any
     
    stages {
      stage('checkout') {
          steps {
                git url: 'git@giturl'
             
          }
        }

  stage('Set Terraform path') {
            steps {
                script {
                    def tfHome = tool name: 'Terraform'
                    env.PATH = "${tfHome}:${env.PATH}"
                }
                sh 'terraform --version'
               
               
            }
        }
        
         stage('Provision infrastructure') {
             
            steps {
                sh 'terraform init'
                sh 'terraform plan -out=plan'
                sh 'terraform apply plan'
              
             
            }
        }
        
      
      
    }
}
