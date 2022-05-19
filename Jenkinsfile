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
        stage('build image')
        {
            steps{
                script {
                    appImage = docker.build("${env.IMAGE_NAME}:${env.BUILD_ID}")
                    
                }
            }
        }
  
        stage('push to AWS ecr')
        {
            steps {
                 checkpoint 'Completed Build'
            }
            steps {
                script {
                    docker.withRegistry('https://232195323397.dkr.ecr.ap-northeast-1.amazonaws.com', 'ecr:ap-northeast-1:aws-issdu-credential') {
                        appImage.push()
                    }
                }
            }
        }
    }    
}
