pipeline {
    agent any

    environment {
        APP_NAME = 'jenkins-static-site'
        VERSION = '1.0'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Fetching source code...'
                git branch: 'main', url: 'https://github.com/Iszzmail/jenkins-pipeline-test.git'

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t ${APP_NAME}:${VERSION} .'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Running container...'
                sh 'docker run -d -p 7070:7070 ${APP_NAME}:${VERSION}'
            }
        }
    }

    post {
        success {
            echo '✅ Site deployed successfully! Visit http://localhost:7070'
        }
        failure {
            echo '❌ Pipeline failed.'
        }
    }
}
