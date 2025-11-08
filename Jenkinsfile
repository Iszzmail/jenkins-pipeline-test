pipeline {
    agent any

    environment {
        APP_NAME = 'jenkins-static-site'
        VERSION = '1.0'
    }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t ${APP_NAME}:${VERSION} .'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Running container...'
                sh 'docker run -d -p 7070:80 ${APP_NAME}:${VERSION}'
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
