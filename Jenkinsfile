pipeline {
    agent any

    environment {
        IMAGE_NAME = "profile-app"
        CONTAINER_NAME = "profile-app-container"
        PORT = 5000
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "Pulling latest code from GitHub..."
                git branch: 'master', url: 'https://github.com/syedtalhahamid/profile-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                script {
                    sh 'docker build -t ${IMAGE_NAME}:latest .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Running Docker container..."
                script {
                    // Stop and remove if already running
                    sh 'docker ps -a -q --filter "name=${CONTAINER_NAME}" | grep -q . && docker rm -f ${CONTAINER_NAME} || true'

                    // Run new container
                    sh 'docker run -d --name ${CONTAINER_NAME} -p ${PORT}:5000 ${IMAGE_NAME}:latest'
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment completed successfully!"
            sh 'docker images'
            sh 'docker ps'
        }
        failure {
            echo "❌ Deployment failed. Please check logs."
        }
    }
}
