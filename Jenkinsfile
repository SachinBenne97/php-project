pipeline {
    agent any
    stages{
        stage('git cloned'){
            steps{
                git url:'https://github.com/sachinbenne97/php-project/', branch: "master"
              
            }
        }

    stages {
        stage('Build docker image') {
            steps {
                script {
                    sh 'docker build -t sachinbenne97/2febimg:v1 .'
                    sh 'docker images'
                }
            }
        }
        
        stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_id', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push sachinbenne97/2febimg:v1'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    def dockerrm = 'sudo docker rm -f My-first-containe221 || true'
                    def dockerCmd = 'sudo docker run -itd --name My-first-containe221 -p 8082:80 sachinbenne97/2febimg:v1'
                    sshagent(['sshkeypair']) {
                        // Change the private IP in the below code
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.255 ${dockerrm}"
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.255 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
