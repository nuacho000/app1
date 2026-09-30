pipeline {
    agent any

    options {
        timestamps()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup') {
            steps {
                sh 'python3 -m venv venv'
                sh 'venv/bin/python -m pip install --upgrade pip'
                sh 'venv/bin/python -m pip install -r requirements.txt'
            }
        }

        stage('Build') {
            steps {
                sh 'venv/bin/python app.py'
            }
        }

        stage('Test') {
            steps {
                sh 'venv/bin/python -m pytest --junitxml=report.xml'
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true, testResults: 'report.xml'
        }
        failure {
            echo 'Сборка завершилась с ошибкой'
        }
    }
}
