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
                sh 'python3 -m venv .venv'
                sh '.venv/bin/pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                sh '.venv/bin/pytest -q'
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
	        docker run -d --name demo-app --network cicd-network demo-app:latest
	        '''
	    }
	}

	stage('Health Check') {
	    steps {
	        sh 'curl -f http://demo-app:8000/health'
	    }
	}
    }
}