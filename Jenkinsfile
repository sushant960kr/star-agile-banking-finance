pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'sushant960kr'
        IMAGE_NAME = 'staragileprojectfinance'
        IMAGE_TAG = 'v1'
    }

    stages {
        stage('Clone Project') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-pat', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_TOKEN')]) {
                    git url: "https://${GIT_USER}:${GIT_TOKEN}@github.com/sushant960kr/star-agile-banking-finance.git", branch: 'master'
                }
            }
        }

        stage('Build Project') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG} ."
                    sh "docker images"
                }
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

        stage('Deploy') {
            steps {
                sh "docker rm -f My-first-containe21211 || true"
                sh "docker run -itd --name My-first-containe21211 -p 8083:8081 ${DOCKERHUB_USERNAME}/${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }

    post {
        success {
            echo "✅ Deployment completed successfully!"
        }
        failure {
            echo "❌ Build failed. Please check above logs."
        }
    }
}
