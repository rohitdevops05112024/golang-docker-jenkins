pipeline {
    agent {
        docker { image 'golang:1.18' }  // Using Golang base image
    }
    environment {
        DOCKER_IMAGE = 'rohit/employee-app'  // Docker image name (update as needed)
        DOCKER_REGISTRY = 'docker.io'  // Docker registry (default is Docker Hub)
        GITHUB_REPO = 'https://github.com/rohitdevops05112024/golang-docker-jenkins.git'  // GitHub repo URL
        BRANCH = 'master'  // Branch to checkout
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the GitHub repository
                git branch: "${BRANCH}", url: "${GITHUB_REPO}"
            }
        }

        stage('Install make') {
            steps {
                script {
                    // Install make if it's not available in the container
                    sh 'apt-get update && apt-get install -y make'
                }
            }
        }

        stage('Build Base Image') {
            steps {
                script {
                    // Run `make build-base`
                    sh 'make build-base'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run tests
                    sh 'go test ./...'
                }
            }
        }

        stage('Build Image') {
            steps {
                script {
                    // Build Docker image
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                }
            }
        }

        stage('Publish Docker Image') {
            steps {
                script {
                    // Push Docker image to registry
                    sh 'docker push ${DOCKER_IMAGE}'
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    // Clean up generated binaries and Docker images
                    sh 'rm -rf employee-app'
                    sh 'docker rmi ${DOCKER_IMAGE}'
                }
            }
        }
    }
    post {
        always {
            // Clean up workspace
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
