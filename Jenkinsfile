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
                withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
                    sh '''
                        # Debug information
                        kubectl --kubeconfig="$KUBECONFIG" version --client
                        kubectl --kubeconfig="$KUBECONFIG" get nodes
        
                        # Actual deployment
                        kubectl --kubeconfig="$KUBECONFIG" apply -f k8s-deployment.yaml
                    '''
                }
            }
        }
    }
}
