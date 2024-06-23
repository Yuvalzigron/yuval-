pipeline {
    agent {
        kubernetes {
            yamlFile 'pod.yaml'
            defaultContainer 'ez-docker-helm-build'
        }
    }
    environment {
        DOCKER_IMAGE = 'yuvalzigron/jenkins_images'
    }
    stages {
        stage('Build docker image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:latest", "--no-cache .")
                }
            }
        }
        stage('Push image to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-creds') {
                        dockerImage.push("latest")
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
