pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                sh '''
                # Create virtual environment
                python3 -m venv venv

                # Activate venv
                source venv/bin/activate

                # Upgrade pip inside venv
                pip install --upgrade pip

                # Install dependencies from requirements.txt
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
