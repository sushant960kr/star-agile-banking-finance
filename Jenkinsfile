pipeline {
    agent any
    stages{
        stage('build project'){
            steps{
                git url:'https://github.com/sushant960kr/star-agile-banking-finance.git', branch: "master"
                
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t sushant960kr/newimage .'
                    
                }
            }
        }
         
        
     stage('Deploy') {
            steps {
                sh 'sudo docker run -itd --name My-first-containe21211 -p 8083:8081 sushant960kr/newimage .'
                  
                }
            }
        
    }
}
