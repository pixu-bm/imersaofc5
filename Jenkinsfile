pipeline {
    agent any

    stages {

        stage ("Build Docker Image") {
            steps {
                script {
                    dockerapp = docker.build("pixubm/imersao5-frontend:${env.BUILD_ID}", "-f ./nextjs/Dockerfile.prod ./nextjs")
                }
            }
        }

        stage ("Push Docker Image") {
            steps {
                script {
                    docker.withRegistry("https://registry.hub.docker.com", "dockerhub_credentials") {
                        dockerapp.push("latest")
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        stage ("Deploy Kubernetes") {
            environment {
                tag_version = "${env.BUILD_ID}"
            }
            steps {
                script {
                    withKubeConfig([credentialsId: "kubeconfig_secret"]) {
                        sh "sed -i 's/{{TAG}}/$tag_version/g' ./k8s/nextjs/deployment.yaml"
                        sh "kubectl apply -f ./k8s/nextjs/deployment.yaml"
                    }
                }
            }
        }
    }
}