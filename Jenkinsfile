pipeline {
    agent any

    environment {
        IMAGE_NAME = 'harshrawte/todo-list'
        IMAGE_TAG = 'latest'
        DOCKER_HUB_CREDENTIALS = 'docker-hub-creds' // ID of credentials in Jenkins
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo '🔄 Cloning repo...'
                git 'https://github.com/harshhrawte/To-Do-List.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                bat 'docker build -t %IMAGE_NAME%:%IMAGE_TAG% .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo '📤 Pushing image to Docker Hub...'
                withCredentials([usernamePassword(credentialsId: "${DOCKER_HUB_CREDENTIALS}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    bat '''
                    docker login -u %DOCKER_USERNAME% -p %DOCKER_PASSWORD%
                    docker push %IMAGE_NAME%:%IMAGE_TAG%
                    '''
                }
            }
        }

        stage('Deploy on Docker Host') {
            steps {
                echo '🚀 Deploying Docker container...'
                bat '''
                docker stop todo-app || echo "Container not running"
                docker rm todo-app || echo "Container not existing"
                docker run -d -p 5000:5000 --name todo-app %IMAGE_NAME%:%IMAGE_TAG%
                '''
            }
        }
    }

    post {
        failure {
            echo '❌ Something went wrong. Check logs.'
        }
    }
}
