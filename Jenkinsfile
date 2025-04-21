pipeline {
    agent any

    environment {
        IMAGE_NAME = "harshrawte/todo-list-app"
        IMAGE_TAG = "v1"
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo '🔄 Cloning test branch...'
                withCredentials([usernamePassword(credentialsId: 'github-credentials-id', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                    git branch: 'test', url: 'https://github.com/harshhrawte/To-Do-List.git', credentialsId: 'github-credentials-id'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                script {
                    bat 'docker build -t %IMAGE_NAME%:%IMAGE_TAG% .'
                }
            }
        }

        stage('Save Docker Image as .tar') {
            steps {
                echo '📦 Saving Docker image to todo-list.tar...'
                script {
                    bat 'docker save -o todo-list.tar %IMAGE_NAME%:%IMAGE_TAG%'
                }
            }
        }
    }

    post {
        failure {
            echo "❌ Something went wrong. Check logs."
        }
        success {
            echo "✅ Pipeline for TEST branch completed successfully!"
        }
    }
}
