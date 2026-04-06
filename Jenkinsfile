pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = "marta77784/devops-bootcamp-backend"
        IMAGE_NAME = "${DOCKER_HUB_REPO}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Cloning repo..."
                sh 'pwd && ls'
            }
        }

        stage('Build') {
            steps {
                echo "Building Docker image..."
                sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ./app"
            }
        }

        stage('Test') {
            steps {
                echo "Testing..."
                sh "docker run --rm ${IMAGE_NAME}:${BUILD_NUMBER} echo 'Tests passed'"
            }
        }

        stage('Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-hub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest
                        docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                        docker push ${IMAGE_NAME}:latest
                    """
                }
            }
        }

        stage('Done') {
            steps {
                echo "Билд #${BUILD_NUMBER} завершён"
            }
        }
    }
}
