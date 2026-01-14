#!/usr/bin.env groovy
library identifier: 'jenkins-shared-library@main', retriever: modernSCM(
    [$class: 'GitSCMSource',
    remote:'https://github.com/Prashzan/jenkins-shared-library.git',
    credentialsId: 'github-credentials']
)

pipeline {   
    agent any
    tools {
        maven 'Maven'
    }
    environment {
        //image on docker hub
        IMAGE_NAME = 'prashzan/demo-app:java-maven-1.0'
    }
    stages {
        stage('build app') {
            steps {
                echo 'building application jar...'
                buildJar()
            }
        }
        stage('build image') {
            steps {
                script {
                    echo 'building the docker image...'
                    buildImage(env.IMAGE_NAME)
                    dockerLogin()
                    dockerPush(env.IMAGE_NAME)
                }
            }
        } 


        stage("deploy") {
            steps {
                script {
                    echo 'deploying docker image to ec2...'
                    def dockerCmd = "docker run -p 3080:3080 -d ${IMAGE_NAME}"
                    sshagent(['ec2-instance-key']){
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@ec2-IP-address ${dockerCmd}"
                    }
                }
            }
        }               
    }
} 
