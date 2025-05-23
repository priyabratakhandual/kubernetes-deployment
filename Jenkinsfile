pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/priyabratakhandual/kubernetes-deployment.git'
            }
        }

        stage('Grant Ingress RBAC Access') {
            steps {
                script {
                    writeFile file: 'ingress-nginx-rbac.yaml', text: '''\
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: ingress-nginx-rolebinding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: ingress-nginx
    namespace: ingress-nginx
'''
                    sh 'kubectl apply -f ingress-nginx-rbac.yaml'
                }
            }
        }

        stage('Install Ingress NGINX on Worker') {
            steps {
                script {
                    sh 'kubectl create namespace ingress-nginx || true'
                    sh 'kubectl delete deployment ingress-nginx-controller -n ingress-nginx || true'
                    sh 'sleep 5'
                    sh 'kubectl apply -f nginx-ingress-deployment.yaml'
                    sh 'kubectl apply -f nginx-ingress-service.yaml'
                    sh 'kubectl rollout status deployment ingress-nginx-controller-v2 -n ingress-nginx'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh 'kubectl apply -f gym-deployment.yaml'
                    sh 'kubectl apply -f gym-service.yaml'
                    sh 'kubectl apply -f gym-ingress.yaml'
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployed! Access the app at: http://65.0.96.15.nip.io"
        }
        failure {
            echo "❌ Deployment failed. Please check the logs."
        }
    }
}
