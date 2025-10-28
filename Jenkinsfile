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

        stage('Build App') {
    steps {
        bat """
        docker run --rm -v "%CD%\\e-app":/app -w /app maven:3.9.5-eclipse-temurin-17 mvn clean package
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
                bat """
                for /f "tokens=*" %%i in ('docker ps -q -f "name=${APP_NAME}"') do (
                    docker stop %%i
                    docker rm %%i
                )
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
