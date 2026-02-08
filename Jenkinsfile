pipeline {
    agent any

    options {
        disableConcurrentBuilds()
    }

    environment {
        IMAGE_NAME = "react-web"
        CONTAINER_NAME = "react-web-ui"
        HOST_PORT = "3000"
        CONTAINER_PORT = "80"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('CI') {
            steps {
                sh '''
                  npm install
                  npm run build
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Deploy React App') {
            steps {
                sh '''
                  docker rm -f $CONTAINER_NAME || true
                  docker run -d \
                    -p $HOST_PORT:$CONTAINER_PORT \
                    --name $CONTAINER_NAME \
                    $IMAGE_NAME:latest
                '''
            }
        }
    }

    post {
        success {
            echo '✅ React CI/CD succeeded'
        }
        failure {
            echo '❌ React CI/CD failed'
        }
    }
}
