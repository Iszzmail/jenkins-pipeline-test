pipeline {
    agent any

    environment {
        APP_NAME = 'flask-jenkins-demo'
        VERSION = '1.0'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Fetching source code...'
                git branch: 'main', url: 'https://github.com/Iszzmail/jenkins-pipeline-test.git'
            }
        }
        stage('Install Pip') {
    steps {
        sh 'apt-get update && apt-get install -y python3-pip'
    }
}

       

        stage('Install Dependencies') {
            steps {
                echo 'Installing Python dependencies...'
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running unit tests...'
                sh 'pytest --maxfail=1 --disable-warnings -q'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${APP_NAME}:${VERSION} ." // Use double quotes for interpolation
            }
        }

        stage('Deploy') {
            steps {
                echo 'Running Flask app container...'
                sh "docker run -d -p 5000:5000 ${APP_NAME}:${VERSION}" // Use double quotes for interpolation
            }
        }
    } // This closing brace was missing in the original code

    post {
        success {
            echo '✅ Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed.'
        }
    }
}
