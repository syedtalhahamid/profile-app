pipeline {
    agent any

    environment {
        EC2_USER = 'ubuntu'
        EC2_HOST = '18.215.165.166'       // replace with your EC2 public IP
        SSH_KEY = '/var/lib/jenkins/.ssh/mynewkey1.pem'  // path to private key on Jenkins
        APP_NAME = 'profile-app'
        APP_DIR = '/home/ubuntu/app'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/syedtalhahamid/profile-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t ${APP_NAME}:latest .
                """
            }
        }

        stage('Deploy on EC2') {
            steps {
                // Copy app code and docker-compose.yml to EC2
                sh """
                scp -i ${SSH_KEY} -r * ${EC2_USER}@${EC2_HOST}:${APP_DIR}/
                scp -i ${SSH_KEY} docker-compose.yml ${EC2_USER}@${EC2_HOST}:${APP_DIR}/docker-compose.yml
                """

                // SSH into EC2 and run Docker Compose
                sh """
                ssh -i ${SSH_KEY} ${EC2_USER}@${EC2_HOST} '
                    cd ${APP_DIR}
                    docker compose down
                    docker compose up -d --build
                '
                """
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
