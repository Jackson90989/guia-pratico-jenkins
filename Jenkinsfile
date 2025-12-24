
pipeline {
  agent any

  stages {
    stage('Build Docker Image') {
      steps {
        echo "Executando o comando Docker Build"
        sh 'echo "Executando o comando Docker Build"'
      }
    }

    stage('Push Docker Image') {
      steps {
        echo "Executando o comando Docker Push"
        sh 'echo "Executando o comando Docker Push"'
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
