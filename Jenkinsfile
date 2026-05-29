// pipeline {

//     agent any

//     environment {
//         IMAGE_NAME = 'amit9693/jenkins-demo'
//         CONTAINER_NAME = 'myapp'
//     }

//     stages {

//         stage('Checkout') {
//             steps {
//                 checkout scm
//             }
//         }

//         stage('Build Docker Image') {
//             steps {
//                 echo 'Building Docker image...'
//                 sh "docker build -t ${IMAGE_NAME}:latest ."
//             }
//         }

//         stage('Deploy Container') {
//             steps {
//                 echo 'Deploying container...'

//                 sh """
//                 docker stop ${CONTAINER_NAME} || true
//                 docker rm ${CONTAINER_NAME} || true

//                 docker run -d \
//                   --name ${CONTAINER_NAME} \
//                   -p 3000:3000 \
//                   ${IMAGE_NAME}:latest
//                 """
//             }
//         }

//         stage('Cleanup') {
//             steps {
//                 echo 'Cleaning dangling Docker images...'
//                 sh 'docker image prune -f'
//             }
//         }
//     }

//     post {

//         success {
//             echo '✅ Pipeline completed successfully!'
//         }

//         failure {
//             echo '❌ Pipeline failed! Please check console logs.'
//         }

//         always {
//             echo 'Pipeline execution finished.'
//         }
//     }
// }



pipeline {

    agent any

    environment {
        IMAGE_NAME = "amit9693/jenkins-demo"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/Amitsingh9693/testjenkins.git'
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