pipeline {
    //agent any
    agent { label 'jenkins-slave'}
    environment { 
        IMAGE_REGISTRY = '232195323397.dkr.ecr.ap-northeast-1.amazonaws.com'
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
                    docker.withRegistry("https://${IMAGE_REGISTRY}", 'ecr:ap-northeast-1:aws-issdu-credential') {
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
                    kubectl set image deployment ecsdemo-nodejs ${IMAGE_REGISTRY}/${IMAGE_NAME}:${MAIN_VER}.${env.BUILD_NUMBER} --record
                """
            }
            post {
                success {
                    sh """
                        echo 'Rollout status :'
                        kubectl rollout status deployment ecsdemo-nodejs

                        echo 'Deployment image version :'
                        kubectl get deploy/ecsdemo-nodejs -o jsonpath={..image}

                    """
                }
            }
            post {
                success {
                    sh """
                    while [ "\$(kubectl get po -l "ecsdemo-nodejs" -o jsonpath='{range .items[?(@.spec.containers[0].image=="${IMAGE_REGISTRY}/${IMAGE_NAME}:${MAIN_VER}.${env.BUILD_NUMBER}")]}{.status.phase}{"\\n"}{end}' | grep Running | head -n 1)" != "Running" ]; do
                    echo Waiting for pod to be ready...
                    sleep 10
                    done;

                    
                    echo 'POD running image verion :'
                    kubectl get pod -l app='ecsdemo-nodejs' -o jsonpath='{.items[*].spec.containers[*].image}'
                    """
                }
            }
        }
    }
}
