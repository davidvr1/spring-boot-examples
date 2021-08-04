pipeline {
  agent none
  stages {
    stage('Checkout Code') {
      steps {
        git(url: 'https://github.com/davidvr1/spring-boot-examples.git', branch: 'davidvr1_sol', changelog: true, poll: true, credentialsId: 'github')
      }
    }

    stage('mvn build') {
      steps {
        sh 'mvn compile'
      }
    }

    stage('test the app') {
      steps {
        sh 'mvn test'
      }
    }

  }
}