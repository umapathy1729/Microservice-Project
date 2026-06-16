pipeline {
    agent any 
    environment {
        EC2_IP = "13.206.38.97"
    }
    stages {
        stage('Git checkout code') {
            steps {
                git branch: 'main', 
                credentialsId: 'Github-credentials', 
                url: 'https://github.com/umapathy1729/Microservice-Project.git'
            }
        }
        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2_key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@${EC2_IP} '
                    set -e
                    rm -rf Microservice-Project
                    git clone git@github.com:umapathy1729/Microservice-Project.git

                    cd Microservice-Project
                    git checkout main
                    git pull origin main

                    docker system prune -f &&
                    docker compose down &&
                    docker compose up -d --build

                    '
                    '''
                }
            }
        }
    }
}
