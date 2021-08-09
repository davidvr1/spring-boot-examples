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
        sh '''cd /home/vagrant/jenkins/workspace/spring-boot-examples_davidvr1_sol/spring-boot-package-war

mvn compile'''
        slackSend(channel: 'david-varshoer', message: 'mvn buil success')
      }
    }

    stage('test the app') {
      steps {
        sh '''cd /home/vagrant/jenkins/workspace/spring-boot-examples_davidvr1_sol/spring-boot-package-war

mvn test

'''
        slackSend(channel: 'david-varshoer', message: 'test succedded')
      }
    }

    stage('packging') {
      steps {
        zip(zipFile: 'package.zip', archive: true, overwrite: true)
        archiveArtifacts(artifacts: 'package.zip', onlyIfSuccessful: true)
        cleanWs(cleanWhenSuccess: true)
        slackSend(channel: 'david-varshoer', message: 'artifact is ready!')
      }
    }

  }
}