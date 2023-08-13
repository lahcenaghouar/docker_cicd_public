pipeline {
  agent { label 'linux' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('ACCES_DOCKERHUB')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t darinpope/dp-alpine:latest .'
      }
    }
    stage('Ajout du tag') {
      steps {
        sh 'docker tag darinpope/dp-alpine:latest  lahcenaghouar/docker_cicd_1:1.0.0 '
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push lahcenaghouar/docker_cicd_1:1.0.0'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
