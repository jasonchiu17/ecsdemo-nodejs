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
                   def customImage = docker.build("${env.IMAGE_NAME}:${env.BUILD_ID}")
                }
            }
        }
        stage('push aws ecr')
            steps{
                docker.withRegistry("https://232195323397.dkr.ecr.ap-northeast-1.amazonaws.com/demo-nodejs", "ecr:ap-northeast-1:aws-issdu-credential") {
                customImage.push()
            }
    }    
}
