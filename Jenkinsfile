pipeline {
    agent any

    environment {
        DEPLOY_YAML = 'deployment.yaml'
        SERVICE_YAML = 'service.yaml'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/priyabratakhandual/kubernetes-deployment.git'  // Replace with actual repo
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh "kubectl apply -f ${DEPLOY_YAML}"
                    sh "kubectl apply -f ${SERVICE_YAML}"  // Optional
                }
            }
        }

        stage('Check Deployment') {
            steps {
                sh 'kubectl get pods -o wide'
            }
        }
    }
}
