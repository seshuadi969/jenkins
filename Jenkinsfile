pipeline {
    agent any

    environment {
        IMAGE_NAME = 'python-docker-app'
        DOCKER_HUB_USER = 'yourdockerhubusername'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'a51f3210-e545-41a5-ad60-1e441c0ceecb', url: 'https://github.com/seshuadi969/jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_HUB_USER}/${IMAGE_NAME}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-creds-id', url: '']) {
                    script {
                        dockerImage.push("latest")
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh 'docker rm -f python-app || true'
                    sh 'docker run -d --name python-app -p 5000:5000 ${DOCKER_HUB_USER}/${IMAGE_NAME}:latest'
                }
            }
        }
    }
}
