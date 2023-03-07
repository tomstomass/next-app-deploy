@Library('github.com/releaseworks/jenkinslib') _
pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
         stage('Clone repository') { 
            steps { 
                script{
                checkout scm
                }
            }
        }

        stage('Build') { 
            steps { 
                script{
                 app = docker.build("underwater")
                }
            }
        }
        
        stage('Test'){
            steps {
                 echo 'Empty'
            }
        }
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'aws-key', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY']]) {
        AWS("--region=eu-west-1 s3 ls")
    }
        stage('Deploy') {
            steps {
                script{
                        docker.withRegistry('203675315902.dkr.ecr.us-east-1.amazonaws.com/docker:latest', 'ecr:us-east-1:aws-credentials') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }
    }
}
