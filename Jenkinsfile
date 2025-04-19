pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'utkarsha-a/portfolio-site'  
        GITHUB_REPO = 'https://github.com/utkarsha-a/devops-portfolio.git'  
    }

    stages {
        stage('Clone Repository') {
            steps {
                git "${GITHUB_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([ credentialsId: 'dockerhub-creds', url: '' ]) {
                    dockerImage.push('latest')
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Ensure kubectl is properly configured (e.g., via service account in Jenkins)
                    sh 'kubectl apply -f k8s/deployment.yaml'
                    sh 'kubectl apply -f k8s/service.yaml'
                }
            }
        }
    }
}
