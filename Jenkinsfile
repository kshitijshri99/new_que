piipeline {
    agent any

    environment {
        
        DOCKER_USER = "kshitijshri99"
        REPO_NAME   = "new_que"
        IMAGE_NAME  = "${kshitijshri99}/${new_que}"
        BUILD_TAG   = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout Source') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    
                    appImage = docker.build("${IMAGE_NAME}:${BUILD_TAG}")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                 
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-pass') {
                        
                        
                        appImage.push("${BUILD_TAG}")
                        
                       
                        appImage.push("latest")
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Successfully pushed ${IMAGE_NAME}:${BUILD_TAG} to DockerHub."
        }
        failure {
            echo "Pipeline failed. Check credentials or DockerHub repository permissions."
        }
    }
}
