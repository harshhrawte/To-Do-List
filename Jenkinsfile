pipeline {
    agent any

    environment {
        IMAGE_NAME = "harshrawte/todo-list-app"
        IMAGE_TAG = "v1"
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo '🔄 Cloning repo...'
                withCredentials([usernamePassword(credentialsId: 'github-credentials-id', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                    git branch: 'main', url: 'https://github.com/harshhrawte/To-Do-List.git', credentialsId: 'github-credentials-id'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                script {
                    bat "docker build -t %IMAGE_NAME%:%IMAGE_TAG% ."
                }
            }
        }

        stage('Save Docker Image as .tar') {
            steps {
                echo '📦 Saving Docker image to todo-list.tar...'
                script {
                    bat "docker save -o todo-list.tar %IMAGE_NAME%:%IMAGE_TAG%"
                }
            }
        }

        stage('Run Container with docker-compose') {
            steps {
                echo '🚀 Running container using docker-compose...'
                script {
                    bat "docker-compose up -d"
                }
            }
        }

        stage('Verify Container Running') {
            steps {
                echo '🔍 Checking running containers...'
                script {
                    bat "docker ps"
                }
            }
        }

        stage('Stop Container') {
            steps {
                echo '🛑 Stopping container...'
                script {
                    bat "docker-compose down"
                }
            }
        }
    }

    post {
        failure {
            echo "❌ Something went wrong. Check logs."
        }
        success {
            echo "✅ Pipeline completed successfully!"
        }
    }
}
