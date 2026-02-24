pipeline {
    agent any

    environment {
        IMAGE_NAME = "pavankumar213/node-auth-app"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/pavankumar90554/node-auth-devops.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    docker.image("${IMAGE_NAME}:${IMAGE_TAG}").push()
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully 🚀'
        }
        failure {
            echo 'Pipeline failed ❌'
        }
    }
}
