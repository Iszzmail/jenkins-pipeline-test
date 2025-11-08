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
    
        stage('Check Pip Installation') {
            steps {
                script {
                    // Check if 'pip' command is found
                    // returnStatus: true prevents the pipeline from failing immediately if the command is not found
                    def pip_installed = sh(script: 'command -v pip || command -v pip3', returnStatus: true)

                    if (pip_installed == 0) {
                        echo "Pip is installed. Proceeding with installation of dependencies."
                        // You can now run your installation commands
                        sh 'pip install -r requirements.txt'
                    } else {
                        echo "Pip is not installed or not in the PATH."
                        // Add steps here to install pip if necessary, or fail the build explicitly
                        // currentBuild.result = 'FAILURE'
                        // error "Pip missing, stopping build."
                    }
                }
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
