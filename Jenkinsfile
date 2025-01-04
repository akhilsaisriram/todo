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
            sh '''#!/bin/bash
            # Check if any container is using port 80 and remove it
            CONTAINER_ID=$(sudo lsof -t -i :80)
            if [ -n "$CONTAINER_ID" ]; then
                sudo docker rm -f $CONTAINER_ID
                echo "Stopped the container using port 80"
            else
                echo "No container running on port 80"
            fi
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


}
