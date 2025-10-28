pipeline {
    agent any

    environment {
        APP_NAME = 'my-jenkins-app'
        APP_PORT = '8081'
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
                bat """
                docker ps -q -f "name=${APP_NAME}" > temp.txt
                for /F %%i in (temp.txt) do docker stop %%i
                for /F %%i in (temp.txt) do docker rm %%i
                del temp.txt
                """
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
