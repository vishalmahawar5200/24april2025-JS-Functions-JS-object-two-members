pipeline {
    agent any 
    stages{
        stage('Docker Install Dependencies'){
            steps{
                sh '''
                    apt update 
                    apt install -y docker.io sudo
                '''
            }  
        }
        stage('Verify Docker Version'){
            steps{
                sh "docker --version"
            }
            
        }
    }
}
