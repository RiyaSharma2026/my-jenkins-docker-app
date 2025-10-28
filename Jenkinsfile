pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/RiyaSharma2026/my-jenkins-docker-app.git'
            }
        }

        stage('Build App') {
            steps {
                // Use Docker to run Maven build
                bat '''
                docker run --rm -v %CD%:/app -w /app maven:3.9.5-eclipse-temurin-17 mvn clean package
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t my-jenkins-app .'
            }
        }

        stage('Run Docker Container') {
            steps {
                bat 'docker run -d -p 8080:8080 my-jenkins-app'
            }
        }
    }

    post {
        always {
            echo 'Build process finished!'
        }
    }
}
