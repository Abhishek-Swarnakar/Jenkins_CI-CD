pipeline {
    agent any
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    stages {
        stage('Checkout SCM') {
            steps {
                git url: 'https://github.com/Abhishek-Swarnakar/Jenkins_CI-CD', branch: 'main'
            }
        }

        stage('Build front-end Docker Image') {
            steps {
                script {
                    sh 'docker build -t abhishekswarnakar/front-end_image:${BUILD_NUMBER} -f ./Front-End/Dockerfile ./Front-End'
                    //sh 'docker push abhishekswarnakar/front-end_image:${BUILD_NUMBER}'
                }
            }
        }

        stage('Run front-end Docker image') {
            steps {
                script {
                    // Stop and remove old container if it exists
                    sh 'docker stop front-end_container_${BUILD_NUMBER} || true'
                    sh 'docker rm front-end_container_${BUILD_NUMBER} || true'

                    // Run new container with a unique name
                    sh 'docker run -d --name front-end_container_${BUILD_NUMBER} -p 8000:8000 abhishekswarnakar/front-end_image:${BUILD_NUMBER}'
                }
            }
        }
    }

    post {
        success {
            echo "PIPELINE FINISHED!!!! THE DOCKER CONTAINER IS RUNNING!!"
        }
        failure {
            echo "PIPELINE FAILED!!!! CHECK THE LOGS FOR DETAILS."
        }
        always {
            script {
                try {
                    // This part is useful if you want to clean up any container created during the build, even if it failed
                    sh 'docker stop front-end_container_${BUILD_NUMBER} || true'
                    sh 'docker rm front-end_container_${BUILD_NUMBER} || true'
                } catch (Exception e) {
                    echo "Error stopping/removing container: ${e}"
                }
            }
        }
    }
}
