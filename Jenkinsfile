
pipeline {
  agent { label 'linux' } // garante agente com Docker instalado

  environment {
    IMAGE_REPO = 'fabricio/guia-jenkins'
    TAG        = "${env.BUILD_NUMBER}"    // usa número sequencial
  }

  stages {

    stage('Build Docker Image') {
      steps {
        script {
          // Se seu Dockerfile está em ./src:
          // -f <Dockerfile> e contexto ./src
          def dockerapp = docker.build("${IMAGE_REPO}:${TAG}", "-f ./src/Dockerfile ./src")
          // Exporta para usar nos próximos stages
          // (em Scripted, você pode jogar para variável global; no Declarative, pode reusar via env ou re-build)
          // Para reusar, uma opção simples é reconstituir pelo nome nos próximos stages:
          // ex.: docker.image("${IMAGE_REPO}:${TAG}")
        }
      }
    }

    stage('Push Docker Image') {
      steps {
        script {
          docker.withRegistry('', 'dockerhub') { // ou 'https://index.docker.io/v1/'
            // Reobtém a referência da imagem
            def dockerapp = docker.image("${IMAGE_REPO}:${TAG}")
            // Push da tag específica e da 'latest'
            dockerapp.push("${TAG}")
            dockerapp.push('latest')
          }
        }
      }
    }

    stage('Deploy no Kubernetes') {
      steps {
        script {
          echo "Atualizando imagem no Deployment e verificando rollout"
          // Exemplo de substituição de imagem
          sh """
            kubectl -n sua-namespace set image deployment/minha-app \
              minha-app=${IMAGE_REPO}:${TAG}

            kubectl -n sua-namespace rollout status deployment/minha-app --timeout=120s
          """
          // OU, se você usa manifests:
          // sh 'kubectl -n sua-namespace apply -f k8s/'
        }
      }
    }
  }
}
