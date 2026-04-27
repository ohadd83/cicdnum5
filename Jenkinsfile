pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: node
    image: node:18
    command:
    - cat
    tty: true

  - name: docker
    image: docker:24
    command:
    - cat
    tty: true
    volumeMounts:
    - name: docker-sock
      mountPath: /var/run/docker.sock

  volumes:
  - name: docker-sock
    hostPath:
      path: /var/run/docker.sock
"""
    }
  }

  stages {

    stage('Install') {
      steps {
        container('node') {
          sh 'npm install'
        }
      }
    }

    stage('Test') {
      steps {
        container('node') {
          sh 'npm test'
        }
      }
    }

    stage('Build Docker') {
      steps {
        container('docker') {
          sh 'docker build -t simple-app:latest .'
        }
      }
    }

    stage('Deploy') {
      steps {
        container('docker') {
          sh '''
            docker stop simple-app || true
            docker rm simple-app || true
            docker run -d -p 8087:3000 --name simple-app simple-app:latest
          '''
        }
      }
    }
  }

  post {
    always {
      deleteDir() // instead of cleanWs (no plugin needed)
    }
  }
}

