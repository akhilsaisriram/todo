pipeline {
    agent any

    environment {
        // Define the Docker image you want to use for building and deploying
        DOCKER_IMAGE = 'newto'
        DOCKER_IMAGE_TAG = 'latest'
    }

    stages {
        stage('Install') {
            steps {
                script {
                    // Install dependencies, if any
                    echo 'Installing dependencies...'
                    sh 'docker --version' // Verify Docker installation
                }
            }
        }

        stage('Stop Services Using Port 80') {
            steps {
                script {
                    echo 'Stopping any service using port 80...'
                    // Stop services using port 80
                    sh '''#!/bin/bash
                    sudo lsof -t -i :80 | xargs -r sudo kill -9
                    '''
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building the Docker image...'
                    // Build the Docker image from Dockerfile
                    sh 'docker build -t $DOCKER_IMAGE:$DOCKER_IMAGE_TAG .'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying the Docker image...'
                    // Run the Docker container
                    sh 'docker run -d -p 80:5000 $DOCKER_IMAGE:$DOCKER_IMAGE_TAG'
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            // Stop containers only if they exist
            sh '''#!/bin/bash
            CONTAINERS=$(docker ps -q)
            if [ -n "$CONTAINERS" ]; then
                docker stop $CONTAINERS
            fi
            docker system prune -f  // Clean up unused Docker resources
            '''
        }
    }
}
