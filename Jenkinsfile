pipeline {
    environment {
        registry = "sachinbenne" // Docker registry username
        registryCredential = 'dockerhub-pwd' // Jenkins credentials ID for Docker Hub
        dockerImage = '' // This will hold the image tag
    }
    agent any
    stages {
        stage('Cloning our Git') {
            steps {
                git 'https://github.com/SachinBenne97/php-project'
            }
        }
        stage('Building our image') {
            steps{
                script {
                    // Build Docker image with the tag using BUILD_NUMBER
                    dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
                }
            }
        }
        stage('Deploy our image') {
            steps{
                script {
                    // Push Docker image to the Docker registry
                    docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Cleaning up') {
            steps{
                // Clean up by removing the Docker image
                sh "docker rmi ${registry}:${BUILD_NUMBER}"
            }
        }
    }
}
