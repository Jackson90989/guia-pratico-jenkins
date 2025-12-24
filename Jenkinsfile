
pipeline {
  agent any

  stages {
    stage('Build Docker Image') {
      steps {
        script {
          // Use aspas duplas para interpolar e corrija o -f
          dockerapp = docker.build(
            "USER/guia-jenkins:${env.BUILD_NUMBER}",
            "-f ./src/Dockerfile ./src"
          )
        }
      }
    }

    stage('Push Docker Image') {
      steps {
        script {
          // Para Docker Hub, você pode usar endpoint vazio ('') com credentialsId 'dockerhub'
          docker.withRegistry('', 'dockerhub') {
            dockerapp.push('latest')
            dockerapp.push("${env.BUILD_NUMBER}")
          }
        }
      }
    }

    stage('Deploy no Kubernetes') {
      steps {
        echo "Executando o comando kubectl apply"
        sh 'echo "Executando o comando kubectl apply"'
      }
    }
  }
}
