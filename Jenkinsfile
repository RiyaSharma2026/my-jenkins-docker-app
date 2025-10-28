pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/RiyaSharma2026/my-jenkins-docker-app.git'
            }
        }

        stage('Build App') {
            steps {
                sh 'npm install || echo "No package.json found"'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-jenkins-docker-app .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                    docker stop my-jenkins-docker-app || true
                    docker rm my-jenkins-docker-app || true
                    docker run -d -p 8080:8080 --name my-jenkins-docker-app my-jenkins-docker-app
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Build and Docker run completed successfully!'
        }
        failure {
            echo '❌ Build failed. Check the logs above.'
        }
    }
}
