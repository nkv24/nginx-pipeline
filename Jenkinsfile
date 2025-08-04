pipeline {
    agent any
    environment {
        IMAGE_NAME = "nginx-local"
        IMAGE_TAG = "v1"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url:  'https://github.com/nkv24/nginx-pipeline.git' // Change to your repo
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Just build normally; host and minikube share Docker daemon
                    sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Ensure your image name and tag match in deployment.yaml
                    sh 'kubectl apply -f /k8s/nginx-deployment.yaml --validate=false'
                }
            }
        }

        stage('Check Deployment') {
            steps {
                script {
                    sh 'kubectl get pods'
                    sh 'kubectl get svc'
                }
            }
        }
    }
}

