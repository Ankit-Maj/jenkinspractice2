pipeline {
    agent any

    environment {
        IMAGE_NAME = "myapp"
        IMAGE_TAG = "latest"
        CONTAINER_NAME = "myapp_container"
        DOCKER_PORT = "9090"
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Source Checkout') {
            steps {
                echo "Pulling latest code from GitHub main branch..."
                checkout scm
            }
        }

        stage('Docker Image Build') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t ${env.IMAGE_NAME}:${env.IMAGE_TAG} ."
            }
        }

        stage('Container Deployment') {
            steps {
                echo "Deploying container..."
                sh """
                    docker stop ${env.CONTAINER_NAME} || true
                    docker rm ${env.CONTAINER_NAME} || true
                    docker run -d \
                        --name ${env.CONTAINER_NAME} \
                        -p ${env.DOCKER_PORT}:80 \
                        ${env.IMAGE_NAME}:${env.IMAGE_TAG}
                """
            }
        }
    }

    post {
        success {
            echo "Deployed! Visit http://localhost:${env.DOCKER_PORT}"
        }
        failure {
            echo "Pipeline failed."
        }
    }
}