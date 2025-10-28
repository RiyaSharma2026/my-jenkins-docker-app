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
                // Checkout explicitly from main branch
                git branch: "${GIT_BRANCH}", url: "${GIT_URL}"
            }
        }

        stage('Build App with Maven Docker') {
            steps {
                // Run Maven inside a Docker container
                bat """
                docker run --rm -v %CD%:/app -w /app maven:3.9.5-eclipse-temurin-17 mvn clean package
                """
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
                // Stop and remove any previous container to avoid conflicts
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
