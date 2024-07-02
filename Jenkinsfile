pipeline {
    agent any
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
                    kubernetesApply(
                        kubeconfigId: 'kubeconfig-id',
                        configs: 'k8s-deployment.yaml'
                    )
                }
            }
        }
    }
}
