pipeline {
    agent any
    
    environment {
        DOCKERHUB_REPO = "abhiram3012/docker-jenkins"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKERHUB_REPO%:%IMAGE_TAG% ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds',
                    usernameVariable: 'USER', passwordVariable: 'PASS')]) {

                    bat """
                    echo %PASS% | docker login -u %USER% --password-stdin
                    """
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                bat "docker push %DOCKERHUB_REPO%:%IMAGE_TAG%"
                bat "docker tag %DOCKERHUB_REPO%:%IMAGE_TAG% %DOCKERHUB_REPO%:latest"
                bat "docker push %DOCKERHUB_REPO%:latest"
            }
        }
    }

    post {
        success {
            echo "Successfully pushed image to Docker Hub!"
        }
    }
}
