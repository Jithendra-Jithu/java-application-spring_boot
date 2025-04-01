pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'rohith1305/java-application:latest'
        DOCKER_CREDENTIALS = '7c910bc4-e2e4-48a4-857c-51be93277e96'  // Jenkins Credentials ID for Docker login
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // Checkout the code from GitHub repository
                    git branch: 'main', url: 'https://github.com/KyathamRohith/java-application.git'
                }
            }
        }

        stage('Build with Maven') {
            steps {
                script {
                    // Build the application with Maven (skip tests)
                    sh 'mvn clean package -DskipTests=true'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub using Jenkins credentials
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin"
                    }

                    // Push the Docker image to the Docker Hub repository
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }
    }

    post {
        // Clean up Docker images to free up space after the pipeline runs
        always {
            sh 'docker system prune -f'
        }
    }
}
