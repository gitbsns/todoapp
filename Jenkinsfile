pipeline {
    agent any

    environment {
        IMAGE_NAME = "todoapp"
        CONTAINER_NAME = "todoapp"
        HOST_PORT = "8081"
        APP_PORT = "3000"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Git se latest code checkout ho raha hai...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Docker image build ho rahi hai...'
                sh 'docker build -t ${IMAGE_NAME}:latest .'
            }
        }

        stage('Test') {
            steps {
                echo 'Basic check ho raha hai ke image ban chuki hai...'
                sh 'docker images | grep ${IMAGE_NAME}'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Purana container hata kar naya deploy ho raha hai...'
                sh '''
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    docker run -d --name ${CONTAINER_NAME} -p ${HOST_PORT}:${APP_PORT} ${IMAGE_NAME}:latest
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline successfully complete - todoapp deploy ho gayi!'
        }
        failure {
            echo '❌ Pipeline fail ho gayi - Jenkins console output check karein.'
        }
    }
}
