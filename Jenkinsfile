pipeline {
    agent any
    tools {
        nodejs 'NodeJS'
    }
    environment {
        DOCKER_HUB_CREDENTIALS_ID = 'dockerhub-jenkins-token'
        VERSION = "${BUILD_NUMBER}"
        DOCKER_HUB_REPO = 'chamseddine96/music'
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: 'jen-doc-git', url: 'https://github.com/Chamseddine96/musician.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                     dockerImage = docker.build("${DOCKER_HUB_REPO}:${VERSION}")
                    dockerImage.tag('latest')
                }
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                script {
                     docker.withRegistry('https://registry.hub.docker.com', "${DOCKER_HUB_CREDENTIALS_ID}") {
                        dockerImage.push("${VERSION}")  
                        dockerImage.push('latest')  
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully!'
        }
        failure {
            echo 'Deployment failed. Please check the logs.'
        }
    }
}


