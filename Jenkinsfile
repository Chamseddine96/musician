pipeline {
    agent any
    tools {
             nodejs 'NodeJS'
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
       

    post {
        success {
            echo 'Deployment completed successfully!'
        }
        failure {
            echo 'Deployment failed. Please check the logs.'
        }
    }
}