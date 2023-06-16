pipeline {
    agent any

    stages {

        stage ("Build Docker Image") {
            steps {
                script {
                    dockerapp = docker.build("pixubm/imersao5-frontend:${env.BUILD_ID}", "-f ./src/Dockerfile.prod ./golang")
                }
            }
        }
    }
}