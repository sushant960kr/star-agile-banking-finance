pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sushant960kr/staragile:v1"
        GIT_REPO = "https://github.com/sushant960kr/star-agile-banking-finance.git"
        CONTAINER_NAME = "my-first-container"
    }

    stages {
        stage('Git Clone') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-creds', passwordVariable: 'GIT_PASS', usernameVariable: 'GIT_USER')]) {
                    git url: "https://${GIT_USER}:${GIT_PASS}@github.com/sushant960kr/star-agile-banking-finance.git", branch: "master"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t sushant960kr/staragile:v1 ."
                    sh "docker images"
                }
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo \$PASS | docker login -u \$USER --password-stdin"
     		    

                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh 'docker push sushant960kr/staragile:v1'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    

                    // Run new container
                    
		    sh 'sudo docker run -d --name my-first-container -p 8083:8080 sushant960kr/staragile:v1'
                }
            }
        }
    }
}
