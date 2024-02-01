pipeline {
  agent {label 'clustermanager'} 
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t vonmarasigan/mywebapp-k8:latest .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push vonmarasigan/mywebapp-k8:latest'
      }
    }
    stage('Deploy') {
            steps {
              script {
                   sh "kubectl apply -f nginx-deployment.yaml"
                }
              }
            }
        }
    }
