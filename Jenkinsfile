pipeline {
    agent any

    stages {
        stage("start minikube") {
            steps {
                echo "starting minikube" 
                bat 'minikube start --driver=hyperv'
            }
        }

        stage('build docker image to minikube') {
            steps {
                echo "building docker image insider minikube docker daemon" 
                bat '''
                minikube -p minikube docker-env --shell cmd > minikube-env.bat 
                call minikube-env.bat 
                docker build -t flask-k8s-ci-cd-assignment:latest "%WORKSPACE%" 
                docker images 
                '''
            }
        }

    }

}
