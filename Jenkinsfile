pipeline {
    agent {
        dockerfile true
    }
    environment {
        DOCKER_REGISTRY = 'https://hub.docker.com/u/jorgedevops20' 
        DOCKER_IMAGE = "${DOCKER_REGISTRY}/frontend-challenge-base" 
        ARGOCD_APP = 'frontend-challenge-base' 
    }
    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning Git repository...'
                git url: 'https://github.com/Jorge-DevOps/frontend-challenge-base', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t ${DOCKER_IMAGE}:latest .'
            }
        }
        stage('Test Docker Image') {
            steps {
                echo 'Testing Docker image...'
                sh '''
                docker run --rm ${DOCKER_IMAGE}:latest npm test
                '''
            }
        }
        stage('Push Docker Image') {
            steps {
                echo 'Pushing Docker image to registry...'
                withDockerRegistry([credentialsId: 'docker-registry-credentials', url: "https://${DOCKER_REGISTRY}"]) {
                    sh 'docker push ${DOCKER_IMAGE}:latest'
                }
            }
        }
        stage('Deploy to Kubernetes with ArgoCD') {
            steps {
                echo 'Deploying application using ArgoCD...'
                sh '''
                argocd app sync ${ARGOCD_APP}
                argocd app wait ${ARGOCD_APP} --timeout 300
                '''
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
