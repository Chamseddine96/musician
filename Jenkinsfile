pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        DOCKER_HUB_CREDENTIALS_ID = 'dockerhub-jenkins-token'
        DOCKER_HUB_REPO = 'chamseddine96/music'
        VERSION = "${BUILD_NUMBER}"  // Use Jenkins BUILD_NUMBER as the version
        SONAR_PROJECT_KEY = 'music'
	SONAR_SCANNER_HOME = tool 'SonarQubeScanner'


    }

    stages {
        stage('Checkout Code') {
            steps {
               git branch: 'main', credentialsId: 'jen-doc-git', url: 'https://github.com/Chamseddine96/musician.git' 
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh 'npm install' // Install Node.js dependencies
                }
            }
        }
     
     

        stage('SonarQube Analysis'){
			steps {
				withCredentials([string(credentialsId: 'music-token', variable: 'SONAR_TOKEN')]) {
				   
					withSonarQubeEnv('SonarQube') {
						sh """
                  				${SONAR_SCANNER_HOME}/bin/sonar-scanner \
                  				-Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                    				-Dsonar.sources=. \
                   				-Dsonar.host.url=http://172.17.0.1:9090 \
                    				-Dsonar.login=${SONAR_TOKEN}
                    				"""
					}	
				}
			}
		}

	

        stage('Build Docker Image') {
            steps {
                script {
                    // Build and tag the image with the version and latest
                    dockerImage = docker.build("${DOCKER_HUB_REPO}:${VERSION}")
                    dockerImage.tag('latest')  // Tag the same image as 'latest'
                }
            }
        }


        stage('Push Image to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', "${DOCKER_HUB_CREDENTIALS_ID}") {
                        // Push both the versioned and the latest tag
                        dockerImage.push("${VERSION}")  // Push versioned image
                        dockerImage.push('latest')       // Push 'latest' tag
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
