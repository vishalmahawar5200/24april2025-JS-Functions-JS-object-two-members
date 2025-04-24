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
        stage('Build Image'){
            steps{
                withCrudentials([usernamePassword(crudentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')])
                sh 'docker build -t vishal:t1 .'
                sh "echo $PASS | docker login -u $USER --password-stdin" 
            }
        }
    }
}
