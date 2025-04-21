pipeline {
    agent any

    environment {
        IMAGE_NAME = "harshrawte/todo-list-app"
        IMAGE_TAG = "v2"
        CONTAINER_NAME = "devops-prac"
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo '🔄 Cloning repo from test branch...'
                git branch: 'test', url: 'https://github.com/harshhrawte/To-Do-List.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building new Docker image with latest code...'
                script {
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                echo '🛑 Stopping and removing old container (if exists)...'
                script {
                    sh """
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    """
                }
            }
        }

        stage('Run New Container') {
            steps {
                echo '🚀 Running new container with updated image...'
                script {
                    sh """
                    docker run -d --name ${CONTAINER_NAME} -p 5000:5000 ${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }

        stage('Save Docker Image') {
            steps {
                echo '💾 Saving image as todo-list.tar...'
                script {
                    sh "docker save -o todo-list.tar ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        success {
            echo "✅ All done! App is live at http://localhost:5000"
        }
        failure {
            echo "❌ Pipeline failed. Check logs!"
        }
    }
}
