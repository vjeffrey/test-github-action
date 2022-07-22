pipeline {
  environment {
    REGISTRY = "docker-example"
  }
  agent any
  stages {
    // stage('Cloning Git') {
    //   steps {
    //     // git 'https://github.com/buildkite/nodejs-docker-example.git'
    //     git 'https://github.com/chris-rock/ci-integration-test'
    //   }
    // }
    // stage('Build') {
    //   steps {
    //     sh 'npm install'
    //   }
    // }
    // stage('Test') {
    //   steps {
    //     sh 'npm test'
    //   }
    // }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build("${REGISTRY}:${env.BUILD_ID}")
        }
      }
    }
    stage('Scan image') {
      environment {
        MONDOO_CLIENT_ACCOUNT = credentials('MONDOO_CLIENT_ACCOUNT')
      }
      steps{
        sh "echo ${MONDOO_CLIENT_ACCOUNT} | base64 -d > mondoo.json"
        sh 'curl -sSL https://mondoo.io/download.sh | bash'
        sh "./mondoo scan -t docker://${REGISTRY}:${env.BUILD_ID} --config mondoo.json"
      }
    }
    // stage('Deploy Image') {
    //   // For a Docker Registry which requires authentication,
    //   // add a "Username/Password" Credentials item from the Jenkins home page and use the
    //   // Credentials ID as a second argument to withRegistry():
    //   environment {
    //     REGISTRY_CREDS = credentials('REGISTRY_CREDS')
    //   }
    //   steps{
    //     script {
    //       docker.withRegistry( '', REGISTRY_CREDS ) {
    //         dockerImage.push()
    //       }
    //     }
    //   }
    // }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi ${REGISTRY}:${env.BUILD_ID}"
      }
    }
  }
}