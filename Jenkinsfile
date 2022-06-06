pipeline {
    //agent any
    agent { label 'jenkins-slave'}
    environment { 
        IMAGE_NAME = 'demo-nodejs'
        MAIN_VER = '1.0'
    }
    stages {
        stage('build info'){
            steps {
                echo "build ${IMAGE_NAME}:${MAIN_VER}.${env.BUILD_NUMBER}"
            }
        }
        stage('build image')
        {
            steps{
                script {
                    appImage = docker.build("${IMAGE_NAME}:${MAIN_VER}.${env.BUILD_NUMBER}")                    
                }
            }           
        }  
        stage('push to AWS ecr')
        {
            steps {
                script {
                    docker.withRegistry('https://232195323397.dkr.ecr.ap-northeast-1.amazonaws.com', 'ecr:ap-northeast-1:aws-issdu-credential') {
                        appImage.push()
                    }
                }
            }
        }
        stage('deploy')
        {
            when { 
               expression { BRANCH_NAME ==~ /(production|staging)/ }
            }            
            steps {
                echo 'Deploying'
            }
        }        
    }    
}
