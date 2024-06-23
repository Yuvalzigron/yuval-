pipeline {
    agent any 
    environment {
        DOCKER_IMAGE_NAME = "your-dockerhub-username/your-image-name"
        DOCKER_IMAGE_TAG = "latest"
    }
    stages {
        stage('Build docker image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}")
                }
            }
        }
        stage('Push image to DockerHub') {
            steps {
                script {
                    // Push the Docker image to DockerHub
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                        docker.image("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
    post {
        success {
            echo "Docker image successfully added to DockerHub"
        }
        failure {
            echo "Failed to build the image and push it to DockerHub"
        }
    }
}
