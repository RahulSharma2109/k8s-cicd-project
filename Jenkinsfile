pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sharmaxrahul/node-app"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/RahulSharma2109/k8s-cicd-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest -f docker/Dockerfile .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'PASS')]) {
                    sh 'docker login -u sharmaxrahul -p $PASS'
                    sh 'docker push $DOCKER_IMAGE:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }
    }
}