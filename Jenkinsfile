pipeline {
    agent any
    stages {
        stage('Build App') {
            steps {
                bat 'mvn clean package'       
            }
        }
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t myapp .' 
            }
        }
        stage('Run Docker Container') {
            steps {
                bat 'docker run -d -p 8080:8080 myapp'
            }
        }
    }
}
