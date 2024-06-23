pipeline {
    agent {
        kubernetes {
            yamlFile 'pod.yaml'
        }
    }
    environment {
        DOCKER_IMAGE = 'yuvalzigron/jenkins_images'
        GITHUB_API_URL = 'https://api.github.com'
        GITHUB_REPO = 'yuvalzigron/yuval-'
        GITHUB_TOKEN = credentials('github-creds')
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
                stage('Push Docker image') {
            when {
                branch 'main'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-creds') {
                        dockerImage.push("latest")
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
