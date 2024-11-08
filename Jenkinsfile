pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'b3bd7a36-6578-4c5c-b068-9c787219612' // Replace with your Jenkins credentials ID
        IMAGE_NAME = 'sumeet382/myapp04' // Replace with your Docker Hub image name
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/sumeet-ai/practice.git' // Replace with your GitHub URL
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${env.IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${env.IMAGE_NAME}:${env.BUILD_NUMBER}").push()
                        docker.image("${env.IMAGE_NAME}:${env.BUILD_NUMBER}").push('latest')
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    docker.image("${env.IMAGE_NAME}:${env.BUILD_NUMBER}").remove()
                }
            }
        }
    }

    post {
        success {
            echo 'Docker image built and pushed successfully!'
        }
        failure {
            echo 'The build or push failed.'
        }
    }
}
