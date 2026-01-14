#!/usr/bin.env groovy

pipeline {   
    agent any
    stages {
        stage("test") {
            steps {
                script {
                    echo "Testing the application..."

                }
            }
        }
        stage("build image") {
            steps {
                script {
                    echo "Building the application..."
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    echo 'deploying docker image to ec2...'
                    def dockerCmd = 'docker run -p 3080:3080 -d image_name:1.0'
                    sshagent(['ec2-instance-key']){
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@ec2-IP-address ${dockerCmd}"
                    }
                }
            }
        }               
    }
} 
