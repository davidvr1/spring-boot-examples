pipeline {
  agent {
    node {
      label 'centos7'
    }

  }
  stages {
    stage('') {
      steps {
        git(url: 'https://github.com/davidvr1/spring-boot-examples.git', branch: 'davidvr1_sol', changelog: true, poll: true)
      }
    }

  }
  environment {
    foo = 'nothing'
  }
}