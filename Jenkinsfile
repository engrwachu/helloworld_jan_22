pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
    registry = '896375864143.dkr.ecr.us-east-1.amazonaws.com/devops-repojenkins'
    registryCredential = 'jenkins-ecr'
    region = 'us-east-1'
    dockerimage = ''
  }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/engrwachu/helloworld_jan_22.git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('docker login'){
            steps{
                script{
                    sh 'aws ecr get-login-password --region "${region}"| docker login --username AWS --password-stdin "${registry}"'
                }
            }
        }

        stage('Deploy image') {
            steps{
                script{ 
                   docker.withRegistry("https://"+registry,"ecr:us-east-1:"+registryCredential) {
                     dockerImage.push()
                 }
               }
            }
        }  
    }
}
