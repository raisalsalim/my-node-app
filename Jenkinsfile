pipeline {
    agent any
    environment {
        DOCKER_REGISTRY = 'localhost:5000'
    }
    stages {
        stage('Build') {
            steps {
                script {
                    docker.build("${DOCKER_REGISTRY}/my-node-app:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push') {
            steps {
                script {
                    docker.withRegistry("http://${DOCKER_REGISTRY}") {
                        docker.image("${DOCKER_REGISTRY}/my-node-app:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Apply Kubernetes manifests
                    sh 'kubectl apply -f k8s/'
                }
            }
        }
    }
}
