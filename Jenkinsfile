pipeline {
    agent any
    environment {
        KUBECONFIG = credentials('kubeconfig')
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/pinsdev24/my-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def app = docker.build('my-app')
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        docker.image('prestiliendocker/my-app').push('latest')
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh """
                        echo "KUBECONFIG: ${KUBECONFIG}"
                        ls -l ${KUBECONFIG}
                        kubectl version --client
                        export KUBECONFIG=${KUBECONFIG}
                        kubectl get nodes
                        kubectl apply -f k8s-deployment.yaml
                    """
                }
            }
        }
    }
}
