pipeline {
    agent any

    stages {
        stage('Fetch Data from GitHub') {
            steps {
                git branch: 'main', url: 'https://github.com/Maryam-Yaqoob/devops-ml-midterm.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install -r requirements.txt
                '''
            }
        }

        stage('Train Model') {
            steps {
                sh '''
                . venv/bin/activate
                python train.py
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ml-fastapi-app .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker rm -f ml-api || true
                docker run -d -p 8000:8000 --name ml-api ml-fastapi-app
                sleep 10
                '''
            }
        }

        stage('Check Metrics API') {
            steps {
                sh 'curl --fail http://localhost:8000/metrics'
            }
        }
    }
}
