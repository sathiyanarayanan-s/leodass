pipeline {
    agent any
    environment {
        // This tells Jenkins exactly where the cluster "key" is
        KUBECONFIG = "C:/ProgramData/Jenkins/.jenkins/.kube/config"
    }
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
                // We use --kubeconfig to force Jenkins to use the right connection
                bat 'kubectl apply -f deployment.yaml --kubeconfig="%KUBECONFIG%" --validate=false'
                bat 'kubectl rollout restart deployment degree-app --kubeconfig="%KUBECONFIG%"'
            }
        }
    }
}
