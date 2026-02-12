pipeline {
    agent any

    stages {

        stage('Install') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker') {
            steps {
                sh 'docker build -t simple-app:latest .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker stop simple-app || true
                    docker rm simple-app || true
                    docker run -d -p 8085:3000 --name simple-app simple-app:latest
                '''
            }
        }
    }
}

