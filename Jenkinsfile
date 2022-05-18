pipeline {
    agent any
    environment { 
        IMAGE_NAME = 'demo-nodejs'
    }
    stages {
        stage('build info'){
            steps {
                echo "build ${env.IMAGE_NAME}:${env.BUILD_NUMBER}"
            }
        }
        stage('docker build')
        {
            steps{
                script {
                   docker.withRegistry("https://232195323397.dkr.ecr.ap-northeast-1.amazonaws.com/demo-nodejs", "ecr:ap-northeast-1:aws-issdu-credential")
                   def appImage = docker.build("${env.IMAGE_NAME}:${env.BUILD_ID}")
                }
            }
        }
        stage('push aws ecr')
        {
            steps{
                script {
                    appImage.push()
                }
            }
        }
    }    
}