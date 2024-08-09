pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:$PATH"
        DOCKER_IMAGE = 'manalalkuraya/my-wiremock-server:latest'
        REGISTRY_CREDENTIALS = credentials('docker-hub-wiremock-server')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Docker Installation') {
            steps {
                sh 'docker --version' // Verify Docker is accessible
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE} .' // Build Docker image
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh 'docker login -u manalalkuraya -p ${REGISTRY_CREDENTIALS_PSW}' // Login to Docker registry
                    sh 'docker push ${DOCKER_IMAGE}' // Push Docker image
                }
            }
        }

        stage('Deploy Docker Container') {
            steps {
                script {
                    sh 'docker stop wiremock-server || true && docker rm wiremock-server || true' // Stop and remove existing container
                    sh 'docker run -d --name wiremock-server -p 8082:8080 ${DOCKER_IMAGE}' // Deploy new container
                }
            }
        }
    }

    post {
        always {
            cleanWs() // Clean workspace after job completion
        }
    }
}
