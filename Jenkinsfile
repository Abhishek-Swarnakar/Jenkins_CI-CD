pipeline {
    agent any
    stages {
        stage('Git-Hub as SourceCode') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/Abhishek-Swarnakar/Jenkins_CI-CD'
            }
        stage('Build front-end Docker Image') {
            steps {
                script{
                    sh 'docker build -t abhishekswarnakar/front-end_image:latest ./Front-end/Dockerfile ./Front-end'
                    sh 'docker push abhishekswarnakar/front-end_image:latest'
                }
            }
        }
        stage('Run front-end Docker image') {
            agent any
            steps {
                script {
                    sh 'docker pull abhishekswarnakar/front-end_image:latest'
                    sh 'docker run -d --name front-end_container -p 8000:8000 abhishekswarnakar/front-end_image:latest '
                    
                }
            }
            
        }

            post {
    
                success {
                    echo "PIPELINE FINISHED!!!! THE DOCKER CONTAINER IS RUNNING!!"
                }
                failure {
                    sh 'docker stop front-end_conatiner || true'
                    sh 'docker rm front-end_conatiner || true'
                }
            }
        }
    }
}
