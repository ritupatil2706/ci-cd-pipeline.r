pipeline {
    agent any
    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/ritupatil2706/ci-cd-pipeline.git.r'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ritupatil2706/flask-app:latest ./app'
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push ritupatil2706/flask-app:latest'
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh']) {
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@98.93.171.207 "docker pull ritupatil2706/flask-app:latest && docker rm -f flask-app || true && docker run -d -p 5000:5000 --name flask-app ritupatil2706/flask-app:latest"'
                }
            }
        }
    }
}
