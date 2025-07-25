pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = credentials('dockerhub-username')
        DOCKERHUB_PASSWORD = credentials('dockerhub-password')
        IMAGE_NAME = "${DOCKERHUB_USERNAME}/static-site"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/siri666519/static-site-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                sh 'echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker stop static-site || true
                    docker rm static-site || true
                    docker run -d -p 8081:80 --name static-site $IMAGE_NAME:latest
                '''
            }
        }
    }
}

