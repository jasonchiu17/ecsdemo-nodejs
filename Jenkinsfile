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
        stage('SonarQube analysis')
        {
            environment {
                SCANNER_HOME = tool 'SonarQubeScanner'
            }
            steps{
                withSonarQubeEnv('poc_sonarqube') {
                sh """
                $SCANNER_HOME/bin/sonar-scanner \
                -Dsonar.projectKey=demo-nodejs \
                -Dsonar.sources=./server.js
                """
                /*
                sh """
                /home/sonar-scanner/bin/sonar-scanner \
                -Dsonar.projectKey=demo-nodejs \
                -Dsonar.sources=./server.js \
                -Dsonar.host.url=http://jenkins.issdu-poc.com:9000 \
                -Dsonar.login=52df3370e1896f58b3adb822cdb2f0350c8ac383
                """
                */
                }
            }
        }
        stage('Build image')
        {
            steps{
                script {
                    appImage = docker.build("${IMAGE_NAME}:${MAIN_VER}.${env.BUILD_NUMBER}")                    
                }
            }           
        }  
        stage('Push image to AWS ECR')
        {
            steps {
                script {
                    docker.withRegistry("https://${IMAGE_REGISTRY}", 'ecr:ap-northeast-1:aws-issdu-credential') {
                        appImage.push()
                    }
                }
            }
        }
        stage('Deploy to AWS EKS')
        {
            /*
            when { 
                expression { BRANCH_NAME == 'develop' }
            }
            */           
            steps {
                echo 'Deploying to dev EKS'
                sh """
                    kubectl set image deployment ecsdemo-nodejs ecsdemo-nodejs=${IMAGE_REGISTRY}/${IMAGE_NAME}:${MAIN_VER}.${env.BUILD_NUMBER} --record
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

                    sh """
                    while [ "\$(kubectl get pod -l app='ecsdemo-nodejs' -o jsonpath='{range .items[?(@.spec.containers[0].image=="${IMAGE_REGISTRY}/${IMAGE_NAME}:${MAIN_VER}.${env.BUILD_NUMBER}")]}{.status.phase}{"\\n"}{end}' | grep Running | head -n 1)" != "Running" ]; do
                    echo Waiting for pod to be ready...
                    sleep 10
                    done;

                    
                    echo 'POD running image verion :'
                    kubectl get pod -l app='ecsdemo-nodejs' -o=jsonpath='{range .items[?(@.status.phase=="Running")]}{.metadata.name}{"\t"}{.status.phase}{"\t"}{.spec.containers[0].image}{end}' |sort
                    """
                }
            }
        }
    }
}
