pipeline {
    agent any

    environment {
        DOCKER_USER = 'kshitijshri99' 
        REPO_NAME   = 'new_que'
        IMAGE_NAME  = "${DOCKER_USER}/${REPO_NAME}"
        BUILD_TAG   = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Prepare Artifacts') {
            steps {
                script {
                    sh 'if [ ! -f index.html ]; then echo "<h1>Build ${BUILD_TAG} successful!</h1>" > index.html; fi'
                }
            }
        }

        stage('Build Image') {
            steps {
                script {
                    appImage = docker.build("${IMAGE_NAME}:${BUILD_TAG}", ".")
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
            echo "SUCCESS: Image ${IMAGE_NAME}:${BUILD_TAG} is now live on DockerHub."
        }
        failure {
            echo "FAILURE: Check if 'dockerhub-pass' ID exists in Jenkins credentials."
        }
    }
}
