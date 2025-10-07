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
                    // Stop and remove existing container if it exists
                    sh '''
                    if [ "$(docker ps -aq -f name=profile-app-container)" ]; then
                        docker rm -f profile-app-container
                    fi
                    '''

                    // Run new container
                    sh 'docker run -d --name profile-app-container -p 5010:5010 profile-app:latest'
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
