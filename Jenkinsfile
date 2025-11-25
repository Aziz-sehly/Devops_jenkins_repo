pipeline {
    agent any

    environment {
        IMAGE_NAME = "azizsehli/spring-app"
        IMAGE_TAG  = "latest"
    }

    stages {

        stage('Docker Build') {
            steps {
                echo "Building Docker image for Spring Boot app..."
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Docker Login') {
            steps {
                echo "Logging into Docker Hub..."
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred', // ton ID Jenkins pour Docker Hub
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh "echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin"
                }
            }
        }

        stage('Docker Push') {
            steps {
                echo "Pushing Docker image to Docker Hub..."
                sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully! Image pushed to Docker Hub."
        }
        failure {
            echo "Pipeline failed. Check logs."
        }
    }
}
