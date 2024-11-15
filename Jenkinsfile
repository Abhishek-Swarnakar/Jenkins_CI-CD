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

                    sh '''
                    ContainerId=$(docker ps -q)
                    ImageId=$(docker images -q)
                    if [ -n "$ContainerId" ] && [ -n "$ImageId" ]; then
                        echo "STOPPING THE CONTAINER: $ContainerId"
                        docker stop $ContainerId
                        

                        if [ $? -eq 0 ]; then
                            echo "REMOVING THE CONATINER: $ContainerId"
                            docker rm $ContainerId
                            echo "IMAGE ID IS: $ImageId"
                            docker rmi -f $ImageId
                            
                        fi
                    else
                        echo "NO RUNNING CONTAINER & IMAGES!!!"
                    fi
                    '''

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
        
    }
}
