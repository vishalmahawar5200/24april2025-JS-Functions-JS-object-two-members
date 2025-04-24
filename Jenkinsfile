pipeline {
    agent any 

    environment{
        DOCKER_IMAGE = "vishalmahawar5200/24april2025"
    }

    stages {
        stage('Docker Install Dependencies') {
            steps {
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
        //1. Function defination is one time process
        stage('Verify Docker Version') { //'Verify Docker Version' formal argument
            steps {
                sh "docker --version"
            }
        }
        stage('Build Image') {
            steps{
                withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh 'docker build -t vishal:t1 .'
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                }
            }
        }
        stage('Push Docker Image'){
            steps{
                script{
                    withCredentials([usernamePassword(crudentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]){
                        def imageTag = "v$(env.BUILD_NUMBER)"
                        sh "docker tag vishal:t1 $DOCKER_IMAGE:${imageTag}"
                        sh "docker push $DOCKER_IMAGE:${imageTag}"   
                    }
                }
            }
        }

    } 
} 