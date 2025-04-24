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
      stage('Start Docker Daemon (if not running)') {
            steps {
                sh '''
                    if ! pgrep dockerd > /dev/null; then
                        echo "Starting Docker daemon..."
                        nohup dockerd > /tmp/dockerd.log 2>&1 &
                        sleep 10
                    else
                        echo "Docker daemon is already running"
                    fi
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
                withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                sh 'docker build -t vishal:t1 .'
                sh "echo $PASS | docker login -u $USER --password-stdin" 
            }
        }
    }
}
