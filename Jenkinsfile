pipeline {
  environment {
    REGISTRY = "docker-example"
  }
  agent any
  stages {

    stage('Building image') {
      steps{
        script {
          dockerImage = image 'node:16.13.1-alpine' }
        }
      }
    stage('Scan image') {
      environment {
        MONDOO_CLIENT_ACCOUNT = credentials('MONDOO_CLIENT_ACCOUNT')
      }
      steps{
        sh "echo ${MONDOO_CLIENT_ACCOUNT} | base64 -d > mondoo.json"
        sh 'curl -sSL https://mondoo.io/download.sh | bash'
        sh "./mondoo scan -t docker --config mondoo.json"
      }
    }
  }
}