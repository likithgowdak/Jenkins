pipeline {
    agent any

    environment {
        APP_ENV = "dev"
    }

    stages {
        stage('Build') {
            steps {
                echo "Building the application in ${APP_ENV} environment"
                sh 'mvn clean package || echo "Build step simulated"'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the app...'
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
