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
                customImage = docker.build("${env.IMAGE_NAME}:${env.BUILD_ID}")
            }
        }
    }
    
}
