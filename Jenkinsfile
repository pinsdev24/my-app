pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/pinsdev24/my-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('my-app')
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com',  'docker-hub-credentials') {
                        docker.image('my-app').push('latest')
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    kubernetesDeploy(
                        kubeconfigId: 'kubeconfig',
                        configs: 'k8s-deployment.yaml',
                        enableConfigSubstitution: true
                    )
                }
            }
        }
    }
}