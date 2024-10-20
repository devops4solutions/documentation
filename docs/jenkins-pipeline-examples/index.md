---
title: "Jenkins Pipeline Examples"
date: "2018-06-13"
categories: 
  - "jenkins"
---

# Jenkins Pipeline

Some examples for Jenkins Pipeline script . Jenkins Pipeline is used to create stages/pipeline of all the steps which are required for building the project.

## Examples:

### **How to initialize software and set path**

stage(‘Terraform Init’) { steps { script { def tfHome = tool name: ‘Terraform’ def anHome = tool name: ‘Ansible’ env.PATH = “${anHome}:${env.PATH}” env.PATH = “${tfHome}:${env.PATH}” } sh ‘terraform — version’ sh ‘ansible — version’

} }

### **How to declare variable**

def my\_list

pipeline { agent any stages { stage(‘stage1’) { steps { script { my\_list = “nidhi” } } }

stage(‘stage2’) { steps { echo “$my\_list”

} } } }

Second Example def test=”nik”

pipeline { agent any stages {

stage(‘stage2’) { steps { echo “$test”

} } } }

Example of pipeline which will use variables/checkout/dockerpush

def source=”/var/jenkins\_home/workspace/mvv-web-pipeline1/MVVWeb/build/libs/.jar” def destination=”MVVWeb/docker/” def DTR=”dtest” def DOCKER\_NAME=”mvv/mvv-web” pipeline { agent any stages { stage(“Checkout”) { steps { git url: ‘ssh://[git@bitbucket.sandata.com](mailto:git@bitbucket.sandata.com):7999/ohio/automation\_scripts.git’ } } stage(“Compile”) { steps { dir (‘MVVWeb’) { sh “./gradlew build” } } } stage(“Copy”) { steps {

sh “cp /var/jenkins\_home/workspace/mvv-web-pipeline1/MVVWeb/build/libs/\*.jar /var/jenkins\_home/workspace/mvv-web-pipeline1/MVVWeb/docker” }

} stage(“Docker build”) { steps { dir(‘MVVWeb/docker’) { sh “docker build -t ${DTR}/${DOCKER\_NAME}:v$BUILD\_ID .” } } }

stage(“Docker push”) { steps { sh “docker login ${DTR} — username=peter — password=mypass” sh “docker push ${DTR}/${DOCKER\_NAME}:v$BUILD\_ID”

}

} } }

// build with parameters

pipeline {
    agent any
    parameters {
        choice(
            // choices are a string of newline separated values
            // [https://issues.jenkins-ci.org/browse/JENKINS-41180](https://issues.jenkins-ci.org/browse/JENKINS-41180)
            choices: 'create\\nupdate',
            description: '',
            name: 'REQUESTED\_ACTION')
            
             choice(
            // choices are a string of newline separated values
            // [https://issues.jenkins-ci.org/browse/JENKINS-41180](https://issues.jenkins-ci.org/browse/JENKINS-41180)
            choices: '100\\n50',
            description: '',
            name: 'percentage')
    }

stages {
        stage ('Create') {
            when {
                // Only say hello if a "greeting" is requested
                expression { params.REQUESTED\_ACTION == 'create' }
            }
            steps {
                echo "Hello, create!"
            }
        }
        
        stage ('Percentage') {
            when {
                // Only say hello if a "greeting" is requested
                expression { params.percentage == '100' }
            }
            steps {
                echo "Hello, 100!"
            }
        }
    }
}

drop down in jenkinspipeline

pipeline {
    agent none

stages {
        stage ('First') {
            agent any
            steps {
                echo "First dummy stage"
            }
        }
        stage ('Input') {
          
            steps {
                script {
                    myStage = input message: 'What service type do you want to run now?', parameters: \[choice(choices: 'Create\\nUpdate', description: '', name: 'Stage')\]
                }
                echo myStage
            }
        }

stage('Stage1') {
            when {
                expression { myStage == 'Create' }
            }
            steps {
                echo "Running Service: Create"
            }
        }

stage('Stage2') {
            when {
                expression { myStage == 'Update' }
            }
            steps {
                echo "Running Service:Update"
            }
        }

}
}

def userInput = input( id: ‘userInput’, message: ‘Let\\’s promote?’, parameters: \[ \[$class: ‘TextParameterDefinition’, defaultValue: ‘uat’, description: ‘Environment’, name: ‘env’\], \[$class: ‘TextParameterDefinition’, defaultValue: ‘uat1’, description: ‘Target’, name: ‘target’\] \]) pipeline { agent any stages {

stage (‘promotion’) { steps{ echo (“Env: “+userInput\[‘env’\]) echo (“Target: “+userInput\[‘target’\])

} } } }
