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

    stage("SSH Into k8s Server") {
        def remote = [:]
        remote.name = 'K8S master'
        remote.host = '192.168.100.133'
        remote.user = 'root'
        remote.password = 'rootroot'
        remote.allowAnyHosts = true

        stage('Put k8s-spring-boot-deployment.yml onto k8smaster') {
            sshPut remote: remote, from: 'k8s-nginx-deployment.yml', into: '.'
        }

        stage('Deploy spring boot') {
          sshCommand remote: remote, command: "kubectl apply -f k8s-nginx-deployment.yml"
        }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }

  
}
