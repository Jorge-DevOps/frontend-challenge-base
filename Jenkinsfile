pipeline {
    agent any
    environment {
        REPOSIOTRY = 'https://github.com/Jorge-DevOps/frontend-challenge-base'
        DOCKER_REGISTRY = 'jorgedevops20' 
        DOCKER_IMAGE = "${DOCKER_REGISTRY}/frontend-challenge-base" 
    }
    stages {

        stage('Clone Repository') {
            steps {
                echo 'Cloning Git repository...'
                git branch: 'main', url: "${REPOSIOTRY}"
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_REGISTRY}/frontend-challenge-base:latest ."
                }
            }
        }
        stage('Push Docker Image') {
             steps {
                 script {
                    sh "docker push ${DOCKER_REGISTRY}/frontend-challenge-base:latest"
                 }
             }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}