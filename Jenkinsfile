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
        slackSend(channel: 'david-varshoer', message: '${env.JOB_NAME} #${env.BUILD_NUMBER} started GIT checkout (${env.BUILD_URL})')
      }
    }

    stage('mvn build') {
      steps {
        sh '''cd spring-boot-package-war 
echo $BUILD_ID
mvn compile'''
        slackSend(channel: 'david-varshoer', message: 'mvn build ${env.BUILD_NUMBER} successeded')
      }
    }

    stage('test the app') {
      steps {
        sh '''cd spring-boot-package-war 
mvn test'''
        slackSend(channel: 'david-varshoer', message: 'test for ${env.JOB_NAME} #${env.BUILD_NUMBER}  succedded ')
      }
    }

    stage('packging') {
      steps {
        sh '''cd spring-boot-package-war 
mvn clean package'''
        zip(zipFile: 'package.zip', archive: true, overwrite: true)
        archiveArtifacts(artifacts: 'package.zip', onlyIfSuccessful: true)
        cleanWs(cleanWhenSuccess: true)
        slackSend(channel: 'david-varshoer', message: 'artifact is ready!', attachments: 'package.zip')
      }
    }

  }
}