node {
    environment { 
        IMAGE_NAME = 'demo-nodejs'
    }
    stage('build info'){
            steps {
                echo "build ${env.IMAGE_NAME}:${env.BUILD_NUMBER}"
            }
        }
        stage('docker build')
        {
            
            steps{
                script{ 
                    customImage = docker.build("${env.IMAGE_NAME}:${env.BUILD_ID}")
                }
            }
        }
    }
    
}
