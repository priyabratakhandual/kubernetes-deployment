pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your/repo.git'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh "kubectl apply -f gym-deployment.yaml"
                    sh "kubectl apply -f gym-service.yaml"
                    sh "kubectl apply -f gym-ingress.yaml"
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployed at http://65.0.96.15.nip.io"
        }
    }
}
