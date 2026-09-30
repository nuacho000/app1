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
                sh '. venv/bin/activate && pip install --upgrade pip'
                sh '. venv/bin/activate && pip install -r requirements.txt'
            }
        }

        stage('Build') {
            steps {
                sh '. venv/bin/activate && python app.py'
            }
        }

        stage('Test') {
            steps {
                sh '. venv/bin/activate && pytest --junitxml=report.xml'
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
