pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "likithgowda/jenkins-demo:latest"
    }

    parameters {
        string(name: 'APP_VERSION', defaultValue: '1.0.0', description: 'Enter your app version')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'qa', 'prod'], description: 'Select environment')
        booleanParam(name: 'DEPLOY', defaultValue: true, description: 'Deploy after build?')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/likithgowdak/Jenkins.git'
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                    echo "$PASS" | docker login -u "$USER" --password-stdin
                    docker push $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@<EC2-IP> "
                      docker pull $DOCKER_IMAGE &&
                      docker stop myapp || true &&
                      docker rm myapp || true &&
                      docker run -d --name myapp -p 80:8080 $DOCKER_IMAGE
                    "
                    '''
                }
            }
        }
    }
}
