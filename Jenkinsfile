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
                bat "docker build -t %IMAGE_NAME%:%IMAGE_TAG% ."
            }
        }
        stage('Container Deployment') {
            steps {
                echo "Deploying container..."
                bat """
                    docker stop %CONTAINER_NAME% || true
                    docker rm %CONTAINER_NAME% || true
                    docker run -d --name %CONTAINER_NAME% -p %DOCKER_PORT%:80 %IMAGE_NAME%:%IMAGE_TAG%
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