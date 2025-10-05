pipeline {
    agent any

    environment {
        IMAGE_NAME = "profile-app:latest"
        CONTAINER_NAME = "profile-app-container"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/syedtalhahamid/profile-app.git', branch: 'master'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run Docker Container') {
            steps {
                // Stop and remove previous container if exists
                sh '''
                if [ $(docker ps -aq -f name=$CONTAINER_NAME) ]; then
                    docker stop $CONTAINER_NAME
                    docker rm $CONTAINER_NAME
                fi
                '''

                // Run new container
                sh 'docker run -d --name $CONTAINER_NAME -p 5000:5000 $IMAGE_NAME'
            }
        }
    }

    post {
        success {
            echo 'App deployed successfully!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
