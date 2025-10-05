pipeline {
    agent any

    environment {
        EC2_USER = 'ubuntu'
        EC2_HOST = '18.215.165.166'  // replace with your EC2 public IP or use parameter
        SSH_KEY = '/home/jenkins/.ssh/mynewkey1.pem'  // Jenkins server key path
        APP_NAME = 'profile-app'
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/syedtalhahamid/profile-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${APP_NAME}:latest")
                }
            }
        }

        stage('Push Docker Image (Optional)') {
            steps {
                script {
                    // Uncomment if pushing to Docker Hub
                    // docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                    //     docker.image("${APP_NAME}:latest").push()
                    // }
                }
            }
        }

        stage('Deploy on EC2') {
            steps {
                script {
                    // Copy docker-compose.yml and updated code to EC2
                    sh """
                    scp -i ${SSH_KEY} docker-compose.yml ${EC2_USER}@${EC2_HOST}:/home/${EC2_USER}/docker-compose.yml
                    scp -i ${SSH_KEY} -r * ${EC2_USER}@${EC2_HOST}:/home/${EC2_USER}/app/
                    """

                    // SSH to EC2 and run Docker Compose
                    sh """
                    ssh -i ${SSH_KEY} ${EC2_USER}@${EC2_HOST} '
                        cd /home/${EC2_USER}/
                        docker compose down
                        docker compose up -d --build
                    '
                    """
                }
            }
        }

    }

    post {
        success {
            echo "Deployment successful!"
        }
        failure {
            echo "Deployment failed!"
        }
    }
}
