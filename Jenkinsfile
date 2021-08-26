pipeline {
  agent {
    node {
      label 'centos7'
    }

  }
  stages {
    stage('error') {
      steps {
        git(url: 'https://github.com/davidvr1/spring-boot-examples.git', branch: 'davidvr1_sol', changelog: true, poll: true, credentialsId: 'davidvr1_sol')
      }
    }

  }
  environment {
    foo = 'nothing'
  }
}