pipeline {
  agent {
    node {
      label 'centos7'
    }

  }
  stages {
    stage('Checkout Code') {
      steps {
        git(url: 'https://github.com/davidvr1/spring-boot-examples.git', branch: 'davidvr1_sol', changelog: true, poll: true, credentialsId: 'github')
        slackSend(channel: 'david-varshoer', message: 'checkout success')
      }
    }

    stage('mvn build') {
      steps {
        sh 'mvn compile'
        slackSend(channel: 'david-varshoer', message: 'mvn buil success')
      }
    }

    stage('test the app') {
      steps {
        sh 'mvn test'
        slackSend(channel: 'david-varshoer', message: 'test succedded')
      }
    }

  }
}