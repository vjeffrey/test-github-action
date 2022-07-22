pipeline {
  environment {
    REGISTRY = "docker-example"
  }
  agent any
  stages {

    stage('Scan') {
      environment {
        MONDOO_CLIENT_ACCOUNT = credentials('MONDOO_CLIENT_ACCOUNT')
      }
      steps{
        sh "env | sort"
        sh "echo ${MONDOO_CLIENT_ACCOUNT} | base64 -d > mondoo.json"
        sh 'curl -sSL https://mondoo.io/download.sh | sh'
        sh "./mondoo scan local --config mondoo.json"
      }
    }
  }
}