pipeline {
    agent any
    stages{
        stage('build project'){
            steps{
                git branch: 'master', url: 'https://github.com/sushant960kr/repository-name.git'
                sh 'mvn clean package'
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t sushant960kr/staragileprojectfinance:v1 .'
                    sh 'docker images'
                }
            }
        }
        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh "docker push sushant960kr/staragileprojectfinance:v1"
                }
            }
        }
         
        
     stage('Deploy') {
            steps {
                sh 'sudo docker run -itd --name My-first-containe21211 -p 8083:8081 sushant960kr/staragileprojectfinance:v1'
                  
                }
            }
        
    }
}
