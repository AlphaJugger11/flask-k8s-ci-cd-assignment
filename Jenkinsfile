pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-k8s-ci-cd-assignment:latest .'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
        stage('Verify Deployment') {
            steps {
                sh 'kubectl rollout status deployment/flask-deployment'
                sh 'kubectl get pods'
            }
        }
    }
}
