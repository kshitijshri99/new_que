pipeline {
    agent any

    environment {
        DOCKER_USER = 'kshitijshri99'
        REPO_NAME   = 'new_que'
        IMAGE_NAME  = "${DOCKER_USER}/${REPO_NAME}"
    }

    stages {
        stage('Deploy to K8s') {
            steps {
                withKubeConfig([credentialsId: 'k8s-config']) {
                    sh 'kubectl apply -f deployment.yaml'
                    sh 'kubectl apply -f service.yaml"
                    sh 'kubectl rollout restart deployment/nginx-app'
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                withKubeConfig([credentialsId: 'k8s-config']) {
                    script {
                        sh 'kubectl get pods'
                        sh 'kubectl get svc'
                        sh 'kubectl rollout status deployment/nginx-app'
                    }
                }
            }
        }
    }
}
