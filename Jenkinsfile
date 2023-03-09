pipeline {
    
    agent any
    
    environment {
        registry = "203675315902.dkr.ecr.us-east-1.amazonaws.com/docker"
    }
    stages{
        
        stage('Checkout') {
            steps {
            checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/tomstomass/next-app-deploy.git']])
            }
        }
        
        stage ('Docker Build') {
            steps {
                script {
                    dockerImage = docker.build registry
                }
            }
        }
        stage ('Docker Push') {
            steps {
                script {
                        sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 203675315902.dkr.ecr.us-east-1.amazonaws.com'
                        sh 'docker push 203675315902.dkr.ecr.us-east-1.amazonaws.com/docker:latest'
                }
            }
        }
        
     // Stopping Docker containers for cleaner Docker run
        stage('stop previous containers') {
        steps {
            sh 'docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm'
         }
       }
       
       stage ('Docker Run') {
           steps {
                script {
                    sh 'docker run -d -p 8096:3000 --rm --name mypythonContainer 203675315902.dkr.ecr.us-east-1.amazonaws.com/docker:latest'
                }
           }
           
       }
    }
}
