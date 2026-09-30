pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install') {
            steps {
                sh 'python3 -m pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                sh 'pytest -q'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t demo-app:latest .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f demo-app || true
                docker run -d --name demo-app -p 8000:8000 demo-app:latest
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh 'curl -f http://localhost:8000/health'
            }
        }
    }
}