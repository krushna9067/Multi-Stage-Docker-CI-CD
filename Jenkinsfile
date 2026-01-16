pipeline {
    agent any
    stages {

        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                // Create and activate virtual environment
                sh '''
                python3 -m venv venv
                source venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-flask-app .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 5000:5000 my-flask-app'
            }
        }
    }
}
