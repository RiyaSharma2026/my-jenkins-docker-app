pipeline {
    agent any

    environment {
        APP_NAME = 'my-jenkins-app'
        APP_PORT = '8080'
        GIT_URL = 'https://github.com/RiyaSharma2026/my-jenkins-docker-app.git'
        GIT_BRANCH = 'main'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${GIT_BRANCH}", url: "${GIT_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                bat """
                docker build -t ${APP_NAME} .
                """
            }
        }

        stage('Stop Previous Container') {
            steps {
                docker rm -f my-jenkins-app || echo "No existing container"

            }
        }

        stage('Run Docker Container') {
            steps {
                bat """
                docker run -d -p ${APP_PORT}:${APP_PORT} --name ${APP_NAME} ${APP_NAME}
                """
            }
        }
    }

    post {
        always {
            echo 'Build process finished!'
        }
    }
}
