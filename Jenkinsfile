pipeline {
    agent any
    environment {
        registry = "theodora19/cicd-demo"
        registryCredential = 'dockerhub'
        dockerImage = ''
        DOCKER_HOST = 'tcp://localhost:2375'
    }
    stages {
        stage('Cloning Git') {
            steps {
                git 'git branch: 'main', url: 'https://github.com/TheodoraDsouza/cicd-demo.git''
            }
        }
        stage('Building Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${registry}")
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    bat "docker run --rm -v //var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latest image ${registry}"
                }
            }
        }
        stage('Deploying Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Clean up') {
            steps {
                bat "docker rmi ${registry}"
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}