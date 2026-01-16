pipeline {
    agent any
    environment {
        IMAGE_NAME = "multi-stage-app"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/krushna9067/Multi-Stage-Docker-CI-CD.git'
            }
        }
        stage('Build & Test') {
            steps {
                sh 'pip install -r requirements.txt'
                sh 'pytest'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME ."
            }
        }
        stage('Run Docker Container') {
            steps {
                sh '''
                docker stop multi-stage-app || true
                docker rm multi-stage-app || true
                docker run -d -p 5000:5000 --name multi-stage-app multi-stage-app
                '''
            }
        }
    }
}
