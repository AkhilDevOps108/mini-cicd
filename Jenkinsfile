pipeline {
    agent any
    
    environment {
        IMAGE = "yourdockerhubusername/miniapp:latest"
    }

    stages {
        stage('Clone') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE} ."
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh "docker push ${IMAGE}"
                }
            }
        }

        stage('Deploy (Local)') {
            steps {
                sh """
                docker rm -f miniapp || true
                docker pull ${IMAGE}
                docker run -d --name miniapp -p 80:3000 ${IMAGE}
                """
            }
        }
    }
}
