pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'sushant960kr'
        IMAGE_NAME = 'staragileprojectfinance'
        IMAGE_TAG = 'v1'
        CONTAINER_NAME = 'My-first-containe21211'
        HOST_PORT = '8083'
        CONTAINER_PORT = '8081'
    }

    stages {
        stage('Build Project') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG} ."
                sh "docker images"
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh "docker push ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh "docker rm -f ${CONTAINER_NAME} || true"
                sh "docker run -itd --name ${CONTAINER_NAME} -p ${HOST_PORT}:${CONTAINER_PORT} ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }

    post {
        success {
            echo "✅ Deployment completed successfully!"
        }
        failure {
            echo "❌ Build failed. Please check logs above."
        }
    }
}
