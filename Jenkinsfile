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
                bat 'docker build -t sathiyaleo/degree-app:latest .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-id', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    bat "echo %PASS% | docker login -u %USER% --password-stdin"
                    bat "docker push sathiyaleo/degree-app:latest"
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                // We add --validate=false to skip the connection error
                bat 'kubectl apply -f deployment.yaml --validate=false'
                bat 'kubectl rollout restart deployment degree-app'
            }
        }
    }
}
