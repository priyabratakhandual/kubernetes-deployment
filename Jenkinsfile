pipeline {
  agent any

  environment {
    KUBECONFIG = "/var/lib/jenkins/.kube/config"
  }

  stages {
    stage('Checkout Code') {
      steps {
        checkout scm
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh '''
          echo "📦 Deploying to worker-node..."
          kubectl apply -f deployment.yaml
          kubectl apply -f service.yaml
          kubectl apply -f ingress.yaml
          echo "✅ Done deploying to worker-node."
        '''
      }
    }
  }

  post {
    success {
      echo '✅ Successfully deployed on worker-node.'
    }
    failure {
      echo '❌ Deployment failed. Check Jenkins logs.'
    }
  }
}
