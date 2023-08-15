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
        sh 'docker build -t nginx:latest .'
      }
    }
    stage('Ajout du tag') {
      steps {
        sh 'docker tag nginx:latest  lahcenaghouar/nginx:1.0.1 '
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push image to docker hub') {
      steps {
        sh 'docker push lahcenaghouar/nginx:1.0.1'
      }
    }

     stage('Copy the file to the master') {
      steps {
        sh 'scp k8s-nginx-deployment.yml index.html  root@192.168.100.133:/root/jenkins/'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }

  
}
