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
                    echo "Building new Docker image..."
                    sh '''
                        # Stop and remove old container if exists
                        docker rm -f profile-app-container || true
                        
                        # Remove old image if exists
                        docker rmi -f profile-app:latest || true
                        
                        # Build new image
                        docker build -t profile-app:latest .
                    '''
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    echo "Running new container..."
                    sh '''
                        # Run new container on port 80 -> 5000
                        docker run -d --name profile-app-container -p 80:5000 profile-app:latest
                    '''
                }
            }
        }
    }

    post {
        success {
            echo " Deployment completed successfully!"
            echo "Access the app at: http://<EC2-PUBLIC-IP>/register"
        }
        failure {
            echo " Deployment failed!"
        }
    }
}
