
pipeline {
    agent any

    environment {
        REGISTRY = "localhost:5000"
        IMAGE_NAME = "anil-demo-app"
    }

    stages {

        stage('1. Checkout Code') {
            steps {
                checkout scm
                echo 'Source code checked out successfully'
            }
        }

        stage('2. Application Build & Test') {
            steps {
                sh 'python3 --version'
                sh 'python3 -m py_compile app.py'
                echo 'Application build and test successful'
            }
        }

        stage('3. Build & Tag Docker Image') {
            steps {
                sh 'docker --version'
                sh """
                    docker build \
                    -t ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} \
                    -t ${REGISTRY}/${IMAGE_NAME}:latest .
                """
                echo 'Docker image built and tagged successfully'
            }
        }

        stage('4. Push Image to Registry') {
            steps {
                sh "docker push ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
                sh "docker push ${REGISTRY}/${IMAGE_NAME}:latest"
                echo 'Docker images pushed successfully'
            }
        }
    }

    post {
        success {
            echo "Pipeline succeeded!"
            echo "Image: ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
        }

        failure {
            echo 'Pipeline failed. Check Console Output for details.'
        }
    }
}
