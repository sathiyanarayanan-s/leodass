pipeline {
    agent any
    stages {
        stage('Checkout GITHUB') {
            steps {
                checkout scm
            }
        }
        stage('Create Docker Image') {
            steps {
                sh 'docker build -t sathiyaleo/degree-app:latest .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-id', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push sathiyaleo/degree-app:latest'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
