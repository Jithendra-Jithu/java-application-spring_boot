pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'jithu145/java-application:latest'
        DOCKER_CREDENTIALS = 'fb16b1ba-d2e9-41bb-8654-d00d3b5b61e6'  // Jenkins Credentials ID for Docker login
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // Checkout the code from GitHub repository
                    git branch: 'main', url: 'git@github.com:Jithendra-Jithu/java-application-spring_boot.git'
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
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS, passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        // Login to Docker Hub (no need to specify the registry URL, it's the default)
                        sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                        
                        // Push the Docker image to the Docker Hub repository
                        sh "docker push ${DOCKER_IMAGE}"
                    }
                }
            }
        }
    }
}
