pipeline {
    //agent any
    agent { label 'jenkins-slave'}
    environment { 
        IMAGE_NAME = 'demo-nodejs'
        MAIN_VER = '1.0'
    }
    stages {
        stage('info'){
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
        stage('push image to AWS ECR')
        {
            steps {
                script {
                    docker.withRegistry('https://232195323397.dkr.ecr.ap-northeast-1.amazonaws.com', 'ecr:ap-northeast-1:aws-issdu-credential') {
                        appImage.push()
                    }
                }
            }
        }
        stage('deploy to dev EKS')
        {
            /*
            when { 
                expression { BRANCH_NAME == 'develop' }
            }
            */           
            steps {
                echo 'Deploying to dev EKS'
                sh """
                    kubectl set image deployment ecsdemo-nodejs ecsdemo-nodejs=232195323397.dkr.ecr.ap-northeast-1.amazonaws.com/${IMAGE_NAME}:${MAIN_VER}.${env.BUILD_NUMBER} --record
                """
            }
            post {
                success {
                    sh """
                        kubectl rollout status deployment ecsdemo-nodejs
                        kubectl get deploy/ecsdemo-nodejs -o jsonpath={..image}
                    """
                }
            }
        }
    }
}
