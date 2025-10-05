pipeline {
    agent any
    
    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/syedtalhahamid/profile-app.git', branch: 'master'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t profile-app:latest ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop and remove container if already running
                    sh "docker rm -f profile-app-container || true"

                    // Run container
                    sh "docker run -d --name profile-app-container -p 5000:5000 profile-app:latest"
                }
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully!"
        }
        failure {
            echo "Deployment failed!"
        }
    }
}
