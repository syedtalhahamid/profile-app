pipeline {
    agent any

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
                    sh 'docker build -t profile-app:latest .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Running Docker container..."
                script {
                    // Stop and remove if already running
                    sh 'docker ps -a -q --filter "name=profile-app-container | grep -q . && docker rm -f profile-app-container || true'

                    // Run new container
                    sh 'docker run -d --name profile-app-container -p 5000:5000 profile-app:latest'
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
