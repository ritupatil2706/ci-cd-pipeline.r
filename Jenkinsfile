pipeline {
  agent any
  environment {
    DOCKERHUB_USER = "<your-dockerhub-username>"
    IMAGE = "${DOCKERHUB_USER}/flask-cicd-demo"
  }
  stages {
    stage('Checkout') {
      steps { git url: 'https://github.com/<your-username>/ci-cd-pipeline.git' }
    }
    stage('Build') {
      steps { sh 'docker build -t ${IMAGE}:$BUILD_NUMBER app/' }
    }
    stage('Login & Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
          sh 'echo $PASS | docker login -u $USER --password-stdin'
          sh 'docker push ${IMAGE}:$BUILD_NUMBER'
          sh 'docker tag ${IMAGE}:$BUILD_NUMBER ${IMAGE}:latest'
          sh 'docker push ${IMAGE}:latest'
        }
      }
    }
    stage('Deploy to EC2') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ssh', keyFileVariable: 'KEYFILE', usernameVariable: 'SSHUSER')]) {
          sh "ssh -o StrictHostKeyChecking=no -i $KEYFILE ${SSHUSER}@<EC2_PUBLIC_IP> 'docker pull ${IMAGE}:$BUILD_NUMBER || true && docker stop cicd_demo || true && docker rm cicd_demo || true && docker run -d --name cicd_demo -p 80:5000 ${IMAGE}:$BUILD_NUMBER'"
        }
      }
    }
  }
  post {
    failure { mail to: 'you@example.com', subject: "Build Failed: ${env.JOB_NAME}", body: "Check Jenkins." }
  }
}
