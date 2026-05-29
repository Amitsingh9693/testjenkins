pipeline {

    agent any

    environment {
        IMAGE_NAME = "YOUR_DOCKERHUB_USERNAME/jenkins-demo"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/YOUR_GITHUB_USERNAME/jenkins-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {

                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {

                        docker.image("${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {

                sh '''
                docker stop myapp || true
                docker rm myapp || true

                docker run -d \
                  --name myapp \
                  -p 3000:3000 \
                  ${IMAGE_NAME}:latest
                '''
            }
        }
    }
}