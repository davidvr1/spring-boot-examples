pipeline {
  agent {
    node {
      label 'centos7'
    }

  }
  stages {
    stage('Checkout') {
      steps {
        git(url: 'https://github.com/davidvr1/spring-boot-examples.git', branch: 'davidvr1_sol', changelog: true, poll: true, credentialsId: 'github')
      }
    }

  }
  environment {
    foo = 'nothing'
  }
}